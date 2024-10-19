## Topología completa

![alt text](./IMG/Topologia.png)

## Cálculo VLSM (Variable Length Subnet Mask)

- Carnets

| No. Carnet |
| :---:      |
| 201532643  |
| 201800918  |
| 201906051  |

Para la sede de Jutiapa se Ejemplificará
### Jutiapa 
#### Paso 1: Ordenamiento

Las VLANs se van a organizar de acuerdo a la cantidad de equipos necesarios para cada una, se van a organizar de mayor a menor.

| VLAN         | ID de VLAN | Equipos |
| :---         | :---:      | ---:    |
| RRHH         | 1Y         | 10      |
| Contabilidad | 2Y         | 4       |
| Ventas       | 3Y         | 25      |
| Informatica  | 4Y         | 12      |

Donde Y es el último dígito del número de carnet con mayor denominación (En este caso sería el 8)

- Ordenados de mayor a menor (Equipos)

| VLAN         | ID de VLAN | Equipos |
| :---         | :---:      | ---:    |
| Ventas       | 38         | 25      |
| Informatica  | 48         | 12      |
| RRHH         | 18         | 10      |
| Contabilidad | 28         | 4       |

#### Paso 2: Asignación de Mascara y Wildcard

Es importante tener en cuenta que al definir la máscara de red es necesario determinar la cantidad de hosts que se desean para la red, para ello se debe considerar dos aspectos escenciales:

* La Parte de Red
* La Parte de Host

Es a través de la `Parte de Host` que se establece la cantidad de direcciones IP disponibles en la red, por lo que resulta crucial realizar cálculos para determinar adecuadamente la cantidad de hosts que la red podrá alojar.

> Para la VLAN `Ventas`

1. Calcular la cantidad de equipos de la `Parte de Host`

Para determinar la cantidad de equipos, se va a emplear la fórmula $2^2$. Se evaluarán varias opciones:

    * Cuando $n = 3$, resulta en $2^3 =  8$ equipos.
    * Cuando $n = 4$, resulta en $2^4 = 16$ equipos.
    * Cuando $n = 5$, resulta en $2^5 = 32$ equipos.
    * Cuando $n = 6$, resulta en $2^6 = 64$ equipos.

Luego de obtener dichos resultados, se determina que la opción que se adapta mejor a la cantidad de equipos que se requieren es $n = 5$.

Ventas       --> $Equipos = 25 -> 2^5$
Informatica  --> $Equipos = 12 -> 2^4$
RRHH         --> $Equipos = 10 -> 2^4$
Contabilidad --> $Equipos = 4  -> 2^3$

**N** Representa la cantidad de bits que se utilizan para identificar la `Parte de Host`.

2. Determinación de la cantidad de bits para la `Parte de Red`

Una vez calculado **N**, se procede a determinar la cantidad de bits utilizados para la `Parte de Red`. Dicho cálculo se realiza dde la siguiente manera:

Ventas       --> $32 - 5 = 27$ bits
Informatica  --> $32 - 4 = 28$ bits
RRHH         --> $32 - 4 = 28$ bits
Contabilidad --> $32 - 3 = 29$ bits

Por lo tanto, concluimos que la cantidad de bits para la `Parte de Red` es de 27 bits, lo que se expresa en **CIDR** como `/27`.

>> **Nota:** Se restan con 32 debido a que se está evaluuando una dirección IPv4, la cual consta de 32 bits.

3. Cálculo de la Máscara de Red (Decimal)

Dado que la dirección IP consta de 32 bits, estos se representan de la siguiente manera:

`Parte de Red` + `Parte de Host`

En este caso, la `Parte de Red` consta de 27 bits, se representa con unos (1's), mientras que la `Parte de Host`, con 5 bits, se representan con ceros (0's). En binario y decimal, sería de la siguiente manera.

$2^7 + 2^6 + 2^5 + 2^4 + 2^3 + 2^2 + 2^1 + 2^0 = 255$ 
$128 + 64 + 32 + 16 + 8 + 4 + 2 + 1 = 255$

        * Ventas
$11111111.11111111.11111111.11100000 = 255.255.255.224$
        * Informatica
$11111111.11111111.11111111.11110000 = 255.255.255.240$
        * RRHH
$11111111.11111111.11111111.11110000 = 255.255.255.240$
        * Contabilidad
$11111111.11111111.11111111.11111000 = 255.255.255.248$

Donde `11100000` sería $128 + 64 + 32 + 0 + 0 + 0 + 0 + 0 = 224$
Donde `11110000` sería $128 + 64 + 32 + 16 + 0 + 0 + 0 + 0 = 240$
Donde `11111000` sería $128 + 64 + 32 + 16 + 8 + 0 + 0 + 0 = 248$

Por lo que la máscara es **255.255.255.224**

4. Cálculo de Wildcard

Para realizar este cálculo, se deben utilizar exclusivamente la cantidad de equpos determinada en el **Paso 1**. 

Ventas       --> $255.255.255.255 - 255.255.255.224 = 31$
Informatica  --> $255.255.255.255 - 255.255.255.240 = 15$
RRHH         --> $255.255.255.255 - 255.255.255.240 = 15$
Contabilidad --> $255.255.255.255 - 255.255.255.248 = 7$

| VLAN         | ID de VLAN | Equipos | Máscara de Red  | Wildcard |
| :---         | :---:      | ---:    | :---:           | :---     |
| Ventas       | 38         | 25      | 255.255.255.224 | 0.0.0.31 |
| Informatica  | 48         | 12      | 255.255.255.240 | 0.0.0.15 |
| RRHH         | 18         | 10      | 255.255.255.240 | 0.0.0.15 |
| Contabilidad | 28         | 4       | 255.255.255.248 | 0.0.0.7  |

