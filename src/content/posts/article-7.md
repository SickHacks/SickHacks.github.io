---
title: "Fork Bomb - Entendiendo el ataque de denegación de servicio interno"
published: 2024-09-20
description: ''
image: ''
tags: [ciberseguridad, malware]
category: 'Malware'
draft: false 
lang: 'es'
---

En el mundo de la ciberseguridad, los ataques de Denegación de Servicio (DoS) suelen asociarse con técnicas que buscan saturar un sistema desde el exterior, pero existe una amenaza mucho más sutil que puede surgir dentro del propio sistema operativo: las fork bombs. En este artículo, exploraremos qué son, cómo funcionan y qué medidas se pueden tomar para mitigar su impacto.

![](img/fork.jpg)

Una fork bomb es un tipo de ataque DoS que se ejecuta a nivel local y afecta directamente a los recursos del sistema. Este ataque explota una función de los sistemas operativos basados en Unix, como Linux, para crear múltiples procesos que se replican rápidamente, saturando la CPU y la memoria del sistema. Como resultado, el sistema se vuelve inutilizable y puede llegar a colapsar.

El término "fork" proviene de la llamada al sistema fork(), que es responsable de crear un nuevo proceso duplicando el proceso actual. En una fork bomb, un programa malicioso invoca esta función en un bucle infinito, generando nuevos procesos hasta que el sistema se queda sin recursos.

## Diseccionando el script

Un ejemplo clásico de una fork bomb es el siguiente script en Bash, que se puede ejecutar en la mayoría de los sistemas Unix

```bash
:(){ :|:& };:
```

Para entenderlo mejor hay que verlo de la siguiente forma:

```bash
:() {
        :|:&
    };
:
```
Desglosemos su contenido:

1. :() Significa que se crea una función llamada :
2. {} Es donde uno escribe el contenido de la función.
3. Dentro de ella encontramos :|: , esto significa que la función se llama a si misma dos veces y pasa su salida de nuevo como entrada, creando una especie de bucle recursivo.
4. Se utiliza & para ejecutar el proceso en segundo plano, permitiendo que el bucle se repita sin esperar a que termine la ejecución de los procesos hijos.
5. Se utiliza ; para cerrar la función.
6. Finalmente, : ejecuta la función, lo que da inicio al ciclo destructivo.

![](https://itsfoss.com/content/images/wordpress/2022/09/fork-bomb-images.png)


## Impacto de una Fork Bomb

El principal objetivo de una fork bomb es consumir todos los recursos del sistema, específicamente los procesos. Los sistemas operativos limitan el número de procesos que pueden estar activos simultáneamente, y una fork bomb explota esta limitación creando miles de procesos en muy poco tiempo.

El efecto es una saturación completa de los recursos, donde los usuarios y el sistema operativo no pueden ejecutar más comandos o procesos, lo que provoca una paralización del sistema. Si bien una fork bomb no destruye datos ni compromete la integridad del sistema, puede requerir un reinicio forzado o un proceso de recuperación para restaurar la funcionalidad.

## ¿Dónde puede ocurrir?

Dado que una fork bomb necesita acceso local al sistema, este tipo de ataque es especialmente preocupante en entornos de multiusuario, como servidores compartidos, entornos académicos, o incluso redes corporativas. Un usuario malintencionado con acceso limitado al sistema puede ejecutar una fork bomb para interrumpir los servicios para otros usuarios.

:::IMPORTANT
### ¿Cómo podemos protegernos de este ataque?
:::

La mejor forma de protegerse de una fork bomb es limitar el número de procesos que cada usuario puede generar. Esto se puede hacer estableciendo políticas de uso de recursos a nivel del sistema operativo. En sistemas Linux, esta protección puede implementarse configurando el archivo /etc/security/limits.conf para restringir el número de procesos permitidos por usuario.

- Limitar procesos con /etc/security/limits.conf

```bash
$ username hard nproc 200
```
Este ajuste asegura que el usuario username no pueda crear más de 200 procesos simultáneamente, lo que limita significativamente el impacto de una fork bomb.
_________
Las fork bombs, aunque técnicamente simples, pueden comprometer la estabilidad de los sistemas si no se toman medidas preventivas. Es crucial establecer políticas claras de uso de recursos y límites en sistemas multiusuario. En ciberseguridad, protegerse de ataques internos es tan importante como enfrentar amenazas externas, y comprender estos métodos es esencial para mantener un sistema estable.

