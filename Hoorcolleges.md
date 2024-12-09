# ?!EX?!

(ethernet)frame(s)

# Hoogcolleges

## H1 fysieke laag

### 1.1 signalen, frequentiespectra, bandbreedtes

### 1.2 Transmissiemedia

fysieke laag (verstuurder) -> kabel of lucht -> fysieke laag (ontvanger)

type bepaalt max capaciteit

!Logaritmische schaal!

- UTP: 1...+-600MHz
- COAX: 800MHz
- Fiber: +0,2dB/km

ISI => inter symbool interferentie

### 1.3 Datatransmissie

Parallelle of seriele transmissie
PATA -> SATA (door Xtalk)

Seriele transmissie

- bits na elkaar
  + geen kloklijn (i.t.t. bv I2C)

Probleem synchronisatie

1. BIT
2. BYTE
3. FRAME

asynchrone transmissie

- RS232, RS422/485 
- 1 karakter/byte per 'toetsaanslag' -> + start- & stopbit
- synchronisatie per byte: "startflank"
- geen bitsync -> bits samplen +- in midden
- framesynchronisatie: ASCII -> STX & ETX

synchrone transmissie

- hogere snelheden -> geen start-/stopbit
- continue bitstroom
- AANDACHTSPUNTEN!
  + Bandbreedte
  + DC wander
  + Synchronisatie(bitsync)
    * bitsync via lijncodes
- Bandbreedte LAN != WAN
- DC wander (=baseline wander)
  + medium = 'C' (capaciteit) ->voor alle DC -> C 'laadt op' -> baseline vergroot
- bitsync
  + LAN: klokcodering
  + WAN: DPLL
  + ... high speed LAN -> DPLL
    * met respect voor Bandbreedte

LAN-bitsynchronisatie

- klokcodering
- principe: DATA + clock
- voor alle pulse/flank -> bitsync
- DC wander = +-0
- 3 niveaus (+/-) -> manchester
- manchester code
  + voor alle flank midden bittijd

WAN-bitsynchronisatie

- Bandbreedte minder
- DC wander != +-0
- bitsync
  + DPLL

AMI code (Alternate Mark Inversion)

- B: f/N (f/R) < 1
- DC wander = 0
- bitsync +- +-

B8Z code (Bipolar (=AMI) with 8 Zero Suppression)

- B: f/N (f/R) < 1
- DC wander = 0
- bitsync +- (beter)

HDB3 code (B8ZS + 4x 0 supp. (-> + "000V"))

- B: f/N (f/R) < 1
- DC wander = 0...
- bitsync ok (nog beter)

Baudreducerende codes

- 2B1Q code
  
  - betere bandbreedte benutting
  - bitrate > baudrate
  - 2B1Q: 2 bits = 1 quat.symbool
  - B: f/N (f/R) << 1
  - DC wander = +- +- 0
  - bitsync +-

- 4B3T code
  
  - R > Rs
  - 4B3T: 4 bits = 3 Tern. symbool
  - 2^4 < 3^3 -> NIET +++, ---, 000, ...
  - B: f/N (f/R) << 1
  - DC wander = +- 0
  - bitsync beter

DPLL: bit-synchronisatie

- voldoende flanken
- corrigeren na flank

byte-synchronisatie: byte-georienteerde prot.

1. bitsync: ... Hunt: -> SYN ->
2. bytesync: ... -> STX/ETX ->
3. frame-sync

byte/frame-sync: bit-georienteerde prot.

- 3-soorten:
  + P2P
    * zender kan blijven sturen: idle byte 0x7F + flag byte 0x7E
    * (transparantie door bitstuffing)
  + MAC: bits tellen
    * shared -> interframe gap -> preamble = 0xAA
    * SOF= 0xAB = "10101011"
    * def. HEADER: adres + lengte bytes tellen = EOF
  + MAC: violations
    * bit encoding violations (J en K)

***EX! bitstream krijgen en zeggen wat de code is / maken***

### 1.4 Interface standards

ethernet

#### 1.4.1 Ethernet 10 base

ethernet 10 base 2 en 10 base 5: bus topologien

- coax dun/dik
- *shared medium* -> voor alle transmissies -> 1 zender

ethernet 10 base T: ster topologie -> UTP

- hub = eca-repeater -> *shared medium* -> voor alle transmissies -> 1 zender
  
  + = 1 ethernet 'segment'

- 2 paren
  
  + 10Bbps
  + baseband = manchester coding
  + CAT3(16MHz) & CAT5(100MHz), 100m max
  + next canceller
  + hub = repeater
    * HALF duplex!
  
  frame-formaat:
  
  - preamble
  - SFD
  - data-length
  - IEEE 802.3

#### 1.4.2 Fast ethernet

100 base T(f) = vervolg 10 base, ... 'compatible'

- = frame!
- = MAC!
- = bekabeling ...?
  + != coax!
  + = UTP! => 100 base TX op CAT5, niet manchester! (en 100 base T4 op CAT3)
  + = fiber! => 100 base TX

100 base T4

- CAT3 => 16MHz!
- 4 paren, 3+1
- powersum next
- 100/3 = 33Mbps
- 8B6T => 25Mbaud
- 256 element van 729 codes
- kloksync+DC-bal

UTP(100 base TX) en FIBER(100 base FX)

- voor alle 2 paren UTP (>=CAT5) of fibers
- lijncode: **4B5B** !! boud**IN**ducerende code

FIBER(100 base FX) en UTP(100 base TX)

