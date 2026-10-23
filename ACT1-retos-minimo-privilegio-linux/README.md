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
- Script `retos.sh`, pendiente de incorporación al repositorio.
- Usuario de laboratorio `becario`, creado por el script.
- Entorno virtualizado y aislado.

> Realiza las pruebas únicamente en la máquina virtual del laboratorio. Crea un snapshot antes de comenzar.

## Preparación y ciclo de trabajo

Prepara el reto que vayas a realizar, sustituyendo `N` por su número:

```bash
sudo bash retos.sh preparar N
```

Comprueba el reto después de aplicar la corrección:

```bash
sudo bash retos.sh comprobar N
```

Al terminar el laboratorio:

```bash
sudo bash retos.sh limpiar
```

## 1. Permisos de ficheros

Apartado pendiente de completar con los comandos ejecutados, la explicación de la corrección y la captura real de Kali, que se mostrará aquí mismo. Las imágenes se guardarán directamente en una única carpeta `evidencias/`, sin subcarpetas por reto.
