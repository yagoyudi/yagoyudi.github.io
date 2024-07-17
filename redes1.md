# Conteúdo de Redes 1

Esse documento é o meu resumo da matéria de Redes 1.

## Modelo de Camadas

Modelo OSI:

    +---------------------+
    |      Aplicação      |
    +---------------------+
    |     Apresentação    |
    +---------------------+
    |       Sessão        |
    +---------------------+
    |     Transporte      |
    +---------------------+
    |        Rede         |
    +---------------------+
    |       Enlase        |
    +---------------------+
    |       Física        |
    +---------------------+

Modelo TCP/IP:

    +---------------------+
    |      Aplicação      | (Aplicação + Apresentação)
    +---------------------+
    |         TCP         | (Sessão + Transporte)
    +---------------------+
    |         IP          |
    +---------------------+
    |       Enlase        |
    +---------------------+
    |       Física        |
    +---------------------+

Cada mensagem faz todo o caminho de envelopamento e desenvelopamento ao subir e
descer na pilha.

Cada camada só vê o que tem no cabeçalho da sua respectiva camada.

Todos os cabeçalhos adicionados na mensagem são sobrecarga. Aumenta o custo de banda.

### Camada Física

É o hardware da rede.

#### Antena

Pega um sinal analógico de dentro e joga para fora, ou vice-versa.

analógico <-> analógico

#### Hub

Espeta um cabo que envia o sinal e todos os outros cabos nele também recebem o
sinal.

#### Switch

Igual o hub, mas escolhe para quem vai enviar o sinal.

#### Placa de rede

digital <-> digital

#### Roteador

Igual o switch, mas para redes ao invés de cabos.

#### Satélite

Reflete o sinal.

#### MoDem 

ModuladorDemodulador.

Modulador: digital -> analógico.

Demodulador: analógico -> digital.

#### Cabos

Existem vários tipos de cabos:
*   Fibra
    *   Monomodal 

        Mais finos, portanto mais rápido e mais caro.

    *   Multimodal

        Mais largo, portanto mais lento e mais barato.

*   Par trançado
    *   cat 3
    *   cat 4
    *   cat 5
    *   cat 5e
    *   cat 6
    *   cat 7
    *   cat 8
*   Telefone
*   Coaxial
    *   Grosso
    *   Fino

### Camada de Enlase

Função:
1. cuidar da comunicação entre duas máquinas **diretamente** ligadas
   (fisicamente).

### Camada de Rede

Funções:
1.  Escolher qual rota vai ser feita (Roteamento).
2.  Identificar as máquinas (Endereço).
    
    Os endereços precisam ser únicos (IP).

### Camada de Transporte

Função:
1. Cuidar da comunicação da origem/destino.

Essa camada não liga para o meio.

Exemplo: cuida da ordenação das mensagens.

### Camada de Sessão

Função:
1. Separar os diferentes aplicativos.

### Camada de Apresentação

Função:
1. Uniformizar as requisições das aplicações.

### Camada da Aplicação

São todos os aplicativos.

Função:
1. Cuidar da interação com o usuário.

## Ondas

Ondas possuem:

* Comprimento: distância entre duas cristas ou dois vales.
* Amplitude: altura da onda.
* Frequência: número de vezes que a onda oscila por segundo.

A quantidade de frequências que você pode usar é limitada pela Anatel aqui no
Brasil.

Ondas sofrem com a **atenuação**.

Atenuação := Conforme a onda viaja, ela perde força.

Quanto maior a frequência, maior a atenuação.

Ondas com frequência maior possuem alcance menor.

Dados são transmitidos nas oscilações da onda. Logo, quanto maior a frequência,
maior a taxa de transmissão.

**Dilema**: encontrar o meio termo entre a taxa de transmissão e o alcance.

Operadoras de telefonia celular entendem que a melhor relação fica entre 700Mhz
e 4Ghz.

## Modulação

É a tranformação de uma onda digital em analógica.

Contrário: demodulação.

