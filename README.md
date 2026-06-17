
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
Posteriormente se creó un Persistent Volume (PV) asociado al recurso NFS y un Persistent Volume Claim (PVC) para permitir su utilización desde los Pods del clúster.

Finalmente se verificó su funcionamiento mediante un Pod de prueba que escribió correctamente un archivo dentro del directorio exportado. La lectura posterior del archivo confirmó el correcto acceso al almacenamiento compartido.

### Persistent Volume y Persistent Volume Claim de NFS

<img width="1332" height="249" alt="nfs-pv-pvc-bound" src="https://github.com/user-attachments/assets/653fbc5c-8fce-485c-a9e0-58e43fa7f673" />

### Prueba de funcionamiento

<img width="1121" height="129" alt="nfs-funcionando" src="https://github.com/user-attachments/assets/c63d22f3-7563-4532-ae3e-c6cddac63c0a" />

---

## Prueba de Tolerancia a Fallos

Con el objetivo de verificar la capacidad de recuperación automática del clúster, se realizó una prueba de tolerancia a fallos sobre la aplicación WordPress.
Inicialmente se comprobó que el Pod de WordPress se encontraba en ejecución junto al resto de los componentes del proyecto.

### Estado inicial de los Pods

<img width="554" height="113" alt="pods-antes-fallo" src="https://github.com/user-attachments/assets/5e3b2ed0-c19c-4940-a430-60b03f54e9f5" />

Posteriormente se eliminó manualmente el Pod de WordPress para simular un fallo de la aplicación.

Kubernetes detectó automáticamente la pérdida de la réplica y creó un nuevo Pod para mantener el estado deseado definido en el Deployment.

### Eliminación y recreación automática del Pod

<img width="664" height="118" alt="wordpress-post-fallo1" src="https://github.com/user-attachments/assets/3d366a24-bcc1-4e93-a9f8-066831a23d90" />

Finalmente se verificó que el ReplicaSet continuaba manteniendo una réplica disponible y que la aplicación permanecía operativa.

### Verificación del ReplicaSet

<img width="534" height="175" alt="wordpress-post-fallo2" src="https://github.com/user-attachments/assets/b200b759-56ab-492b-9980-d4f034a50d27" />


## Implementación de Helm

Con el objetivo de simplificar la gestión y distribución de aplicaciones Kubernetes, se utilizó Helm como gestor de paquetes para Kubernetes.

Inicialmente se instaló Helm en el entorno de trabajo y se verificó su correcto funcionamiento.

### Instalación de Helm

<img width="1301" height="141" alt="instalacion-helm" src="https://github.com/user-attachments/assets/8b538c0c-9a58-4e29-97eb-adcabfcffeda" />

Posteriormente se creó un Chart denominado 'wordpress-chart', el cual contiene las plantillas necesarias para desplegar recursos Kubernetes.

Se validó el funcionamiento del Chart utilizando el comando 'helm template', generando los manifiestos Kubernetes a partir de las plantillas definidas en el Chart.

### Validación del Chart

<img width="551" height="818" alt="helm-template" src="https://github.com/user-attachments/assets/a36cfc2c-e3a1-411d-84e7-2379edc15ba8" />

Finalmente se empaquetó el Chart en un archivo comprimido '.tgz', permitiendo su reutilización y distribución.

### Empaquetado del Chart

<img width="976" height="76" alt="helm-package" src="https://github.com/user-attachments/assets/7eaba87f-7532-4354-b441-5084f6c51c5a" />

---

## Resultados Obtenidos

Se logró desplegar correctamente una aplicación WordPress con base de datos MariaDB sobre Kubernetes utilizando K3s.

Además, se implementaron mecanismos de persistencia mediante PV y PVC, almacenamiento compartido con NFS, acceso mediante Ingress utilizando wordpress.local, tolerancia a fallos mediante la recreación automática de Pods y empaquetado de recursos con Helm.

El proyecto permitió integrar y validar el funcionamiento conjunto de todas estas tecnologías en un entorno funcional.
