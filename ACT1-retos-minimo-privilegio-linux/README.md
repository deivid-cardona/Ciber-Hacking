# Actividad — Retos de mínimo privilegio en Linux

Práctica de **Ciber Hacking** en Kali Linux sobre el principio de mínimo privilegio. En cada reto se analiza una configuración insegura, se aplica una corrección y se comprueba que el uso legítimo sigue funcionando.

## Laboratorio

Las pruebas se realizan únicamente en la máquina virtual aislada de Kali, con un snapshot previo. El usuario `becario` representa al usuario sin autorización administrativa y la cuenta administradora aplica las correcciones. El script `retos.sh` está pendiente de incorporación al repositorio.

Preparación de cada reto, sustituyendo `N` por un número del 1 al 7:

```bash
sudo bash retos.sh preparar N
```

Comprobación después de corregirlo:

```bash
sudo bash retos.sh comprobar N
```

Limpieza al terminar:

```bash
sudo bash retos.sh limpiar
```

Los apartados siguientes describen el procedimiento y lo que se espera comprobar. Las capturas de cada reto se guardan directamente en `evidencias/`.

## Reto 1 — Permisos de ficheros

Se analiza `/srv/empresa/nominas.csv`, preparado con propietario `root`, grupo `rrhh` y permisos `666`. Con esta configuración, cualquier usuario que pueda acceder a la ruta puede leer y modificar el fichero. La inspección con `ls -l` permite identificar los permisos de propietario, grupo y otros.

La prueba como `becario` consiste en leer las nóminas ficticias y añadir una entrada de prueba. Así se muestra el impacto sobre la confidencialidad y la integridad de los datos. La corrección prevista es `sudo chmod 660 /srv/empresa/nominas.csv`, conservando el propietario y el grupo previstos: root y RR. HH. mantienen lectura y escritura, mientras que los demás usuarios pierden ambos permisos.

La comprobación debe mostrar que `becario` ya no puede leer ni escribir y que `ana_rrhh` conserva la lectura y escritura necesarias. No basta con que el comprobador automático indique que el reto está superado: se documentan también estas pruebas manuales.

Lección de seguridad: los permisos deben responder a una necesidad de trabajo, no concederse a todos para evitar problemas de acceso.

![Reto 1 — prueba del ataque](evidencias/reto1_ataque.png)

## Reto 2 — Grupo sudo

Se revisan los grupos de `becario` mediante `id` y sus autorizaciones mediante `sudo -l`. El laboratorio lo incorpora al grupo `sudo`, que en la configuración de Kali utilizada en el reto permite obtener una sesión administrativa con su propia contraseña.

La prueba controlada permite observar la diferencia entre iniciar sesión como un usuario normal y disponer de una autorización que permite actuar como root. La corrección prevista es retirar a `becario` del grupo mediante `sudo gpasswd -d becario sudo`, sin modificar los permisos de la cuenta administradora.

La verificación debe realizarse también desde una sesión nueva de `becario`, ya que una sesión abierta puede conservar sus grupos anteriores. Se debe comprobar que no obtiene una nueva sesión administrativa y que sigue pudiendo realizar operaciones ordinarias en su directorio personal. Las sesiones root abiertas durante la prueba deben cerrarse: quitar un grupo no revoca por sí solo una shell privilegiada ya iniciada.

Lección de seguridad: los grupos administrativos se asignan solo a quien los necesita y su retirada debe acompañarse de la revisión de sesiones existentes.

![Reto 2 — prueba del ataque](evidencias/reto2_ataque.png)

![Reto 2 — corrección](evidencias/reto2_arreglo.png)

## Reto 3 — Reglas sudoers

Se analiza `/etc/sudoers.d/reto3` y la salida de `sudo -l`. El laboratorio autoriza a `becario` a ejecutar `/usr/bin/find` como root sin contraseña. Aunque el objetivo legítimo es limpiar temporales, conceder acceso libre a ese programa permite operaciones que van más allá de la tarea prevista.

La corrección consiste en editar la regla con `sudo visudo -f /etc/sudoers.d/reto3` para autorizar únicamente el script administrativo `/usr/local/sbin/limpiar_tmp.sh`. La regla prevista es `becario ALL=(root) NOPASSWD: /usr/local/sbin/limpiar_tmp.sh ""`. Las comillas vacías restringen su ejecución a una llamada sin argumentos. El script y su ruta deben permanecer bajo control administrativo y sin escritura para `becario`.

La comprobación debe mostrar que ya no se permite ejecutar `find` directamente con sudo, que el intento original deja de funcionar y que la limpieza autorizada sigue ejecutándose. El reto 2 debe estar corregido para que una autorización administrativa de grupo no invalide esta restricción.

Lección de seguridad: limitar sudo a un programa no es suficiente cuando ese programa permite ejecutar otras acciones con privilegios.