Utilizada quando o canal de comunicação é analógico.
Exemplo: Ondas de rádio para transmissão no ar.

### Modulação em amplitude (AM/ASK)

Sinal possui **duas amplitudes**, uma para o bit 0 e outra para o bit 1.

0: baixa amplitude.
1: alta amplitude.

A **frequência permanece constante**, muda somente a amplitude.

Exemplo:
* Rádios AM.

ASK := AM + portadora.

O recepetor precisa ver aonde está a crista da onda para ver se o bit é 0 ou 1.

### Modulação em frequência (FM/FSK)

Sinal possui **duas frequências**, uma para o bit 0 e outra para o bit 1.

0: baixa frequência.
1: alta frequência.

A **amplitude permanece constante**, muda somente a frequência.

Exemplo:
* Rádios FM.

FSK := FM + portadora.

Receptor precisa "contar" oscilações da onda para ver se o bit é 0 ou 1.

### Modulação em fase (PM/PSK)

Sinal possui **duas fases**, uma para o bit 0 e outra para o bit 1.

0: começa indo para cima.
1: começa indo para baixo.

**Amplitude e frequência permanecem constantes**.

Receptor precisa ver o "sentido" da onda para ver se o bit é 0 ou 1.

PSK := PM + portadora.

### Modulação em quadratura de amplitude (QAM)

QAM-4 := 4 amplitudes diferentes ou 2 amplitudes e 2 fases.
* transmite 2 bits de uma vez.

QAM-8 := 4 amplitudes + 2 fases.
* transmite 3 bits de uma vez.

QAM-...

Até onde podemos ir?
R: QAM-256.
* transmite 8 bits de uma vez.

Desvantagens do QAM-256:
* pontos das constalações ficam cada vez mais juntos, maior chance de erro.
* hardware cada vez mais sensível e caro.

Vantagens do QAM-256:
* correção de erro implícita e automática.

## Codificação

Codifica uma onda digital utilizada dentro do computador em uma onda digital
transmitida fora do computador.

Utilizada quando o meio de transmissão é digital.
Exemplo: Rede Ethernet.

Deve garantir a transmissão de dados e o sincronismo entre origem e destino.

Polaridade:
* Toda "onda" possui o repouso.
* Unipolar: um polo, além do repouso.
* Bipolar: dois polos, além do repouso.

### Return-to-zero (RZ)

Bipolar.
* 1: pulso para cima.
* 0: pulso para baixo.
* sempre volta ao repouso antes do fim do intervalo.

### Taxa de transmissão x Taxa de sinalização

Taxa de sinalização indica quantas oscilações o sinal pode efetuar por segundo.

Quem define a quantidade de dados que pode ser enviada é a taxa de sinalização.

Fazer a onda oscilar mais vezes do que o suportado pelo fio pode fazer o fio
esquentar e romper.

RZ: precisa de duas oscilações/bit.

### Non-return-to-zero (NRZ)

Unipolar:
* 1: pulso para cima.
* 0: pulso para baixo.

Pior caso NRZ: 01010101...010101.

Problema do NRZ:
Quando não tem oscilações. Dessa forma, não tem como sincronizar.
Uma vez perdido o sincronismo, deve-se reiniciar a transmissão.

O NRZ é ideal para:
* Transmissões a curta distência.
* Transmissões nas quais o sincronismo não é problema.

### NRZ Invert on ones (NRZ-I)

1: inverte a linha.
0: fica onde está.

Pior caso do NRZ-I: sequência de 1's.

Problema do NRZ-I:
* sequência de 0's (perde o sincronismo).

Solução:
* a cada 4 bits incluir um bit 1 na transmissão.
* receptor deve retirar o bit 1 adicional.
* para transmitir 8 bits de dados, deve-se transmitir 10 bits na linha.

### Manchester

Unipolar:
* 1: transmição para cima no meio do intervalo.
* 0: para baixo no meio do intervalo.

