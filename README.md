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

Con estos pasos, deberías poder desplegar y verificar el stack ELK correctamente en tu entorno Kubernetes. Asegúrate de revisar los logs y estados de los recursos para abordar cualquier problema específico que pueda surgir durante el despliegue.

---

## Index Pattern
Pagina oficial de [Elastic](https://www.elastic.co/guide/en/kibana/7.17/index-patterns.html)

1. **Iniciar sesión en Kibana**: Accede a tu instancia de Kibana a través de la URL correspondiente en tu navegador.

2. **Ir a la pestaña Management (Gestión)**: En la barra lateral izquierda de Kibana, haz clic en "Management" (Gestión).

3. **Seleccionar "Index Patterns" (Patrones de índice)**: Dentro de la sección de Gestión, selecciona "Index Patterns" (Patrones de índice). Aquí verás una lista de los patrones de índice existentes si los hay.

4. **Crear un nuevo patrón de índice**: Para crear uno nuevo, haz clic en el botón "Create index pattern" (Crear patrón de índice).

5. **Configurar el patrón de índice**:
   - **Nombre del patrón de índice**: Ingresa el nombre del patrón de índice. Puedes usar comodines (*) para coincidir con múltiples índices si es necesario.
   - **Configuración de tiempo (opcional)**: Si tus datos tienen una marca de tiempo, puedes configurar el campo de tiempo aquí para optimizar las consultas temporales.
   
6. **Guardar el patrón de índice**: Haz clic en "Next step" (Siguiente paso) para confirmar la configuración del patrón de índice.

7. **Explorar el patrón de índice**: Una vez creado el patrón de índice, Kibana te llevará automáticamente al área de descubrimiento de datos donde puedes comenzar a explorar y visualizar tus datos utilizando diferentes herramientas y paneles dentro de Kibana.

Al seguir estos pasos, habrás configurado con éxito un patrón de índice en Kibana para comenzar a trabajar con tus datos desde Elasticsearch.

## Amazon ECR Public Gallery

Amazon Elastic Container Registry [ECR](https://gallery.ecr.aws/)
Amazon ECR Public Gallery es una solución poderosa para la gestión y distribución pública de imágenes de contenedores, facilitando la colaboración y el uso compartido de aplicaciones contenedorizadas en la comunidad global. Con sus características de escalabilidad, integración con AWS y facilidad de uso, es una herramienta esencial para desarrolladores y organizaciones que trabajan con contenedores Docker.