#### Paso 3: Asignación de ID Red, Primera IP, Última IP e IP Broadcast

Donde la Red Interna de jutiapa con el ID de red 192.168.XX.0/24 (XX número de grupo de desarrolladores de proyecto)

* Grupo 13

Por lo tanto `192.168.13.0/24`

1. Asignación de ID Red

    * Ventas
    Se asigna la red interna inicial (192.168.13.0) + 31 -> 0 + 31 = 31
    * Informatica
    Se asigna la red interna inicial (192.168.13.32) + 15 -> 32 + 15 = 47
    * RRHH
    Se asigna la red interna inicial (192.168.13.48) + 15 -> 48 + 15 = 63
    * Contabilidad
    Se asigna la red interna inicial (192.168.13.64) + 7 -> 64 + 7 = 71

2. Asignación de Primera IP

    * Ventas
    Sería la IP siguiente en la secuencia (192.168.13.1)
    * Informatica
    Sería la IP siguiente en la secuencia (192.168.13.33)
    * RRHH
    Sería la IP siguiente en la secuencia (192.168.13.49)
    * Contabilidad
    Sería la IP siguiente en la secuencia (192.168.13.65)

3. Asignación de Última IP

    * Ventas
    Sería (IP del Broadcast - 1) -> 192.168.13.30
    * Informatica
    Sería (IP del Broadcast - 1) -> 192.168.13.46
    * RRHH
    Sería (IP del Broadcast - 1) -> 192.168.13.62
    * Contabilidad
    Sería (IP del Broadcast - 1) -> 192.168.13.70

4. Asignación del Broadcast

    La dirección del broadcast es la última dirección IP asignable. Se determina por la cantidad de Host que hay disponibles.

    > **Nota:** Una forma para visualizar la cantidad de Host que se tienen es por medio del Wildcard

    * Ventas
    Sería (IP del Broadcast) -> 192.168.13.31
    * Informatica
    Sería (IP del Broadcast) -> 192.168.13.47
    * RRHH
    Sería (IP del Broadcast) -> 192.168.13.63
    * Contabilidad
    Sería (IP del Broadcast) -> 192.168.13.71

* Tabla con asignaciones de IP siguientes:

| VLAN         | ID de VLAN | Equipos | Máscara de Red  | Wildcard | ID Red        | Primera IP    | Última IP     | IP Broadcast  |
| :---         | :---:      | ---:    | :---:           | :---     | :---          | :---          | :---          | :---          |
| Ventas       | 38         | 25      | 255.255.255.224 | 0.0.0.31 | 192.168.13.0  | 192.168.13.1  | 192.168.13.30 | 192.168.13.31 |
| Informatica  | 48         | 12      | 255.255.255.240 | 0.0.0.15 | 192.168.13.32 | 192.168.13.33 | 192.168.13.46 | 192.168.13.47 |
| RRHH         | 18         | 10      | 255.255.255.240 | 0.0.0.15 | 192.168.13.48 | 192.168.13.49 | 192.168.13.62 | 192.168.13.63 |
| Contabilidad | 28         | 4       | 255.255.255.248 | 0.0.0.7  | 192.168.13.64 | 192.168.13.65 | 192.168.13.70 | 192.168.13.71 |

### Escuintla 
#### Paso 1: Ordenamiento

Las VLANs se van a organizar de acuerdo a la cantidad de equipos necesarios para cada una, se van a organizar de mayor a menor.

| VLAN         | ID de VLAN | Equipos |
| :---         | :---:      | ---:    |
| RRHH         | 1Y         | 5       |
| Ventas       | 3Y         | 20      |

Donde Y es el último dígito del número de carnet con mayor denominación (En este caso sería el 8)

- Ordenados de mayor a menor (Equipos)

| VLAN         | ID de VLAN | Equipos |
| :---         | :---:      | ---:    |
| Ventas       | 38         | 20      |
| RRHH         | 18         | 5       |

#### Paso 2: Asignación de Mascara y Wildcard

1. Calcular la cantidad de equipos de la `Parte de Host`

    * Cuando $n = 3$, resulta en $2^3 =  8$ equipos.
    * Cuando $n = 4$, resulta en $2^4 = 16$ equipos.
    * Cuando $n = 5$, resulta en $2^5 = 32$ equipos.
    * Cuando $n = 6$, resulta en $2^6 = 64$ equipos.

Ventas       --> $Equipos = 20 -> 2^5$
RRHH         --> $Equipos = 5  -> 2^3$

**N** Representa la cantidad de bits que se utilizan para identificar la `Parte de Host`.

2. Determinación de la cantidad de bits para la `Parte de Red`

Una vez calculado **N**, se procede a determinar la cantidad de bits utilizados para la `Parte de Red`. Dicho cálculo se realiza dde la siguiente manera:

Ventas       --> $32 - 5 = 27$ bits -> **CIDR::** `/27`
RRHH         --> $32 - 3 = 29$ bits -> **CIDR::** `/29`

>> **Nota:** Se restan con 32 debido a que se está evaluuando una dirección IPv4, la cual consta de 32 bits.

3. Cálculo de la Máscara de Red (Decimal)

Dado que la dirección IP consta de 32 bits, estos se representan de la siguiente manera:

$2^7 + 2^6 + 2^5 + 2^4 + 2^3 + 2^2 + 2^1 + 2^0 = 255$ 
$128 + 64 + 32 + 16 + 8 + 4 + 2 + 1 = 255$

        * Ventas
