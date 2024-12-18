# Elastic Container Service

## ¿Qué es ECS?

Amazon Elastic Container Service (ECS) es un servicio de orquestación de contenedores que ayuda a correr, parar y administrar contenedores Docker en un cluster.

### Infraestructura Amazon ECS

1. La infraestructura de un ECS puede ser ejecutada dentro de una instancia EC2. El responsable es quién elige la capacidad de la instancia

2. Aws Fargate (Sin servidor). Este es un motor de cálculos por uso. No se necesita administrar servidores, gestionar la planificación de la capacidad ni realizar un aislamiento de un contenedor para su seguridad.

3. Máquinas virtuales (VM)

### Componentes Claves de un ECS

1. **Cluster**

Un cluster es como un data center para contenedores. Un grupo de recursos que pueden ser EC2 o Fargate.

2. **Task Definition**

Permite definir en detalle el contenedor.

- Que imagen usará el contenedor
- Requerimientos de CPU y memoria
- Mapeo de puertos
- Storage

3. **Service**

Define el número de tareas que correran de un mismo contenedor. Define que balanceador de carga utilizará y administra el reemplazo de una tarea en caso de fallar.

4. **Tipos de lanzamientos**

- Fargate o EC2

### Ventajas de un ECS

1. **Escalabilidad**: Fácil de escalar contenedores
2. **Alta disponibilidad**: Se puede correr en distintas zonas de disponibilidad
3. **Integración**: Fácil de integrar con otros servicios de AWS
4. **Cost-effective**: Pagar solo por uso del recurso
5. **Seguridad**: Integración con IAM y aislamiento

### Ejemplos

- Crear un cluster ECS básico

### 1 Crear un cluster ECS básico

En este ejemplo se contruye un cluster ecs básico. Además, se crea un IAM role para que las tasks de ecs puedan usar. Permite a las tasks de ecs asumir este role. Este role es necesario para traer imágenes desde ECR, enviar logs a Cloudwatch y acceder a otros servicios de AWS.

Para desplegar este stack se debe ejecutar:

```bash
aws cloudformation create-stack \
  --stack-name basic-ecs-cluster \
  --template-body file://ECS/01_ecs_base.yml \
  --capabilities CAPABILITY_IAM
```