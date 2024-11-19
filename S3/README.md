# Templates básicos para crear un bucket S3 con Cloudformation

En este repositorio se implementan templates de s3 para cloudformation con propiedades básicas para iniciar a crear un s3 bucket.

**Amazon S3 (Simple Storage Service)** es un servicio de almacenamiento de objetos altamente escalable que ofrece durabilidad, disponibilidad, seguridad y rendimiento.

- Crear un simple bucket S3
- Crear un bucket s3 con versionamiento y cifrado

## 1 Crear un simple bucket S3

Para crear el bucket S3 se debe ejecutar el siguiente comando:

```bash
aws cloudformation create-stack --stack-name s3-example --template-body file://S3/01_s3_base.yml
```

## 2 Crear un bucket s3 con versionamiento y cifrado

Para este ejemplo se configura el cifrado del lado del servidor para el bucket s3. Además utiliza el algoritmo de cifrado AES-256 donde todos los objetos almacenados en el bucket se cifrarán automáticamente.

Por último, se implementan buenas prácticas de seguridad, en este caso, se bloquea el acceso a todo el público.

Para crear actualizar el bucket S3 se debe ejecutar el siguiente comando:

```bash
aws cloudformation update-stack --stack-name s3-example --template-body file://S3/02_s3_base.yml
```