---
title: Agregar IP al Prompt de BASH
date: 2024-08-12
categories: [Linux]
tags: [bash]     # TAG names should always be lowercase
---

Al momento de practicar hacking ético resulta util tener a la vista la ip local y la de tu objetivo directamente en la terminal que estas utilizado, si tu objetivo al igual que el mio es agregar estas IPs directamente al prompt de tu bash, estas en el articulo correcto.

![Resultado final](/assets/img/prompt-bash-1.png)

Para lograr esto editamos el archivo **.bashrc** con tu editor de preferencia, pero antes hacemos un backup de la configuración

```bash
cp .bashrc .bashrc-bckup
vim .bashrc 
```
Buscamos la linea **PS1**, este seria el apartado que muestra el prompt o titulo de bash.

## Editando *.bashrc*

1. Encima de **PS1** agregamos un comentario para identificar lo que estamos trabajando.
```bash
# IP local y Objetivo de hacking
```

2. Bajo nuestro comentario recién creado agregamos una función para obtener la IP de la interfaz y almacenarla en una variable.
*Reemplaza int00 por el nombre de la interfaz que deseas*
```bash
ATTACKER=$(get_ip)
```
```bash
# Funcion para obtener la IP
function get_ip() {
    local ip=$(ip addr show int00 2>/dev/null | grep 'inet ' | awk '{print $2}' | cut -d/ -f1)
    if [ -n "$ip" ]; then
        echo "$ip"
    fi
}
```

3. Agregamos otra función para definir el target y refrescar el **.bashrc** para que detecte los valores de las variables.
```bash
# Funcion para cambiar el Objetivo de hacking
function target() {
    # Verificar que se haya pasado un argumento
    if [ -z "$1" ]; then
        TARGET=""
    else
        TARGET="$1"
    fi

    # Refrescar bashrc
    source "$HOME/.bashrc"
}
````
4. Aquí el punto mas importante, dentro de **PS1** debes insertar la siguiente linea de código para que muestre las variables que hemos definido
```bash
[\e[1;32m\]󰆧 $ATTACKER\e[0;31m]-[\e[1;33m\]󰀖 $TARGET\e[0;31m]
```

Es posible que tengas mas de una linea que diga PS1 o incluso una función, debes ir probando hasta que encuentres el lugar adecuado.

Te muestro donde esta ubicado en mi caso.

![Ubicacion de variables en mi PS1](/assets/img/prompt-bash-2.png)

## Funcionamiento

Una vez que hayas agregado las funciones anteriores a tu bash solo debes llamar la función target y definir el Objetivo, por ejemplo:
```bash
target 10.10.10.50
```
Para limpiar el objetivo solo debes llamar la función target  sin ningún parámetro.
```bash
target
```

Felicidades! Ya lograsmos el objetivo.
