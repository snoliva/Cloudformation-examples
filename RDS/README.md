# Template básico para crear una RDS con Cloudformation

En este repositorio se implementan templates de rds para cloudformation en diferentes escenarios y sus propiedades básicas para iniciar una instancia de base de datos.

- Crear una instancia básica de base de datos mysql
- Agregar security group con acceso a la base de datos
- Configurar los días que se conservarán los backups de la base de datos y el output del endpoint
- Borrar el stack y eliminar snapshots

## 1 Crear una instancia básica de base de datos mysql

Para crear el stack en cloudformation debemos ejecutar `aws cloudformation create-stack --stack-name rds-example --template-body file://RDS/01_rds_base.yml --parameters ParameterKey=DBPassword,ParameterValue=yNhs1234`

## 2 Agregar security group con acceso a la base de datos

Para actualizar el stack en cloudformation debemos ejecutar `aws cloudformation update-stack --stack-name rds-example --template-body file://RDS/02_rds_base.yml --parameters ParameterKey=DBPassword,ParameterValue=yNhs1234`

## 3 Configurar los días que se conservarán los backups de la base de datos y el output del endpoint

Para configurar los días que se conservarán los backups, se debe agregar la propiedad `BackupRetentionPeriod`. Además se agrega el horario de preferencia para realizar estos backups con la propiedad `PreferredBackupWindow` y el output del endpoint de la base de datos mysql.

Para actualizar el stack en cloudformation debemos ejecutar `aws cloudformation update-stack --stack-name rds-example --template-body file://RDS/02_rds_base.yml --parameters ParameterKey=DBPassword,ParameterValue=yNhs1234`

## 4 Borrar el stack y eliminar snapshots

Para borrar el stack se debe ejecutar `aws cloudformation delete-stack --stack-name rds-example` 

**IMPORTANTE**
Si se utiliza la capa gratuita, al borrar una rds, por defecto tomará una snapshot la cual se debe borrar para que no genere cobros.

Se debe navegar hacia el menu de la consola RDS. Desde ahí, en el menú buscar la opción Snapshots, luego seleccionar los snapshots creados por el stack y seleccionar la opción borrar desde el menú de acciones.