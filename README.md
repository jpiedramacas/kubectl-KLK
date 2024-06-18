### Configuración de ELK para Kubernetes/microservicios

Este repositorio contiene los archivos de configuración necesarios para desplegar un stack ELK (Elasticsearch, Logstash, Kibana) en un entorno de Kubernetes. A continuación se explica el propósito de cada archivo de configuración:

- **elastic-service.yml**: Define el servicio para Elasticsearch.
- **elastic.yml**: Configura el despliegue de Elasticsearch como un StatefulSet.
- **filebeat-daemon-set.yml**: Despliega Filebeat como un DaemonSet para enviar logs a Logstash.
- **kibana.yml**: Despliega Kibana para la visualización de logs.
- **kibana.png**: Imagen ilustrativa de Kibana.
- **logstash-config.yml**: Configura Logstash para el formato de logs que Elasticsearch puede entender.
- **logstash-deployment.yml**: Despliega Logstash como un Deployment.
- **rbac.yml**: Define los roles y permisos necesarios (RBAC) para que los componentes de ELK accedan a recursos en Kubernetes.
- **web-deployment.yml**: Ejemplo de despliegue de una aplicación web para visualización de logs en Kibana.

Claro, aquí te dejo una revisión corregida enfocada únicamente en la sección de despliegue del stack ELK en Kubernetes, eliminando la parte del clonado del repositorio y mostrando los comandos necesarios para desplegar cada archivo individualmente y luego todos juntos:

---


### Pasos para el Despliegue

1. **RBAC (Role-Based Access Control)**
   
   ```bash
   kubectl apply -f rbac.yml
   ```

   - **Descripción**: Crea un service account con acceso a lectura a servicios, endpoints y namespaces.

2. **Elasticsearch**

   ```bash
   kubectl apply -f elastic.yml
   ```

   - **Descripción**: Define la configuración y el StatefulSet para Elasticsearch.

3. **Servicio para Elasticsearch**

   ```bash
   kubectl apply -f elastic-service.yml
   ```

   - **Descripción**: Crea un servicio para exponer Elasticsearch dentro del clúster.

4. **Logstash**

   ```bash
   kubectl apply -f logstash-config.yml
   kubectl apply -f logstash-deployment.yml
   ```

   - **Descripción**: Configura Logstash para recibir, procesar y enviar logs a Elasticsearch.

5. **Filebeat**

   ```bash
   kubectl apply -f filebeat-daemon-set.yml
   ```

   - **Descripción**: Despliega un DaemonSet de Filebeat para enviar logs a Logstash.

6. **Kibana**

   ```bash
   kubectl apply -f kibana.yml
   ```

   - **Descripción**: Despliega Kibana para visualizar y analizar los logs almacenados en Elasticsearch.

### Verificación del Despliegue

Una vez desplegados todos los componentes, verifica que los pods estén en estado `Running`:

```bash
kubectl get pods --namespace kube-system
```

### Acceso a Kibana

Para acceder a Kibana desde Minikube, obtén la URL utilizando el comando:

```bash
minikube service kibana-logging -n kube-system
```

Esto debería proporcionarte una URL donde puedes acceder a Kibana desde tu navegador web.

### Despliegue Conjunto

Si deseas desplegar todos los recursos juntos después de haber corregido y verificado cada archivo por separado:

```bash
kubectl apply -f rbac.yml -f elastic.yml -f elastic-service.yml -f logstash-config.yml -f logstash-deployment.yml -f filebeat-daemon-set.yml -f kibana.yml
```

Esto aplicará todos los archivos YAML en secuencia para configurar todo el stack ELK en tu clúster Kubernetes.

---

Con estos pasos, deberías poder desplegar y verificar el stack ELK correctamente en tu entorno Kubernetes. Asegúrate de revisar los logs y estados de los recursos para abordar cualquier problema específico que pueda surgir durante el despliegue.


