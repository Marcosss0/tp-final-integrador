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

##Instalacion de Kubernetes

Para la implementación del proyecto se seleccionó K3s, una distribución ligera de Kubernetes adecuada para entornos de laboratorio y aprendizaje.

### Instalación de K3s

La instalación se realizó utilizando el script oficial:

curl -sfL https://get.k3s.io | sh -

### Configuración de kubectl

Luego de la instalación se configuró el acceso al clúster mediante el archivo kubeconfig.

Comandos utilizados:

mkdir -p ~/.kube
sudo cp /etc/rancher/k3s/k3s.yaml ~/.kube/config
sudo chown marcos:marcos ~/.kube/config
chmod 600 ~/.kube/config

Se verificó el correcto funcionamiento del nodo mediante:
kubectl get nodes

Resultado obetenido en caputra (k3s.instalado)
