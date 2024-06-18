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

### Configuración y Despliegue

#### Prerequisitos

Asegúrate de tener un clúster de Kubernetes configurado. Si no lo tienes, sigue los pasos necesarios para configurar tu clúster Kubernetes antes de proceder con los siguientes comandos.

#### Pasos para desplegar el stack ELK

1. **Aplicar los permisos RBAC**:
   ```bash
   kubectl apply -f rbac.yml
   ```

2. **Desplegar Elasticsearch**:
   ```bash
   kubectl apply -f elastic.yml
   ```

3. **Crear el servicio para Elasticsearch**:
   ```bash
   kubectl apply -f elastic-service.yml
   ```

4. **Verificar Elasticsearch**:
   Puedes verificar que Elasticsearch esté corriendo correctamente mediante:
   ```bash
   kubectl port-forward -n kube-system svc/elasticsearch-logging 9200:9200
   ```
   Opcionalmente, accede a http://localhost:9200 desde tu navegador.

5. **Desplegar Logstash**:
   ```bash
   kubectl apply -f logstash-config.yml
   kubectl apply -f logstash-deployment.yml
   ```

6. **Desplegar Filebeat**:
   ```bash
   kubectl apply -f filebeat-daemon-set.yml
   ```

7. **Desplegar Kibana**:
   ```bash
   kubectl apply -f kibana.yml
   ```

8. **Acceder a Kibana**:
   Si has configurado el servicio como LoadBalancer, obtén la IP pública del balanceador de carga:
   ```bash
   minikube service kibana-logging -n kube-system
   ```
   Accede a la URL proporcionada desde tu navegador para visualizar Kibana.

9. **Desplegar una aplicación web (opcional)**:
   Si deseas visualizar logs de una aplicación web específica, despliégala utilizando:
   ```bash
   kubectl apply -f web-deployment.yml
   ```

### Despliegue Completo

Para desplegar todos los componentes ELK en una sola operación, ejecuta el siguiente comando:

```bash
kubectl apply -f rbac.yml -f elastic.yml -f elastic-service.yml -f logstash-config.yml -f logstash-deployment.yml -f filebeat-daemon-set.yml -f kibana.yml
```

Este comando ejecuta todas las configuraciones de una vez, asegurando que todos los componentes de ELK estén desplegados correctamente en tu clúster Kubernetes.

Con estos pasos, deberías tener un stack ELK completamente funcional en tu entorno de Kubernetes, listo para la agregación y visualización de logs de tus microservicios.
