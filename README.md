# 🛡️ Tor Browser Aislado y Anti-Forense en Docker

Configuración de Docker Compose para ejecutar **Tor Browser** de forma aislada, protegiendo el sistema anfitrión (host) de malware y eliminando los rastros de navegación.

Fue ejecutado en **Debian 13 (Trixie) XFCE de 64 bits**.

## ¿Cuál es el Objetivo?

El objetivo de este proyecto no es el anonimato de red (para eso esta Tails OS), sino la **seguridad del host** y la **amnesia forense**.

1.  **Aislamiento:** El navegador se ejecuta en un contenedor sellado. Si descargas o ejecutas malware desde la darknet, quedará atrapado dentro del contenedor y no podrá infectar tu sistema Debian.
2.  **Anti-Forense (Amnesia Manual):** La configuración está diseñada para no escribir ningún dato en el disco duro. Al destruir el contenedor, todos los rastros de la sesión (historial, cookies, caché) se eliminan.

## Implementación y Uso

### Requisitos Previos

  * Un sistema host Debian (o similar) con **Docker** y **Docker Compose** (plugin) instalados.
  * El usuario estándar debe pertenecer al grupo docker.
  * Ver los pasos de instalación del proyecto Checkmk RAW 2.4.x en Docker - https://github.com/Chelo2025/Docker-Checkmk
  * Reiniciar el sistema operativo para que se apliquen los cambios.

### Revisar la Configuración

Este docker-compose.yml está optimizado para la seguridad y se ejecuta como un usuario sin privilegios (no-root).

Revisar correctamente la resolución de pantalla configurada.

Ejecuta id -u y id -g en tu terminal

Reemplazar 1000 si tus IDs son diferentes.

### Iniciar el Contenedor

Este comando levanta el servicio en segundo plano (-d), hay que estar en la ruta donde se encuentre el archivo docker-compose.yml para ejecutar el comando:

docker compose up -d

### Conectar y Navegar

1.  Abre tu navegador Firefox.
2.  Visita la dirección: http://localhost:5800
3.  Dentro de la pestaña, verás el **Tor Browser**. Haz clic en "Connect" para iniciar la conexión a la red Tor.

## DESTRUCCIÓN ANTI-FORENSE

Para que esta configuración sea anti-forense, **NO BASTA CON REINICIAR LA PC**. Si solo reinicias, el contenedor "detenido" permanecerá en tu disco duro con todo el historial.

Debes **destruir manualmente** el contenedor cada vez que termines tu sesión.

### 1. Detener y Destruir el Contenedor

Abre una terminal, estar en la ruta del archivo docker-compose.yml y ejecuta:

docker compose down

### 2. ¿Qué hace este comando?
docker compose down detiene (stop) y **elimina** (rm) el contenedor. Al eliminarlo, se destruye todo su sistema de archivos interno (que solo vivía en la RAM y en capas temporales de Docker), borrando de forma irrecuperable todo tu historial, caché y cookies.

## Estructura Anti-Forense

**NO utiliza volúmenes persistentes** a propósito.

  * El perfil del navegador se escribe en el sistema de archivos efímero del contenedor.
  * Al no haber un "volumen", no hay datos que persistan en el disco duro del host después de ejecutar docker compose down.

## Advertencias de Seguridad

1.  **Amnesia Manual:** Tu privacidad depende de que **tú te acuerdes** de ejecutar docker compose down. Si se te olvida, dejas un rastro forense.
2.  **No es Anónimo (A Nivel de Red):** Tu Proveedor de Internet (ISP) puede ver que tu IP se está conectando a la red Tor. Para un anonimato de red profesional, usa una **VPN (ProtonVPN, Mullvad)** en tu host Debian 13 *antes* de iniciar el contenedor.

## Autor

### Marcelo Martinez - Chelo2025

🎓 Estudiante de Licenciatura en Tecnologías Digitales

🛡️ Técnico Superior en Redes Informáticas

🎓 Estudiante en Diplomado en Administración de Redes Linux, Ciberseguridad y Hacking Ético

🔗 GitHub: [https://github.com/Chelo2025](https://github.com/Chelo2025)
