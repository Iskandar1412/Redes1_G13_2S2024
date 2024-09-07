# Proyecto 1

| Nombre                                  | Carnet        | Usuario Git |
| -------                                 | ---------     | -------- |
| Carlos Jezeh Gedeoni Toscano Palacios   |   201532643   | [CarlosJezeh777](https-//github.com/CarlosJezeh777) |
| Juan Francisco Urbina Silva             |   201906051   | [Iskandar1412](https-//github.com/Iskandar1412) |
| Luis Eduardo Monroy Pérez               |   201800918   | [LempDnote](https-//github.com/LempDnote) |

## Documentación 

Para este proyecto se utilizarán VLANS definidas de la siguiente manera-

|Departamento|VLAN|ID de red|
|----        |----|----|
|RRHH        |1X  |192.168.1X.0/24|
|Secretaria  |2X  |192.168.2X.0/24|
|Contabilidad|3X  |192.168.3X.0/24|
|IT          |4X  |192.168.4X.0/24|

Donde se utilizará la suma de el último dígito de todos los carnets (caso en el que sea mayor a 9 la suma, se tomará el último dígito de la suma)

* 3 + 1 + 8 => 12 > 9 --> 2

Se utilizará 2 para las definiciones

|Departamento |VLAN |ID de red|
|----         |---- |----|
|RRHH         |12   |192.168.12.0/24|
|Secretaria   |22   |192.168.22.0/24|
|Contabilidad |32   |192.168.32.0/24|
|IT           |42   |192.168.42.0/24|

Tomar en cuenta que el `/24` hace referencia a la máscara de subred `255.255.255.0`

### Topología

![](./Images/Topologia.png)

### 🌐 Conexión Switches

> Área Administrativa

| Origen      | Puerto Origen | Destino | Puerto Destino |
| ----        | -----         | ----    | -----          |
|CONTABILIDAD2|f0             |S6       |f0/1|
|SECRETARIA   |f0             |S3       |f0/1|
|RRHH         |f0             |S5       |f0/1|
|IT_2         |f0             |S4       |f0/1|
|IT_3         |f0             |Switch0  |f0/2|
|RRHH3        |f0             |Switch0  |f0/1|
|Switch0      |f0/24          |SW14     |f0/24|
|S3           |f0/24          |SW7      |f0/24|
|S4           |f0/24          |SW9      |f0/24|
|S5           |f0/24          |SW8      |f0/24|
|S6           |f0/24          |SW10     |f0/24|
|SW7          |f0/24          |S3       |f0/24|
|SW7          |f0/23          |SW10     |f0/23|
|SW8          |f0/24          |S5       |f0/24|
|SW8          |f0/23          |SW9      |f0/23|
|SW8          |f0/21          |SW10     |f0/22|
|SW8          |f0/22          |SW14     |f0/23|
|SW9          |f0/24          |S4       |f0/24|
|SW9          |f0/23          |SW8      |f0/23|
|SW9          |f0/20          |SW10     |f0/20|
|SW9          |f0/21          |SW10     |f0/21|
|SW9          |f0/22          |SW14     |f0/22|
|SW10         |f0/24          |S6       |f0/24|
|SW10         |f0/23          |SW7      |f0/23|
|SW10         |f0/22          |SW8      |f0/21|
|SW10         |f0/20          |SW9      |f0/21|
|SW10         |f0/20          |SW9      |f0/21|
|SW14         |f0/24          |Switch0  |f0/24|
|SW14         |f0/23          |SW8      |f0/22|
|SW14         |f0/22          |SW9      |f0/22|

> Área Central

| Origen      | Puerto Origen | Destino | Puerto Destino |
| ----        | -----         | ----    | -----          |
|SECRETARIA_1 |f0             |Switch1  |f0/1|
|S_RRHH       |f0             |S9       |f0/1|
|S_CONTA      |f0             |S9       |f0/2|
|S_IT         |f0             |S1       |f0/1|
|S1           |f0/24          |SW5      |f0/24|
|S9           |f0/24          |SW6      |f0/24|
|Switch1      |f0/24          |SW3      |f0/24|
|SW1          |f0/20          |SW2      |f0/19|
|SW1          |f0/24          |SW3      |f0/22|
|SW1          |f0/21          |SW4      |f0/23|
|SW1          |f0/23          |SW5      |f0/23|
|SW1          |f0/22          |SW6      |f0/21|
|SW1          |f0/19          |SW15     |f0/22|
|SW2          |f0/19          |SW1      |f0/20|
|SW2          |f0/18          |SW4      |f0/21|
|SW2          |f0/23          |SW15     |f0/24|
|SW3          |f0/22          |SW1      |f0/24|
|SW3          |f0/20          |SW4      |f0/22|
|SW3          |f0/23          |SW6      |f0/23|
|SW3          |f0/24          |Switch1  |f0/24|
|SW4          |f0/23          |SW1      |f0/21|
|SW4          |f0/21          |SW2      |f0/18|
|SW4          |f0/22          |SW3      |f0/20|
|SW5          |f0/24          |S1       |f0/24|
|SW5          |f0/23          |SW1      |f0/23|
|SW5          |f0/22          |SW6      |f0/22|
|SW5          |f0/21          |SW15     |f0/21|
|SW6          |f0/24          |S9       |f0/24|
|SW6          |f0/21          |SW1      |f0/22|
|SW6          |f0/23          |SW3      |f0/23|
|SW6          |f0/22          |SW5      |f0/22|
|SW15         |f0/22          |SW1      |f0/19|
|SW15         |f0/24          |SW2      |f0/23|
|SW15         |f0/21          |SW5      |f0/21|

> Oficina

| Origen        | Puerto Origen | Destino | Puerto Destino |
| ----          | -----         | ----    | -----          |
|CONTABILIDAD_1 |f0             |S7       |f0/3|
|IT_1           |f0             |S7       |f0/1|
|SECRETARIA1    |f0             |S7       |f0/2|
|SECRETARIA2    |f0             |S8       |f0/1|
|RRHH1          |f0             |S8       |f0/2|
|RRHH2          |f0             |S8       |f0/3|
|S7             |f0/24          |SW12     |f0/24|
|S8             |f0/24          |SW13     |f0/24|
|SW11           |f0/24          |SW12     |f0/22|
|SW11           |f0/23          |SW13     |f0/22|
|SW12           |f0/24          |SW7      |f0/24|
|SW12           |f0/22          |SW11     |f0/24|
|SW12           |f0/23          |SW13     |f0/23|
|SW13           |f0/24          |SW8      |f0/24|
|SW13           |f0/22          |SW11     |f0/23|
|SW13           |f0/23          |SW12     |f0/23|

> Area Administrativa ↔️ Area Central

| Origen        | Puerto Origen | Destino | Puerto Destino |
| ----          | -----         | ----    | -----          |
|SW7            |f0/20          |SW2      |f0/24|
|SW7            |f0/22          |SW3      |f0/21|
|SW7            |f0/21          |SW4      |f0/24|

> Area Administrativa ↔️ Oficina

| Origen        | Puerto Origen | Destino | Puerto Destino |
| ----          | -----         | ----    | -----          |
|SW7            |f0/19          |SW12     |f0/21|

> Oficina ↔️ Area Central

| Origen        | Puerto Origen | Destino | Puerto Destino |
| ----          | -----         | ----    | -----          |
|SW11           |f0/22          |SW2      |f0/21|
|SW12           |f0/20          |SW2      |f0/22|
|SW13           |f0/21          |SW2      |f0/20|
|SW13           |f0/20          |SW15     |f0/23|

### 🔢 Asignación IP's

|Dispositivo        |VLAN|IPv4|
|-----              |----|----|
|CONTABILIDAD_1     |32  |192.168.32.10|
|CONTABILIDAD2      |32  |192.168.32.11|
|IT_1               |42  |192.168.42.10|
|IT_2               |42  |192.168.42.11|
|IT_3               |42  |192.168.42.12|
|RRHH               |12  |192.168.12.10|
|RRHH1              |12  |192.168.12.11|
|RRHH2              |12  |192.168.12.12|
|RRHH3              |12  |192.168.12.13|
|SECRETARIA         |22  |192.168.22.10|
|SECRETARIA1        |22  |192.168.22.11|
|SECRETARIA2        |22  |192.168.22.12|
|SECRETARIA_1       |22  |192.168.22.13|
|S_RRHH             |22  |192.168.22.14|
|S_CONTA            |22  |192.168.22.15|
|S_IT               |22  |192.168.22.16|

### Switches Clientes

* Area Administrativa

> SW7, SW8, SW10, SW14

* Area Central

> SW2, SW3, SW4, SW5, SW6, SW15

* Oficina A

> SW11, SW12, SW13

- Configuraciones STP

> SW1 --> root bridge para VLAN 12, 22, 32, 42

### Switches Servidores

* Area Central

> SW1

#### Configuracion SW1 (Servidor)

- Configuración inicial

```bash
enable
conf t
no ip domain-lookup
hostname SW1
do w
```

- Modo Truncal (configuración)

```bash
interface range fa0/19-24
switchport trunk encapsulation dot1q
switchport mode trunk
exit
do w
```

- Creación VLAN (configuración)

```bash
vlan 12
name RRHH
vlan 22
name SECRETARIA
vlan 32
name CONTABILIDAD
vlan 42
name IT
exit
do w
```

- Configuración Protocolo

```bash
vtp version 2
vtp mode server
vtp domain G13
vtp password usac
do w
```

### Switches Transparentes

* Area Administrativa

> SW9