Pior caso: sequência de 1s ou de 0s (precisa de duas transições para cada bit).

2 oscilações/bit.

Vantagem: código auto-sincronizável.
Desvantagem: transição no meio do intervalo carrega dados e sincronismo.

Uso: Ethernet.

### Manchester diferencial

Unipolar:
* sempre tem transição no meio do intervalo.
* 1: ausência de transição no início do intervalo.
* 0: presença de transição no início do intervalo.

Pior caso: sequência de 0s (precisa de 2 oscilações/bit).

Vantagens:
* código auto-sincronizável.
* transição no meio carrega somente sincronismo.
* presença ou ausência de transição no início carrega os dados.

### Alternate Mark Invertion (AMI)

Bipolar:
* 1: alterna um pulso para cima com um pulso para baixo.
* 0: fica no repouso.

Pior caso: sequência de 1s (precisa de 2 oscilações/bit).

Problema: sequência de 0s (sem sincronismo).

Vantagem:
* deteção de erro (dois pulso pro mesmo sentido "seguidos" = erro).

Uso: telefone fixo.

## Duplexação

### Simplex

Comunicação em sentido único.

A -> B.

Exemplo:
* mouse do computador.
* rádio.

### Half-duplex (ou Semi-duplex)

Comunicação em ambas as direções, porém nunca ao mesmo tempo.

A -> B.
A <- B.

Exemplo:
* Walk-talk

### Duplex (ou Full-duplex)

Comunicação em ambas as direções.
Pode ser simultânea.

A <-> B.

O objetivo é criar um canal duplex.
Isso pode ser feito com dois simplex.
Mas, se isso não for possível, usamos duplexação.

### Duplexação

#### TDD (Time Division Duplexing)

Fatia o tempo do canal em slots.

Em um slot transmite e no outro recebe.

É necessário ter um tempo de guarda entre os slots, porque a inversão não é
instantânea.
Isso evita colisões.

Isso ocorre tão rápido que considera-se um duplex.

    frequência
    ^
    |        |        |        |        |        |
    |        |        |        |        |        |
    | A -> B | Guarda | B -> A | Guarda | A -> B | ...
    |        |        |        |        |        |
    |----------------- tempo --------------------->

#### FDD (Frequency Division Duplexing)

Divide a frequência disponível em duas.
Uma faixa de frequência transmite e outra recebe.

    frequência
    ^
    |
    |              A -> B
    |
    | ------------------------------------
    |              Guarda
    | -------------------------------------
    |
    |              B -> A
    |
    ---------------------------------------> tempo

#### TDD vs FDD

TDD:
* máquina transmite utilizando toda a banda pela "metade" do tempo.
* precisa de um intervalo de tempo (intervalo de guarda) entre transmissão e
  recepção para inverter o canal.
* redes sem fio (wifi).

FDD:
* máquina transmite utilizando "metade" da banda todo o tempo.
* precisa de duas antenas (uma para receber e uma para transmitir) ou de uma
  antena capaz de realizar as duas tarefas simunltâneas.
* telefonia celular.

## Multiplexação

Dividir um canal entre múltiplos usuários (computadores).

### TDM

Fatia o tempo do canal entre os usuários e entrega um slot para cada um.

Deve ter um tempo de guarda entre cada slot.

Cada máquina transmite por um tempo utilizando todo o canal.

Quadro := sequência de slots sem repetição.
Os quadros são repetidos indefinidamente no canal.

O que ocorre se um slot ficar vazio?
R: O slot pertence a máquina, se ela não quer transmitir, ele fica vazio.

#### TDM x TDMA

Time Division Multiplexing:
* diversas transmissões simultâneas.
* podem ser de uma mesma máquina.

Time Division Multiple Access:
* exatamente o mesmo conceito, mas aplicado a máquinas diferentes.
* exemplo: diversos celulares conversando com a torre ao mesmo tempo.

#### Melhorias

