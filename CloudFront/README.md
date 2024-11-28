# Amazon CloudFront: Introducción

Amazon CloudFront es un servicio de red de entrega de contenido (CDN) que distribuye datos, videos, aplicaciones y API de manera segura y con baja latencia a usuarios de todo el mundo. Utiliza una red global de nodos para garantizar que el contenido se entregue desde el servidor más cercano al usuario, optimizando la velocidad y la experiencia del cliente. [1](https://docs.aws.amazon.com/whitepapers/latest/secure-content-delivery-amazon-cloudfront/introduction.html)

## ¿Cómo funciona CloudFront?

CloudFront actúa como una capa intermedia entre el origen de su contenido (como Amazon S3, EC2 o un servidor personalizado) y sus usuarios finales. Aquí hay una explicación simple:

**Sin CloudFront:**

- Cuando un usuario solicita contenido desde una ubicación remota (por ejemplo, Tokio), la solicitud debe viajar directamente al servidor de origen, que podría estar en un lugar distante como los EE. UU. Esto puede resultar en tiempos de carga lentos debido a la latencia.

**Con CloudFront:**

- La primera vez que se solicita contenido, CloudFront lo obtiene del servidor de origen y almacena una copia en su nodo más cercano al usuario.

Para futuras solicitudes, CloudFront entrega el contenido directamente desde este nodo, reduciendo significativamente los tiempos de carga.

## Beneficios clave

- **Entrega rápida de contenido:** Reduce la latencia al servir contenido desde nodos cercanos.

- **Mejor experiencia de usuario:** Sitios web y aplicaciones más rápidos.

- **Seguridad avanzada:** Integración con AWS Shield y AWS Web Application Firewall (WAF) para proteger contra ataques DDoS y amenazas comunes.

- **Modelo de pago por uso:** Pague solo por los datos transferidos y las solicitudes realizadas.

- **Compatibilidad total con AWS:** Integración perfecta con servicios como S3, EC2, Lambda, y más.

## Casos de uso comunes

- Entrega de contenido estático y dinámico (imágenes, CSS, JavaScript).
- Transmisión de video en vivo y bajo demanda.
- Distribución de actualizaciones de software a usuarios globales.
- Aceleración de puntos finales de API.
- Soporte para sitios web completos con configuraciones personalizadas.

**Sin CloudFront:**

Un usuario en Asia accede directamente a un contenedor S3 ubicado en los EE. UU., enfrentándose a demoras por la distancia.

**Con CloudFront:**

El contenido solicitado se almacena y sirve desde un nodo en Asia, minimizando la latencia y mejorando la experiencia del usuario.

CloudFront es como una red mundial de tiendas locales: en lugar de que todos los usuarios accedan al almacén central, pueden obtener el contenido rápidamente desde la tienda más cercana.

## Nota para principiantes

Si bien CloudFront no es difícil de entender en la teoría, contiene varias propiedades que es importante conocer y comprender desde un punto de vista técnico. En este repositorio, explicaremos cada una de las propiedades utilizadas en los ejemplos modelados para CloudFormation, facilitando su aprendizaje y aplicación práctica.

# Ejemplos

- Distribución básica de CloudFront con S3 Origin


## 1 Distribución básica de CloudFront con S3 Origin

A continuación se explican los componentes principales:

**Distribution Configuration**

`Enabled: true`: Esto activa la distribución de CloudFront

**Default Cache Behavior**

`TargetOriginId`: Enlace al origen (S3 bucket)

`ViewerProtocolPolicy: redirect-to-https`: Tres posibles valores:

1. `redirect-to-https:` redirecciona automáticamente HTTP a HTTPS
2. `https-only:` solo permite solicitudes HTTPS
3. `allow-all:` permite tanto HTTP como HTTPS

`CachePolicyId:` Utiliza la política de almacenamiento en caché administrada de AWS (Caching Optimized)

- Esta identificación específica es para el almacenamiento en caché óptimo de contenido estático.
- Controla durante cuánto tiempo CloudFront conserva el contenido en su caché.

**Origins**

`DomainName:` Nombre de dominio del bucket s3.

`Id:` Identificador único usado para referenciar al origin.

`S3OriginConfig:` Configuración específica para el s3 bucket.

Para crear el stack debemos ejecutar:

```bash
aws cloudformation create-stack --stack-name cdn-example --template-body file://CloudFront/01_cfront_base.yml
```

## 2 Simple CloudFront with S3 for static website

Un ejemplo de sitio web básico.

Para crear el stack debemos ejecutar:

```bash
aws cloudformation create-stack --stack-name cdn-example --template-body file://CloudFront/02_cfront_base.yml
```

## 3 Cloudfront con manejo de errores personalizados

Para este ejemplo se agregan las opciones para el manejo de errores personalizados en el sitio web estático que se va a construir.

Para crear el stack debemos ejecutar:

```bash
aws cloudformation create-stack --stack-name cdn-example --template-body file://CloudFront/03_cfront_base.yml
```