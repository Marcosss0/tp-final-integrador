# Instalación y preparación del entorno

## Configuración inicial de la máquina virtual

Se utilizó una máquina virtual con Ubuntu Server 24.04 para el desarrollo del proyecto.

Características asignadas:

- RAM: 8 GB
- CPU: 2 núcleos
- Disco virtual: 25 GB

## Expansión del almacenamiento

Se amplió el volumen lógico LVM para utilizar la totalidad del espacio disponible del disco virtual.

Comandos utilizados:

sudo lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
sudo resize2fs /dev/ubuntu-vg/ubuntu-lv

Resultado:
- Espacio total disponible: 23 GB
- Espacio libre aproximado: 15 GB

Evidencia: 01-disco-expandido.png
