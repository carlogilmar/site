+++
date = "2016-09-26T21:22:27-05:00"
title = "Git para Principiantes"
categories= ['git']
tags = ['programming']
+++

---
# Para iniciar...

Recientemente he descubierto el gran y amplio mundo que implica el uso de la herramienta Git para proyectos de software.

Aún buscando entre tutoriales, y libros, el primer acercamiento es algo similar a lanzarse a un mar sin saber nadar, por ello mismo lo primero que hice fue hacer uso de la Facilitación Gráfica para poder contar con una historia que era Git y cómo funcionaba. Aquí va la primera entrega de lo que espero sean muchas.

Este querido amigo nos servirá para monitorear los archivos de un proyecto, y así controlar las versiones y avances.

Les presento al R2D2-Git y las funciones básicas para tenerlo funcionando en un proyecto.

![Hola Git](/blog/uno.jpg)

Git se ejecuta dentro de la carpeta principal de nuestro proyecto y reconoce TODO lo qué hay dentro.

![Git init](/blog/dos.jpg)

Entonces, a partir de ese momento ya no estarás solo en el desarrollo del proyecto, porque Git estará para apoyarte.

Git verá todos los archivos, pero es necesario indicarle los archivos que deseamos versionar.

Veamos como ir usando Git poco a poco.

---
# ESTADO DE LOS ARCHIVOS QUE MONITOREA GIT

Primero debemos preguntarle qué ve en nuestro proyecto el pequeño amigo Git, el hablara con la verdad y dirá cada archivo qué hay que no este monitoreando.

![Status](/blog/tres.jpg)

---
# DILE A GIT DONDE PONER EL OJO

Una vez que sabemos el estado de aquello que no está monitoreando, podemos indicarle a Git qué archivo debe monitorear, podemos iniciar por uno.

![Add](/blog/cuatro.jpg)

Si volvemos a pedirle el STATUS, verás que ya no cuenta el archivo agregado.

---

# AL CONFESIONARIO CON GIT

Una vez que Git ya está monitoreando el archivo, habrá que indicarle una descripción corta de qué es el archivo, qué se modificó, etc.

![Commit](/blog/cinco.jpg)

---

# REPETIR

Si agregamos un archivo (ADD) dentro del proyecto, y le damos un COMMIT, entonces estará siendo monitoreado por Git. Es necesario ir agregando cada archivo a Git mediante el ADD-COMMIT.

![Rutine](/blog/siete.jpg)
![Rutine 2](/blog/ocho.jpg)

Una vez que el STATUS no muestre ningún archivo, tendremos nuestro proyecto versionado bajo la mirada y el monitor de Git.

Si modificaramos un archivo, automáticamente el STATUS mostrará que hubo un cambio e indicará el archivo, de igual forma lo hará si se añaden nuevos archivos al proyecto.

![Diff](/blog/seis.jpg)

Es posible identificar las diferencias existentes en un archivo, bastará con indicarle a git.

Y finalmente aplicaremos el ADD y COMMIT para monitorearlo por Git.

Notarán que Git irá diciendo que está funcionando sobre algo llamado "Master", que es la rama principal, y creada por defecto, que será tema para otra entrega.

# Review

Comandos de esta publicación de Git

```
git init //Para iniciar Git en tu proyecto

git status //Para ver los archivos no monitoreados

git add file.sm //Indicarle a git que contemple un archivo

git commit -m "Descripcion" //Indicarle una descripcion del archivo agregado anteriormente

git diff file.sm //Git te mostrará las modificaciones

```
