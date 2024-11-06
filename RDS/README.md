# Template básico para crear una RDS con Cloudformation

En este repositorio se implementan templates de rds para cloudformation en diferentes escenarios y sus propiedades básicas para iniciar una instancia de base de datos.

- Crear una instancia básica de base de datos mysql

## 1 - Crear una instancia básica de base de datos mysql

Para crear el stack en cloudformation debemos ejecutar `aws cloudformation create-stack --stack-name rds-example --template-body file://RDS/01_rds_base.yml --parameters ParameterKey=DBPassword,ParameterValue=yNhs1234`