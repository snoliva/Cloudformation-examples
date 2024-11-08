# Template básico para crear una RDS con Cloudformation

En este repositorio se implementan templates de rds para cloudformation en diferentes escenarios y sus propiedades básicas para iniciar una instancia de base de datos.

- Crear una instancia básica de base de datos mysql
- Agregar security group con acceso a la base de datos
- Configurar los días que se conservarán los backups de la base de datos y el output del endpoint
- Habilitar la instancia rds para Multi-AZ y habilitar el servicio de monitoreo en RDS
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

## 5 Borrar el stack y eliminar snapshots

Para borrar el stack se debe ejecutar `aws cloudformation delete-stack --stack-name rds-example` 

**IMPORTANTE**
Si se utiliza la capa gratuita, al borrar una rds, por defecto tomará una snapshot la cual se debe borrar para que no genere cobros.

Se debe navegar hacia el menu de la consola RDS. Desde ahí, en el menú buscar la opción Snapshots, luego seleccionar los snapshots creados por el stack y seleccionar la opción borrar desde el menú de acciones.