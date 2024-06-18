### Configuración de ELK para Kubernetes/microservicios

Este repositorio contiene los archivos de configuración necesarios para desplegar un stack ELK (Elasticsearch, Logstash, Kibana) en un entorno de Kubernetes. A continuación, se explica el propósito de cada archivo de configuración:

- **elastic-service.yml**: Define el servicio para Elasticsearch.
- **elastic.yml**: Configura el despliegue de Elasticsearch como un StatefulSet.
- **filebeat-daemon-set.yml**: Despliega Filebeat como un DaemonSet para enviar logs a Logstash.
- **kibana.yml**: Despliega Kibana para la visualización de logs.
- **logstash-config.yml**: Configura Logstash para el formato de logs que Elasticsearch puede entender.
- **logstash-deployment.yml**: Despliega Logstash como un Deployment.
- **rbac.yml**: Define los roles y permisos necesarios (RBAC) para que los componentes de ELK accedan a recursos en Kubernetes.
- **web-deployment.yml**: Ejemplo de despliegue de una aplicación web para visualización de logs en Kibana.

### Despliegue

1. **RBAC (Role-Based Access Control)**

   ```bash
   kubectl apply -f rbac.yml
   ```

   - **Descripción**: Este comando crea una cuenta de servicio con los permisos necesarios para leer servicios, endpoints y namespaces.

2. **Elasticsearch**

   ```bash
   kubectl apply -f elastic.yml
   ```

   - **Descripción**: Este comando define la configuración y despliega un StatefulSet para Elasticsearch.

3. **Servicio para Elasticsearch**

   ```bash
   kubectl apply -f elastic-service.yml
   ```

   - **Descripción**: Este comando crea un servicio para exponer Elasticsearch dentro del clúster.

4. **Logstash**

   ```bash
   kubectl apply -f logstash-config.yml
   kubectl apply -f logstash-deployment.yml
   ```

   - **Descripción**: Estos comandos configuran Logstash para recibir, procesar y enviar logs a Elasticsearch.

5. **Filebeat**

   ```bash
   kubectl apply -f filebeat-daemon-set.yml
   ```

   - **Descripción**: Este comando despliega Filebeat como un DaemonSet para enviar logs a Logstash.

6. **Kibana**

   ```bash
   kubectl apply -f kibana.yml
   ```

   - **Descripción**: Este comando despliega Kibana para visualizar y analizar los logs almacenados en Elasticsearch.

### Despliegue Conjunto

Si deseas desplegar todos los recursos juntos después de haber corregido y verificado cada archivo por separado:

```bash
kubectl apply -f rbac.yml -f elastic.yml -f elastic-service.yml -f logstash-config.yml -f logstash-deployment.yml -f filebeat-daemon-set.yml -f kibana.yml
```

Esto aplicará todos los archivos YAML en secuencia para configurar todo el stack ELK en tu clúster Kubernetes.

### Verificación del Despliegue

Una vez desplegados todos los componentes, verifica que los pods estén en estado `Running`:

```bash
kubectl get pods --namespace kube-system
```

Si alguno de los pods no está funcionando correctamente, puedes verificar los eventos relacionados con ese pod específico:

```bash
kubectl describe pod <nombre-del-pod> --namespace kube-system
```

### Acceso a Kibana

Para acceder a Kibana desde Minikube, obtén la URL utilizando el comando:

```bash
minikube service kibana-logging -n kube-system
```

Esto debería proporcionarte una URL donde puedes acceder a Kibana desde tu navegador web.

Si `minikube service` sigue presentando problemas, puedes reenviar manualmente el puerto de Kibana:

```bash
kubectl port-forward service/kibana-logging 5601:5601 --namespace kube-system
```

Luego, accede a Kibana en `http://localhost:5601`.


---

Con estos pasos, deberías poder desplegar y verificar el stack ELK correctamente en tu entorno Kubernetes. Asegúrate de revisar los logs y estados de los recursos para abordar cualquier problema específico que pueda surgir durante el despliegue.
