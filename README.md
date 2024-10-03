sequenceDiagram
    
    participant DHCP1 as Servidor DHCP 1
    participant Usuario
    participant DHCP2 as Servidor DHCP 2

    %% El usuario enciende su computadora y se conecta a DHCP1
    Usuario ->> DHCP1: DHCPDISCOVER
    Usuario ->> DHCP2: DHCPDISCOVER
    DHCP1 ->> Usuario: DHCPOFFER
    DHCP2 ->> Usuario: DHCPOFFER
    Usuario ->> DHCP1: DHCPREQUEST
    Usuario ->> DHCP2: DHCPREQUEST
    DHCP1 ->> Usuario: DHCPACK
    DHCP1 ->> Usuario: IP Grant for 8 Hours
    Note left of Usuario: Connected for 1 hour

    %% Interrupción del servicio en el Servidor DHCP1
    Usuario -x DHCP1: Service interruption (3 minutes)
    Note left of Usuario: User disconnected for 3 minutes

    %% Reconexión al servidor DHCP2 después de la caída de DHCP1
    Usuario ->> DHCP2: DHCPREQUEST
    DHCP2 ->> Usuario: IP Grant for 8 Hours
    Note right of Usuario: Connected for 4 hour

    %% Intento de renovación a las 4 horas
    Usuario ->> DHCP2: DHCPREQUEST
    DHCP2 ->> Usuario: DHCPACK
    Note right of Usuario: Extended connection for an additional 8 hours

    %% Usuario sigue conectado
    Note right of Usuario: Connected for 12 hours total

    %% Desconexión del usuario durante 1 hora
    Usuario -x DHCP2: User disconnection (1 hour)

    %% Reconexión del usuario
    Usuario ->> DHCP2: DHCPREQUEST
    DHCP2 ->> Usuario: IP Grant for 8 Hours
    Note right of Usuario: Connected for 5 hour

    %% Usuario se desconecta definitivamente
    Usuario -x DHCP2: Definitive disconnection
