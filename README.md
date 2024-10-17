# GitHub essentials for the Idiot mind

En este repo voy a recopilar información y scripts para tener cerca en el caso de que me olvide como usar Git-(Hub)

# Clonar y registrar claves SSH

Esta info la saqué del siguiente link

[A quick GitHub SSH clone example](https://www.theserverside.com/blog/Coffee-Talk-Java-News-Stories-and-Opinions/github-clone-with-ssh-keys)

Si bien está dentro de la documentación de github, esta explicación me resultó mas sencilla.

## Generar clave SSH en windows y linux

De la misma forma que para trabajar en un cluster debo dar mi clave SSH, acá pasa lo mismo. Para generarla debo usar el comando

```bash
ssh-keygen -o -t rsa -C “ssh@github.com”
```

donde `-C “ssh@github.com”` es un comentario que va al final, puede servir para etiquetar la computadora o para indicar mi correo electronico.

queda guardada en el directorio .ssh de la computadora, en linux está en

```bash
/home\username\.ssh
```

En windows está en 

```bash
C:\Users\PC
```

luego de ejecutar esa linea de comando deberiamos tener dos archivos en la carpeta, `id_rsa`  y `id_rsa.pub`, donde el archivo .pub es la clave pública.

La clave que le interesa a github es la publica. Simplemente copiamos el contenido del archivo (lo podemos abrir con un editor de texto plano), en linux podemos usar el comando `cat id_rsa.pub`, dentro de la carpeta .ssh, por supuesto.

<aside>
➡️ Si vas a trabajar usando WSL (Windows Subsystem for Linux) ejecuta el comando keygen desde el bash y luego buscá la carpeta ssh dentro de los archivos del subsistema.

</aside>

## Informar a Github de mi SSH-Key

Para esto hay que ir a las opciones (Settings) y buscar la tab de SSH keys, ahi se pone New SSH Key y se agregan los datos.

# Trabajar localmente y Pushear a GitHub desde Ventana de Comandos

Esta información fue obtenida desde el siguiente link

[How to git push an existing project to GitHub](https://www.theserverside.com/blog/Coffee-Talk-Java-News-Stories-and-Opinions/How-to-push-an-existing-project-to-GitHub)

Igual que antes, esto está documentado, pero este tipo lo explico de manera muy sencilla.

En su pagina describe dos formas de hacer esto, una a la que llama facil y otra a la que llama apropiada. Por ahora voy a hacer énfasis en la forma fácil, ya que cumple con el propósito que necesito.

## Easy Way

<aside>
⚠️ Esta manera sirve asumiendo que el repo que vas a usar ya tiene un cierto historial de commits, No te permite empezar desde cero. Aunque es una boludes bypasear esta situación.

</aside>

A continuación voy a describir el proceso y comentar detalles según como me funcionó trabajar.

**Se asume** que ya tenés gestionada la clave SSH y que github está cómodo con la computadora que estás usando.

1. Clonar el repo en la carpeta de interés usando SSH
    En la pagina ejemplifican usando HTTPS, pero para mis pruebas usé SSH. No se si hará la diferencia. Funcionó tanto en windows como en linux.

2. Modificá los archivos
    O sea, trabajá en tus códigos

3. `git add <archivo modificado>` es el comando para guardar en git un archivo que hayas modificado. En el caso de que hayas modificado varios (y, quizas, no te acuerdes cual) usá `git add .`.

4. `git commit -m “<titulo del commit>”` para archivar las modificaciones

5. `git push` para que se actualice el repo de github

Y eso es todo. Bastante facilito.

## Proper Way

No presté atención a como trabajar con esta manera pues la anterior me alcanza y me sobra.

# Uso de `.gitignore`

`.gitignore` es un archivo ubicado en el directorio de trabajo y es un archivo que Git usa para saber que cosas no debería cargar. Es particularmente util para cuando los compiladores generan archivos “basura” o desechables que no son de interés para tener archivados. Por ejemplo, archivos `.log` o los `.mod` de fortran.

La siguiente pagina tiene una buena descripción de las distintas formas de armar cosas en el archivo.

[Archivo .gitignore: ignorar archivos en Git | Atlassian Git Tutorial](https://www.atlassian.com/es/git/tutorials/saving-changes/gitignore)

- Aquí hay un ejemplo de configuración de archivo
  
  ```bash
  # Compiled source #
  ###################
  *.com
  *.class
  *.dll
  *.exe
  *.o
  *.so
  
  # Packages #
  ############
  # it's better to unpack these files and commit the raw source
  # git has its own built in compression methods
  *.7z
  *.dmg
  *.gz
  *.iso
  *.jar
  *.rar
  *.tar
  *.zip
  
  # Logs and databases #
  ######################
  *.log
  *.sql
  *.sqlite
  
  # OS generated files #
  ######################
  .DS_Store
  .DS_Store?
  ._*
  .Spotlight-V100
  .Trashes
  ehthumbs.db
  Thumbs.db
  ```
  
    Robado del siguiente [link](https://gist.github.com/octocat/9257657)
  
  - Archivo personal para trabajar en fortran
    
    ```bash
    # Modulos #
    *.mod
    
    # Ejecutable #
    *.o
    ```

<aside>
⚠️ En la medida que vaya trabajando con fortran o python voy a ir dejando por sentado ejemplos o plantillas de archivos .gitignore para que queden guardadas.

</aside>

# ¿Como actualizo el repo?

Cada vez que vas a trabajar en un repo local es saludable hacer un 

```fortran
git pull
```

Esto toma el repositorio remoto y actualiza los repo locales.

Es saludable hacerlo antes de comenzar a trabajar para no generar inconsistencias en los commits

# ¿Como chequeo si modifiqué los archivos y no hice `commits`?

Para eso está el comando `git status` que muestra el estado del directorio de trabajo y del área del entorno de ensayo.

Si no hay modificaciones se muestra

```fortran
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

Si las hay se ve

```fortran
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
    modified:   For/Observables/subrmsr.f90
    modified:   Read/contp.dat

no changes added to commit (use "git add" and/or "git commit -a")

```

y avisa que archivos fueron modificados
