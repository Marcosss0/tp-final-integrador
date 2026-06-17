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

* Secret para las credenciales.
* Persistent Volume Claim para persistencia.
* Deployment para la ejecución del contenedor.
* Service para comunicación interna.

**Capturas:** Despliegue y funcionamiento de MariaDB.

---

## Despliegue de WordPress

Se desplegó WordPress mediante un Deployment conectado al servicio de MariaDB. La aplicación quedó accesible desde el clúster y posteriormente mediante Ingress.

**Capturas:** Despliegue y acceso a WordPress.

---

## Persistencia de Datos

Se utilizaron Persistent Volumes y Persistent Volume Claims para garantizar la persistencia de la información almacenada por WordPress y MariaDB.

Esto permite conservar los datos incluso ante reinicios o recreación de Pods.

**Capturas:** PVC y almacenamiento persistente.

---

## Configuración de Ingress

Se configuró un recurso Ingress utilizando Traefik para exponer la aplicación mediante el dominio local:

```text
wordpress.local
```

Esto permitió acceder a la aplicación sin necesidad de utilizar direcciones IP o puertos manuales.

**Capturas:** Configuración del Ingress y acceso desde navegador.

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
