# Templates para Cloudformation
Repositorio que contiene templates trabajados con Cloudformation.

## ¿Qué es Cloudformation?

Imagina que quieres construir una casa. En lugar de ir pieza por pieza a comprar los materiales y luego armar todo, podrías tener un plano detallado de la casa. Ese plano sería como una plantilla que le entregas a un constructor, y él se encarga de construir la casa siguiendo tus especificaciones exactas.

CloudFormation es como ese plano para la infraestructura en la nube de AWS. Es una herramienta que te permite definir y provisionar todos los recursos que necesitas para tu aplicación (como servidores, bases de datos, redes) a través de un solo archivo, llamado plantilla. Esta plantilla es escrita en un lenguaje similar a JSON o YAML, que es fácil de leer y entender.

**¿Por qué usar CloudFormation?**

- Simplificación de la administración de la infraestructura: En lugar de crear y configurar cada recurso de forma manual a través de la consola de AWS, puedes crear una plantilla y desplegar toda tu infraestructura con un solo comando.

- Replicación simplificada: Si necesitas crear múltiples entornos (desarrollo, pruebas, producción), puedes utilizar la misma plantilla y personalizarla ligeramente para cada entorno.
- Control y seguimiento de los cambios: CloudFormation te permite versionar tus plantillas, lo que facilita realizar cambios y rastrear su historial. Además, puedes ver un registro detallado de todos los cambios realizados en tu infraestructura.

- Automatización: CloudFormation se integra con otras herramientas de automatización como AWS CodePipeline para crear pipelines de CI/CD y desplegar aplicaciones de manera automatizada.

- Infraestructura como código: Con CloudFormation, tu infraestructura se convierte en código, lo que facilita la colaboración, la revisión por pares y la gestión de cambios.

**¿Cómo funciona CloudFormation?**

- Creación de la plantilla: Escribes una plantilla de CloudFormation en formato JSON o YAML, definiendo los recursos que necesitas y sus propiedades.

- Ejecución de la plantilla: Ejecutas la plantilla utilizando la AWS CLI o la consola de AWS.
Provisionamiento de recursos: CloudFormation crea y configura los recursos especificados en la plantilla.

- Actualización de la plantilla: Si necesitas realizar cambios en tu infraestructura, simplemente modificas la plantilla y vuelves a ejecutarla. CloudFormation detecta los cambios y actualiza los recursos en consecuencia.

**En resumen:**

CloudFormation es una herramienta poderosa que te permite definir, provisionar y administrar tu infraestructura en AWS de manera declarativa y automatizada. Al utilizar CloudFormation, puedes mejorar la eficiencia, la consistencia y la escalabilidad de tus aplicaciones en la nube.

## Repositorios

- Templates básicos de EC2: [EC2](./EC2)
- Templates básicos de RDS: [RDS](./RDS)
- Templates básicos de ELB: [ELB](./LoadBalancer)

