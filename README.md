# tp-final-integrador

# Trabajo Final Integrador - Administración de Sistemas Avanzada. Despliegue de WordPress y MariaDB sobre Kubernetes con NFS, Ingress y Helm.

## Alumno
Marcos Leal

## Objetivo del Proyecto

El objetivo de este proyecto fue implementar una aplicación multicapa sobre Kubernetes utilizando K3s, integrando almacenamiento persistente, acceso mediante Ingress, almacenamiento compartido mediante NFS, tolerancia a fallos y gestión de despliegues con Helm.

## Arquitectura de la Solución

La arquitectura implementada está compuesta por los siguientes componentes:

* K3s como plataforma de orquestación de contenedores.
* WordPress como aplicación web.
* MariaDB como base de datos.
* Persistent Volumes (PV) y Persistent Volume Claims (PVC) para persistencia.
* Servidor NFS para almacenamiento compartido.
* Ingress mediante Traefik para acceso externo.
* Helm para empaquetado y administración de despliegues.

### Diagrama lógico

```
Usuario
   │
   ▼
wordpress.local
   │
   ▼
Ingress (Traefik)
   │
   ▼
WordPress
   │
   ▼
MariaDB
   │
   ▼
PVC / PV
   │
   ▼
NFS
```
## Instalación de K3s

Se instaló K3s como distribución ligera de Kubernetes sobre Ubuntu Server 24.04. Una vez finalizada la instalación, se verificó el correcto funcionamiento del clúster mediante el comando:

<img width="495" height="69" alt="k3s-instalado" src="https://github.com/user-attachments/assets/bf6a3030-4dd8-487a-914e-58f54abb9e4f" />

---

## Despliegue de MariaDB

Se implementó MariaDB como motor de base de datos para WordPress. Para ello se crearon:

- Secret para almacenar la contraseña de acceso.
- Persistent Volume Claim (PVC) para garantizar la persistencia de los datos.
- Deployment para ejecutar el contenedor MariaDB.
- Service para permitir la comunicación interna dentro del clúster.

La correcta implementación fue verificada comprobando que el Pod se encontraba en estado Running y que el PVC asociado estaba correctamente vinculado (Bound).

### Service de MariaDB

<img width="775" height="97" alt="mariadb-service" src="https://github.com/user-attachments/assets/9f2d3985-6b8c-4cbf-9930-c665bb5cca38" />

### Pod y PVC de MariaDB

<img width="1123" height="121" alt="mariadb-running-pvc-bound" src="https://github.com/user-attachments/assets/ee03fa5c-6ec7-4ae5-8728-eacd7c3ad38a" />

---

## Despliegue de WordPress

Se desplegó WordPress mediante un Deployment conectado al servicio de MariaDB. La aplicación quedó accesible desde el clúster y posteriormente mediante Ingress.

<img width="646" height="67" alt="wordpress-mariadb-running" src="https://github.com/user-attachments/assets/ef5898d3-c908-4267-849e-392c06edc291" />

### Acceso a WordPress mediante Ingress

Una vez desplegada la aplicación y configurado el Ingress, se verificó el acceso a WordPress utilizando el dominio local configurado para el proyecto.

<img width="1490" height="819" alt="wordpress-instalacion" src="https://github.com/user-attachments/assets/68938fbf-9867-4f12-a171-5769189be77d" />

---

## Persistencia de Datos

Se utilizaron Persistent Volumes (PV) y Persistent Volume Claims (PVC) para proporcionar almacenamiento persistente a los componentes del proyecto. Los volúmenes fueron utilizados tanto por WordPress y MariaDB como por el almacenamiento compartido basado en NFS.

<img width="1332" height="249" alt="nfs-pv-pvc-bound" src="https://github.com/user-attachments/assets/ec3db58e-7288-48bd-a38d-31d8c2924803" />

---

## Configuración de Ingress

Para permitir el acceso externo a la aplicación se configuró un recurso Ingress utilizando Traefik para exponer la aplicación mediante el dominio local 'wordpress.local'.

<img width="654" height="68" alt="ingress-creado" src="https://github.com/user-attachments/assets/0fa3ce0e-23c4-4b9d-9b30-4494af29663a" />

Posteriormente se verificó la configuración del recurso comprobando el host asociado, la clase Ingress utilizada y el Service de destino dentro del clúster Kubernetes.

<img width="655" height="243" alt="ingress-describe" src="https://github.com/user-attachments/assets/e75d41b2-4b4c-4102-b068-42f142570926" />

---

## Implementación de NFS

Se configuró un servidor NFS para proporcionar almacenamiento compartido dentro del entorno.

Posteriormente se creó:

* Persistent Volume NFS.
* Persistent Volume Claim NFS.

Finalmente se verificó su funcionamiento mediante un Pod de prueba que escribió correctamente un archivo dentro del recurso compartido.

**Capturas:** Configuración y prueba de NFS.

---

## Prueba de Tolerancia a Fallos

Se realizó una prueba de self-healing eliminando manualmente el Pod de WordPress.

Kubernetes detectó la pérdida de la réplica y creó automáticamente un nuevo Pod, manteniendo la disponibilidad de la aplicación.

**Capturas:** Eliminación y recreación automática del Pod.

---

## Implementación de Helm

Se instaló Helm como gestor de paquetes para Kubernetes.

Durante la implementación se realizaron las siguientes tareas:

* Instalación de Helm.
* Creación de un Chart.
* Validación mediante `helm template`.
* Empaquetado mediante `helm package`.

**Capturas:** Instalación y empaquetado del Chart.

---

## Resultados Obtenidos

Se logró desplegar correctamente una aplicación multicapa basada en WordPress y MariaDB sobre Kubernetes, incorporando persistencia de datos, almacenamiento compartido mediante NFS, acceso mediante Ingress, tolerancia a fallos y empaquetado mediante Helm.

---

## Conclusiones

Este proyecto permitió aplicar conceptos avanzados de administración de sistemas relacionados con Kubernetes, almacenamiento persistente, servicios de red, tolerancia a fallos y automatización de despliegues.

La integración de estas tecnologías permitió construir una solución funcional, escalable y alineada con prácticas modernas de infraestructura y orquestación de contenedores.
