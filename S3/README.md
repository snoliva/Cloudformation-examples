# Templates básicos para crear un bucket S3 con Cloudformation

En este repositorio se implementan templates de s3 para cloudformation con propiedades básicas para iniciar a crear un s3 bucket.

**Amazon S3 (Simple Storage Service)** es un servicio de almacenamiento de objetos altamente escalable que ofrece durabilidad, disponibilidad, seguridad y rendimiento.

- Crear un simple bucket S3

## 1 Crear un simple bucket S3

Para crear el bucket S3 se debe ejecutar el siguiente comando:

```bash
aws cloudformation create-stack --stack-name s3-example --template-body file://S3/01_s3_base.yml
```