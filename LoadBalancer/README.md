# Template básico para crear un balanceador de carga

En este repositorio se implementan templates de elastic load balancing para cloudformation en diferentes escenarios y con sus propiedades básicas para iniciar un balanceador de carga.

- Ejemplo básico para levantar un application load balancer

# Introducción

Elastic Load Balancing (ELB) es un servicio de AWS diseñado para distribuir automáticamente el tráfico de aplicaciones entrantes entre múltiples instancias de destino, como instancias de Amazon EC2, contenedores y direcciones IP en una o varias zonas de disponibilidad (AZs).

Esto permite que tus aplicaciones mantengan un rendimiento óptimo, evitando sobrecargas en un solo recurso y proporcionando tolerancia a fallos al redirigir el tráfico a destinos disponibles en caso de fallo en uno de ellos.

ELB ofrece tres tipos principales de balanceadores de carga:

- **Application Load Balancer (ALB):** Ideal para aplicaciones HTTP/HTTPS. ALB opera en la capa 7 del modelo OSI, permitiendo balancear tráfico basado en el contenido (content-based routing), redirecciones, y manejo de solicitudes específicas.

**Network Load Balancer (NLB):** Trabaja en la capa 4 del modelo OSI, gestionando tráfico a nivel de red (TCP/TLS/UDP). Es ideal para aplicaciones que requieren alta velocidad de procesamiento y baja latencia.

**Gateway Load Balancer (GWLB):** Dirige el tráfico hacia instancias específicas de seguridad o inspección en la nube, facilitando la implementación de soluciones de seguridad y monitoreo a escala.

## 1 Ejemplo básico para levantar un application load balancer

Para crear el stack se debe ejecutar el comando `aws cloudformation create-stack \
  --stack-name alb-example-stack \
  --template-body file://LoadaBalancer/01_alb_base.yaml \
  --parameters \
    ParameterKey=VpcId,ParameterValue=vpc-xxxx \
    ParameterKey=SubnetId1,ParameterValue=subnet-xxxx \
    ParameterKey=SubnetId2,ParameterValue=subnet-yyyy \
  --capabilities CAPABILITY_IAM`

