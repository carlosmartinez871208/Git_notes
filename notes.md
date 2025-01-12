# Notas para el curso de control de versiones con git y github.

Repositorio (Proyecto que contiene el codigo del proyecto).
    Repositorio central: Todo se extrae y guarda en un solo un solo lugar.
    Repositorio distribuido: Cada usuario tiene una copia del repositorio.

git --version: comando para obtener la version de git.
git --help: comando para abrir una ayuda rapida.

git config --global user.name "Carlos Martinez" : agrega nombre de usuario de git
git config --global user.email "carlos.martinez.871208@gmail.com" : agrega correo electronico del usuario.
git config --global init.defaultBranch main
git config --global -e : nos abre el archivo de configuraciones globales.
                         Una vez que abre la terminal escribir la tecla 'a' para poder editar.
                         Para salir de la terminal (esc + : +wq!) (w: write, q: quit).
git init: initializa el repositorio.
          Ejemplo: cd ../Makefile (for change directory to Makefile)
                   once in Makefile folder use following command.
                   git init 
git status: brinda informacion del status del repositorio.
git status --short: me brinda un resumen mas corto del status del repopsitorio.
git add nombre_del_archivo: agrega el archivo elegido por su nombre.
git add . : agrega todos los archivos.
git add *.c: agrega todos los archivos .c del proyecto.
git reset nonbre_del_archivo: remueve un archivo del repositorio.
git branch: Nos brinda el nombre del branch.
git log: nos proporciona informacion del repositorio.
git diff: muestra en consola los cambios hecho en el repositorio.
git difftool: abre en un programa ya configurado los cambios hechos.
git commit -m "Mensaje_con _respecto_al_commit" : Nos crea una instantanea del proyecto y agrega un mensaje relativo a lo hecho en el proyecto.
git checkout -- . : Recontruye el proyecto a como estaba en el ultimo commit.
git branch -m master main: este comando renombra la rama master a main.

## Creaccion de alias.
git config --global alias.s status: genera un alias 's' para el comando status
git config --global alias.s "status --short": genera un alias para el comando cuando se escribe git s se llama: git status --short
# Alias para git config --global -e: git cc:
                                             git config --global alias.cc "config --global -e" : cc (change configuration)
# git config --global alias.sl "log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)' --all": sl (short log)

git diff --staged: Nos muestra la diferencia de los ultimos cambios ya anadidos al "staging area" con el ultimo commit.
git difftool --staged: Nos muestra la diferencia de los ultimos cambios ya anadidos al "staging area" con el ultimo commit, con la herramienta "meld".
# Alias para git difftool --staged: git config --global alias.ds "difftool --staged": ds (d: difftoll; s=staged)

git commit --amend -m: Commando para "remendar" o corregir el mensaje del ultimo commit.
git commit --amend: Nos permite agregar comentarios mas completo en multiples lineas.
git reset --soft HEAD^2: hace soft reset de dos commits antes.
git commit -am "mensaje": Ya nos agrega al stage area y nos pide agregar un mensaje.
git reset --mixed <commit_id>: Nos regresa al commit que elegimos, pero nos pone como Unstage los cambios despues de ese commit.
git reset --hard: Deja el repositorio como estaba en ese commit.

## Inicio de proceso
## Proceso para unir varios commits en uno solo.
git rebase -i HEAD~2: trabajaremos desde dos commits antes del HEAD. (HEAD~3 seria trabajar en 3 commits antes del HEAD).
Por ejemplo si queremos combinar los ultimos dos commits:
    c
    b
    a
Usamos el comando git reabse -i HEAD~2: esto es debido a que despues del commit a queremos el commit con la union de c y b
-i: Nos indica que queremos hacerlo de manera interactiva.
Ya dentro de VIM o de NANO no sale algo parecido a esto.
pick b76d157 b
pick a931ac7 c

# Rebase df23917..a931ac7 onto df23917
#
# Commands:
#  p, pick = use commit
#  r, reword = use commit, but edit the commit message
#  e, edit = use commit, but stop for amending
#  s, squash = use commit, but meld into previous commit
#  f, fixup = like "squash", but discard this commit's log message
#
# If you remove a line here THAT COMMIT WILL BE LOST.
# However, if you remove everything, the rebase will be aborted.
#

Lo que hacemos aqui es que el ultimo commit 'c' le agregamos squash porque lo queremos combinar con 'b'
pick   b76d157 b
squash a931ac7 c

Al guardar nos sale un mensaje parecido a esto:
# This is a combination of 2 commits.
# The first commit's message is:
b
# This is the 2nd commit message:
c

Damos guardar.
$ git log --pretty=oneline
18fd73d3ce748f2a58d1b566c03dd9dafe0b6b4f b and c
df239176e1a2ffac927d8b496ea00d5488481db5 a

git rebase --abort: cancela el proceso de rebase.

git reflog: este comando nos permite ver todo lo que ha sucedio en el repositorio.
Por ejemplo si hemos dado "git reset --hard" y ya no podemos ver con "git log": podemos ver el historial de git en el repositorio usando: git reflog
    Esto nos muestra el historial de los commits y aparte si usamos git reset --hard <commit> nos regresaria el repositorio a como estaba en ese commit aunque no lo podamos ver con el git log.

