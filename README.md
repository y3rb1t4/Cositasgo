# Aprendiendo Golang Al Estilo BlackHat







## ¿POR QUÉ USAR GO PARA HACKEAR?

Antes de Go, podías priorizar la facilidad de uso utilizando lenguajes de tipado dinámico—como Python, Ruby o PHP a costa del rendimiento y la seguridad. Alternativamente, podías elegir un lenguaje de tipado estático, como C o C++, que ofrece un alto rendimiento y seguridad pero no es muy amigable para el usuario. Go está despojado de gran parte de la fealdad de C, su ancestro principal, haciendo el desarrollo más amigable para el usuario. Al mismo tiempo, es un lenguaje de tipado estático que produce errores de sintaxis en tiempo de compilación, aumentando tu seguridad de que tu código realmente se ejecutará de manera segura. Como es compilado, se desempeña más óptimamente que los lenguajes interpretados y fue diseñado con consideraciones de computación multicore, haciendo que la programación concurrente sea muy fácil.
Estas razones para usar Go no conciernen específicamente a los profesionales de seguridad. Sin embargo, muchas de las características del lenguaje son particularmente útiles para hackers y adversarios:


## Capitulo 1 -Introducción a fundamentos de Go: 

Sintaxis básica, ecosistema, herramientas, IDEs, gestión de dependencias. Esencial para comprender e implementar ejemplos avanzados.

## Capitulo 2 - Introducción a TCP en Go: 

Escáneres de puertos, concurrencia, I/O, proxies TCP, y recreación de características de seguridad de Netcat.

## Capitulo 3 - Creación de clientes HTTP:

Para interacciones web con servidores modernos, manejo de formatos con Shodan y Metasploit, y uso de motores de búsqueda para extracción de metadatos y perfilado organizacional.


## Capitulo 2

* Writing a TCP Scanner
* Building a TCP Proxy
* * Using **io.Reader** and **io.Writer**
* Creating a TCP echo server
* Proxying a TCP client
* Replicating **NetCat** for Command execution

Estructura de aprendizaje para Go sobre TCP, Scanners y Proxies:

Fundamentos de Go: Antes de sumergirte en los detalles de TCP, Scanners y Proxies, es importante tener una sólida comprensión de los fundamentos de Go. Esto incluye la sintaxis básica, el ecosistema, las herramientas, los IDEs y la gestión de dependencias.

Introducción a TCP en Go: Una vez que tengas una buena comprensión de los fundamentos de Go, puedes comenzar a aprender sobre TCP. Esto incluirá aprender sobre cómo Go maneja las conexiones TCP, cómo escribir un escáner de puertos y cómo construir un proxy TCP.

Escáneres de puertos: Aprenderás a escribir un escáner de puertos en Go, lo que te permitirá identificar puertos abiertos en una red.

Construcción de un Proxy TCP: Aprenderás a construir un proxy TCP en Go, lo que te permitirá reenviar tráfico de red a través de un servidor intermedio.

Uso de io.Reader y io.Writer: Estos son dos interfaces fundamentales en Go que te permitirán leer y escribir datos en una conexión TCP.

Creación de un servidor de eco TCP: Aprenderás a crear un servidor de eco TCP, que es un servidor que simplemente devuelve los datos que recibe.

Proxy de un cliente TCP: Aprenderás a proxy un cliente TCP, lo que te permitirá interceptar y modificar el tráfico de red de un cliente.

Replicación de NetCat para la ejecución de comandos: NetCat es una herramienta de red que se utiliza a menudo para la depuración y la exploración de redes. Aprenderás a replicar algunas de sus funcionalidades en Go.

Creación de clientes HTTP: Aunque no es específicamente sobre TCP, aprender a crear clientes HTTP en Go te permitirá interactuar con servidores web modernos, manejar formatos con Shodan y Metasploit, y usar motores de búsqueda para la extracción de metadatos y el perfilado organizacional.

Proyectos prácticos: Finalmente, la mejor manera de aprender es haciendo. Trata de construir tus propios proyectos utilizando lo que has aprendido. Esto podría incluir la construcción de tu propio escáner de puertos, proxy TCP o cliente HTTP.

```golang
package main

import (
    "fmt"
    "net"
    "sort"
    "time"
)

func worker(ports, results chan int) {
    for p := range ports {
        address := fmt.Sprintf("localhost:%d", p)
        conn, err := net.Dial("tcp", address)
        if err != nil {
            results <- 0
            continue
        }
        conn.Close()
        results <- p
    }
}

func main() {
    ports := make(chan int, 100)
    results := make(chan int)
    var openports []int

    for i := 0; i < cap(ports); i++ {
        go worker(ports, results)
    }

    go func() {
        for i := 1; i <= 1024; i++ {
            ports <- i
        }
    }()

    for i := 0; i < 1024; i++ {
        port := <-results
        if port != 0 {
            openports = append(openports, port)
        }
    }

    close(ports)
    close(results)
    sort.Ints(openports)
    for _, port := range openports {
        fmt.Printf("%d open\n", port)
    }
}
```