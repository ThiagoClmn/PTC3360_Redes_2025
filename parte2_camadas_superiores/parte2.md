<!--PARTE 2-->
<!--
2. Introdução às camadas superiores (7 aulas)

2.3. Camada de rede: endereçamento IP e como funciona um roteador
-->
# Parte 2 - Camadas superiores

Referência:
- Aplicação: Kurose, Seções 2.1 e 2.2
- Transporte:
- Rede:

- Neste capítulo do curso, será feita uma introdução a aspectos das 3 camadas superiores da pilha de protocolos Internet (Aplicação, Transporte e Rede).
- A abordagem utilizada é a top-down, começando da camada de aplicação, mais próxima dos usuários, em direção à camada física.
- Esses assuntos são detalhados em cursos posteriores de graduação e pós-graduação.

## Aplicação
### Camada de aplicação
Alguns aplicativos de rede:
- E-mail
- Whatsapp
- Waze e Google Maps
- BitTorrent
- Jogos online
- Streaming de vídeo
- Videoconferência (Meet e Zoom)
- Redes socias
- Dentre outros...

#### Arquiteturas de aplicação
1. Cliente-servidor
    - Servidor:
        - em host sempre ligado
        - tem endereço IP permanente
        - podem estar em data centers, pensando em escala (servidor virtual)
    - Cliente:
        - comunicam-se com servidor e não diretamente entre si
        - podem se conectar de forma intermitente
        - podem ter endereços IP dinâmicos
    - Exemplos: Web, e-mail, streaming, ...

2. *Peer-to-peer* (P2P)
    - Dependência mínima de servidores dedicados
    - Comunicação direta ente sistemas finais (peers)
    - Peers requerem serviço de outros
    peers – provêm serviço para outros
    peers em retorno
        - Autoescalável – novos peers trazem
        novas demandas de serviço, mas
        também fornecem serviço
    - Peers conectam-se intermitentemente e mudam endereço IP
        - Gerenciamento mais complexo
    - Exemplo: BitTorrent, Bitcoin, Bluetooth

#### Sockets

