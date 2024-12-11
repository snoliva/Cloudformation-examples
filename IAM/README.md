# IAM Services (AWS Identity and Access Management)

IAM es el servicio de AWS que permite administrar los accesos de los servicios y recursos de forma segura. IAM permite controlar:

- Quién puede acceder a los recursos (Autenticación)
- A qué recursos pueden acceder (Autorización)
- Cómo pueden acceder a estos recursos (Permisos)

### Ejemplos

Este repositorio contiene los siguientes ejemplos:

- Política básica de acceso lectura-escritura a S3
- Política de acceso de EC2 y S3
- Política de acceso para desarrollo
- Política de acceso basado en ambientes

### 1 Política básica de acceso lectura-escritura a S3

En este ejemplo se construye:

- Acceso simple a cierto recurso
- Sentencias básica de permiso
- Enfocado en un solo servicio

Para construir el stack se debe ejecutar el siguiente comando:

```bash
aws cloudformation create-stack \
  --stack-name s3-readonly-policy \
  --template-body file://IAM/01_iam_base.yml \
  --parameters ParameterKey=BucketName,ParameterValue=nombre_bucket
```

### 2 Política de acceso de EC2 y S3

En este ejemplo se construye:

- Acceso a múltiples servicios
- Roles a asumir
- Múltiples tipos de recursos

Para construir el stack se debe ejecutar el siguiente comando:

```bash
aws cloudformation create-stack \
  --stack-name ec2-s3-access-policy \
  --template-body file://IAM/02_iam_base.yml \
  --parameters ParameterKey=BucketName,ParameterValue=nombre_bucket
```

### 3 Política de acceso para desarrollo

Este ejemplo modela un escenario donde se necesita acceso para un S3 bucket, DynamoDB, y Cloudwatch logs.

Para S3 se tienen los siguientes privilegios:

- Leer objetos (GetObject)
- Escribir objetos (PutObject)
- Listar el contenido de un bucket (ListBucket)
- Acceder a un bucket específico mediante un parámetro

Para DynamoDB se tienen los siguientes privilegios:

- Consultar tabla
- Explorar tabla
- Leer elementos
- Escribir elementos
- Acceder a una tabla específica mediante un parámetro

Para Cloudwatch logs se tienen los siguientes privilegios:

- Crear grupos de registros
- Crear flujos de registros
- Escribir eventos de registro
- Acceder a grupos de registros específicos de desarrollo

Para construir el stack se debe ejecutar:

```bash
aws cloudformation create-stack \
  --stack-name developer-policy \
  --template-body file://IAM/03_iam_base.yml \
  --parameters \
    ParameterKey=DevBucket,ParameterValue=bucket-name \
    ParameterKey=TableName,ParameterValue=table-name
```

### 4 Política de acceso basado en ambientes

En este ejemplo se modela el caso de una política que se basa en ambientes. Lo que quiere decir, para que la política se haga efectiva, el recurso debe llevar el Tag con el nombre del parámetro asignado en la política y el valor asignado a ese parámetro. En este caso, se tiene el tag con el nombre `Environment` y el valor puede ser `dev, test o prod`.

Para crear el stack debemos ejecutar:

```bash
aws cloudformation create-stack \
  --stack-name dev-environment-policy \
  --template-body file://IAM/04_iam_base.yml \
  --parameters ParameterKey=Environment,ParameterValue=dev
```

Ejemplo de recurso con el tag asignado:

```bash
aws ec2 create-tags \
  --resources i-1234567890abcdef0 \
  --tags Key=Environment,Value=dev
```