$11111111.11111111.11111111.11100000 = 255.255.255.224$
        * RRHH
$11111111.11111111.11111111.11111000 = 255.255.255.248$

Donde `11100000` sería $128 + 64 + 32 + 0 + 0 + 0 + 0 + 0 = 224$
Donde `11111000` sería $128 + 64 + 32 + 16 + 8 + 0 + 0 + 0 = 248$

4. Cálculo de Wildcard

Para realizar este cálculo, se deben utilizar exclusivamente la cantidad de equpos determinada en el **Paso 1**. 

Ventas       --> $255.255.255.255 - 255.255.255.224 = 31$
RRHH         --> $255.255.255.255 - 255.255.255.248 = 7$

| VLAN         | ID de VLAN | Equipos | Máscara de Red  | Wildcard |
| :---         | :---:      | ---:    | :---:           | :---     |
| Ventas       | 38         | 25      | 255.255.255.224 | 0.0.0.31 |
| RRHH         | 18         | 10      | 255.255.255.248 | 0.0.0.7  |

#### Paso 3: Asignación de ID Red, Primera IP, Última IP e IP Broadcast

Donde la Red Interna de jutiapa con el ID de red 192.148.XX.0/24 (XX número de grupo de desarrolladores de proyecto)

* Grupo 13

Por lo tanto `192.148.13.0/24`

1. Asignación de ID Red

    * Ventas
    Se asigna la red interna inicial (192.148.13.0) + 31 -> 0 + 31 = 31
    * RRHH
    Se asigna la red interna inicial (192.148.13.32) + 7 -> 32 + 7 = 39

2. Asignación de Primera IP

    * Ventas
    Sería la IP siguiente en la secuencia (192.148.13.1)
    * RRHH
    Sería la IP siguiente en la secuencia (192.148.13.33)

3. Asignación de Última IP

    * Ventas
    Sería (IP del Broadcast - 1) -> 192.148.13.30
    * RRHH
    Sería (IP del Broadcast - 1) -> 192.148.13.38
    
4. Asignación del Broadcast

    La dirección del broadcast es la última dirección IP asignable. Se determina por la cantidad de Host que hay disponibles.

    > **Nota:** Una forma para visualizar la cantidad de Host que se tienen es por medio del Wildcard

    * Ventas
    Sería (IP del Broadcast) -> 192.148.13.31
    * RRHH
    Sería (IP del Broadcast) -> 192.148.13.39
    
* Tabla con asignaciones de IP siguientes:

| VLAN         | ID de VLAN | Equipos | Máscara de Red  | Wildcard | ID Red        | Primera IP    | Última IP     | IP Broadcast  |
| :---         | :---:      | ---:    | :---:           | :---     | :---          | :---          | :---          | :---          |
| Ventas       | 38         | 25      | 255.255.255.224 | 0.0.0.31 | 192.148.13.0  | 192.148.13.1  | 192.148.13.30 | 192.148.13.31 |
| RRHH         | 18         | 10      | 255.255.255.248 | 0.0.0.7  | 192.148.13.32 | 192.148.13.33 | 192.148.13.38 | 192.148.13.39 |

### Quiche 
#### Paso 1: Ordenamiento

Las VLANs se van a organizar de acuerdo a la cantidad de equipos necesarios para cada una, se van a organizar de mayor a menor.

| VLAN         | ID de VLAN | Equipos |
| :---         | :---:      | ---:    |
| RRHH         | 1Y         | 12      |
| Contabilidad | 2Y         | 10      |
| Ventas       | 3Y         | 36      |
| Informatica  | 4Y         | 21      |

Donde Y es el último dígito del número de carnet con mayor denominación (En este caso sería el 8)

- Ordenados de mayor a menor (Equipos)

| VLAN         | ID de VLAN | Equipos |
| :---         | :---:      | ---:    |
| Ventas       | 38         | 36      |
| Informatica  | 48         | 21      |
| RRHH         | 18         | 12      |
| Contabilidad | 28         | 10      |

#### Paso 2: Asignación de Mascara y Wildcard

1. Calcular la cantidad de equipos de la `Parte de Host`

Para determinar la cantidad de equipos, se va a emplear la fórmula $2^2$. Se evaluarán varias opciones:

    * Cuando $n = 3$, resulta en $2^3 =  8$ equipos.
    * Cuando $n = 4$, resulta en $2^4 = 16$ equipos.
    * Cuando $n = 5$, resulta en $2^5 = 32$ equipos.
    * Cuando $n = 6$, resulta en $2^6 = 64$ equipos.

Ventas       --> $Equipos = 36 -> 2^6$
Informatica  --> $Equipos = 21 -> 2^5$
RRHH         --> $Equipos = 12 -> 2^4$
Contabilidad --> $Equipos = 10 -> 2^4$

**N** Representa la cantidad de bits que se utilizan para identificar la `Parte de Host`.

2. Determinación de la cantidad de bits para la `Parte de Red`

Una vez calculado **N**, se procede a determinar la cantidad de bits utilizados para la `Parte de Red`. Dicho cálculo se realiza dde la siguiente manera:

Ventas       --> $32 - 6 = 26$ bits -> **CIDR::** `/26`
Informatica  --> $32 - 5 = 27$ bits -> **CIDR::** `/27`
RRHH         --> $32 - 4 = 28$ bits -> **CIDR::** `/28`
Contabilidad --> $32 - 4 = 28$ bits -> **CIDR::** `/28`

>> **Nota:** Se restan con 32 debido a que se está evaluuando una dirección IPv4, la cual consta de 32 bits.

3. Cálculo de la Máscara de Red (Decimal)

