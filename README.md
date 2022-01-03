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
   