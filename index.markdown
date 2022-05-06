---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
nav_order: 1
---

# Algemeen



## Inhoud
 
- [Algemeen](#algemeen)
  - [Inhoud](#inhoud)
  - [Inleiding](#inleiding)
  - [Flow chart](#Flow-chart)
  - [Rolverdeling](#Rolverdeling)
  - [Detectie personen](#Detectie-personen)
  - [Gasmetingen](#Gasmetingen)
  - [Werking](#Werking)
  - [Microcontroller](#Microcontroller)
  - [Gebruikte sensoren](#Gebruikte-sensoren)
  - [PCB](#PCB)
  - [Communicatie](#Communicatie)

## Inleiding

In ets- en soldeerlokalen kunnen tijdens de werkprocessen volatile organic compounds (VOC) vrijkomen, dit zijn onder andere componenten van brandstoffen of oplosmiddelen. Deze kunnen schadelijk zijn voor de gezondheid bij een te hoge concentratie ervan in de lucht. Het is dan ook belangrijk deze gassen in de gaten te houden en een waarschuwing te verzenden wanneer bepaalde waarden overschreden worden, zeker wanneer personen aanwezig zijn in de vervuilde ruimte.

## Flow chart
![flow chart](flow.PNG)

## Rolverdeling

|                 | Technisch             | niet technisch            |
|-----------------|-----------------------|---------------------------|
| Roeland Osaer   | Software sensor       | Verantwoordelijke verslag |
| Mathieu Martens | Hardware schema       | Presentatie               |
| Viktor Morre    | Software communicatie | Communicatie groepen      |
| Glen Smet       | Hardware PCB design   | Project manager           |


## Detectie personen
Aangezien het belangrijk is te weten of er personen in de ruimte zijn of niet wordt dit als eerste besproken. Zo mag er indien er niemand gedetecteerd in de ruimte in kwestie wordt minder frequent gemeten worden maar moet er nog steeds een melding vertsuurd worden als de treshold overschreden wordt.
Indien er wel een persoon gezien wordt, moet er frequenter gemeten worden en moet er lokaal een alarm afgegaan bovenop op de melding. De threshold en frequenties van meten zijn opgenomen in het blokschema. Dit was het eerste concept van de IOT node. Maar na het testen en verdiepen in de gekozen gassensor hebben we ontdekt dat er geen mogelijkheid is om een IAQ meting te doen op een ander interval dan 5m. De detectie zal dus vooral belangrijk zijn om de buzzer te laten afgaan indien iemand zich in de ruimte bevindt en deze aan te manen de ruimte te verluchten of even te verlaten tot de concentratie van de VOC's gedaald is.

## Gasmetingen
De gasmetingen worden vanzelfsprekend gedaan door de lucht in de ruimte te meten. Op basis daarvan wordt beslist of deze niet al dan niet vervuild is, of er VOC's aanwezig zijn en in welke concentratie. Bij een goede waarde hoeft er niet meteen iets te gebeuren en kan er terug in sleep mode gegaan worden, de gemeten waarde kan eventueel opgeslagen worden of verstuurd naar een centrale eenheid. Wanneer de waarden te hoog worden bevonden wordt een geluidssignaal afgespeeld (enkel indien er beweging was gedetecteerd) en onmiddellijk een update verzonden naar de database.

## Werking
De architectuur bestaande uit een ESP32-C3-WROOM-02-N4 controller met daarop een bewegingssensor en gassensor die gebruikt zullen als volgt. Indien er beweging gedetecteerd wordt, zal een timer gestart worden van 5 min. Terwijl deze timer loopt kunnen we aannemen dat er iemand in de ruimte aanwezig is. Het doel van deze timer is om te vermijden dat er niet doorlopend geswitched wordt tussen aanwezigheid en afwezigheid van personen door het even niet detecteren van beweging. Er zal ook een tweede timer lopen van 5 min voor het correct uitlezen van de gassensor. De gassensor zal zich in ULP (Ultra Low Power modus) bevinden en om de 5 min samplen. Dit is de meest zuinige modus die ons toelaat om een accurate meting te doen. Indien de gemeten waarde lager is dan 125 zal de data worden opgeslagen en zal de esp32 terug in slaap gaan. Wanneer er 12 metingen zijn opgeslagen zal de data worden doorgestuurd. Dit zorgt ervoor dat in de databank elk uur een meting wordt ontvangen ongeacht of er beweging is of niet. Indien de waarde van 125 overschreden werd zal er meteen een alarm afgaan in de vorm van een buzzer alsook zal de data worden doorgestuurd zodat er een melding naar de gebruiker kan gaan. Alle timers zullen rtc gestuurd zijn op de esp 32. De minimum data dat wordt doorgestuurd is 9 bits aangezien de IAQ varieert tussen 0 en 500. De gemeten data wordt via een wifi module verzonden naar een centraal punt. Tussen de metingen zal de ESP zich in sleep mode bevinden. Hij wordt wakker gemaakt door een periodiek tijdsignaal of door een interrupt indien een beweging geregistreerd wordt.

## Microcontroller
ESP32-C3-WROOM-02-N4: 
Uit onze eerste energie berekeningen volgt dat we de esp32 kunnen voeden met 5 AA batterijen zodat deze een autonomie heeft van 1 jaar.
Als microcontroller kiezen we ervoor om een low power esp 32 te gebruiken. Deze heeft als voordeel dat het verbruik in slaap aanzienlijk lager is. We kozen voor deze microcontroller omdat we er al mee gewerkt hadden zodat we hier niet te veel tijd mee verloren aangezien de deadlines redelijk strak zijn.

## Gebruikte sensoren

Als gassensor gebruiken we de [BME680](https://www.bosch-sensortec.com/media/boschsensortec/downloads/datasheets/bst-bme680-ds001.pdf) van Bosch.
Om de bewegingen in de ruimte te registreren gebruiken we de [EKMB1305112K](https://www.mouser.be/datasheet/2/315/bltn_eng_papirs-1365490.pdf), een PIR sensor van Panasonic.

## PCB 
Hieronder zijn de top en bottom views te zien van onze PCB. Centraal bevindt zich de esp32, rechts bevindt zich de BME680 (U4) en links bevindt zich de PIR sensor. De twee sensoren staan zo ver als mogelijk uit elkaar omdat de PIR sensor gevoelig is voor warmte en de BME680 redelijk wat warmte genereert. Naast deze zaken zijn ook enkele basis voorzieningen aanwezig zoals vele testpunten, een LDO, een test LED en knop en extra pinheaders om een BME680 module te kunnen bevestigen (dit is voor in het geval dat we er niet in slagen om de BME680 aan onze PCB te solderen). 
![Top view PCB](Top.png)
![Bottom view PCB](Bottom.png)

## Communicatie
Als communicatieprotocol werd er gekozen voor MQTT, enerzijds omdat dit ook als low power protocol beschouwd wordt, doordat de hoeveelheid data die verzonden wordt klein (kleine code footprint en kleine message size) is. Er wordt ook enkel geluisterd naar kanalen die voor elke specifieke node van toepassing zijn, waardoor het ook gemakkelijk te implementeren is samen met de nodes van de andere groepen. Er kan eventueel nog gekeken worden om de MQTT-SN te gebruiken, dit is een variant van MQTT die de efficiëntie nog verbeterd bij beide de transmissie en energieverbruik.

## Software
Alle software wordt geschreven in cpp op het arduino framework via Plafrom IO in Visual studio code. Voor het project is de BME680 onze hoofd component en bepaald de voledige software flow. Om de sensor in ULP (ultra low power) te laten werken is het namelijk verplicht om een meting te doen elke 5 min. Enkel en allen op deze manier kan de bibliotheek die we gebruiken een AIQ (indoor air quality) cijfer berkenen. Dit is een cijfer tussen 0 en 500 die de qualiteit van de lucht bepaald. Een tabel is te zien in figuur....Tussen de metingen kan de sensor in deepsleep. Hiervoor moet telkens alle gegevens opgeslaan worden in een stuk geheugen die blijvend is tijdens een deepsleep net voor hij in deep sleep gaat om dan net na hij uit deepsleep gaat weer opgehaald word om een nieuwe meting te doen. De IOT node kan ook op 2 andere manieren uit deep_sleep gehaald worden. Via een drukknop en via de PIR-sensor. De eerste wordt gebruikt voor een manuele doorsturing van de laatste 12 metingen. Terwijl de tweede de detectie van een persoon doet. Omdat via deze twee interupts de ESP uit deep_sleep gehaald wordt en zo de timer die gezet is na een meting gereset wordt, moet de ESP er voor zorgen dat na de interupt de ESP weer in slaap gaat voor exact de juist tijd zodat de 5 minuten tijd tussen twee metingen behouden wordt. De manier waarop dit gedaan wordt, wordt uitgelegd aan de hand van een flowchart. 
![AIQ categorieen](aiq.png)
![setup flowchart](setup flwochart.png)
