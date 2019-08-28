# Como usar varias versiones de angular al mismo tiempo
## ¿Cuando usar?
 Me he visto en la necesidad de manejar al menos 3 versiones de angular diferentes de una importante empresa en telecomunicaciones y por motivos de estandarización se maneja cada proyecto con sus respectivas versiones de node, angular cli, bower, etc...

 Si bien podríamos hablar de dockerización del entorno de desarrollo -lo que en mi humilde opinión creo sería una mejor práctica-, he optado por una solución muy simple que no requiere de proponer cambios a gigantes y me hace la vida mucho más facil a la hora de trabajar con multiples proyectos y su respectiva sopa de versiones.

## Desinstalando la versión global (opcional)
Primero debemos tener en cuenta que si tenemos una versión global única nos podríamos confundir y terminar perdiendo el tiempo pensando en la razón de nuestros errores en consola cuando usamos el comando ng, por lo que lo vamos a desinstalar globalmente y solo manejar nuestras versiones configuradas.

    $ npm uninstall -g angular-cli

## Usando 'Alias'
Una herramienta usada por todos aquellos que tenemos tiempo usando linux es el comando 'alias' el cual te permite crear un nuevo comando en consola a partir de otro comando o serie de estos. por ejemplo:

    $ alias holamundo='echo Hola\ Mundo!'
    $ holamundo
    Hola Mundo!

*Nota: los alias que creemos tambien pueden eliminarse usando el comando 'unalias'*

## Lógica: Creando la estructura
Ya que tenemos la herramienta principal que es 'alias', lo que debemos hacer es crear una estructura de carpetas que nos permita versionar nuestras herramientas, en este caso en particular solo hablaremos de Angular y de dos versiones en concreto, angular 6 y angular 8 para no complicar demasiado la idea, sin embargo puede ser aplicado a otras herramientras, o casos.

    -/angv
    --/ng6
    --/ng8

Podemos crear estas las carpetas ejecutando :

    $ mkdir ./angv/ng6 ./angv/ng8 -p

Ahora debemos crear un archivo 'package.json' para cada una de estas carpetas que hemos creado, y acá viene algo de mágia.

## Instalando la versión 8 de Angular CLI

Lo primero que debemos hacer es entrar en la carpeta que hemos designado para ello:

    $ cd angv/ng8

Si quisieras crear un repositorio donde puedas luego descargarte toda tu estructura de versiones lo cual es buena idea si debes trabajar en diferentes máquinas- crear un archivo package.json el cual puedes inicializar usando el respectivo npm init, sin embargo para este caso que no aplica como proyecto he preferido crear un objeto vacio.

    $ echo '{}' > package.json

Luego ya podemos instalar nuestro paquete de angular cli de manera LOCAL (sin el respectivo -g, en cambio se usa la opción -s para escribirlo en el package.json si leiste el parrafo anterior y si no solo estás copiando los comandos uno a uno)

    $ npm install -s angular-cli@8.3.1

Para cuando acabe deberíamos tener ya en nuestro node_modules de la carpeta ng8 nuestra librería instalada en 'node_modules', a nosotros en particular nos interesa es ejecutar el binario 'ng' que se encuentra en 'node_modules/@angular/cli/bin/ng' el cual es quien se ejecuta al escribir el comando ng en caso de haberlo instalado global.

Lo que haremos será crear un alias para acceder a este binario desde cualquier ruta, haremos algo similar a modificar las variables de entorno del sistema operativo para leer nuestra nueva ruta, pero de una manera mucho más simple:

    alias ng8="/<RUTA A TU CARPETA 'angv'>/ng8/node_modules/@angular/cli/bin/ng"

De esta manera cuando ejecutemos el comando 'ng8 v' tendremos algo como esto:


    $ ng8 v

        _                      _                 ____ _     ___
        / \   _ __   __ _ _   _| | __ _ _ __     / ___| |   |_ _|
      / △ \ | '_ \ / _` | | | | |/ _` | '__|   | |   | |    | |
      / ___ \| | | | (_| | |_| | | (_| | |      | |___| |___ | |
    /_/   \_\_| |_|\__, |\__,_|_|\__,_|_|       \____|_____|___|
                    |___/


    Angular CLI: 8.3.1
    Node: 10.16.3
    OS: linux x64
    Angular:
    ...

    Package                      Version
    ------------------------------------------------------
    @angular-devkit/architect    0.803.1
    @angular-devkit/core         8.3.1
    @angular-devkit/schematics   8.3.1
    @angular/cli                 8.3.1
    @schematics/angular          8.3.1
    @schematics/update           0.803.1
    rxjs                         6.4.0


¡Listo! ahora hagamos lo mismo con la versión 6:

    $ cd ../ang6 // Nos ubicamos en la carpeta ang6
    $ echo '{}' > package.json
    $ npm install -s angular-cli@6.2.4
    $ alias ng6="/<RUTA A TU CARPETA 'angv'>/ng6/node_modules/@angular/cli/bin/ng"
    $ng6 v

        _                      _                 ____ _     ___
        / \   _ __   __ _ _   _| | __ _ _ __     / ___| |   |_ _|
      / △ \ | '_ \ / _` | | | | |/ _` | '__|   | |   | |    | |
      / ___ \| | | | (_| | |_| | | (_| | |      | |___| |___ | |
    /_/   \_\_| |_|\__, |\__,_|_|\__,_|_|       \____|_____|___|
                    |___/


    Angular CLI: 6.2.4
    Node: 10.16.3
    OS: linux x64
    Angular:
    ...

    Package                      Version
    ------------------------------------------------------
    @angular-devkit/architect    0.8.4
    @angular-devkit/core         0.8.4
    @angular-devkit/schematics   0.8.4
    @angular/cli                 6.2.4
    @schematics/angular          0.8.4
    @schematics/update           0.8.4
    rxjs                         6.2.2
    typescript                   2.9.2


De esta manera hemos creado nuestros comandos ng8 y ng6 para las versiones de Angular CLI 8 y 6 respectivamente, sin embargo para control de versiones de node por ejemplo es mucho mejor usar herramientas especializadas como NVM.

Espero el contenido les sirva para sus proyectos profesionales y ayudar a tener un mejor control sobre las versiones de dependencias de sus proyectos, mientras surjan mejores opciones esta es la que he optado dado el caso en particular, queda de parte de cada quien determinar cual es la mejor opción para su caso.
