# Intentando Configurar la Instalacion de Golang

Configurando GOROOT para Definir la Ubicación del Binario de Go
A continuación, el sistema operativo necesita saber cómo encontrar la instalación de Go. En la mayoría de los casos, si has instalado Go en la ruta predeterminada, como /usr/local/go en un sistema basado en *Nix/BSD, no tienes que tomar ninguna acción aquí. Sin embargo, en el caso de que hayas elegido instalar Go en una ruta no estándar o estés instalando Go en Windows, necesitarás indicarle al sistema operativo dónde encontrar el binario de Go.
Puedes hacer esto desde tu línea de comandos configurando la variable de entorno reservada GOROOT a la ubicación de tu binario. Configurar variables de entorno es específico del sistema operativo. En Linux o macOS, puedes agregar esto a tu ~/.profile



```golang
package main

import (
    "fmt"
)

func main() {
    fmt.Println("Gophers Happy Hacking!")
}
```
