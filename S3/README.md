# Templates básicos para crear un bucket S3 con Cloudformation

En este repositorio se implementan templates de s3 para cloudformation con propiedades básicas para iniciar a crear un s3 bucket.

**Amazon S3 (Simple Storage Service)** es un servicio de almacenamiento de objetos altamente escalable que ofrece durabilidad, disponibilidad, seguridad y rendimiento.

- Crear un simple bucket S3
- Crear un bucket s3 con versionamiento y cifrado
- Actualización bucket s3 con reglas de ciclo de vida

## 1 Crear un simple bucket S3

Para crear el bucket S3 se debe ejecutar el siguiente comando:

```bash
aws cloudformation create-stack --stack-name s3-example --template-body file://S3/01_s3_base.yml
```

## 2 Crear un bucket s3 con versionamiento y cifrado

Para este ejemplo se configura el cifrado del lado del servidor para el bucket s3. Además utiliza el algoritmo de cifrado AES-256 donde todos los objetos almacenados en el bucket se cifrarán automáticamente.

Por último, se implementan buenas prácticas de seguridad, en este caso, se bloquea el acceso a todo el público.

Para actualizar el bucket S3 se debe ejecutar el siguiente comando:

```bash
aws cloudformation update-stack --stack-name s3-example --template-body file://S3/02_s3_base.yml
```

## 3 Actualización bucket s3 con reglas de ciclo de vida

Para este template de S3, se agregan reglas para el ciclo de vida de los archivos dentro del bucket.

**Regla 1 "TransitionToIA"**: Después de 90 días los archivos a un "Standard-IA" storage. En palabras simples, refiere a trasladar archivos que raramente se ocupan a un espacio más barato.

**Regla 2 "TransitionToGlacier"**: Después de 180 días, mueve los archivos a un "Glacier" storage. Similar a mover archivos muy viejos a un storage de largo plazo.

**Regla 3 "ExpireObjects"**: Automáticamente borra los archivos después de 365 días.

Para crear actualizar el bucket S3 se debe ejecutar el siguiente comando:

```bash
aws cloudformation update-stack --stack-name s3-example --template-body file://S3/03_s3_base.yml
```