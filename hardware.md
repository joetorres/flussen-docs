# Central de controle

Tech Stack:

- Placa DTWonder usando um microcontrolador ESP32 Relay Board.
- Projeto em C++ / platformio.

Central de controle será suada para contorlar até duas bombas d'águas.


Bomba 1, usada para reservatórios, sempre será conectada ao relay 1.

A central poderá ser conectada a rede via Wifi, ou RJ45.
Cada central deve ser associada uma chave unica, para controle interno e relatórios futuros.

## Lista de entradas e saídas

* Relay 1 - COM - energia - NO Bomba
* I1 - Botão para funcionalidades de reset e restauracão de fábrica.
* I2 - reservado
* I3 - Saida para Led de diagnóstico (pode ser acoplado um led para que acenda em ritmos diferentes, exibindo  códigos de erro diferentes.)
* I5 - sensor 25%
* I6 - sensor 50%
* I7 - sensor 75%
* I8 - sensor 100%
* V+ - positivo fonte 12v
* V- - negativo fonte 12v

## Códigos de erro para led de diagnóstico

* Led acesso sólido - operação normal
* Led piscando 1 vez - aguardando conexão bluetooth
* Led piscando 2 vezes - Aguardando dados de wifi
* Led pistcando 3 vezes - Conectando com servidor pela primeira vez
* Led piscando 1, intervalo, piscando 2 vezes - checar sensor 25%
* Led piscando 1, intervalo piscando 3 vezes - checar sensor 50%
* Led piscando 1, intervalo piscando 3 vezes - checar sensor 75%
* Led piscando 1, intervalo, piscando 3 vezes - checar sensor 100%

## Diagrmaa de setup de central de controle

```mermaid
flowchart TD;
    INI[Início] --> LOGIN(usuário faz login);
    LOGIN --> DECISAO{usuário tem centrais de controle cadastradas?};
    DECISAO -->|sim| CENTRAIS(Lista centrais de controle cadastradas);
    CENTRAIS --> SELECIONA_CENTRAL(Seleciona uma central de controle);
    SELECIONA_CENTRAL --> GERENCIAMENTO(Abre gerenciamento da central de controle);
    GERENCIAMENTO --> FIM[Final]
    DECISAO -->|Não| PERMISSAO_B(solicita permissões de bluetooth e wifi);
    PERMISSAO_B --> PERMISSAO_BOK(concede permissões de bluetooth);
    PERMISSAO_BOK --> LISTA_CENTRAIS(Lista centrais não sincronizadas);
    LISTA_CENTRAIS --> SELECIONA_CENTRAL2(Seleciona uma central);
    SELECIONA_CENTRAL2 --> LISTA_WIFI(Lista redes sem fio);
    LISTA_WIFI --> SELECTIONA_WIFI(Seleciona rede sem fio);
    SELECTIONA_WIFI --> SENHA_WIFI(Solicita senha da rede);
    SENHA_WIFI--> SENHA_WIFI2(Digita a senha e clica em prosseguir);
    SENHA_WIFI2 --> ENVIO_WIFI(Envia para a placa o SSID da rede e senha via bluetooth);
    ENVIO_WIFI --> WIFI_OK(Placa loga na rede, envia ok para app e desliga bluetooth);
    WIFI_OK --> TIPO(Pergunta tipo de central de controle. Cisterna ou Reservatório);
    TIPO --> ESCOLHA_TIPO(Usuário escolhe tipo, e abre a lista de dispositivos conectados);
    ESCOLHA_TIPO --> GERENCIAMENTO;

```
