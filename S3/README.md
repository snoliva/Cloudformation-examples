# Templates básicos para crear un bucket S3 con Cloudformation

En este repositorio se implementan templates de s3 para cloudformation con propiedades básicas para iniciar a crear un s3 bucket.

**Amazon S3 (Simple Storage Service)** es un servicio de almacenamiento de objetos altamente escalable que ofrece durabilidad, disponibilidad, seguridad y rendimiento.

- Crear un simple bucket S3
- Crear un bucket s3 con versionamiento y cifrado
- Actualización bucket s3 con reglas de ciclo de vida
- Configurar un S3 para un sitio web estático

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

Para actualizar el bucket S3 se debe ejecutar el siguiente comando:

```bash
aws cloudformation update-stack --stack-name s3-example --template-body file://S3/03_s3_base.yml
```

## 4 Configurar un S3 para un sitio web estático

Para este ejemplo se configura un bucket como sitio web estático. Se configura con `index.html` como página principal y `error.html` para el manejo de errores.

Además se agrega una configuración de reglas CORS (Cross-Origin Resource Sharing) que:

- Permite el acceso al sitio desde cualquier dominio
- Solo permite GET requests
- Establece un tiempo de caché de 3000 segundos

Y por último, se crea una política para el bucket.

Para crear el bucket S3 se debe ejecutar el siguiente comando:

```bash
aws cloudformation update-stack --stack-name s3-example --template-body file://S3/04_s3_base.yml
```