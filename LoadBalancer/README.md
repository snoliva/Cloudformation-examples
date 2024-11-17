# Template básico para crear un balanceador de carga

En este repositorio se implementan templates de elastic load balancing para cloudformation en diferentes escenarios y con sus propiedades básicas para iniciar un balanceador de carga.

- Ejemplo básico para levantar un application load balancer
- Redirección básica de HTTP a HTTPS
- Grupos de destino múltiples basado en rutas
- Custom Health personalizado con parámetros específicos de monitoreo
- Configuración del peso del tráfico

# Introducción

**Elastic Load Balancing (ELB)** es un servicio de AWS diseñado para distribuir automáticamente el tráfico de aplicaciones entrantes entre múltiples instancias de destino, como instancias de Amazon EC2, contenedores y direcciones IP en una o varias zonas de disponibilidad (AZs).

Esto permite que tus aplicaciones mantengan un rendimiento óptimo, evitando sobrecargas en un solo recurso y proporcionando tolerancia a fallos al redirigir el tráfico a destinos disponibles en caso de fallo en uno de ellos.

ELB ofrece tres tipos principales de balanceadores de carga:

- **Application Load Balancer (ALB):** Ideal para aplicaciones HTTP/HTTPS. ALB opera en la capa 7 del modelo OSI, permitiendo balancear tráfico basado en el contenido (content-based routing), redirecciones, y manejo de solicitudes específicas.

- **Network Load Balancer (NLB):** Trabaja en la capa 4 del modelo OSI, gestionando tráfico a nivel de red (TCP/TLS/UDP). Es ideal para aplicaciones que requieren alta velocidad de procesamiento y baja latencia.

- **Gateway Load Balancer (GWLB):** Dirige el tráfico hacia instancias específicas de seguridad o inspección en la nube, facilitando la implementación de soluciones de seguridad y monitoreo a escala.

## 1 Ejemplo básico para levantar un application load balancer

Para crear el stack se debe ejecutar el comando:

```
aws cloudformation create-stack \
    --stack-name alb-example-stack \
    --template-body file://LoadaBalancer/01_alb_base.yaml \
    --parameters \
    ParameterKey=VpcId,ParameterValue=vpc-xxxx \
    ParameterKey=SubnetIDAZ1,ParameterValue=subnet-xxxx \
    ParameterKey=SubnetIDAZ2,ParameterValue=subnet-yyyy \
    --capabilities CAPABILITY_IAM
```

## 2 Redirección básica de HTTP a HTTPS

Para este ejemplo, es necesario tener un certificado SSL en AWS. Para aquello se debe pedir o importar un certificado en el servicio ACM. Luedo para obtener el ARN del certificado se debe:

1. Ir al servicio Ceritificate Manager
2. Encontrar el certificado
3. Copiar el ARN del certificado.

Para actualizar el stack se debe ejecutar el comando:

```
aws cloudformation update-stack \
    --stack-name alb-example-stack \
    --template-body file://LoadBalancer/02_alb_base.yml \
    --parameters \
    ParameterKey=VpcId,ParameterValue=vpc-xxxx \
    ParameterKey=SubnetIDAZ1,ParameterValue=subnet-xxxx \
    ParameterKey=SubnetIDAZ2,ParameterValue=subnet-yyyy \
    ParameterKey=CertificateArn,ParameterValue=arn:aws:acm:region:xxxxx:certificate/xxxxxxxx \
    --capabilities CAPABILITY_IAM
```

## 3 Grupos de destino múltiples basado en rutas

En este ejemplo se crea una regla de enrutamiento con una ruta específica para el grupo de destino seleccionado. En este caso, enruta las solicitudes con la ruta "/api/*" al grupo de destino de la API. Otras solicitudes van al grupo de destino predeterminado.

Para actualizar el stack se debe ejecutar el comando:

```
aws cloudformation update-stack \
    --stack-name alb-example-stack \
    --template-body file://LoadBalancer/02_alb_base.yml \
    --parameters \
    ParameterKey=VpcId,ParameterValue=vpc-xxxx \
    ParameterKey=SubnetIDAZ1,ParameterValue=subnet-xxxx \
    ParameterKey=SubnetIDAZ2,ParameterValue=subnet-yyyy \
    ParameterKey=CertificateArn,ParameterValue=arn:aws:acm:region:xxxxx:certificate/xxxxxxxx \
    --capabilities CAPABILITY_IAM
```

## 4 Custom Health personalizado con parámetros específicos de monitoreo

Para actualizar el stack se debe ejecutar el comando:

```
aws cloudformation update-stack \
    --stack-name alb-example-stack \
    --template-body file://LoadBalancer/02_alb_base.yml \
    --parameters \
    ParameterKey=VpcId,ParameterValue=vpc-xxxx \
    ParameterKey=SubnetIDAZ1,ParameterValue=subnet-xxxx \
    ParameterKey=SubnetIDAZ2,ParameterValue=subnet-yyyy \
    ParameterKey=CertificateArn,ParameterValue=arn:aws:acm:region:xxxxx:certificate/xxxxxxxx \
    --capabilities CAPABILITY_IAM
```

## 5 Configuración del peso del tráfico

Para este ejemplo, se divide el tráfico dependiendo del peso. En este caso, `BlueTargetGroup` recibe el 90% del tráfico y `GreenTargetGroup` recibe el 10% del tráfico.

### ¿Para qué me sirve esta configuración?

Este ejemplo sirve de buena práctica cuando actualizamos a una nueva versión de una aplicación. En primera instancia el entorno **Blue** recibe el 90% del tráfico. El entorno **Green** recibe el 10% del tráfico, por lo que al actualizar una aplicación a una nueva versión, se debe realizar al entorno **Green**. Una vez realizada la actualización, se debe realizar una transición gradual, puede ser 50-50 para terminar en un 0-100.

```
Blue Environment (90% tráfico)
    ├── Versión Actual (v1.0)
    └── Tráfico producción

Green Environment (10% traffic)
    ├── Nueva versión (v1.1)
    └── Tráfico de testing
```

| Stage | Description | Blue Weight | Green Weight | Status |
|-------|-------------|-------------|--------------|--------|
| 1 | División Inicial | 90% | 10% | Testing nueva versión |
| 2 | División Igual | 50% | 50% | Monitoreo estabilidad |
| 3 | Cambio de mayoría | 10% | 90% | Preparando el corte completo |
| 4 | Corte Final | 0% | 100% | Migración completa |

En caso de problemas, se debe realizar un rollback siguiendo los siguientes pasos:

1. Regresar inmediatamente a la distribución de peso anterior
2. Investigar errores en el entorno **Green**
3. Solucionar problemas y reiniciar el proceso de implementación

Para actualizar el stack se debe ejecutar el comando:

```
aws cloudformation update-stack \
    --stack-name alb-example-stack \
    --template-body file://LoadBalancer/05_alb_base.yml \
    --parameters \
    ParameterKey=VpcId,ParameterValue=vpc-xxxx \
    ParameterKey=SubnetIDAZ1,ParameterValue=subnet-xxxx \
    ParameterKey=SubnetIDAZ2,ParameterValue=subnet-yyyy \
    ParameterKey=CertificateArn,ParameterValue=arn:aws:acm:region:xxxxx:certificate/xxxxxxxx \
    --capabilities CAPABILITY_IAM
```
El comando para actualizar los pesos es:

```bash
# Update weights using AWS CLI
aws elbv2 modify-listener --listener-arn $LISTENER_ARN \
--default-actions Type=forward,ForwardConfig="{
  TargetGroups=[
    {TargetGroupArn=$BLUE_TARGET_GROUP,Weight=90},
    {TargetGroupArn=$GREEN_TARGET_GROUP,Weight=10}
  ]
}"
```