Dado que la dirección IP consta de 32 bits, estos se representan de la siguiente manera:

$2^7 + 2^6 + 2^5 + 2^4 + 2^3 + 2^2 + 2^1 + 2^0 = 255$ 
$128 + 64 + 32 + 16 + 8 + 4 + 2 + 1 = 255$

        * Ventas
$11111111.11111111.11111111.11000000 = 255.255.255.192$
        * Informatica
$11111111.11111111.11111111.11100000 = 255.255.255.224$
        * RRHH
$11111111.11111111.11111111.11110000 = 255.255.255.240$
        * Contabilidad
$11111111.11111111.11111111.11110000 = 255.255.255.240$

Donde `11000000` sería $128 + 64 + 0 + 0 + 0 + 0 + 0 + 0 = 192$
Donde `11100000` sería $128 + 64 + 32 + 0 + 0 + 0 + 0 + 0 = 224$
Donde `11110000` sería $128 + 64 + 32 + 16 + 0 + 0 + 0 + 0 = 240$

4. Cálculo de Wildcard

Para realizar este cálculo, se deben utilizar exclusivamente la cantidad de equpos determinada en el **Paso 1**. 

Ventas       --> $255.255.255.255 - 255.255.255.192 = 63$
Informatica  --> $255.255.255.255 - 255.255.255.224 = 31$
RRHH         --> $255.255.255.255 - 255.255.255.240 = 15$
Contabilidad --> $255.255.255.255 - 255.255.255.240 = 15$

| VLAN         | ID de VLAN | Equipos | Máscara de Red  | Wildcard |
| :---         | :---:      | ---:    | :---:           | :---     |
| Ventas       | 38         | 36      | 255.255.255.192 | 0.0.0.63 |
| Informatica  | 48         | 21      | 255.255.255.224 | 0.0.0.31 |
| RRHH         | 18         | 12      | 255.255.255.240 | 0.0.0.15 |
| Contabilidad | 28         | 10      | 255.255.255.240 | 0.0.0.15 |

#### Paso 3: Asignación de ID Red, Primera IP, Última IP e IP Broadcast

Donde la Red Interna de jutiapa con el ID de red 192.178.XX.0/24 (XX número de grupo de desarrolladores de proyecto)

* Grupo 13

Por lo tanto `192.178.13.0/24`

1. Asignación de ID Red

    * Ventas
    Se asigna la red interna inicial (192.178.13.0) + 63 -> 0 + 63 = 63
    * Informatica
    Se asigna la red interna inicial (192.178.13.64) + 31 -> 64 + 31 = 95
    * RRHH
    Se asigna la red interna inicial (192.178.13.96) + 15 -> 96 + 15 = 111
    * Contabilidad
    Se asigna la red interna inicial (192.178.13.112) + 15 -> 112 + 7 = 127

2. Asignación de Primera IP

    * Ventas
    Sería la IP siguiente en la secuencia (192.178.13.1)
    * Informatica
    Sería la IP siguiente en la secuencia (192.178.13.65)
    * RRHH
    Sería la IP siguiente en la secuencia (192.178.13.97)
    * Contabilidad
    Sería la IP siguiente en la secuencia (192.178.13.113)

3. Asignación de Última IP

    * Ventas
    Sería (IP del Broadcast - 1) -> 192.178.13.62
    * Informatica
    Sería (IP del Broadcast - 1) -> 192.178.13.94
    * RRHH
    Sería (IP del Broadcast - 1) -> 192.178.13.110
    * Contabilidad
    Sería (IP del Broadcast - 1) -> 192.178.13.126
    
4. Asignación del Broadcast

    La dirección del broadcast es la última dirección IP asignable. Se determina por la cantidad de Host que hay disponibles.

    > **Nota:** Una forma para visualizar la cantidad de Host que se tienen es por medio del Wildcard

    * Ventas
    Sería (IP del Broadcast) -> 192.178.13.63
    * Informatica
    Sería (IP del Broadcast) -> 192.178.13.95
    * RRHH
    Sería (IP del Broadcast) -> 192.178.13.111
    * Contabilidad
    Sería (IP del Broadcast) -> 192.178.13.127
    
* Tabla con asignaciones de IP siguientes:

| VLAN         | ID de VLAN | Equipos | Máscara de Red  | Wildcard | ID Red         | Primera IP     | Última IP      | IP Broadcast   |
| :---         | :---:      | ---:    | :---:           | :---     | :---           | :---           | :---           | :---           |
| Ventas       | 38         | 36      | 255.255.255.192 | 0.0.0.63 | 192.178.13.0   | 192.178.13.1   | 192.178.13.62  | 192.178.13.63  |
| Informatica  | 48         | 21      | 255.255.255.224 | 0.0.0.31 | 192.178.13.64  | 192.178.13.65  | 192.178.13.94  | 192.178.13.95  |
| RRHH         | 18         | 12      | 255.255.255.240 | 0.0.0.15 | 192.178.13.96  | 192.178.13.97  | 192.178.13.110 | 192.178.13.111 |
| Contabilidad | 28         | 10      | 255.255.255.240 | 0.0.0.15 | 192.178.13.112 | 192.178.13.113 | 192.178.13.126 | 192.178.13.127 |

### Petén 
#### Paso 1: Ordenamiento

Las VLANs se van a organizar de acuerdo a la cantidad de equipos necesarios para cada una, se van a organizar de mayor a menor.

| VLAN         | ID de VLAN | Equipos |
| :---         | :---:      | ---:    |
| RRHH         | 1Y         | 10      |
| Ventas       | 3Y         | 30      |
| Informatica  | 4Y         | 15      |

