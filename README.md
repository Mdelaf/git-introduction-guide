# Guía Introductoria a Git

Martín De la Fuente

2018

## Tabla de contenidos

1. [¿Qué es Git?](#whatisit)
2. [Repositorio local y repositorio remoto](#repos)
3. [Actualización de tres etapas](#three-steps)
4. [Servicios que ofrecen Git](#services)
5. [Comandos básicos](#basic-cmds)
6. [Parámetros](#params)
7. [Ejemplo de introducción](#example)
8. [Otros comandos útiles](#more-cmds)
9. [Situaciones comunes](#situations)

## ¿Qué es Git? <a name="whatisit"></a>

Git es un sistema de versionamiento que nos permite guardar archivos (generalmente código) en un repositorio compartido por varios usuarios. La característica principal es que podemos dejar un "historial" de todos los cambios que hacemos y todos los archivos que modificamos. Esto nos permite volver a estados anteriores de manera sencilla. Además,  distintos usuarios pueden trabajar sobre los archivos y el sistema automáticamente puede hacer la unión de cambios que afectan un mismo archivo (en caso de no ser trivial se le pide al usuario resolver conflictos). 



## Repositorio local y repositorio remoto <a name="repos"></a>

En Git existe una copia local de los archivos (almacenada en tu computador) y una copia remota (almacenada en algun servidor externo). Cada vez que nosotros hacemos modificaciones en nuestro repositorio local el sistema detecta esos cambios, y usando algunos comandos podemos subir esos cambios al repositorio remoto donde los demás podrán verlos. De manera inversa, cuando otros usuarios suben cambios al repositorio remoto, nosotros podemos actualizar nuestro repositorio local usando otro comando que sincroniza nuestros archivos con los del repositorio remoto. 



## Actualización de tres etapas <a name="three-steps"></a>

Para aquellos que están acostumbrados a usar otros servicios como Dropbox, se darán cuenta que cuando uno modifica un archivo, este inmediatamente se actualiza en la nube (servidor remoto). En Git esto no es así, uno decide manualmente qué cambios quiere subir y cuándo quiere subirlos. Además cada conjunto de cambios se suele identificar con una descripción. Es por esto que el proceso para subir los cambios al servidor remoto consta de tres etapas:

- Etapa 1: una vez que modificamos uno o varios archivos en nuestro repositorio local, debemos llevarlos a un estado de preparación (*staging area*).
- Etapa 2 : cuando hemos puesto uno o más archivos en el estado de preparación debemos hacer definitivos los cambios. Para ello crearemos un "conjunto de cambios" (_commit_) que toma todos los archivos en el _staging area_ y los hace definitivos (al hacerse definitivos salen del _staging area_). Además debemos agregar una descripción a este conjunto de cambios. 
- Etapa 3: por último subimos nuestro "conjunto de cambios" al repositorio remoto. 

Cada una de estas etapas se puede llevar a cabo usando los comandos que Git nos ofrece mediante la línea de comandos. Más adelante veremos los comandos con detalle, pero podemos anticipar que `git add`, `git commit` y `git push` se utilizan para las etapas 1, 2 y 3 respectivamente. 

Aunque en un principio pueden parecer engorroso tener que hacer tantos pasos, esto tiene su razón de ser. Lo fundamental de usar Git es que uno pueda tener un registro claro de los cambios que se han hecho y para ello cada conjunto de cambios (que de ahora en adelante llamaremos _commit_)  debe tener una descripción clara de los cambios que afecta. 

La idea detrás de esto es que en cada commit hayan cambios que se relacionan entre sí y pueden ser agrupados bajo una descripción no muy general. Por su parte, el área de preparación nos permite definir qué archivos entran al commit al momento de crearlo.



## Servicios que ofrecen Git <a name="services"></a>

Git es el sistema que define toda esta estructura de organización de los cambios, los estados y los comandos para realizar acciones. Es por esto que nosotros podemos descargar Git en nuestras casas, instalar un servidor de Git en nuestro propio PC y usarlo de manera privada. 

Sin embargo, por temas prácticos solemos usar servidores de terceros. Existen muchas empresas que implementan Git y ofrecen sus servidores de manera mucho más cómoda, con interfaces web que nos permiten visualizar los cambios en el navegador, así como también hacer comentarios, revisar la actividad, ver los usuarios que participan y una serie de funcionalidades extras. Además tiene la ventaja de que el servidor remoto estará siempre disponible y los archivos alojados ahí no se van a perder. 

Dentro de los servicios que ofrecen Git más comunes, están Github, Gitlab y Bitbucket. Todos funcionan de manera similar, implementando Git y ofreciendo formas de visualizar los cambios en el repositorio remoto. En Bitbucket por ejemplo podemos crear nuestros propios repositorios de manera gratuita (públicos o privados) mientras que en Github debemos pagar si queremos repositorios privados.



## Comandos básicos <a name="basic-cmds"></a>

#### `git clone <repo-url>`
- Crea un repositorio local que es una copia exacta del repositorio remoto especificado.

#### `git pull`
- Actualiza nuestro repositorio local con lo que se encuentre en el repositorio remoto.

#### `git status`
- Nos muestra los archivos modificados, agregados y eliminados en nuestro repositorio local, y cuáles de  ellos están en el _staging area_.

#### `git diff`
- Muestra las diferencias específicas (línea a línea) que hay en cada uno de los archivos modificados en el repositorio local.

#### `git add <file>`
- Agrega el archivo al *staging area*.
- El parametro -A sirve para incluir también archivos eliminados.

#### `git reset <file>`
- Quita un archivo del staging area (opuesto a git add).

#### `git commit -m "comment"`
- Crea un conjunto de cambios (_commit_) a partir de todo lo que se encuentre en el _staging area_.
- Todos los archivos modificados que estaban en el _staging area_ pasan a convertirse en cambios definitivos y por lo tanto salen de ahí.

#### `git push origin <branch>`
- Sube todos los commits al repositorio remoto.
- Como no estamos trabajando con _branches_, `<branch>` corresponderá a `master` que es la rama principal por defecto. 



## Parámetros <a name="params"></a>

Para los comandos que reciben el parámetro `<file>` podemos usar algunos de los siguientes ejemplos. 

|Expresión|Archivos que considera|
|--|--|
|`file.txt`|Solo el archivo con nombre _file.txt_|
|`folder1/file.txt`|Solo el archivo que está dentro de la carpeta _folder1_ y tiene nombre _file.txt_|
|`folder1/*`|Todos los archivos que están dentro de la carpeta _folder1_|
|`folder1/*.txt`|Todos los archivos que están dentro de la carpeta _folder1_ y tienen extensión _txt_|
|`.`|Todos los archivos|

Para los comandos que reciben el parámetro `<commit-id>` podemos usar algunos de los siguientes ejemplos. 

|Expresión|Commit que considera|
|--|--|
|`ed6c73fc16578ec53ea374585df2b965ce9f4a31`|Commit con id _ed6c73fc16578ec53ea374585df2b965ce9f4a31_|
|`ed6c73f`|Commit con id corto _ed6c73f_|
|`HEAD`|Último commit de la rama actual|
|`HEAD^1`|Penúltimo commit de la rama actual|
|`HEAD^2`|Antepenúltimo commit de la rama actual|



## Ejemplo de introducción <a name="example"></a>

Nos acaban de contratar en una empresa que esta creando una nueva red social muy prometedora. Primero nos dicen que la URL del repositorio alojado en Github es https://github.com/socialnetworkcompany/socialnetwork.git.

Para empezar a trabajar debemos clonar el repositorio en nuestro computador. Para ello usamos:

`git clone https://github.com/socialnetworkcompany/socialnetwork.git`

Una vez hecho esto, nuestro repositorio local y el repositorio remoto estarán sincronizados. Cada cambio que hagamos localmente será detectado por el sistema. 

En nuestro primer día nos pidieron que trabajáramos actualizando cómo se ve el perfil de los usuarios. Además nos entregaron una nueva política de privacidad que debíamos actualizar para hacerla pública cuando los usuarios iniciaran sesión.

Antes de empezar a trabajar debemos asegurarnos de que nuestros archivos locales estén sincronizados con los del repositorio remoto:

`git pull`

El comando nos devuelve el siguiente mensaje:

`Current branch master is up to date.`

Estamos sincronizados, entonces podemos ponernos a trabajar. 

Después de trabajar un par de horas corremos el comando `git status` y nos muestra lo siguiente:

    En la rama master 
    Su rama está actualizada con «origin/master».
    
    Cambios no preparados para el commit:   (use «git add <archivo>...» para actualizar lo que se confirmará)   (use «git checkout -- <archivo>...» para descartar cambios en el directorio de trabajo)
    
            modificado: website/controllers/login.php
            modificado: website/documents/EULA.txt
            modificado: website/views/feed.html
            modificado: website/views/profile.html

Esos cuatro archivos son los que modificamos mientras trabajábamos. Los dos últimos son los que cambiamos para hacer arreglos en las vistas del perfil de usuario, y los dos primeros están relacionados con la actualización en las políticas de privacidad. 

En esta empresa son sumamente exigente con que los commits deben ser atómicos y describir bien sus cambios. Ahora nosotros debemos subir nuestros cambios al repositorio remoto. 

Primero crearemos un commit para los cambios relacionados al perfil de usuario. Agregamos los archivos involucrados al _staging area_ usando el comando:

`git add *.html`

 Para verificar que se agregaron bien, volvemos a correr `git status`:

    En la rama master
    Su rama está actualizada con «origin/master».
    
    Cambios para hacer commit:
      (use «git reset HEAD <archivo>...» para sacar del stage)
    
            modificado: website/views/feed.html
            modificado: website/views/profile.html
    
    Cambios no preparados para el commit:
      (use «git add <archivo>...» para actualizar lo que se confirmará)
      (use «git checkout -- <archivo>...» para descartar cambios en el directorio de trabajo)
    
            modificado: website/controllers/login.php
            modificado: website/documents/EULA.txt

Efectivamente los archivos html ya se encuentran en el _staging area_, por lo tanto podemos crear el commit con un comentario breve que describa los cambios:

`git commit -m "Improved profile view"`

Si volvemos a correr `git status` nos devuelve:

    En la rama master
    Su rama está delante de «origin/master» para 1 commits.
      (use "git push" to publish your local commits)
    
    Cambios no preparados para el commit:
      (use «git add <archivo>...» para actualizar lo que se confirmará)
      (use «git checkout -- <archivo>...» para descartar cambios en el directorio de trabajo)
    
            modificado: website/controllers/login.php
            modificado: website/documents/EULA.txt

Es decir los cambios en los archivos html se hicieron permanentes (por eso ya no hay archivos en el _staging area_) y además se nos informa que ya tenemos un commit listo para subir al servidor remoto. 

Nos queda hacer otro commit relacionado a los cambios en la política de privacidad. Para ello agregamos todos los archivos que quedan al _staging area_ usando `add`:

`git add .`

Y creamos el segundo commit.

`git commit -m "Changed EULA and added log in prompt"`

Volvemos a correr `git status` y nos muestra que hay dos commits listos para subir al servidor remoto y que no hay archivos modificados:

    En la rama master
    Su rama está delante de «origin/master» para 2 commits.
      (use "git push" to publish your local commits)
    
    nothing to commit, working directory clean

Por último subimos ambos commits:

`git push origin master`

Ahora todo el resto de los desarrolladores podrán ver los cambios que hicimos. 


## Otros comandos útiles <a name="more-cmds"></a>

#### `git checkout <file>` (!)
- Deshace todos los cambios hechos en `<file>` desde el último commit.
- Los cambios se pierden y no pueden ser recuperados.

#### `git checkout <commit-id> <file>` (!)
- Cambia `<file>` al estado en el que se encontraba en el `<commit-id>`.
- Los cambios se pierden y no pueden ser recuperados.

#### `git reset --hard <commit-id>` (!)
- Elimina todos los cambios hechos desde que se hizo `<commit-id>`.
- No es util una vez que se hizo el push, ya que solo afecta el repositorio local.
- ELIMINA DE MANERA IRREVERSIBLE TODOS LOS CAMBIOS DEL REPOSITORIO LOCAL.

#### `git revert <commit-id>`
- Crea un 'commit de reversion' para el `<commit-id>`.
- El commit de reversion contiene todos los cambios opuestos a `<commit-id>`.
- Es una manera de deshacer lo hecho en `<commit-id>` pero sin eliminarlo como se hace en el comando anterior.
- Funciona bien cuando el commit ya fue *pusheado*, pues se crea el commit de reversión y se *pushea* nuevamente.
- Para salir del editor usamos la combinación [Esc] + :wq + [Enter].

#### `git log`
- Entrega informacion de cada commit.
- Con el parametro `--oneline` se pueden ver de manera mas resumida.
- Con el parametro `-p` vemos el detalle del ultimo commit.
- Para salir se debe apretar 'q'.



## Situaciones comunes <a name="situations"></a>

#### Modifiqué y guardé un archivo. No he hecho commit ni push pero quiero descartar los cambios.
Deshacemos todos los cambios hechos en el archivo desde su último commit
`git checkout <file>`

#### Hice varios cambios en el repositorio, además hice add y commits con los cambios. No hice push pero ahora quiero descartar los cambios
Usamos el siguiente comando para ver los últimos commits que hicimos y sus respectos _id_.
`git log --oneline` 

Escogemos el commit hasta el cual queremos deshacer los cambios y usamos reset (se recomienda respaldar antes de hacer esto ya que los cambios se pierden de manera irreversible)
`git reset --hard <commit-id>` 

#### Hice varios cambios en el repositorio, luego hice add, un commit y push. Quiero descartar los cambios.
Creamos un commit que revierta los cambios hechos en el último commit (esto revertirá el commit en el repositorio local)
`git revert HEAD^1`

Hacemos push para subir el nuevo commit al repositorio remoto
`git push origin <branch>`

#### Llevo algunos días editando un archivo, le he hecho varios commits y pushs pero lo quiero volver al estado que estaba en `<commit-id>`.
Revertimos los cambios en el archivo de manera local
`git checkout <commit-id> <file>`

Lo actualizamos en el repositorio remoto
`git push origin <branch>`
