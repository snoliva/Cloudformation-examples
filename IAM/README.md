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
- Política de seguridad multiservicios
- Política multiregional avanzada con condiciones
- Creación de recursos IAM
- Eliminar stack

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

### 5 Política de seguridad multiservicios

En esta política se construyen los accesos de lectura y escritura para un S3 bucket bajo el protocolo HTTPS. Esto lo define la condición `aws:SecureTransport': 'true`. 

Además, permite el encriptado, desencriptado y la generación de claves de datos para el encriptado usando una llave KMS específica.

Por último, construye una política para recuperar secretos desde AWS Secrets Manager solo si comienza con un prefix específico y deniega la operación de borrar claves KMS y secretos.

Para crear el stack debemos ejecutar:

```bash
aws cloudformation create-stack \
  --stack-name multi-service-security-policy \
  --template-body file://IAM/05_iam_base.yml \
  --parameters \
    ParameterKey=SecureBucket,ParameterValue=bucket-name \
    ParameterKey=KMSKeyId,ParameterValue=kms-key-id \
    ParameterKey=SecretPrefix,ParameterValue=secret-prefix
```

### 6 Política multiregional avanzada con condiciones

Esta política contiene 4 principales reglas:

**1. Acceso regional (RegionalAccess)**

- Permite las operaciones (run, start, stop instances)
- Solo en las regiones primarias y secundarias
- Solo para usuarios con el tag role 'Admin'

**2. Acceso basado en tiempo (TimeBasedAccess)**

- Permite modificaciones y reinicios en RDS
- Solo durante un tiempo
- Controla cuando pueden ocurrir los cambios en la base de datos

**3. Acceso basado en IP (IpBasedAccess)**
- Permite todas las operaciones de DynamoDB
- Solo desde rangos de IP específicos (redes privadas)
- Restringe el acceso a redes internas

**4. MFA requerido (MFARequired)**
- Deniega todas las acciones a excepción el cambio de contraseña e información del usuario
- Si el factor de autenticación no está presente
- Obliga el uso de MFA para operaciones sensibles

Para construir el stack debemos ejecutar:

```bash
aws cloudformation create-stack \
  --stack-name advanced-iam-policy \
  --template-body file://06_iam_base.yml \
  --parameters \
    ParameterKey=PrimaryRegion,ParameterValue=us-east-1 \
    ParameterKey=SecondaryRegion,ParameterValue=us-west-2 \
    ParameterKey=MaintenanceWindowStart,ParameterValue="2023-01-01T00:00:00Z" \
    ParameterKey=MaintenanceWindowEnd,ParameterValue="2024-01-01T00:00:00Z" \
    ParameterKey=AllowedIpRanges,ParameterValue="10.0.0.0/8\,172.16.0.0/12" \
  --capabilities CAPABILITY_IAM
```

### 7 Creación de recursos IAM

En este ejemplo se construyen 3 recursos IAM:

- Se construye un usuario iam y se agrega al grupo Developers
- Se contruye el grupo llamado Developers con 2 políticas de AWS
- Se construye un role que EC2 puede asumir

Para construir el stack debemos ejecutar:

```bash
aws cloudformation create-stack \
  --stack-name iam-resources \
  --template-body file://07_iam_base.yml \
  --parameters \
    ParameterKey=UserName,ParameterValue=john.doe \
    ParameterKey=UserPassword,ParameterValue=YourSecurePassword123! \
    ParameterKey=DevelopersGroup,ParameterValue=Developers \
  --capabilities CAPABILITY_IAM
```

### 8 Eliminar stack

Para eliminar el stack, se debe ejecutar el siguiente comando `aws cloudformation delete-stack --stack-name name_of_stack`