Donde Y es el último dígito del número de carnet con mayor denominación (En este caso sería el 8)

- Ordenados de mayor a menor (Equipos)

| VLAN         | ID de VLAN | Equipos |
| :---         | :---:      | ---:    |
| Ventas       | 38         | 30      |
| Informatica  | 48         | 15      |
| RRHH         | 18         | 10      |

#### Paso 2: Asignación de Mascara y Wildcard

1. Calcular la cantidad de equipos de la `Parte de Host`

Para determinar la cantidad de equipos, se va a emplear la fórmula $2^2$. Se evaluarán varias opciones:

    * Cuando $n = 3$, resulta en $2^3 =  8$ equipos.
    * Cuando $n = 4$, resulta en $2^4 = 16$ equipos.
    * Cuando $n = 5$, resulta en $2^5 = 32$ equipos.
    * Cuando $n = 6$, resulta en $2^6 = 64$ equipos.

Ventas       --> $Equipos = 30 -> 2^5$
Informatica  --> $Equipos = 15 -> 2^5$
RRHH         --> $Equipos = 10 -> 2^4$

**N** Representa la cantidad de bits que se utilizan para identificar la `Parte de Host`.

2. Determinación de la cantidad de bits para la `Parte de Red`

Una vez calculado **N**, se procede a determinar la cantidad de bits utilizados para la `Parte de Red`. Dicho cálculo se realiza dde la siguiente manera:

Ventas       --> $32 - 5 = 27$ bits -> **CIDR::** `/27`
Informatica  --> $32 - 5 = 27$ bits -> **CIDR::** `/27`
RRHH         --> $32 - 4 = 28$ bits -> **CIDR::** `/28`

>> **Nota:** Se restan con 32 debido a que se está evaluuando una dirección IPv4, la cual consta de 32 bits.

3. Cálculo de la Máscara de Red (Decimal)

Dado que la dirección IP consta de 32 bits, estos se representan de la siguiente manera:

$2^7 + 2^6 + 2^5 + 2^4 + 2^3 + 2^2 + 2^1 + 2^0 = 255$ 
$128 + 64 + 32 + 16 + 8 + 4 + 2 + 1 = 255$

        * Ventas
$11111111.11111111.11111111.11100000 = 255.255.255.224$
        * Informatica
$11111111.11111111.11111111.11100000 = 255.255.255.224$
        * RRHH
$11111111.11111111.11111111.11110000 = 255.255.255.240$

Donde `11100000` sería $128 + 64 + 32 + 0 + 0 + 0 + 0 + 0 = 224$
Donde `11110000` sería $128 + 64 + 32 + 16 + 0 + 0 + 0 + 0 = 240$

4. Cálculo de Wildcard

Para realizar este cálculo, se deben utilizar exclusivamente la cantidad de equpos determinada en el **Paso 1**. 

Ventas       --> $255.255.255.255 - 255.255.255.224 = 31$
Informatica  --> $255.255.255.255 - 255.255.255.224 = 31$
RRHH         --> $255.255.255.255 - 255.255.255.240 = 15$

| VLAN         | ID de VLAN | Equipos | Máscara de Red  | Wildcard |
| :---         | :---:      | ---:    | :---:           | :---     |
| Ventas       | 38         | 30      | 255.255.255.224 | 0.0.0.31 |
| Informatica  | 48         | 15      | 255.255.255.224 | 0.0.0.31 |
| RRHH         | 18         | 10      | 255.255.255.240 | 0.0.0.15 |

#### Paso 3: Asignación de ID Red, Primera IP, Última IP e IP Broadcast

Donde la Red Interna de jutiapa con el ID de red 192.158.XX.0/24 (XX número de grupo de desarrolladores de proyecto)

* Grupo 13

Por lo tanto `192.158.13.0/24`

1. Asignación de ID Red

    * Ventas
    Se asigna la red interna inicial (192.158.13.0) + 31 -> 0 + 31 = 31
    * Informatica
    Se asigna la red interna inicial (192.158.13.32) + 31 -> 32 + 31 = 63
    * RRHH
    Se asigna la red interna inicial (192.158.13.64) + 15 -> 64 + 15 = 79

2. Asignación de Primera IP

    * Ventas
    Sería la IP siguiente en la secuencia (192.158.13.1)
    * Informatica
    Sería la IP siguiente en la secuencia (192.158.13.33)
    * RRHH
    Sería la IP siguiente en la secuencia (192.158.13.65)

3. Asignación de Última IP

    * Ventas
    Sería (IP del Broadcast - 1) -> 192.158.13.30
    * Informatica
    Sería (IP del Broadcast - 1) -> 192.158.13.62
    * RRHH
    Sería (IP del Broadcast - 1) -> 192.158.13.78
    
4. Asignación del Broadcast

    La dirección del broadcast es la última dirección IP asignable. Se determina por la cantidad de Host que hay disponibles.

    > **Nota:** Una forma para visualizar la cantidad de Host que se tienen es por medio del Wildcard

    * Ventas
    Sería (IP del Broadcast) -> 192.158.13.31
    * Informatica
    Sería (IP del Broadcast) -> 192.158.13.63
    * RRHH
    Sería (IP del Broadcast) -> 192.158.13.79
    
* Tabla con asignaciones de IP siguientes:

