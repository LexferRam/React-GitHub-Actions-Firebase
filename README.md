# GitHub Actions with Firebase

1. Crear proyecto en firebase(tener instalado firebase cli: firebase --version)
2. Crear repositorio en github
3. firebase login
4. firebase init 
5. (seleccionar **hosting**)
6. seleccionar **use an existing project**
7. seleccionar proyecto creado en el paso 1
8. ejecutar: firebase deploy
9. Agregar en el .gitignore : *.cache
10. Obtener token de firebase para agreagrlo en secrets de github: firebase login:ci, en settings> secrets> new repository secret
11. En repo de gitlab seleccionar tab 'actions' y seleccionar crear personalizado, y agregar el siguiente codigo en un archivo main.yml: 
    
```yml
# This is a basic workflow to help you get started with Actions

name: Build and Deploy to Firebase

# Controls when the workflow will run
on:
  # Triggers the workflow on push or pull request events but only for the master branch
  push:
    branches: [ master ]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  build:
    name: Build
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      - name: Checkout repository
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
        uses: actions/checkout@v2

      # Runs a single command using the runners shell
      - name: Install dependencies
        run: npm ci

      # Runs a set of commands using the runners shell
      - name: Build dependencies
        run: npm run build
        
      - name: Archive production artifact
        uses: actions/upload-artifact@main
        with:
          name: build
          path: build
        
  deploy:
    name: Deploy
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
        uses: actions/checkout@v2
      - name: Download artifact
        uses: actions/download-artifact@main
        with:
          name: build
          path: build
      - name: Deploy to firebase
        uses: w9jds/firebase-action@master
        with:
          args: deploy 
        env:
          FIREBASE_TOKEN: ${{secrets.FIREBASE_TOKEN}}

```
   
10. Actulizar nuestro repo local, y ahora cada vez que hagamos push a la rama master de github, automaticamente se hara el build y el deploy a firebase