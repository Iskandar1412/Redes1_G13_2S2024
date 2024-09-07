## **Manual Técnico para la Configuración de una Red Local con VLANs, VTP y STP**

### **Objetivos del Proyecto**

1.  **General:** Demostrar conocimientos adquiridos sobre la creación de una red local pequeña.
2.  **Específicos:**
    -   Configurar VLANs y VTP para segmentar la red.
    -   Configurar STP para garantizar redundancia y evitar bucles.
    -   Usar Packet Tracer para desarrollar la topología.
    -   Utilizar Wireshark para capturas de paquetes.

### **Herramientas Necesarias**

-   **Software:** Cisco Packet Tracer para simulación de red.
-   **Hardware:** Switches y computadoras portátiles para pruebas.

### **Descripción del Proyecto**

-   Crear una red local para "Solución al Cliente S.A." con 4 departamentos: Contabilidad, Secretaría, RRHH, y IT.
-   Usar VLANs para separar los departamentos y asegurar que no haya tránsito de datos entre ellos.
-   Implementar redundancia y conectividad mediante STP.
-   Configurar VTP en modo servidor, cliente y transparente.

### **Configuración de la Red**

La red se divide en tres áreas:

1.  **Área Administrativa:** Switches en modo cliente y uno en modo transparente.
2.  **Área Central:** Contiene el switch raíz del STP y el servidor VTP.
3.  **Oficina A:** Contiene los dispositivos de los departamentos.

### **Configuración de VLANs y VTP**

1.  **Crear VLANs:**
    
    -   Se conecto a cada switch mediante consola, Telnet o SSH.
    -   se ingreso en el modo de configuración global
        
2.  **Configurar VTP:**

Se muestra las configuraciones mas importantes de lo switchs 
-   **Servidor VTP (en SW1):**
configuracion inicial

	    enable
	    config t 
	    no ip domain-lookup
	    hostname SW1
	    do w
       
	modo truncal

	    interface range fa0/19-24
	    switchport trunk encapsulation dtlq
	    switchport mode trunk 
	    exit
	    do w

	creacion de vlan
	
		vlan 12 
        name RRHH
        vlan 22
        name Secretaria
        vlan 32
        name Contabilidad
        vlan 42
        name IT
        exit 
        do w
	        
   -   **Cliente VTP ( SW7, SW8, SW10, SW14, SW2, SW3, SW4, SW5, SW6, SW15, SW11, SW12, SW13):**
       se muestra como se configuro el Switch 8 en cliente
       
	        enable
	        conf t
	        no ip domain-lookup
	        hostname SW8
	        do w
	        -- truncal
	        interface range fa0/1-3
    		switchport trunk encapsulation dot1q
    		switchport mode trunk
    		exit
    		do w
			-- Protocolo
			vtp mode client
			vtp domain G13
			vtp password usac
			do w
			-- ACCESO
			int f0/1
			switchport mode access
			switchport access vlan 42
			do w


        
-   **Transparente VTP ( SW9):**

			enable
			conf t
			no ip domain-lookup
			hostname SW9
			do w
			-- Modo
			vtp mode transparent
			vtp domain G13
			vtp password usac
			do w


 -   **configuracion de STP:**

		
        		enable
			conf t
			spanning-tree mode rapid-pvst