| VLAN         | ID de VLAN | Equipos | Máscara de Red  | Wildcard | ID Red        | Primera IP    | Última IP     | IP Broadcast  |
| :---         | :---:      | ---:    | :---:           | :---     | :---          | :---          | :---          | :---          |
| Ventas       | 38         | 30      | 255.255.255.224 | 0.0.0.31 | 192.158.13.0  | 192.158.13.1  | 192.158.13.30 | 192.158.13.31 |
| Informatica  | 48         | 15      | 255.255.255.224 | 0.0.0.31 | 192.158.13.32 | 192.158.13.33 | 192.158.13.62 | 192.158.13.63 |
| RRHH         | 18         | 10      | 255.255.255.240 | 0.0.0.15 | 192.158.13.64 | 192.158.13.65 | 192.158.13.78 | 192.158.13.79 |

### Izabal 
#### Paso 1: Ordenamiento

Las VLANs se van a organizar de acuerdo a la cantidad de equipos necesarios para cada una, se van a organizar de mayor a menor.

| VLAN         | ID de VLAN | Equipos |
| :---         | :---:      | ---:    |
| RRHH         | 1Y         | 10      |
| Contabilidad | 2Y         | 5       |
| Ventas       | 3Y         | 25      |

Donde Y es el último dígito del número de carnet con mayor denominación (En este caso sería el 8)

- Ordenados de mayor a menor (Equipos)

| VLAN         | ID de VLAN | Equipos |
| :---         | :---:      | ---:    |
| Ventas       | 38         | 25      |
| RRHH         | 18         | 10      |
| Contabilidad | 28         | 5       |

#### Paso 2: Asignación de Mascara y Wildcard

1. Calcular la cantidad de equipos de la `Parte de Host`

Para determinar la cantidad de equipos, se va a emplear la fórmula $2^2$. Se evaluarán varias opciones:

    * Cuando $n = 3$, resulta en $2^3 =  8$ equipos.
    * Cuando $n = 4$, resulta en $2^4 = 16$ equipos.
    * Cuando $n = 5$, resulta en $2^5 = 32$ equipos.
    * Cuando $n = 6$, resulta en $2^6 = 64$ equipos.

Ventas       --> $Equipos = 25 -> 2^5$
RRHH         --> $Equipos = 10 -> 2^4$
Contabilidad --> $Equipos = 5  -> 2^3$

**N** Representa la cantidad de bits que se utilizan para identificar la `Parte de Host`.

2. Determinación de la cantidad de bits para la `Parte de Red`

Una vez calculado **N**, se procede a determinar la cantidad de bits utilizados para la `Parte de Red`. Dicho cálculo se realiza dde la siguiente manera:

Ventas       --> $32 - 5 = 27$ bits -> **CIDR::** `/27`
RRHH         --> $32 - 4 = 28$ bits -> **CIDR::** `/28`
Contabilidad --> $32 - 3 = 29$ bits -> **CIDR::** `/29`

>> **Nota:** Se restan con 32 debido a que se está evaluuando una dirección IPv4, la cual consta de 32 bits.

3. Cálculo de la Máscara de Red (Decimal)

Dado que la dirección IP consta de 32 bits, estos se representan de la siguiente manera:

$2^7 + 2^6 + 2^5 + 2^4 + 2^3 + 2^2 + 2^1 + 2^0 = 255$ 
$128 + 64 + 32 + 16 + 8 + 4 + 2 + 1 = 255$

        * Ventas
$11111111.11111111.11111111.11100000 = 255.255.255.224$
        * RRHH
$11111111.11111111.11111111.11110000 = 255.255.255.240$
        * Contabilidad
$11111111.11111111.11111111.11111000 = 255.255.255.248$

Donde `11100000` sería $128 + 64 + 32 + 0 + 0 + 0 + 0 + 0 = 224$
Donde `11110000` sería $128 + 64 + 32 + 16 + 0 + 0 + 0 + 0 = 240$
Donde `11111000` sería $128 + 64 + 32 + 16 + 8 + 0 + 0 + 0 = 248$

4. Cálculo de Wildcard

Para realizar este cálculo, se deben utilizar exclusivamente la cantidad de equpos determinada en el **Paso 1**. 

Ventas       --> $255.255.255.255 - 255.255.255.224 = 31$
RRHH         --> $255.255.255.255 - 255.255.255.240 = 15$
Contabilidad --> $255.255.255.255 - 255.255.255.248 = 7$

| VLAN         | ID de VLAN | Equipos | Máscara de Red  | Wildcard |
| :---         | :---:      | ---:    | :---:           | :---     |
| Ventas       | 38         | 25      | 255.255.255.224 | 0.0.0.31 |
| RRHH         | 18         | 10      | 255.255.255.240 | 0.0.0.15 |
| Contabilidad | 28         | 5       | 255.255.255.248 | 0.0.0.7  |

#### Paso 3: Asignación de ID Red, Primera IP, Última IP e IP Broadcast

Donde la Red Interna de jutiapa con el ID de red 192.167.XX.0/24 (XX número de grupo de desarrolladores de proyecto)

* Grupo 13

Por lo tanto `192.167.13.0/24`

1. Asignación de ID Red

    * Ventas
    Se asigna la red interna inicial (192.167.13.0) + 31 -> 0 + 31 = 31
    * RRHH
    Se asigna la red interna inicial (192.167.13.32) + 15 -> 32 + 15 = 47
    * Contabilidad
    Se asigna la red interna inicial (192.167.13.48) + 7 -> 48 + 7 = 55

2. Asignación de Primera IP

    * Ventas
    Sería la IP siguiente en la secuencia (192.167.13.1)
    * RRHH
    Sería la IP siguiente en la secuencia (192.167.13.33)
    * Contabilidad
    Sería la IP siguiente en la secuencia (192.167.13.49)