TDM com servidor:
* ao invés de ter slots fixos para as máquinas existe um servidor na rede.
* máquinas devem pedir slots livres para o servidor.
* devem informar por quanto tempo vão utilizar o canal.
* servidor aloca slots enquanto tiver livre.
* pode alocar um ou mais slots para cada máquina.
* depois avisa que está ocupado e máquinas devem esperar.
* custo a mais por esse gerenciamento.

TDM com reserva:
* igual ao TDM com servidor, mas o servidor aceita reservas futuras.

### FDM

Fatia o tamanho do canal (a faixa de frequência) em bandas menores (canais
menores) e entrega um canal pequeno para cada usuário.

Deve ter uma frequência guarda entre cada canal.

Cada máquina transmite todo o tempo utilizanod um canal pequina (velocidade
reduzida).

#### FDM x FDMA

Frequency Division Multiplexing:
* diversas transmissões simultâneas.
* podem ser de uma mesma máquina.

Frequency Division Multiple Access:
* exatamente o mesmo conceito, mas aplicado a máquinas diferentes.

### CDM (Multiplexação por Divisão de Código)

Baseada nas funções de Walsh.
Cada máquina recebe uma função de Walsh para utilizar.
A função define o formato da onde que a máquina deve transmitir.

Todas as máquinas transmitem utilizando o canal todo, todo o tempo.
Elas se somam, mas é possível que o receptor separe as ondas e identifique cada
sinal.
O receptor faz meio que um casamento de padrões.

Permite 32 transmissões simultâneas.

#### Problemas

1. Para funciona, todas as máquinas devem iniciar a transmissão de cada bit ao
   mesmo tempo (transmissões sincronizadas).
2. Tempo para transmitir um bit é maior que em TDM e FDM.

#### CDM x CDMA

Code Division Multiplexing:
* diversas transmissões simultâneas.
* podem ser de uma mesma máquina.

Code Division Multiple Access:
* exatamente o mesmo conceito.
* mas aplicado a máquinas diferentes.

### OFMD (Multiplexação por Divisão Ortogonal de Frequência)

Parecida com FDM.
Mas os sub-canais são sobrepostos.
Resultando em um número maior de sub-canais maiores.

O pico de cada frequência é único e o resto pode se sobrepor.

Uma das multiplexações mais modernas e mais avançadas utilizadas hoje em dia.
Utilizado em todas as redes sem fio.

### WDM (Multiplexação por Divisão de Comprimento de Onda)

Similar ao FDM.
Mas ao invés de utilizar a frequência como parâmetro para a divisão, utiliza o
comprimento de onda.

Utilizado em fibra ótica.

## Duplexação x Multiplexação

Duplexação:
* permite enviar e receber em um canal.
* somente duas máquinas conectadas.

Multiplexação:
* permite muitas máquinas compartilharem o canal.
* somente em sentido.

É possível unir as duas técnicas?
Ou seja, multiplas máquinas dividindo um canal com transmissão bidirecional.
R: Sim.

### TDD + TDM

Dois grupos: upload e download.
Um slot para cada máquina fazer upload.
Um slot para cada máquina fazer download.

Quadro := parte de upload + download.

### TDD + FDM

Dividir a frequência para cada máquina e, dentro de cada frequência, dividir em
slots de tempo para upload e download.

### FDD + TDM

Duas frequências: uma para upload e outra para download.
Um slot de tempo para cada máquina.

Mais utilizado.

### FDD + FDM

Dois frequências: upload e download.
Dentro de cada frequência, uma frequência para cada máquina.

### Existem mais possibilidades

CDM + TDD.
CDM + FDD.
OFDM + TDD.
OFDM + FDD.

## Enlace

Responsável pela comunicação de duas máquinas diretamente conectadas.

Dividido em duas sub-camadas:
1.  Controle de Acesso ao Meio (MAC)

    Responsável por definir quais máquinas e por quanto tempo podem utilizar o
    meio de transmissão.

2.  Controle Lógico de Enlace (LLC)

    Possui 4 funções:

    1. Enquadramento
    2. Sequencialização
    3. Detecção de erros
    4. Controle de fluxo

