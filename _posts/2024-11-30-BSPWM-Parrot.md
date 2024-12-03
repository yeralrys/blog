---
title: Instalar BSPWM en Parrot OS
date: 2024-11-30
category: [Linux]
tags: [Parrot]
---

BSPWM es un gestor de ventanas tipo mosaico que organiza las ventanas en un árbol binario completo. En este caso he realizado la instalacion dentro de Parrot OS en una maquina virtual, pero puedes realizar la instalacion en cualquier entorno deseado, solo debes conocer como funciona tu distribucion pero en principio los pasos y configuracion han de ser los mismos.


## 1. Instalar los paquetes necesarios.

   Utilizar *sudo* si no estas como root.
   
~~~bash
apt install bspwm polybar dmenu
~~~

## 2. Configuracion basica.

   Utilizaremos los archivos de configuracion inicial que incluyen los paquetes.

   + Creamos los directorios de configuracion dentro de **/home/usuario/.config/**

~~~bash
mkdir {bspwm,polybar,sxhkd}
~~~

   + Copiamos la configuracion basica de los programas dentro de las carpetas creadas.

~~~bash
cp /usr/share/doc/bspwm/examples/bspwmrc ~/.config/bspwm
cp /usr/share/doc/bspwm/examples/sxhkdrc ~/.config/sxhkd
cp /usr/share/doc/polybar/examples/config.ini ~/.config/polybar
~~~

   + configuramos el archivo **~/.config/sxhkd** y cambiamos la parte de **urxvt** utilizando nuestro favorito(gnome_terminal, konsole, terminator, etc). 

~~~
# terminal emulator
super + Return
	urxvt -> Terminal preferido
~~~


## 3. Crear los archivos para que polybar se inicie junto junto con BSPWM.

~~~bash
touch ~/.config/polybar/launch.sh
~~~
   

   + Editamos el archivo con nuestro editor de texto favorito.

~~~bash
#!/usr/bin/env bash

# Terminate already running bar instances
killall -q polybar
# If all your bars have ipc enabled, you can also use 
# polybar-msg cmd quit

# Launch bar1
echo "---" | tee -a /tmp/polybar1.log /tmp/polybar2.log
polybar example 2>&1 | tee -a /tmp/polybar1.log & disown

echo "Bars launched..."
~~~

   **example** es el nombre de la barra que utiliza polybar, en caso de agregar otra configurion de polybar tambien se debe cambiar esta parte.

   + Le asignamos permisos de ejecucion.

~~~bash
chmod +x ~/.config/polybar/launch.sh
~~~

   Agregamos la siguiente linea dentro de **bspwmrc** que es el archivo que copiamos dentro del paso 2.

~~~bash
$HOME/.config/polybar/launch.sh
~~~

   Recomiendo agregar la linea antes de que inicie **bspc monitor** y despues de **skhkd &**

## 4. Solo queda iniciar session dentro del enorno  de BSPWM.


[VIDEO Tutorial](https://youtu.be/etD81Yx-P1U)
   

Referencias:
[BSPWM](https://github.com/baskerville/bspwm)
[Sxhkd](https://github.com/baskerville/sxhkd)
[Polybar](https://github.com/polybar/polybar)