3. Asignación de Última IP

    * Ventas
    Sería (IP del Broadcast - 1) -> 192.167.13.30
    * RRHH
    Sería (IP del Broadcast - 1) -> 192.167.13.46
    * Contabilidad
    Sería (IP del Broadcast - 1) -> 192.167.13.54
    
4. Asignación del Broadcast

    La dirección del broadcast es la última dirección IP asignable. Se determina por la cantidad de Host que hay disponibles.

    > **Nota:** Una forma para visualizar la cantidad de Host que se tienen es por medio del Wildcard

    * Ventas
    Sería (IP del Broadcast) -> 192.167.13.31
    * RRHH
    Sería (IP del Broadcast) -> 192.167.13.47
    * Contabilidad
    Sería (IP del Broadcast) -> 192.167.13.55
    
* Tabla con asignaciones de IP siguientes:

| VLAN         | ID de VLAN | Equipos | Máscara de Red  | Wildcard | ID Red        | Primera IP    | Última IP     | IP Broadcast  |
| :---         | :---:      | ---:    | :---:           | :---     | :---          | :---          | :---          | :---          |
| Ventas       | 38         | 25      | 255.255.255.224 | 0.0.0.31 | 192.167.13.0  | 192.167.13.1  | 192.167.13.30 | 192.167.13.31 |
| RRHH         | 18         | 10      | 255.255.255.240 | 0.0.0.15 | 192.167.13.32 | 192.167.13.33 | 192.167.13.46 | 192.167.13.47 |
| Contabilidad | 28         | 5       | 255.255.255.248 | 0.0.0.7  | 192.167.13.48 | 192.167.13.49 | 192.167.13.54 | 192.167.13.55 |

----- 

### Sede Jutiapa

* ID Red = 192.168.XX.0/24

Grupo 13 (192.168.13.0/24)

| VLAN         | ID de VLAN | Equipos | Máscara de Red  | Wildcard | ID Red        | Primera IP    | Última IP     | IP Broadcast  |
| :---         | :---:      | ---:    | :---:           | :---     | :---          | :---          | :---          | :---          |
| Ventas       | 38         | 25      | 255.255.255.224 | 0.0.0.31 | 192.168.13.0  | 192.168.13.1  | 192.168.13.30 | 192.168.13.31 |
| Informatica  | 48         | 12      | 255.255.255.240 | 0.0.0.15 | 192.168.13.32 | 192.168.13.33 | 192.168.13.46 | 192.168.13.47 |
| RRHH         | 18         | 10      | 255.255.255.240 | 0.0.0.15 | 192.168.13.48 | 192.168.13.49 | 192.168.13.62 | 192.168.13.63 |
| Contabilidad | 28         | 4       | 255.255.255.248 | 0.0.0.7  | 192.168.13.64 | 192.168.13.65 | 192.168.13.70 | 192.168.13.71 |

### Sede Escuintla

* ID Red = 192.148.XX.0/24

Grupo 13 (192.148.13.0/24)

| VLAN         | ID de VLAN | Equipos | Máscara de Red  | Wildcard | ID Red        | Primera IP    | Última IP     | IP Broadcast  |
| :---         | :---:      | ---:    | :---:           | :---     | :---          | :---          | :---          | :---          |
| Ventas       | 38         | 25      | 255.255.255.224 | 0.0.0.31 | 192.148.13.0  | 192.148.13.1  | 192.148.13.30 | 192.168.13.31 |
| RRHH         | 18         | 10      | 255.255.255.248 | 0.0.0.7  | 192.148.13.32 | 192.148.13.33 | 192.148.13.38 | 192.168.13.39 |

### Sede Quiche

* ID Red = 192.178.XX.0/24

Grupo 13 (192.178.13.0/24)

| VLAN         | ID de VLAN | Equipos | Máscara de Red  | Wildcard | ID Red         | Primera IP     | Última IP      | IP Broadcast   |
| :---         | :---:      | ---:    | :---:           | :---     | :---           | :---           | :---           | :---           |
| Ventas       | 38         | 36      | 255.255.255.192 | 0.0.0.63 | 192.178.13.0   | 192.178.13.1   | 192.178.13.62  | 192.168.13.63  |
| Informatica  | 48         | 21      | 255.255.255.224 | 0.0.0.31 | 192.178.13.64  | 192.178.13.65  | 192.178.13.94  | 192.168.13.95  |
| RRHH         | 18         | 12      | 255.255.255.240 | 0.0.0.15 | 192.178.13.96  | 192.178.13.97  | 192.178.13.110 | 192.168.13.111 |
| Contabilidad | 28         | 10      | 255.255.255.240 | 0.0.0.15 | 192.178.13.112 | 192.178.13.113 | 192.178.13.126 | 192.168.13.127 |

### Sede Petén

* ID Red = 192.158.XX.0/24

Grupo 13 (192.158.13.0/24)

| VLAN         | ID de VLAN | Equipos | Máscara de Red  | Wildcard | ID Red        | Primera IP    | Última IP     | IP Broadcast  |
| :---         | :---:      | ---:    | :---:           | :---     | :---          | :---          | :---          | :---          |
| Ventas       | 38         | 30      | 255.255.255.224 | 0.0.0.31 | 192.158.13.0  | 192.158.13.1  | 192.158.13.30 | 192.158.13.31 |
| Informatica  | 48         | 15      | 255.255.255.224 | 0.0.0.31 | 192.158.13.32 | 192.158.13.33 | 192.158.13.62 | 192.158.13.63 |
| RRHH         | 18         | 10      | 255.255.255.240 | 0.0.0.15 | 192.158.13.64 | 192.158.13.65 | 192.158.13.78 | 192.158.13.79 |

### Sede Izabal