![Reto 3 — prueba del ataque](evidencias/reto3_ataque.png)

![Reto 3 — desarrollo y corrección](evidencias/reto3_completo.png)

## Reto 4 — Bit SUID

Se inspecciona `/usr/local/bin/verlog`, una copia de `cat` preparada con SUID de root. El listado de permisos muestra una `s` en la posición de ejecución del propietario. En el entorno del reto, esto permite que la copia lea archivos con los privilegios efectivos de su propietario.

La prueba controlada como `becario` demuestra el acceso indebido a un archivo protegido del laboratorio. Para corregirlo sin eliminar la herramienta se utiliza `sudo chmod u-s /usr/local/bin/verlog`: se retira el privilegio especial y se mantiene el funcionamiento ordinario de lectura.

La verificación debe mostrar que el bit SUID ha desaparecido, que la herramienta ya no permite a `becario` leer el archivo protegido y que sí puede leer un archivo de prueba al que el usuario tenga acceso normal. Los hashes de contraseñas no se publican en la captura.

Lección de seguridad: un ejecutable con privilegios especiales debe tener una justificación concreta y un alcance estrictamente limitado.

![Reto 4 — desarrollo y corrección](evidencias/reto4_completo.png)

## Reto 5 — Capabilities

Se revisa `/usr/local/bin/pyherramienta` con `getcap`. El script prepara una copia de Python con `cap_setuid`, una capability que permite cambiar identificadores de usuario y que resulta peligrosa en un intérprete de propósito general.

La prueba controlada del laboratorio permite observar el cambio de identidad con privilegios. La corrección prevista es `sudo setcap -r /usr/local/bin/pyherramienta`, que retira las capabilities del ejecutable sin impedir su uso ordinario como intérprete.

La comprobación debe mostrar que `getcap` ya no devuelve la capability para esa copia, que la prueba de cambio a UID 0 falla como `becario` y que el intérprete sigue ejecutando código ordinario sin privilegios. No se deben confundir las capabilities de esta copia de laboratorio con las de otros binarios legítimos del sistema.

Lección de seguridad: una sola capability puede conceder mucho más poder del que necesita una herramienta.

![Reto 5 — desarrollo y corrección](evidencias/reto5_completo.png)

## Reto 6 — Tareas programadas con cron

Se revisan `/etc/cron.d/reto6`, `/opt/tareas/limpieza.sh` y los permisos de su directorio. La tarea ejecuta el script como root cada minuto, pero el laboratorio lo prepara con permisos `777`, permitiendo que un usuario no autorizado cambie su contenido.

La prueba controlada añade una acción de identificación al script y comprueba qué usuario la ejecuta. La corrección consiste en retirar esa línea de prueba, mantener el contenido legítimo de limpieza y asegurar el propietario y los permisos tanto del script como de su carpeta. La configuración prevista es `root:root` y permisos `755` para `/opt/tareas` y `/opt/tareas/limpieza.sh`, sin directorios anteriores escribibles por `becario`.

La validación debe demostrar que `becario` no puede editar ni sustituir el script y que cron sigue ejecutando la limpieza prevista. Para comprobar el funcionamiento se puede utilizar un archivo `.old` creado expresamente para la práctica y verificar su retirada tras el siguiente ciclo. La existencia de un resultado antiguo de la prueba no demuestra una ejecución nueva.

Lección de seguridad: la integridad de una tarea privilegiada depende del script y de todos los directorios que permiten acceder a él o sustituirlo.

![Reto 6 — desarrollo y corrección](evidencias/reto6_completo.png)

## Reto 7 — Servicio web

Se inspeccionan el proceso de la intranet, su configuración en `/etc/systemd/system/intranet.service` y el puerto `8080`. El laboratorio ejecuta el servidor como root y publica `/`, exponiendo archivos que no forman parte de la web.

La corrección requiere dos cambios: ejecutar el servicio con una cuenta dedicada sin privilegios y restringir el directorio publicado a `/srv/web`. En la unidad de systemd se establece `User=intranet` y `ExecStart=/usr/bin/python3 -m http.server 8080 --directory /srv/web`, creando previamente la cuenta de servicio si no existe y asegurando que puede leer el contenido web. Después se recarga la configuración con `sudo systemctl daemon-reload` y se reinicia mediante `sudo systemctl restart intranet`.

La comprobación debe mostrar que el proceso ya no pertenece a root, que el acceso HTTP a `/etc/shadow` falla y que la página de la intranet sigue respondiendo. Parar el servicio no es una solución válida, porque elimina también el uso legítimo. El servicio permanece limitado al laboratorio aislado.

Lección de seguridad: un servicio debe disponer de los mínimos privilegios y publicar únicamente los recursos que necesita servir.

![Reto 7 — desarrollo y corrección](evidencias/reto7_completo.png)

## Resumen de la práctica

![Resumen final de los retos](evidencias/resumen_final.png)
