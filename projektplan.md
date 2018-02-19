# Projektplan Elektromekaniskt projekt 2018
5 hp projektarbete som del av civilingenjörsutbildningen i Teknisk fysik, inriktning inbyggda system.
## Deltagare
* Jacob Olofsson
* Thomas Danielsson
* Aksel Wennström

## Design
I detta projekt ska vi konstruera en robot som kan spela othello mot en mänsklig motståndare. Vi har delat upp designen i tre stycken huvuddelar:
* Spelplan med sensor
* Mjukvara / AI
* Mekanisk förflyttning av pjäser
AI:t ska kunna få information om hur spelplanen ser ut, beräkna ett drag och sedan skicka information om hur draget ska utföras till en robotarm eller annan mekanism som utför draget på den fysiska spelplanen.

### Minsta fungerade produkt (MVP)
Den minsta fungerande produkten vi har är en robot som kan läsa av spelplanen och göra ett slumpmässigt "lagligt" drag.
* Roboten matas med pjäs av rätt färg från mänsklig spelare
* AI:t spelar alltid som samma färg
* AI:t har inget minne
* AI:t utför ett drag efter extern input
* Mäsnkliga motståndaren måste vända på pjäserna som påverkas av draget
* Statuslampor visar i vilket state programmet är
* Mekanismen ska gå att nollställas efter utfört eller misslyckat drag

### Funktioner att lägga till efter MVP
Eftert den minsta fungerande produkten har implementerats kommer följande funktioner läggas till:
1. Det går att välja färg på AI:t
1. Efter AI:t utfört ett drag vänder den alla pjäser som blir påverkade av draget
1. Mekanismen plockar upp nya pjäser utan mänsklig hjälp
1. AI:n gör "smarta" drag
1. Möjlighet att ställa in betänketid och/eller svårighetsgrad
1. Display som kan visa mer information om state
1. AI:t känner av när motståndaren utfört sitt drag och fortsätter automatiskt med sitt drag utan input
1. Möjlighet att ha två olika AI:n som spelar mot varandra
1. Bygga två stycken robotar som spelar mot varandra

### Designval
Följande val behöver göras för designen:

#### Spelplan med sensor
* Vilken storlek ska planen ha och hur många rutor ska det vara?
* Vilken sorts sensor ska användas för att känna av färg/position av pjäser?
  * Kamera - Beräkna ljusstyrkan i varje punkt där en pjäs kan befinna sig
  * Fotosensor under planen - Planen görs i genomskinligt material med en fotosensor under
  * Elektrisk / magnetisk krets - Konstruera pjäserna så att de sluter olika kretsar när de placeras på planen

Slutsats: Standard Othello spelas på 8x8. Varje ruta bör vara 3x3cm med en avskiljare som är max 1cm hög för att en mmänsklig spelare enkelt ska kunna lyfta upp och placera pjäser. Vi väljer att sätta linjära halleffektsensorer under planen och sätta magneter i spelpjäserna. Med en halleffektsensor går det att känna av riktning på magneten eller om en ruta är tom.
#### Förflyttning av pjäser
* Hur ska pjäserna plockas upp?
  * Elktromagnet
  * Gripklo
* Hur ska "upplockaren" röra sig?
  * I en bana  (som på en 3D-skrivare)
  * En arm med leder
* Hur ska pjäserna byta färg?
  * Vända på pjäserna?
  * Byta ut pjäserna?
* Vilken sorts motor ska användas?

Slutsats: Eftersom pjäserna har en magnet är det praktiskt att använda en elektromagnet för att plocka upp pjäserna. För att ha så få frihetsgrader i systemet som möjligt används en bana likt i en 3D-skrivare. För enkelhet i en första design kommer roboten varken att vända eller byta ut pjäser, detta lämnas till människospelaren. Stegmoterer bör användas för förflyttningen i sidled och en servomotor för upplockandet av pjäser.

#### Mjukvara
* Hur ska programmet struktureras?
  * Hur ska interfacen mellan de olika programdelarna se ut?
* Vilken AI-algorithm ska användas?
  * Monte Carlo
  * Min/max

Slutsats: TBD

#### Hårdvara
Valet av processor kommer att styras i stor del av hur mjukvaran, sensorn och hur förflyttningen och spelplanen designas.
* Vad för sorts processor ska användas?
  * IC
  * Arduino
  * Raspberry Pi
  * Dator
* Ska samma processor användas för AI, sensor och förflyttning eller ska separata processorer som kommunicerar med varandra användas?
* Strömkälla

Slutsats: För enkelhet bör samma processor användas till allt då det finns lite att tjäna i att ha separat hårdvara för de olika delarna. Minst 8 analoga ingångar och 12 digitala utgångar krävs för designen och en någorlunda snabb processor. Förslagsvis en arduino Mega. Separata strömkällor bör användas till motorer och arduino för att minska störningar.

## Skiss
Spelplanen är uppdelad i 8x8 rutor med en avskiljare mellan varandra för att pjäserna inte ska kunna glida över till rutan brevid. Spelplanen är kopplad till en arduinos 8 digital out, 8 Analog in för multiplex av 64 sensorer. Spelplanen har även 1 digital in för knappen samt 4 digital out för LED kopplad till arduinon för I/O.
<img src="./Ritningar/spelbräde.svg">

### Vändningsarmen
Vändnings armen är en servo motor kopplad till två metal armar vartpå det sitter en electromagnet. Denna mekanism är det som kommer att plocka up och lägga ner pjäserna för Othello spelet. 
<img src="./Ritningar/lever.svg">

## Delar
| Del | Benämning | Antal | Butik 1 | Butik 2 |
| --- | --------- | ----- | ------- | ------- |
| 0.1 | Arduino Mega | 1st | x | y |
| 1.1 | Linjär Halleffektsensor | 64st | https://www.electrokit.com/allegro-a1301-sip3-halleffektsensor-2-5mv-g.52933 | y |
| 1.2 | Kopplingskabel | 16x220cm | x | y |
| 1.3 | Avdelare, plastglas | 14x24cm | x | y |
| 1.4 | Plan, plastglas | 33x33cm | x | y |
| 1.5 | Spelpjäs, trä | 1x45x45cm | x | y | 
| 1.6 | Neodymmagnet | 64st | x | y |
| 1.7 | LED | 4st | x | y |
| 1.8 | Knapp | 1st | x | y |
| 1.9 | Gummifötter | 4st | x | y |
| 3.1 | Servo, aurdino controlled | 1st | https://www.elfa.se/sv/servomotor-180-vdc-parallax-900-00005/p/17319437?q=servo&page=2&origPos=2&origPageSize=50&simi=99.63 | https://www.m.nu/servo-motorer-robotics/micro-servo-sg92r | 
| 3.2 | Metal rod | 1st | x | y | 
| 3.3 | Electromagnet | 1st | x | y |
| 3.4 | Screw for micro servo SG92R | 2st | x | y |



## Tidsplan