* ID Red = 192.167.XX.0/24

Grupo 13 (192.167.13.0/24)

| VLAN         | ID de VLAN | Equipos | Máscara de Red  | Wildcard | ID Red        | Primera IP    | Última IP     | IP Broadcast  |
| :---         | :---:      | ---:    | :---:           | :---     | :---          | :---          | :---          | :---          |
| Ventas       | 38         | 25      | 255.255.255.224 | 0.0.0.31 | 192.167.13.0  | 192.167.13.1  | 192.167.13.30 | 192.167.13.31 |
| RRHH         | 18         | 10      | 255.255.255.240 | 0.0.0.15 | 192.167.13.32 | 192.167.13.33 | 192.167.13.46 | 192.167.13.47 |
| Contabilidad | 28         | 5       | 255.255.255.248 | 0.0.0.7  | 192.167.13.48 | 192.167.13.49 | 192.167.13.54 | 192.167.13.55 |

## FLSM Obtenidos (Cálculo)

Para el cálculo de FLSM se han considerado 16 (14 utilizables) hosts por subred

PACKET TRACER<br/>
Router (Router-PT)<br/>
Switch Capa 3 (3560-24PS) (CENTRAL, JUTIAPA, QUICHE, PETEN, ESCUINTLA, IZABAL, J1, J2, ESW1, ESW2, ESW4, ESW3)<br/>
Switch Capa 2 (2960-24TT) (SW5)<br/>
ASA0 (5506-X)

### Router Central

| Interfaz          | IP Red    | Máscara Red   |
| :---              | :---      | :---:         |
| fa5/0 (IZABAL)    | 10.0.0.1  | 255.255.255.0 |
| fa6/0 (PETEN)     | 10.0.0.17 | 255.255.255.0 |
| fa7/0 (ESCUINTLA) | 10.0.0.33 | 255.255.255.0 |
| fa8/0 (JUTIAPA)   | 10.0.0.49 | 255.255.255.0 |
| fa9/0 (QUICHE)    | 10.0.0.65 | 255.255.255.0 |
| gi4/0 (ASA0)      | 10.0.0.81 | 255.255.255.0 |

### Router JUTIAPA

| Interfaz          | IP Red     | Máscara Red   |
| :---              | :---       | :---:         |
| fa0/0 (J2)        | 10.0.0.97  | 255.255.255.0 |
| fa1/0 (J1)        | 10.0.0.113 | 255.255.255.0 |
| fa5/0 (PETEN)     | 10.0.0.129 | 255.255.255.0 |
| fa6/0 (IZABAL)    | 10.0.0.145 | 255.255.255.0 |
| fa7/0 (QUICHE)    | 10.0.0.161 | 255.255.255.0 |
| fa8/0 (ESCUINTLA) | 10.0.0.177 | 255.255.255.0 |
| fa9/0 (CENTRAL)   | 10.0.0.193 | 255.255.255.0 |

### Router QUICHE

| Interfaz          | IP Red     | Máscara Red   |
| :---              | :---       | :---:         |
| fa1/0 (ESW4)      | 10.0.0.209 | 255.255.255.0 |
| fa5/0 (ESCUINTLA) | 10.0.0.225 | 255.255.255.0 |
| fa6/0 (IZABAL)    | 10.0.1.1   | 255.255.255.0 |
| fa7/0 (JUTIAPA)   | 10.0.1.17  | 255.255.255.0 |
| fa8/0 (PETEN)     | 10.0.1.33  | 255.255.255.0 |
| fa9/0 (CENTRAL)   | 10.0.1.49  | 255.255.255.0 |

### Router PETEN

| Interfaz          | IP Red     | Máscara Red   |
| :---              | :---       | :---:         |
| fa1/0 (ESW2)      | 10.0.1.65  | 255.255.255.0 |
| fa5/0 (JUTIAPA)   | 10.0.1.81  | 255.255.255.0 |
| fa6/0 (CENTRAL)   | 10.0.1.97  | 255.255.255.0 |
| fa7/0 (ESCUINTLA) | 10.0.1.113 | 255.255.255.0 |
| fa8/0 (QUICHE)    | 10.0.1.129 | 255.255.255.0 |
| fa9/0 (IZABAL)    | 10.0.1.145 | 255.255.255.0 |

### Router ESCUINTLA

| Interfaz          | IP Red     | Máscara Red   |
| :---              | :---       | :---:         |
| fa1/0 (SW5)       | 10.0.1.161 | 255.255.255.0 |
| fa5/0 (QUICHE)    | 10.0.1.177 | 255.255.255.0 |
| fa6/0 (CENTRAL)   | 10.0.1.193 | 255.255.255.0 |
| fa7/0 (PETEN)     | 10.0.1.209 | 255.255.255.0 |
| fa8/0 (JUTIAPA)   | 10.0.1.225 | 255.255.255.0 |
| fa9/0 (IZABAL)    | 10.0.2.1   | 255.255.255.0 |

### Router IZABAL

| Interfaz          | IP Red    | Máscara Red   |
| :---              | :---      | :---:         |
| fa1/0 (ESW3)      | 10.0.2.17 | 255.255.255.0 |
| fa5/0 (CENTRAL)   | 10.0.2.33 | 255.255.255.0 |
| fa6/0 (JUTIAPA)   | 10.0.2.49 | 255.255.255.0 |
| fa7/0 (QUICHE)    | 10.0.2.65 | 255.255.255.0 |
| fa8/0 (PETEN)     | 10.0.2.81 | 255.255.255.0 |
| fa9/0 (ESCUINTLA) | 10.0.2.97 | 255.255.255.0 |
