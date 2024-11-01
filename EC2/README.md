# Template básico para crear una instancia EC2 con Cloudformation

En este repositorio se implementan templates de ec2 para cloudformation en diferentes escenarios y sus propiedades básicas para iniciar una instancia ec2.

- Crear una instancia básica de EC2
- Agregar un security group con la habilitación de protocolos SSH, HTTP y HTTPS

Para cada ejemplo, es útil validar la sintaxis del template en caso de errores mediante el comando `aws cloudformation validate-template --template-body file://ruta_template`

## 1 Crear una instancia básica de EC2

Para crear el stack en cloudformation debemos ejecutar `aws cloudformation create-stack --stack-name ec2-rds-example --template-body file://01_ec2_base.yml`

## Recursos del template

El template de la instancia ec2 está compuesto de 3 partes:
1. `Parameters` (Parámetros): Parámetros que se referencian en la sección Resource.
2. `Ec2Server`: Recurso EC2 donde se define la instancia con sus propiedades.
3. `SGInstancia`: Recurso SecurityGroup donde se define el security group para la instancia EC2

## Parámetros

`NombreSG`: Nombre del security group.

`DescripcionSG`: Descripción del security group.

`VPCID`: Id de la VPC donde se alojan las subnets.

`SubnetID`: Id de la subnet de la zona de disponibilidad AZ1 que alojará la instancia ec2.

`SubnetIDAZ2`: Id de la subnet de la zona de disponibilidad AZ2 (En caso de existir).

`TipoInstancia`: Tipo de instancia. (Solo instancias de capa gratuita).

`AMIID`: Id de la imagen del sistema operativo a utilizar.

## Tags
`NombreInstancia`: Nombre que tendrá la instancia ec2.

`Ambiente`: Ambiente donde se creará la instancia ec2 (desarrollo - test - producción).

`Propietario`: Persona a cargo de la instancia ec2.

`Proposito`: Descripción del proposito de la instancia ec2. Ej: Instancia para app X.