- Aplicativo envia/recebe mensagens de/para uma interface de software ([API -Application Programming Interface](https://en.wikipedia.org/wiki/API)) : socket
- Aplicativo transmissor envia mensagem pela porta
- Ele confia na infraestrutura de transporte do outro lado da porta para entregar mensagem ao socket no aplicativo receptor
- Aplicativo receptor lê dados do socket

#### Endereçamento de processos

- Para receber mensagens, processo precisa ter identificador
- Dispositivo host tem endereço IP de 32 bits único (IPv4)
- Endereço IP do host em que o processo roda é suficiente para identificar o processo? $\rarr$ Não. Muitos processos podem estar rodando no mesmo host!

❖ Identificador inclui tanto endereço IP (32 bits) quanto número da porta associado com processo no host.
- Exemplos de números de porta:
    - Servidor web (HTTP): 80
    - Servidor web (HTTPS): 443
    - Servidor de e-mail (SMTP): 25
    - Gerenciados pela Internet Assigned Numbers Authority (IANA)
    - http://www.iana.org
- Para enviar mensagem HTTP para servidor web www.usp.br:
    - Endereço IP: 200.144.248.41
    - Número de porta: 80
- Mais em breve…

O protocolo da camada de aplicação define:
- Tipos de mensagens trocadas
    - Por exemplo, requisição, resposta
- Sintaxe da mensagem
    - Campos da mensagem e como eles são delineados
- Semântica das mensagens
    - Significado das informações dos campos
- Regras para quando e como aplicativos enviam e respondem mensagens

Protocolos abertos:
- Definidos em RFCs
- Permitem interoperabilidade
- Exemplos:
    - HTTP [[RFC 9110](https://www.rfc-editor.org/rfc/rfc9110)]
    - SMTP [[RFC 5321](https://tools.ietf.org/html/rfc5321)]

Protocolos proprietários:
    - Por exemplo, Microsoft Teams.

SLIDE 15 DE 31 (PARTE DE CAMADA DE APLICAÇÃO)

### Princípios da transferência confiável de dados (recorte da camada de transporte)
...

### Camada de rede
...

## Transporte
<!-- 2.2. Camada de transporte: Princípios da transferência confiável de dados -->
...

## Rede
Referência:
- Parte 1: (Kurose, Seção 4.1)
- Parte 2: (Kurose, Seções 1.4 e 4.2)

### Introdução à camada de rede
Sobre a camada de rede:
- Transporta segmentos do host fonte para o host destino
- No lado transmissor encapsula segmentos em datagramas
- No lado receptor, entrega segmentos para camada de transporte
- Protocolos da camada de rede em todo host e roteador
- Roteador examina campos de cabeçalho em todos os datagramas que passam por ele

#### Duas funções-chave da camada de rede
- Repasse (forwarding)
    - mover pacotes da entrada do roteador para saída apropriada (local)
    - Hardware – escala de nanosegundos
- Roteamento (routing)
    - determinar rota tomada por pacotes da fonte ao destino (global)
    - Software – escala de segundos
    - Algoritmos de roteamento (fica para o 4º ano)

Uma analogia para entender essas duas funções:
- Roteamento: processo de planejar viagem da origem ao destino
- Repasse: processo de passar por uma única encruzilhada

<p align="center">
    <img 
    src="./img/rede_analogia-repasse-rotea.png" 
    alt="a" width="500">
</p>

#### Plano de dados e plano de controle
**Plano de Dados**:
- Função local executada por cada roteador
- Determina como um datagrama chegando por uma porta de entrada do roteador é repassado para uma porta de saída
- Função de repasse (basicamente)

**Plano de Controle**:
- Lógica para rede inteira
- Determina como datagrama são repassados entre roteadores ao longo do caminho fim-a-fim do host fonte até o host destino
- 2 abordagens para plano de controle:
    - Algoritmos de roteamento tradicionais: implementados nos roteadores
    - Software-Defined Networking (SDN): implementados em servidores (remotos)

### Dentro de um roteador
#### Arquitetura básica de um roteador
Duas funções chaves do roteador:
- Rodar protocolos e algoritmos de roteamento
- Repassar ou comutar datagramas de enlace de entrada para enlace de saída

<p align="center">
    <img src="./img/rede_arquit_roteador.png" alt="Arquitetura de roteador da CISCO">
</p>

Link de vídeo explicativo: [o que há dentro de um roteador/switch/ ponto
de acesso Wifi doméstico](https://www.youtube.com/watch?v=q_8dpXrEwZI)

#### Tabela de repasse: ideia inicial
...

**Casamento de prefixo mais longo**: Quando busca-se entrada de tabela de repasse para dado endereço de destino, usa-se prefixo de endereço mais longo que casa com endereço desejado.

#### Funções da porta de entrada
...

...

#### Mecanismos de agendamento
- Agendamento FIFO
- Agendamento por prioridade
- Round robin ("rodízio")
- Weighted Fair Queuing (WFQ)

#### Transbordamento
- Pode haver também transbordamento do buffer da porta de saída.
- Política de qual pacote descartar é chamada de administração ativa de fila.
- Exemplos:
    - Tail drop: elimina pacote chegando
    - Prioridade: elimina/remove pacote baseado em prioridade
    - Aleatório: elimina/remove aleatoriamente

#### Neutralidade da rede

O que é a neutralidade da rede?
- Técnica: como uma rede de acesso deve alocar e compartilhar seus recursos
    - Mecanismos: agendamento de pacotes, administração ativa de filas
- Princípios sociais, econômicos e políticos.
    - Proteção à liberdade de expressão
    - Encorajamento da inovação e competição
- Implementada via políticas públicas, normas técnicas e leis.

Países diferentes usam abordagens diferentes para a neutralidade da rede.

A neutralidade da rede é regulada no Brasil com o Marco Civil da Internet, Lei n. º 12.965/2014, sancionada em 2014 e regulamentada em 2016.

> Seção I: Da neutralidade da rede ([Lei n. º 12.965/2014](https://www.planalto.gov.br/ccivil_03/_ato2011-2014/2014/lei/l12965.htm))
>> O responsável pela transmissão, comutação ou roteamento tem o dever de tratar de forma isonômica quaisquer pacotes de dados, sem distinção por conteúdo, origem e destino, serviço, terminal ou aplicação.



### Atrasos e vazão
Comutação de pacotes: store-and-forward

- Leva $L/R$ segundos para transmitir (inserir) pacote de $L$ bits em um enlace a $R$ bits/s
- *Store and forward* (adotado em geral em roteadores): pacote inteiro precisa chegar no roteador antes que possa ser transmitido ao próximo enlace

No exemplo abaixo, atraso fim-fim para 1 pacote = 2L/R (assumindo zero atraso de propagação)

<p align="center">
    <img src="./img/rede_atrasos-vazao.png" alt="Exemplo genérico de atraso de comutação">
</p>

#### Fontes de atraso de pacotes
<p align="center">
    <img src="./img/rede_fontes_atraso_pacote.png" alt="Ilustração de cada fonte de atraso na transmissão de pacotes">
</p>

- $d_{proc}$: processamento no nó
    - Detecção de erros em bits
    - Determinação do enlace de saída
    - De nano a microssegundos
- $d_{fila}$: atraso de fila
    - Tempo esperando no enlace de saída para transmissão
    - Depende do nível de congestionamento no roteador
    - De micro a milissegundos
- $d_{trans}$: atraso de transmissão
    - de nano a milissegundos
    - $L$: comp. do pacote (bits)
    - $R$: capacidade do enlace (bits/s)
    - $d_{trans} = L/R$
- $d_{prop}$: atraso de propagação
    - milisegundos (WAN)
    - $s$: comprimento do enlace físico
    - $v$: vel. de propagação no meio (~2x108 m/s)
    - $d_{prop} = s/v$

Avaliando atrasos de transmissão e de propagação:
- Enlace longos e taxas de transmissão (R) altas – dprop predomina
- Enlace curtos e taxas de transmissão (R) baixas – dtrans predomina
- Em muitos casos, uma delas é bem mais importante que a outra

...

#### Vazão (*throughput*)

- Vazão: taxa (bits/s) em que bits são transferidos entre fonte/destino
    - Instantânea: taxa num certo instante de tempo
    - Média: taxa média sobre um período de tempo

<p align="center">
    <img src="./img/rede_vazao.png" alt="Ilustração da vazão de transmissão">
</p>

...

No cenário internet:

<p align="center">
    <img src="./img/rede_vazao_cenario_internet.png" alt="Ilustração da vazão de transmissão" width="450">
</p>

10 conexões partilham conexões (de forma justa) no enlace gargalo backbone R bits/s

- Vazão por conexão: $min(R_c,R_s,R/10)$
- Na prática: $R_c$ or $R_s$ é geralmente o gargalo


### Endereçamento IP
...

### Repasse generalizado e SDN
...
