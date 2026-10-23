# Actividad — Retos de mínimo privilegio en Linux

Práctica de **Ciber Hacking** en Kali Linux sobre el principio de mínimo privilegio. En cada reto se observa una configuración insegura, se corrige y se comprueba que el uso legítimo sigue funcionando.

## Laboratorio

Realiza la práctica únicamente en la máquina virtual aislada de Kali. Crea un snapshot antes de empezar. El script `retos.sh` se añadirá a `archivos/` una vez revisado.

Prepara un reto sustituyendo `N` por su número del 1 al 7:

```bash
sudo bash retos.sh preparar N
```

Al terminar la corrección, ejecuta la comprobación del reto:

```bash
sudo bash retos.sh comprobar N
```

Para limpiar el laboratorio al acabar:

```bash
sudo bash retos.sh limpiar
```

## Reto 1 — Permisos de ficheros

**Tema:** permisos demasiado abiertos sobre las nóminas. Documentación del reto y evidencia de Kali:

![Evidencia del reto 1](evidencias/reto-1-permisos-ficheros.png)

## Reto 2 — Grupo sudo

**Tema:** pertenencia innecesaria de un usuario al grupo administrativo. Documentación y evidencia:

![Evidencia del reto 2](evidencias/reto-2-grupo-sudo.png)

## Reto 3 — Reglas sudoers

**Tema:** autorización excesiva para ejecutar un programa con privilegios. Documentación y evidencia:

![Evidencia del reto 3](evidencias/reto-3-sudoers.png)

## Reto 4 — Bit SUID

**Tema:** ejecutable de prueba con SUID de root. Documentación y evidencia:

![Evidencia del reto 4](evidencias/reto-4-suid.png)

## Reto 5 — Capabilities

**Tema:** capability que concede privilegios innecesarios a un ejecutable. Documentación y evidencia:

![Evidencia del reto 5](evidencias/reto-5-capabilities.png)

## Reto 6 — Tareas programadas con cron

**Tema:** script ejecutado por root que puede ser modificado por usuarios no autorizados. Documentación y evidencia:

![Evidencia del reto 6](evidencias/reto-6-cron.png)

## Reto 7 — Servicio web

**Tema:** servicio ejecutado con privilegios excesivos y con un directorio publicado demasiado amplio. Documentación y evidencia:

![Evidencia del reto 7](evidencias/reto-7-servicio-web.png)
