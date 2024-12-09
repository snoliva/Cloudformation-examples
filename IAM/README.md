# IAM Services (AWS Identity and Access Management)

IAM es el servicio de AWS que permite administrar los accesos de los servicios y recursos de forma segura. IAM permite controlar:

- Quién puede acceder a los recursos (Autenticación)
- A qué recursos pueden acceder (Autorización)
- Cómo pueden acceder a estos recursos (Permisos)

### Ejemplos

Este repositorio contiene los siguientes ejemplos:

- Política básica de acceso lectura-escritura a S3
- Política de acceso de EC2 y S3

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