- 4B5B lijncode
  + 2^4<2^5: 16 element van 32 codes => kloksync+DC-bal
  + 4B5B: 100*5/4 = 125Mbaud = boudINducerend
  + 100 baseFX -> NRZI: 1=transitie, 0=niks
    * 16 element van 32: sync -> transitie / 2bits, max 2x '0'... +-

#### 1.4.3 Gigabit ethernet

1000 base = 10x, vervolg 100 (en 10) base -> idem frame, CSMA/CD, structuur!
implementaties;

- 2-wire(-X) of 4-wire

fiber: 2-wire...-X

- 1000 base SX: 850nm op MMF (275m tot 550m)
- 1000 base LX: 1310nm (550m MMF tot 5km SMF)

koper

- 1000 base-CX: korte afstand, STP nu meer 1000baseT
- 4-w: 1000 bast T => IEEE802.3ab op CAT5 => CAT5e

fiber: signaalcode = 8B/10B ~ 4B/5B

- 8B/10B = baud**IN**ducerend => +25% = 1,25Gbps
- zelfs /4 onmogelijk op UTP van 100MHz, CAT5
  + 1000baseT (niet –X op CAT5, -X op fiber of CAT6/7)

koper: CAT5e / CAT6

- 4 paren FDx = 8 transmitters en receivers
- full duplex: hybrid

signaalcode 1000bastT: 4D-PAM5

- 4 paren => 4 **D**imensies
- PAM5 => 5 levels, 2 bits/level
- 2*4 = 8bit/symb => 256 mogelijkheden
- mogelijke symbolen: 4 paren, 5 levels => 5^4 = 625
  + 256 element van 625 => code redundancy
    * trellis / viterbi => zelfcorrigerende code
- symbol rate = 125 Mbaud
  + baudrate ~ 100baseTX! (alleen veel complexte symbool)

#### 1.4.4 10Gbase

10Gbase = 10x, vervolg 1000 (en 100/10) base

- idem frame, structuur, NIET CSMA/CD
- Link naar WAN !! (SONET, SDH)
  + 2 'subopties': 'R' of 'W'
  + R versies -> op 'dark fiber'
  + W versies -> WAN (SDH)
- 64B/66B codering

SONET = Synchronous Optical Networking
SDH = Synchronous Digital Hiearchy

4 versies, op fiber:

- 10GBASE-S
- 10GBASE-l
- 10GBASE-E
- 10GBASE-LX4

2 versies, op koper:

- 10GBASE-GR
- 10GBASE-T

#### 1.4.6 RS232

oud/veel gebruikt

- 'unbalanced' signalen V 3...18V
- A-synchrone transmissie
  + niet bitsync
  + /byte: start + stopbit
  + NRZ-L code
  + max 19200bps
  + max +- 15m
  + 'common ground'
- ook 20mA current loop, niet standaard
  + = UART, U -> I, evt. Optocouplers
  + sensor/actuator bus

#### 1.4.7 RS422/RS485

RS422 = P2P, RS485 = multidrop
op TP: differential (= 'balanced')

- hogere afstand en datarate
  rec: enkel verschilspanning CMRR hoger
  + afsluiten op kar. imp. => x 10 (I * rate)
    Spanningen: T => +- 2...6V, R => +- 0,2...6V
    Full duplex

RS458 2w: multidrop => 'enable' control

- L2: token passing

RS485 4w: 1 master, # slaves

vuistregel afsluiting: tp << bittijd

- sampling midden bittijd => reflecties weg

bv. 4000ft = 1200m

- 1 'trip' = 1200m / (0,66 * 3 * 10^8) = 6us
- 3 trips = 18us
- 9600 baud: 1/2bit = 1/9600/2 = 52us
- 115200 baud: 1/2bit = 1/115200/2 = 4,3us << 18us ... => afsluiten

## H2 Datalink

### Errordetectie

### Foutcontrole

### Continuee RQ

## H2 Flow control

### Transparantie

verschil kunnen zien tussen controlbytes en databytes

## H2 L2-protocollen: LLC

LAN: shared medium => L2 opgesplitst in 2 delllagen

- DLP = LLC = 802.2

- MAC lag = 802.3

- LAN: DLP = LLC (802.2)

- OSI L2: LLC + MAC (802.3, -4, -5) (MAC = afh. van L1)

- voor alle MAC: standaard services aan LLC

## H2 protocollen: LLC

2 types:

- CL-unack

- CO-ack (CL-ack)

CL: minimaal, BER lager => err/flow: L4

werking:

- Dest. & source addr: 1 byte != MAC, = LLC-SAP

- geen CRC => MAC laag

- control: I, U of S, 1 byte

## H2 MAC protocollen

Logische topologie

- ring
  
  - P2P tss DTE
  
  - 'medium' = ring

- bus
  
  - voor alle DTE 'hangen' aan
  
  - = kabel = 'medium'

beiden -> MAC: medium delen

fysieke topolofie = ster

logische topologie = bus

(hub = bus (of ring) in sterpunt)

als sterpunt = switch -> logisch ster netwerk

- fysieke topologie = logische topologie

- GEEN CSMA/CD, wel P2P netwerk

Historiek:

- aloha: geen MAC

- slotted aloha: tijdsloten -> TDM

- collision avoidance: res.kanaal

- collision detection + carrieer sensing

## MAC: CSMA/CD op bus

## H2.5 Ethernet

## H3 De netwerklaag



**IP adressering belangrijk voor schriftelijk ex**

**IP adressering VLSM (oef 7) voor ex**
