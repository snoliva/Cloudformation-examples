# Template básico para crear una RDS con Cloudformation

En este repositorio se implementan templates de rds para cloudformation en diferentes escenarios y sus propiedades básicas para iniciar una instancia de base de datos.

- Crear una instancia básica de base de datos mysql
- Agregar security group con acceso a la base de datos
- Configurar los días que se conservarán los backups de la base de datos y el output del endpoint
- Habilitar la instancia rds para Multi-AZ y habilitar el servicio de monitoreo en RDS
- Crear un cluster de Aurora postgres con una instancia y una replica
- RDS SQL Server con opciones personalizadas y grupos de parámetros
- Borrar el stack y eliminar snapshots

## 1 Crear una instancia básica de base de datos mysql

Para crear el stack en cloudformation debemos ejecutar `aws cloudformation create-stack --stack-name rds-example --template-body file://RDS/01_rds_base.yml --parameters ParameterKey=DBPassword,ParameterValue=yNhs1234`

## 2 Agregar security group con acceso a la base de datos

Se agrega un security group con acceso al puerto mysql 3306. Como buena práctica, el acceso se debe dar a una IP específica algo como `CidrIp: "10.x.x.x/32"`. Para este ejemplo utilizamos el acceso desde internet `CidrIp: "0.0.0.0/0"` para efectos prácticos, **acceso que no es recomendable**.

Para actualizar el stack en cloudformation debemos ejecutar `aws cloudformation update-stack --stack-name rds-example --template-body file://RDS/02_rds_base.yml --parameters ParameterKey=DBPassword,ParameterValue=yNhs1234`

## 3 Configurar los días que se conservarán los backups de la base de datos y el output del endpoint

Para configurar los días que se conservarán los backups, se debe agregar la propiedad `BackupRetentionPeriod`. Además se agrega el horario de preferencia para realizar estos backups con la propiedad `PreferredBackupWindow` y el output del endpoint de la base de datos mysql.

Para actualizar el stack en cloudformation debemos ejecutar `aws cloudformation update-stack --stack-name rds-example --template-body file://RDS/03_rds_base.yml --parameters ParameterKey=DBPassword,ParameterValue=yNhs1234`

## 4 Habilitar la instancia rds para Multi-AZ y habilitar el servicio de monitoreo en RDS

Para habilitar el despliegue Multi-AZ para alta disponibilidad, debemos agregar la propiedad `MultiAZ: true`. Además para habilitar el monitoreo para la recopilación de métricas de la instancia rds, se agrega la propiedad `MonitoringInterval: 60` donde la recopilación se genera cada 60 segundos.

Por último, al habilitar el monitoreo, se debe agregar el ARN del rol de IAM que permite que RDS envíe métricas de monitoreo a Amazon CloudWatch Logs. Para aquello se agrega el recurso de tipo `AWS::IAM::Role`

Para actualizar el stack en cloudformation debemos ejecutar `aws cloudformation update-stack --stack-name rds-example --template-body file://RDS/04_rds_base.yml --parameters ParameterKey=DBPassword,ParameterValue=yNhs1234`

## 5 Crear un cluster de Aurora postgres con una instancia y una replica

En este ejemplo dejamos de utilizar aurora-mysql para utilizar aurora-postgres. En esta ocasión se configurará un cluster con una instancia y una replica de lectura.
Para acceder a la replica de lectura, se debe utilizar el `ReadeEndpoint` output que está configurado en el template, el cual automáticamente realiza el balanceo de carga de lectura a través de todas las replicas presentes (1 en este caso).

`aws cloudformation create-stack --stack-name rds-cluster-example --template-body file://RDS/05_rds_base.yml --parameters ParameterKey=DBPassword,ParameterValue=yNhs1234`

**IMPORTANTE**

Tener en cuenta que la ejecución de esta configuración de clúster Aurora PostgreSQL no está cubierta por el nivel gratuito de AWS y generará cargos. El nivel gratuito de AWS para RDS incluye 750 horas por mes de uso de instancias de una sola zona de disponibilidad db.t2.micro, db.t3.micro o db.t4g.micro, que no son compatibles con los clústeres Aurora. Esta plantilla está pensada únicamente como un ejemplo educativo. Después de la prueba, se recomienda eliminar la pila para evitar costos inesperados. Puede hacerlo mediante el comando `aws cloudformation delete-stack --stack-name rds-cluster-example`

## 6 RDS SQL Server con opciones personalizadas y grupos de parámetros

El ejemplo 6 crea una instancia SQL Server Express Edition ya que está habilitada para usarse en la capa gratuita. Se agregar como ejemplo de configuración, parámetros como `max_worker_threads: '1024'` y `fill_factor: '70'`

`max_worker_threads`: Define el máximo de tareas concurrentes que puede realizar la base de datos.
`fill_factor`: Controla el grado de saturación de cada página por parte de SQL Server cuando crea o reconstruye índices.

`aws cloudformation create-stack --stack-name rds-cluster-example --template-body file://RDS/06_rds_base.yml --parameters ParameterKey=DBPassword,ParameterValue=yNhs1234`

## 7 Borrar el stack y eliminar snapshots

Para borrar el stack se debe ejecutar `aws cloudformation delete-stack --stack-name nombre_stack` 

**IMPORTANTE**
Si se utiliza la capa gratuita, al borrar una rds, por defecto tomará una snapshot la cual se debe borrar para que no genere cobros.

Se debe navegar hacia el menu de la consola RDS. Desde ahí, en el menú buscar la opción Snapshots, luego seleccionar los snapshots creados por el stack y seleccionar la opción borrar desde el menú de acciones.