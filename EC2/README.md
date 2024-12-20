# Template básico para crear una instancia EC2 con Cloudformation

En este repositorio se implementan templates de ec2 para cloudformation en diferentes escenarios y sus propiedades básicas para iniciar una instancia ec2.

- Crear una instancia básica de EC2
- Agregar un security group con la habilitación de protocolos SSH, HTTP y HTTPS
- Asignar una dirección ip (IP Address) y un output con la dirección del sitio web
- Crear un template parametrizado con los ejemplos 1, 2 y 3
- Agregar un volume EBS a la instancie EC2
- Aumentar el tamaño del disco raíz
- Agregar datos de usuario
- Eliminar el stack

Para cada ejemplo, es útil validar la sintaxis del template en caso de errores mediante el comando `aws cloudformation validate-template --template-body file://ruta_template`

## 1 Crear una instancia básica de EC2

Para crear el stack en cloudformation debemos ejecutar `aws cloudformation create-stack --stack-name ec2-example --template-body file://01_ec2_base.yml`

## 2 Agregar a la instancia EC2 un security group con tráfico entrante hacia los protocolos SSH, HTTP y HTTPS

Se le agrega el recurso de tipo `AWS::EC2::KeyPair` para la llave SSH. Para obtener la llave debemos ejecutar los siguientes comandos:

`aws ec2 describe-key-pairs --filters Name=key-name,Values=nombre_key --query KeyPairs[*].KeyPairId --output text`

Este comando nos devuelve el ID de la llave como por ejemplo `key-123456789`.
Este ID lo ocupamos para guardar nuestra llave mediante el comando `aws ssm get-parameter --name /ec2/keypair/key-05abb699beEXAMPLE --with-decryption --query Parameter.Value --output text > new-key-pair.pem`

Para actualizar el stack en cloudformation debemos ejecutar `aws cloudformation update-stack --stack-name ec2-example --template-body file://02_ec2_base.yml`

## 3 Asignar una dirección ip (IP Address) y un output con la dirección del sitio web

Se agrega el recurso de tipo `AWS::EC2::EIP` para asignar una ip pública a la máquina EC2.

Para actualizar el stack en cloudformation debemos ejecutar `aws cloudformation update-stack --stack-name ec2-example --template-body file://03_ec2_base.yml`

## 4 Crear un template parametrizado con los ejemplos 1, 2 y 3

Se actualiza el template ec2, agregándole parámetros y tags obteniendo un archivo yaml dinámico.

Para actualizar el stack en cloudformation debemos ejecutar la siguiente línea de comandos, pasando los parametros que se necesitan:

`aws cloudformation update-stack --stack-name ec2-example --template-body file://04_ec2_base.yaml \
    --parameters ParameterKey=NombreSG,ParameterValue=sgrp-webapp-dev \
    ParameterKey=TipoInstancia,ParameterValue=t2.micro \
    ParameterKey=KeyPairName,ParameterValue=ec2keyexample`

## 5 Agregar un volume EBS a la instancia EC2

Se agregan los recursos de tipo `AWS::EC2::Volume` y `AWS::EC2::VolumeAttachment`. El recurso de tipo `AWS::EC2::Volume` crea un nuevo volume ebs y el recurso `AWS::EC2::VolumeAttachment` adjunta el nuevo volume ebs a la instancia ec2.
Además se agrega al output del template, el ID del nuevo volume ebs.

Para actualizar el stack en cloudformation debemos ejecutar `aws cloudformation update-stack --stack-name ec2-example --template-body file://05_ec2_base.yml`

## 6 Aumentar el tamaño del disco raíz

Para aumentar el tamaño del disco raíz se debe agregar la propiedad `BlockDeviceMappings`, propiedad de tipo array. En el caso del disco raíz, solo se puede sobreescribir las propiedades **volume size**, **volume type**, **volume encryption settings**, y la configuración `DeleteOnTermination`

Para actualizar el stack en cloudformation debemos ejecutar `aws cloudformation update-stack --stack-name ec2-example --template-body file://06_ec2_base.yml`

## 7 Agregar datos o script de usuario

Los parámetros o script se almacenarán como datos de usuario. Estos se ejecutan al iniciar la instancia. Se deben codificar en base64 ya que los datos de usuarios está limitados a 16KB.

Para actualizar el stack en cloudformation debemos ejecutar `aws cloudformation update-stack --stack-name ec2-example --template-body file://07_ec2_base.yml`

## 8 Eliminar un stack

Para eliminar el stack, se debe ejecutar el siguiente comando `aws cloudformation delete-stack --stack-name ec2-example`

------------------------------------------------------------------------

## Recursos del template (ec2_template.yml)

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