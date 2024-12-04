# VPC

Piensa en una VPC como la propia sección privada en la nube de AWS similar a tener un propio edificio de oficinas.

Amazon VPC es un servicio de AWS que te permite lanzar recursos de AWS en una red virtual aislada y definida por el usuario. Funciona de manera similar a una red tradicional en un centro de datos, con la flexibilidad de escalar automáticamente y configurarse según tus necesidades

### Conceptos básicos 🏢
- Una VPC es la red privada dentro de la nube AWS

- Es como tener un propio espacio aislado donde puedes construir los recursos (servidores, base de datos, etc)

- Se tiene el completo control sobre quién puede entrar y quién puede salir.

### Componentes claves 🔑

**1. IP Address Range (CIDR Block)**

```
MyVPC:
  Type: AWS::EC2::VPC
  Properties:
    CidrBlock: 10.0.0.0/16  # This can host 65,536 IP addresses
```
- Es como asignar números de calle al edificio
- Ejemplo: 10.0.0.0/16 te da direcciones de 10.0.0.0 a 10.0.255.255

**2. Subnets**

Las subnets vienen siendo algo como los diferentes pisos del edificio

```
PublicSubnet:
  Type: AWS::EC2::Subnet
  Properties:
    VpcId: !Ref MyVPC
    CidrBlock: 10.0.1.0/24  # This can host 256 IP addresses
```

- Subdivisión dentro de tu VPC
- Puede ser pública (accesible desde internet) o privada (solo interno)

**3. Internet Gateway** 

Intenet Gateway es la entrada principal

```
InternetGateway:
  Type: AWS::EC2::InternetGateway
  # Allows communication with the internet
```

- Permite a los recursos dentro de la VPC acceder a internet
- Es requerido para las subnets públicas

**4. Route Tables**

Las route tables (tablas de rutas) se puede definir como la dirección del edificio

```
RouteTable:
  Type: AWS::EC2::RouteTable
  Properties:
    VpcId: !Ref MyVPC
```

- Define hacia donde el tráfico de la red debería ir
- Son como las señaléticas que dirigen a las personas hacia donde deben ir

### Analogía Visual

```
Internet
    ↕️
[Internet Gateway]
    ↕️
+----------------+ VPC (10.0.0.0/16)
|  Public Subnet |
| (10.0.1.0/24) |
|    🖥️  🖥️     |
+----------------+
|Private Subnet  |
| (10.0.2.0/24) |
|    💾  💾     |
+----------------+
```

### Casos de uso comúnes

**1. Aplicación Web**

- Subnet pública: Web servers
- Subnet privada: Base de datos

**2. Entorno de desarrollo**

- VPCs separadas para ambiente de desarrollo, testing y producción

**3. Red corportativa**

- Conectar la red de oficina hacia los recursos de AWS

### Mejores prácticas

**1. Planificación de IP

- Usar rangos de IP privadas (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16)
- Plan de crecimiento

**2. Seguridad**

- Usar subnets privadas para recursos sensibles
- Implementar red ACLs y grupos de seguridad

**3. Disponibilidad**

- Crear subnets in multiples zonas de disponibilidad
- Plan de redundancia

### Ejemplos

En este repositorio se muestran los siguientes ejemplos básicos para los recursos de VPC

- Crear una VPC con una sola subred
- Crear una VPC con una red pública y una red privada

### 1 Crear una VPC con una sola subred

Este ejemplo contiene los recursos mínimos para crear una VPC con una subnet.

Para crear el stack debemos ejecutar:

```bash
aws cloudformation create-stack --stack-name vpc-example --template-body file://VPC/01_vpc_base.yml
```

### 2 Crear una VPC con una red pública y una red privada

Este ejemplo introduce el concepto de red pública y red privada.

Para crear el stack debemos ejecutar:

```bash
aws cloudformation create-stack --stack-name vpc-example --template-body file://VPC/02_vpc_base.yml
```