### MAC

Definir quais máquinas podem usar o meio.

Definir por quanto tempo as máquinas podem usar o meio.

Similar as técnicas de multiplexação, mas multiplexação precisa de uma
pré-definição ou de um servidor definindo a forma de uso do
tempo/frequência/código.

Aqui, a ideia é fazer tudo distribuído, sem nenhum servidor ou pré-definição.

#### Aloha

Utilizado em uma das primeiras redes criadas no mundo (AlohaNet).

AlohaNet:
* Foi criado em 1970.
* Rede sem fio criada pela universidade do Havaí.
* uma das primeiras redes do mundo.
* primeira rede sem fio do mundo.
* interligava os campus da universidade do Havaí.

Simplesmente manda dados para quem quiser, quando quiser.

Problema: muitas colisões.

`~[10, 20]% de taxa de entrega.`

#### Aloha discreto (Slotted Aloha)

Tentativa de resolver o problema de colisões.

Máquinas devem ouvir antes de transmitir.

Se tiver uma transimissão em andamento:
	Máquina deve esperar a transmissão acabar.
Senão: # canal está livre
	Pode transmitir.

`~[30, 40]% de taxa de entrega.`

#### CSMA (Carrier Sense Multiple Access)

Adaptação do Aloha discreto para ser utilizado no fio (barramento).
Rede em broadcast.

Basicamente é um Aloha discreto portado para dentro do fio.

#### CSMA não persistente.

Caso o canal esteja ocupada estabelece uma espera aleatória para tentar
novamente.

Intervalo de espera é um valor inteiro entre (0, 2^n-1], onde n é o número de
tentativas de transmitir esta mensagem.
Depois de n = 11, n fica constante.
n = 16 significa pânico na rede, desiste da transmissão.

#### CSMA/CD

Baseado no CSMA não-persistente.
**Com Collision Detection.**

Enquanto transmite fica ouvindo o canal.
Se detectar colisão, para de transmitir.
A colisão sempre acontece no início da transmissão.
Depois que o cabo está "cheio" não pode mais ocorrer colisão, pois máquinas
devem ouvir antes de transmitir.

Quando os bits colidem, eles fazem um OR e tudo vira uma sequência de 1s.
Uma máquina coloca 0 no fio e ouve 1. Logo, ela percebe que ouve uma colisão.

A colisão só pode ocorrer nos primeiros 64 bytes da transmissão.
Isso limita o tamanho da rede.
O sinal deve atingir toda a rede dentro do tempo necessário para transmitir 64
bytes.

Vantagens:
* Se der colisão, transmite somente 64 bytes e para.
* Não gasta tempo da rede transmitindo mensagens que vão ser jogadas fora.

`~92% de taxa de entrega.`

Utilizado na Ethernet.

#### CSMA/CA

Novas redes sem fio não conseguiram usar CSMA/CD, porque não da pra transmitir
e ouvir ao mesmo tempo.

Volta ao CSMA.
Problema da colisão volta.

Problema novo: problema do terminal oculto.

CSMA com Collision Avoidance.
Duas novas mensagens:
* RTS (Request-To-Send)
* CTS (Clear-To-Send)

Ordem:
1. RTS.
2. CTS.
3. Dados.
4. ACK.

Pode ocorrer colisão?
R: Sim, mas em mensagens RTS/CTS.

Vantagem: mensagens RTS e CTS são mensagens pequenas, perto de 64 bytes.

Desvantagem: precisa enviar 1 mensagem RTS e 1 CTS pelo menos para cada
mensagem de dados.

#### Passagem de bastão

Como uma corrida de revezamento.
O bastão vai passando pela rede.
Quem está com o bastão pode transmitir.
Se tudo funcionar corretamente, não tem colisão.

Duas formas de fazer a passagem de bastão:
1. Temporização do bastão.
	* Problema: perde tempo quando não está transmitindo.
