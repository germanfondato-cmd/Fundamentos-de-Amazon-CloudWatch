# Servidor-Web-Monitoreo-Dashboard

Dashboard de Amazon CloudWatch para monitorear la instancia EC2 del servidor web, desplegada dentro de la VPC de desarrollo.

## Contenido del repositorio

* `dashboard.json`: definición del dashboard (se puede importar en CloudWatch con *Actions > View/edit source*).
* `README.md`: este documento.

## Cómo reproducirlo

1. Verificar que la instancia EC2 esté en ejecución (estado *running*) dentro de la VPC.
2. CloudWatch > Dashboards > Create dashboard > `Servidor-Web-Monitoreo-Dashboard`.
3. Widget **Line**: EC2 > Per-Instance Metrics > `CPUUtilization`.
4. Widget **Stacked area**: EC2 > Per-Instance Metrics > `NetworkIn` y `NetworkOut`.
5. Guardar y exportar el JSON desde *Actions > View/edit source*.



## Qué significan los datos

### Widget 1: Utilización de CPU (Line)

* **Métrica:** `CPUUtilization` (namespace `AWS/EC2`), promedio cada 5 minutos.
* **Qué mide:** porcentaje de la capacidad de cómputo asignada que está en uso.
* **Cómo leerla:** valores bajos y estables indican holgura; picos sostenidos por encima del 70-80% sugieren que el servidor está "estresado" y conviene optimizar, escalar verticalmente (tipo de instancia mayor) o usar Auto Scaling.

### Widget 2: Tráfico de red (Stacked area)

* **`NetworkIn`:** bytes recibidos por la instancia (peticiones HTTP, conexiones SSH, descargas).
* **`NetworkOut`:** bytes enviados por la instancia (respuestas del servidor, transferencia hacia Internet).
* **Cómo leerla:** el área apilada muestra el tráfico total y la proporción entre entrada y salida. Un aumento de `NetworkOut` suele corresponder a más visitas o descargas; picos inesperados pueden indicar uso anómalo.

## Observaciones

* Con métricas básicas, EC2 publica datos cada 5 minutos, por lo que los cambios tardan unos minutos en reflejarse.
* Si la instancia está detenida, los gráficos aparecen vacíos o planos.
* Para generar actividad de prueba se accedió al servidor por HTTP/SSH y se observó el aumento en los gráficos.

