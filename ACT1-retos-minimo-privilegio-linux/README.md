# Actividad — Retos de mínimo privilegio en Linux

> Práctica de Ciber Hacking para aprender a identificar, corregir y verificar configuraciones inseguras en un laboratorio Linux.

## Descripción

En esta actividad individual trabajarás con siete retos preparados mediante el script `retos.sh`, en una máquina virtual de Kali Linux. En cada reto analizarás una configuración insegura, reproducirás su efecto en el entorno controlado y aplicarás una corrección basada en el principio de mínimo privilegio.

El objetivo es demostrar que comprendes qué estaba mal configurado, qué medidas permiten corregirlo y cómo comprobar que la solución es efectiva sin impedir el funcionamiento legítimo del sistema.

## Objetivos

- Identificar configuraciones inseguras relacionadas con usuarios, permisos, servicios y tareas programadas.
- Observar sus consecuencias dentro del laboratorio.
- Aplicar correcciones con privilegios de administración.
- Verificar que la configuración corregida bloquea el acceso no autorizado.
- Comprobar que las funciones legítimas siguen operativas.
- Documentar el procedimiento con comandos, resultados y capturas propias.

## Retos de la actividad

| Reto | Tema |
|---:|---|
| 1 | Permisos de ficheros |
| 2 | Pertenencia al grupo `sudo` |
| 3 | Reglas de `sudoers` y permisos sobre programas |
| 4 | Bit SUID |
| 5 | Capabilities de Linux |
| 6 | Tareas programadas con `cron` |
| 7 | Usuario y directorio de un servicio web |

## Entorno de trabajo

- Máquina virtual de Kali Linux.
- Permisos de administración mediante `sudo`.
- Script `retos.sh`, que se incorporará en la carpeta `archivos/` tras revisar su funcionamiento.
- Usuario de laboratorio `becario`, creado por el script.
- Entorno virtualizado y aislado.

> **Importante:** realiza esta actividad únicamente en la máquina virtual preparada para el laboratorio. No ejecutes el script en un equipo de producción ni en sistemas ajenos. Antes de comenzar, crea un snapshot de la máquina virtual para poder restaurarla si fuera necesario.

## Preparación y ciclo de trabajo

Desde el directorio donde se encuentre `retos.sh`, prepara un reto con:

```bash
sudo bash retos.sh preparar N
```

Sustituye `N` por el número del reto, del `1` al `7`. Sigue las indicaciones que muestra el script para observar la configuración inicial y realizar las pruebas como `becario`. Después, vuelve a la sesión administrativa para aplicar la corrección y verificar el resultado.

Para comprobar un reto:

```bash
sudo bash retos.sh comprobar N
```

Al terminar el laboratorio, puedes limpiar los elementos creados por el script con:

```bash
sudo bash retos.sh limpiar
```

El script también permite preparar todos los retos mediante `preparar todos` y comprobarlos conjuntamente mediante `comprobar todos`. Para aprender y documentar cada configuración por separado, se recomienda trabajar en orden y preparar un reto cada vez.

## Documentación de la práctica

Documenta cada reto en el PDF de entrega. Para cada uno, incluye:

- **Configuración inicial:** evidencia de la configuración insegura, obtenida con comandos de inspección adecuados.
- **Prueba en el laboratorio:** evidencia de que el efecto de la mala configuración se reproduce como `becario`.
- **Corrección aplicada:** comandos ejecutados como administrador y una breve explicación de su función.
- **Verificación:** prueba de que el acceso no autorizado ya no funciona y de que el uso legítimo continúa operativo.
- **Lección aprendida:** una frase que resuma el principio de seguridad observado en el reto.

Las capturas deben mostrar claramente el prompt con el usuario y el nombre de la máquina, el comando ejecutado y su salida. Utiliza resultados obtenidos por ti durante la práctica; no sustituyas las evidencias por ejemplos o salidas copiadas.

## Entrega

Entrega un único documento PDF con los siete retos en orden. El documento debe incluir las evidencias, los comandos utilizados, la explicación de las correcciones y la lección aprendida en cada reto.

No incluyas contraseñas personales, claves, datos reales ni información sensible en las capturas o en los archivos publicados.

## Estructura del repositorio

```text
ACT1-retos-minimo-privilegio-linux/
├── README.md
├── archivos/
└── evidencias/
    └── README.md
```

La carpeta `evidencias/` se reserva para organizar las capturas de la práctica. El PDF final es el documento de entrega.

## Recursos

- [GTFOBins](https://gtfobins.github.io/) — referencia para estudiar riesgos asociados al uso inseguro de determinados binarios.
- [Manual de sudoers](https://www.sudo.ws/docs/man/sudoers.man/)
- [Linux capabilities(7)](https://man7.org/linux/man-pages/man7/capabilities.7.html)
- [MITRE ATT&CK](https://attack.mitre.org/) — marco de referencia para técnicas relacionadas con escalada de privilegios y tareas programadas.