2. Antecipação do bastão.
	* Problema: quando ninguém quer transmitir nada.

#### Resumo

Rede com fio: CSMA/CD.
Rede sem fio: CSMA/CA.
Anel: passagem de bastão.

Porque não usar CSMA/CA em redes com fio?
R: overhead do RTS/CTS.

### LLC

O LLC (Logical Link Control) possui 4 tarefas:
* Enquadramento
* Sequencialização
* Detecção de erros
* Controle de fluxo

#### Enquadramento

Em transmissões assíncronas, a origem deve sinalizar aonde começa e aonde
termina as mensagens.

Duas formas de fazer isso:
1. Delimitador de início + delimitador de fim.

Problema: e se o delimitador de fim aparecer dentro da mensagem?
Solução: se encontrar um 0 seguido por 5 1s, adiciona mais um 0. Quando
o receptor receber, ele remove esse 0.

2. Delimitador de início + campo de tamanho.

##### Comparativo

Delimitador de fim:
* Mensagens sem limite de tamanho.
* Deve procurar o delimitador de fim no meio da mensagem para alterá-lo.

vs

Campo tamanho:
* Mensagens com tamanho máximo (maior número que pode ser representado no campo
  tamanho).
* Pode ter qualquer sequência nos dados.

#### Sequencialização

Deve indicar a sequência correta que as mensagens saíram da origem.

Adiciona-se um campo na mensagem com um número sequencial de 0 a XX.
XX é o maior número que pode ser representado com a quantidade de bits do campo
tamanho.

Cuidado:
A sequência deve ser sem sinal (unsigned).
Quando estourar o contador, ele deve voltar a 0.
Por exemplo, se for de 8 bits, vai de 0 a 255.

#### Controle de Fluxo

Duas formas de fazer:
1. Para e espera
2. Janelas deslizantes

##### Para e espera.

Utilizado em redes semi-duplx.
Envia uma mensage, para e fica esperando uma resposta.
Envia uma mensagem de cada vez.

Os timeouts são sempre no lado do cliente.

##### Janelas deslizantes.

Utilizado em redes duplex.

Define uma "janela" com X mensagens.
Pode enviar até X mensagens sem receber nenhuma confirmação.

Aceitação coletiva:
Qualquer resposta (ACK ou NACK) da mensagem 4 (por exemplo) aceita todas as
mensagens transmitidas antes da 4.

As janelas podem mudar de tamanho.

Existem duas formas de implementar as janelas deslizantes:

1.  Volta-N.

    Quando há um NACK, retransmite toda a janela.

    Usado em redes nas quais você tem muito erro.

2.  Repetição seletiva.

    Quando há um NACK, retransmite só o que deu erro dentro da janela ou o que
    é novo na janela.

    Usado em redes nas quais você tem pouco erro.

#### Detecção de erros

Função: **descobrir** que ocorreu erro durante a transmissão.
SOMENTE DESCOBRIR.

##### Paridade horizontal

Adicionar um bit em cada linha indicando se a quantidade de 1s é par (ou
ímpar).

Faz um XOR bit a bit e o resultado é o que vai no campo de paridade.

Problema: inverte 2 bits na mesma linha.

##### Paridade vertical

Adicionar um bit em cada coluna indicando se a quantidade de 1s é par (ou
ímpar).

Problema: inverte 2 bits na mesma coluna.

##### Paridade longitudinal

Paridade horizontal + vertical.

Problema: inverte um quadrado 2x2.

Sobrecarga: 1 bit para cada byte + 1 byte.

##### Checksum

1. Soma os dados.
2. Trunca o resultado para 8 bits.
3. A checksum é o que falta pra número do item 2 dar 0.

O checksum é o complemento da soma dos dados.

Manda no final da mensagem o checksum e checa se deu 0 a soma do checksum com
a mensagem.

Sobrecarga: 1 byte.

##### CRC (Ciclic Redundant Code)

Mensagem original + polinômio gerador.