# Comando para ignorar archivos de un tipo
agregar el archivo .gitignore en .git folder.
    dentro de ese archivo agregar las carpetas y archivos a ignorar.

# Branches (ramificaciones).
    Son lineas de desarrollo alternativas que se completan el objetivo es unirlas (merge) en alguna parte del tiempo.
    merge fast-forward:No hay ningun cambio y pueden ser integradas en la rama de donde se derivo.
    merge manual: Se da cuando git no puede resolverlo por el mismo.

# Para crear una rama.
git branch <branch_name>: Crea el branch.
git checkout <branch_name>: se mueve al branch.
git checkout -b <branch_name>: Crea un branch nuevo y se mueve a ese branch.

# Para hacer un merge desde mi branch_name a mi master o main (branch): 
    git checkout main: para moverme al branch main
    git merge <branch_to_be_merge_into_current_branch>: Esto hace un merge en la rama actual de lo que hay en otra rama.

Una vez que una rama ha cumplido su proposito el objetivo es borrarla, para evitar conflictos.
    git branch -d <branch_name>: borrar una rama.

git branch -a: nos permite ver todos los branches: locales y remotos. Se para verificar que el commit no exista.

# Automatic merge: Git por si mismo se encarga de unir los cambios para evitar errores.
    Solo nos pide el mensaje del commit.

# Merge con conflitos: 
    Se usa el mismo comando: git merge <branch_to_be_merged_into_current_branch>
    Sin embargo GIT nos manda el siguiente mensaje:
        Auto-merging notes.md
        CONFLICT (content): Merge conflict in notes.md
        Automatic merge failed; fix conflicts and then commit the result.
    Esto significa que tenemos que resolver los conflictos manualmente.
    Podemos usar el siguiente comando:
        git mergetool: para abrir un programa de resolucion de conflictos, ahi resolvemos manualmente y cerramos cuando todo este igual.
        Si todo esta bien nos depsliega el siguiente mensaje:
            Merging:
            notes.md

            Normal merge conflict for 'notes.md':
            {local}: modified file
            {remote}: modified file
        
        Usamos git status para ver el status de nuestro repositorio.
            On branch main
            All conflicts fixed but you are still merging.
            (use "git commit" to conclude merge)

            Changes to be committed:
            modified:   notes.md
        Nos pide que hagamos commit para el merge: git commit -m "merge related message"

## TAGS: Son una referencia a un commit en especifico.
    Para crear un tag, se pueden usar nombres o versiones semanticas.
    git tag <tag_name>, por ejemplo:
        git tag Guide_V1
    git tag -d <tag_name>: nos sirve para borrar un tag.
    git tag -a v1.0.0 -m "Version 1.0.0 de la guia"
        vX            .X                         .X
        Version mayor: Se agrego algo importante: Bug fix. 
    gti tag -a v0.1.0 <commit_id> -m "Version alfa de la app": Se agrega el tag en un commit en especifico.
    git show <tag_name>: Para mostrar todo con respecto del tag.

Stash
Nos sirve para deshacer cambios pero no perderlos, es usado cuando se requiere un cambio de emergencia y nos ayuda a no perder los cambios que tenemos actualmente.
git stash: Nos genera un stash con los cambios que tenemos actualmente.
git stash list: Nos genera una lista con los stash.
git stash pop: Recupera la informacion que tenemos en el stash mas reciente.
git stash clear: borra todo el stash o todos los stash.
git stash apply stash@{stash_deseado}: Nos permite jalar el stash deseado.
git stash drop stash@{stash_deseado}: Nos permiter borrar un stash deseado.
git stash show stash@{stash_deseado}: Nos permite ver un poco mas informacion del stash.
git stage save "Nombre del stage": Nos permite nombrar un stash y asi es mas facil localizarlo.
git stage list --stat: Para ver mas informacion del stash.

Rebase: Para actualizacion de rama.
    git rebase -i HEAd~numero_de_commits.
        Nos permite ordenar commits.
        Nos permite corregir mensajes de los commits.
        Unir commits.
        Separar commits.
git rebase branch_name: se actualiza  nuestro branch con respecto el branch_name. i.e.g. git rebase main
    git rebase main: Nos permite actualizar nuestro branch actual con el branch main.
git rebase -i HEAD~2:
    Para cambiar el nombre solo cambiamos pick a reword.
    pick b76d157 b se cambia por: r b76d157 b
    pick a931ac7 c se cambia por: r a931ac7 c
    Despues nos sale un editor parecido al ejemplo del squash para cambiar el mensaje.

git checkout -- file_name, nos regresar el archivo seleccionado a la forma que tiene en el ultimo commit.
git checkout -- .: Nos regresa todo el repositorio a como estaba en el ultimo commit.

#############################################################################################################################################################
GITHUB

git remote add origin git@github.com:carlosmartinez871208/Git_notes.git: Este comando nos permite anadir el repositorio en git a un proyecto en igt hub
git branch -M main
git push -u origin main: 
