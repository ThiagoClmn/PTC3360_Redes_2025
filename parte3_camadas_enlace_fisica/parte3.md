# Parte 3 - Camadas de enlace e física

<!--
3.1 Introdução
Referência para estudo: Kurose, Seção 5.1 e notas de aula.

3.2 Controle de acesso ao canal compartilhado e endereçamento MAC

Referência para estudo: Kurose, Seções 6.3 e notas de aula.

3.3 Endereçamento MAC e switches

Referência para estudo: Kurose, Seções 6.4 e notas de aula.

3.4 Camada Física: meios de transmissão

Referência para estudo: notas de aula

3.5 Camada Física: Rádio-enlaces

Referência para estudo: Mariotto, Cap. 4, Wentworth, Cap. 8 e notas de aula.

Referência: (Kurose, Seções 1.1 e 1.2)
-->

Referências:
- Kurose, Seções 6.1 e 6.3
- Tanembaum, Seção Seção 4.2

## Introdução

A camada de enlace trata da transferência de quadros entre elementos vizinhos na rede. Alguns protocolos importantes dessa camada são:
- Ethernet
- 802.11 (WiFi)
- DOCSIS

### Terminologia
- Nós: dispositivos que rodam protocolos da camada de enlace (camada 2) - hosts, roteadores, switches, pontos de acesso WiFi, etc.
- Enlaces: canais de comunicação que conectam nós adjacentes ao longo do
caminho de comunicação
    - enlaces com fio
    - enlaces sem fios
    - LANs
- Quadro: pacote na camada de enlace
    - encapsula datagrama (pacote na camada de rede)

Camada de enlace de dados tem a responsabilidade de transferir datagramas de um nó a um nó fisicamente adjacente através de um enlace

### Onde a camada de enlace é implementada?

- Em cada e todos os nós
- Em hosts, camada de enlace implementada em “adaptador” (ou cartão de interface de rede NIC) ou na placa-mãe (on board)
- Exemplos: [Controlador de interface de rede Ethernet](https://en.m.wikipedia.org/wiki/Network_interface_controller) e 802.11- implementa camadas de enlace e física
- Exemplo de [datasheet](https://www.ti.com/lit/ds/symlink/dp83815.pdf)
- Afixa-se no sistema de barramentos do host
- Combinação de hardware, software, [firmware](https://pt.wikipedia.org/wiki/Firmware)

### Comunicação entre adaptadores

<img src="./img/enlace_intro-comunicação-adaptadores.png" alt="Comunicação entre adaptadores" width="500">

- Lado que envia:
    - Encapsula datagrama em quadro
    - Adiciona bits verificadores de erro, informações para comunicação confiável (RDT), controle de fluxo, etc.
- Lado que recebe:
    - Procura por erros, RDT, controle de fluxo, etc.
    - Extrai datagrama, passa para camada superior no lado recebendo

### Principais serviços da camada de enlace

- Enquadramento e endereçamento:
    - Encapsulamento do datagrama em quadro, adicionando cabeçalho
    - Endereços “MAC” (Medium Access Control) usados nos cabeçalhos de quadro para identificar fonte e destino
- Detecção e correção de erro:
    - Erros causados por atenuação do sinal, ruído, interferências.
    - Receptor detecta presença de erros e pode tomar várias possíveis providências:
        - Simplesmente descarta quadro
        - Descarta quadro e solicita retransmissão
        - Corrige erro(s) em bit(s) sem necessitar de retransmissão
- Acesso compartilhado ao enlace:
    - Regras para acesso ao canal se o meio é compartilhado

## Controle de acesso ao canal compartilhado

### Enlaces de acesso múltiplo e protocolos
2 tipos de enlace:
- Ponto-a-ponto:
    - PPP para acesso discado (dial up)
    - Enlace ponto-a-ponto entre switch ethernet e host
- De difusão ou broadcast (meio compartilhado):
    - Ethernet com barramento
    - Upstream do DOCSIS (Internet por empresa de TV a cabo)
    - LAN sem fio 802.11 (Wifi)
### Particionamento de canal


### Acesso aleatório
#### Slotted ALOHA
- Criado por [Norman Abramson](https://en.wikipedia.org/wiki/Norman_Abramson) em 1969 no Hawaii
    - ALOHA: Additive Links On-line Hawaii Area
- Foi insipiração inicial para CSMA do Ethernet e Wi-Fi

Hipóteses:
- Todos os quadros têm mesmo comprimento $L$ bits
- Tempo dividido em slots de $L/R$ segundos (tempo para transmitir 1 quadro)
- Nós começam a transmitir apenas no início do slot
- Nós são sincronizados
- Se 2 ou mais nós transmitem em mesmo slot, todos os nós são avisados da colisão

Operação:
- Quando chega novo quadro, nó transmite no próximo slot
- Se houve colisão: nó tenta retransmitir quadro após um número aleatório de slots
- Se houve transmissão e não há colisão: ok!

<img src="./img/enlace_slotted_aloha.png" alt="Transmissão no Slotted ALOHA" width="500">

Prós:
- Se houver um único nó ativo,
ele transmite continuamente
na taxa total do canal
- Altamente descentralizado:
apenas slots nos nós
precisam ser sincronizados
- Simples

Contras:
- Colisões, slots perdidos
- Slots desocupados
- Colisões poderiam ser detectadas em menos tempo do que o necessário para transmitir pacotes
- Sincronização de relógio

##### **Eficiência**

É a fração dos slots bem sucedidos.

> Tráfego ofertado ($G$):  Número médio de tentativas de transmissão ou retransmissões em um dado instante

Suponha que $N$ nós geram um tráfego $G\leq N$, que é originado das transmissões ou retransmissões que ocorrem com probabilidade $p = G/N$ por nó.

Probabilidade de que dado nó tenha sucesso em um slot = $p(1-p)^{N-1}$

Probabilidade de que qualquer nó
tenha sucesso:
$$
E = Np(1-p)^{N-1} = G\left(1 - \dfrac{G}{N}\right)^{N-1}
$$

<div style="border: 1px solid #ccc; padding: 10px; background-color: #0000;">
1) Mostrar que a eficiência máxima do slotted ALOHA é

$$
E_{max} = \left( 1 - \dfrac{1}{N} \right)^{N-1}
$$
e ocorre para G = 1.

Solução: Derivada nula de E em relação a G (ponto crítico):

$$
\dfrac{\partial E}{\partial G} = 0
$$

2) Mostrar que 
$$
\lim_{N \to \infty} = E_{max} = \dfrac{1}{e}
$$
</div>
<br>
<img src="./img/enlace_slotted_aloha_eficiencia_graf.png" width="700">

#### ALOHA puro
#### CSMA
#### Exemplos: Ethernet e Wi-Fi




### Revezamento











## Endereçamento MAC e *switches*
...

<!--
## Camada física: meios de transmissão
...

## Rádio enlaces
...
-->

<!-- 
[The ALOHANET - Surfing for Wireless Data - História do ALOHA pelo seu inventor](https://www.eng.hawaii.edu/wp-content/uploads/2020/06/THE-ALOHANET-%E2%80%94-SURFING-FOR-WIRELESS-DATA.pdf) 
-->

## Links extras:
- [The ALOHANET - Surfing for Wireless Data - História do ALOHA pelo seu inventor](./extra/THE-ALOHANET-—-SURFING-FOR-WIRELESS-DATA.pdf)