Exemplo:
$$x³ + 1$$

$$1x³ + 0x² + 0x + 1$$

1 0 0 1 (número que você vai usar para dividir a mensagem truncada).

O resto é somado na mensagem truncada.

No receptor:
O resto da divisão tem que ser 0.

Sobrecarga: n bits adicionais (n é o grau do nosso polinômio).

## Ethernet

## WiFi

IEEE 802.11.

Tecnologia mais madura para comunicações sem fio.

Padrão para WLAN.

Todas as placas de rede do padrão 802.11 podem ser configuradas em um dos dois
modos de operação. Nunca nos dois ao mesmo tempo.

Em modalidade **com infraestrutura**, a placa se comunica sempre com um **ponto
de acesso**.

Em modalidade **sem infraestrutura** (ad hoc), a placa se comunica sempre com
**outras unidades**.

1.  com infraestrutura.
    *   Pontos de acesso (access points).
    *   Todas as comunicações devem passar pelo ponto de acesso.
    *   Todas as unidades de rede devem estar dentro do raio de alcance do
        ponto de acesso.

2.  sem infraestrutura (ou ad hoc).
    *   Chamadas de redes ad hoc.
    *   Unidades se comunicam diretamente umas com as outras.
    *   Não existe ponto de acesso ou outro tipo de unidade centralizadora.
    *   Unidades devem servir de roteadores para as mensagens das outras
        unidades.

MAC: CSMA/CA

Entrega de dados confiável. O mecanismo de transferência de dados básico
envolve uma troca de dois ou quatro quadros (dados, ACK, CTS/RTS opcionais).

Controle de acesso: DFWMAC (Distributed Foundation Wireless MAC).

WPA2: usa AES.

### VANETs

Redes para veículos.

### DTN

Redes tolerantes a atrasos e desconexões.

Exemplo: redes interplanetárias.

## Bluetooth

802.15

Tecnologia para conexões sem fio de curta distância.

As transmissões por frequências de rádio.

Raio de 10m (novas especificações permitem raio de alcance de 100m).

*   Robustez

    *   Deve ser capaz de operar em locais com muita interferência.

    *   Utiliza a frequência de 2.4 GHz.

    *   Para minimizar os problemas de interferências, o Bluetooth usa uma
        técnica chamada de **salto de frequência**.

    *   Salto de frequência: cada máquina fica pulando de frequência após
        determinado tempo.

*   Baixa complexidade
*   Baixo consumo de energia
*   Baixo custo

O Bluetooth pode operar em dois estados:
* Standby
* Connection

    *   4 possíveis modos de conexão:
        * active: transmitindo ou recebendo.
        * sniff: monitorando as transmissões ao seu redor.
        * hold: nenhuma comunicação (usado para economizar bateria).
        * park: fazendo parte da rede, mas não participa do tráfego dos dados. E
          não pode transmitir ou receber dados.

    *   Quando dois dispositivos bluetooth estabelecem uma conexão, eles criam
        uma rede pessoal chamada **piconet**.

    *   Piconet:

        *   Pode conter até oito dispositivos diferentes (1 mestre e 7 escravos).

        *   Dispositivos individuais podem pertencer a mais de uma piconet
            simultaneamente.

        *   O mestre estabele os parâmetros físicos (como será o salto de
            frequências).

        *   Os escravos devem sincronizar seus clocks com o mestre (os saltos de
            frequência devem ser efetuados ao mesmo tempo)

        *   O MAC é realizado pelo TDMA (coordenado pelo mestre).

    *   Scatternet
        
        *   Uma rede que é um conjunto de piconets.

        *   Pode conter no máximo 10 piconets (80 dispositivos).

        *   Todas as comunicações entre piconets são realizadas através dos
            mestres.

## xDSL
## Cable Modem

## Referências

* <https://www.youtube.com/watch?v=-Us6uoQVHOI&list=PLM3N1gr3bmJaVBCbTtdZrK91GyzSsnwlV&index=1>

