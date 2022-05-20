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
  - [Rolverdeling](#Rolverdeling)
  - [Detectie personen](#Detectie-personen)
  - [Gasmetingen](#Gasmetingen)
  - [Eerste concept](#Eerste-concept)
  - [Microcontroller](#Microcontroller)
  - [Gebruikte sensoren](#Gebruikte-sensoren)
  - [Communicatie](#Communicatie)

## Inleiding

In ets- en soldeerlokalen kunnen tijdens de werkprocessen volatile organic compounds (VOC) vrijkomen, dit zijn onder andere componenten van brandstoffen of oplosmiddelen. Deze kunnen schadelijk zijn voor de gezondheid en milieu wanneer hiervan een te hoge concentratie is in de lucht. Het is dan ook belangrijk deze gassen in de gaten te houden en een waarschuwing te verzenden wanneer bepaalde waarden overschreden worden, zeker wanneer personen aanwezig zijn in de vervuilde ruimte.


## Rolverdeling

|                 | Technisch             | niet technisch            |
|-----------------|-----------------------|---------------------------|
| Roeland Osaer   | Software sensor       | Verantwoordelijke verslag |
| Mathieu Martens | Hardware schema       | Presentatie               |
| Viktor Morre    | Software communicatie | Communicatie groepen      |
| Glen Smet       | Hardware PCB design   | Project manager           |


## Detectie personen
Aangezien het belangrijk is te weten of er personen in de ruimte aanwezig zijn of niet wordt dit als eerste besproken. Zo mag indien niemand gedetecteerd wordt in de ruimte in kwestie minder frequent gemeten worden, maar moet er nog steeds een melding verstuurd worden als de treshold overschreden wordt.
Indien er wel een persoon gedetecteerd wordt, moet er frequenter gemeten worden en moet er ook lokaal een alarm afgegaan bovenop op de melding. Dit was het eerste concept van de IOT node. Maar na het testen en verdiepen in de gekozen gassensor hebben we ontdekt dat er geen mogelijkheid is om een IAQ meting te doen op een ander interval dan vijf minuten. De detectie zal dus enkel belangrijk zijn om de buzzer te laten afgaan indien iemand zich in de ruimte bevindt en deze aan te manen de ruimte te verluchten of even te verlaten tot de concentratie van de VOC's gedaald is.

## Gasmetingen
De gasmetingen worden vanzelfsprekend gedaan door de lucht in de ruimte te meten. Op basis daarvan wordt beslist of deze niet al dan niet vervuild is, of er VOC's aanwezig zijn en in welke concentratie. Bij een goede waarde hoeft er niet meteen iets te gebeuren en kan er terug in sleep mode gegaan worden, de gemeten waarde kan eventueel opgeslagen of verstuurd worden naar een centrale eenheid. Wanneer de waarden te hoog worden bevonden, wordt een geluidssignaal afgespeeld (enkel indien er beweging was gedetecteerd) en onmiddellijk een update verzonden naar de database.

## Eerste concept
De architectuur bestaande uit een ESP32-C3-WROOM-02-N4 controller met daarop een bewegingssensor en gassensor die gebruikt zullen worden als volgt. Indien er beweging gedetecteerd wordt, zal er een timer gestart worden van vijf minuten. Terwijl deze timer loopt kunnen we aannemen dat er iemand in de ruimte aanwezig is. Het doel van deze timer is om te vermijden dat er niet doorlopend gewisseld wordt tussen aanwezigheid en afwezigheid van personen door het even niet detecteren van beweging. Er zal ook een tweede timer lopen van vijf minuten voor het correct uitlezen van de gassensor. De gassensor zal zich in ULP (Ultra Low Power modus) bevinden en om de vijf minuten samplen. Dit is de meest zuinige modus die ons toelaat om een accurate meting te doen. Indien de gemeten waarde lager is dan 125 zal de data worden opgeslagen en zal de esp32 terug in slaap gaan. Wanneer er 12 metingen zijn opgeslagen zal de data worden doorgestuurd. Dit zorgt ervoor dat in de databank elk uur een meting wordt ontvangen ongeacht of er beweging is of niet. Indien de waarde van 125 overschreden wordt, zal er meteen een alarm afgaan in de vorm van een buzzer alsook zal de data worden doorgestuurd, zodat er een melding naar de gebruiker kan gaan. Alle timers zullen rtc gestuurd zijn op de esp 32. Het minimum aan data dat wordt doorgestuurd is 9 bits aangezien de IAQ varieert tussen 0 en 500. De gemeten data wordt via een wifi module verzonden naar een centraal punt. Tussen de metingen zal de ESP zich in sleep mode bevinden. Hij wordt wakker gemaakt door een periodiek tijdsignaal of door een interrupt indien een beweging geregistreerd wordt.

## Microcontroller
ESP32-C3-WROOM-02-N4: 
Als microcontroller kiezen we ervoor om een low power esp 32 te gebruiken. Deze heeft als voordeel dat het verbruik in slaap aanzienlijk lager is. We kozen voor deze microcontroller omdat we al ervaring hadden met de standaard esp32 en deze in grote lijnen hetzelfde is. Hierdoor zullen we minder tijd verliezen wat nodig is aangezien de deadlines redelijk strak zijn.
Uit onze eerste energie berekeningen volgt dat we de esp32 kunnen voeden met 5 AA batterijen zodat deze een autonomie heeft van één jaar.

Na het oplossen van de fouten op het pcb zoals beschreven, kregen we nog steeds problemen met het uploaden van de code naar de esp32-c3. De fout bleek te komen van de BME680 library van Bosh zelf. De compiler vond de voor gecompileerde bibliotheek niet, we hebben toen door middel van flags de bibliotheek voor de esp32 meegegeven.  Toen kregen we een andere foutmelding namelijk dat de architectuur van de bibliotheek niet werd ondersteund. Na nog wat extra opzoekingswerk kwamen we erop uit dat dat deze foutmelding betekende dat de esp32-c3 nog niet ondersteund wordt ondanks dat hij aan alle specificaties voldoet voor de bibliotheek. We hebben dan maar besloten om voorlopig als microcontroller de standaard esp32 te gebruiken om het werkende geheel te kunnen tonen, hoewel dit gepaard gaat met een mindere energie efficiëntie. Ondertussen heeft Bosh ook al bevestigd dat de C3 inderdaad nog niet compatibel is met de vereiste library.

## Gebruikte sensoren

Als gassensor gebruiken we de [BME680](https://www.bosch-sensortec.com/media/boschsensortec/downloads/datasheets/bst-bme680-ds001.pdf) van Bosch.
Om de bewegingen in de ruimte te registreren gebruiken we de [EKMB1305112K](https://www.mouser.be/datasheet/2/315/bltn_eng_papirs-1365490.pdf), een PIR sensor van Panasonic. Deze heeft een horizontale openingshoek van 150° en een verticale van 36°. Groot genoeg dus om alle bewegingen in een lokaal waar te nemen ongeacht de locatie waar de node geplaatst wordt.


## Communicatie
Als communicatieprotocol werd er gekozen voor MQTT, enerzijds omdat dit ook als low power protocol beschouwd wordt, doordat de hoeveelheid data die verzonden wordt klein (kleine code footprint en kleine message size) is. Er wordt ook enkel geluisterd naar kanalen die voor elke specifieke node van toepassing zijn, waardoor het ook gemakkelijk te implementeren is samen met de nodes van de andere groepen. Er kan eventueel nog gekeken worden om de MQTT-SN te gebruiken, dit is een variant van MQTT die de efficiëntie nog verbeterd bij beide de transmissie en energieverbruik.

## Software
Alle software wordt geschreven in cpp op het arduino framework via Plafrom IO in Visual studio code. Voor het project is de BME680 onze hoofd component en bepaald de volledige software flow. Om de sensor in ULP (ultra low power) te laten werken is het namelijk verplicht om een meting te doen elke 5 min. Enkel en alleen op deze manier kan de bibliotheek die we gebruiken een IAQ (indoor air quality) cijfer berekenen. Dit is een cijfer tussen 0 en 500 die de kwaliteit van de lucht bepaalt. Zoals in de tabel te zien is hieronder. Tussen de metingen door kan de sensor in deepsleep gaan. Hiervoor moet telkens alle gegevens opgeslaan worden in een stuk geheugen die blijvend is tijdens een deepsleep. Dit gebeurd net voor hij in deepsleep gaat en om dan net nadat hij uit deepsleep komt terug opgehaald te worden om een nieuwe meting te doen. De IOT node kan ook op 2 andere manieren uit deepsleep gehaald worden. Namelijk via een drukknop en via de PIR-sensor. De eerste wordt gebruikt voor een manuele doorsturing van de laatste 12 metingen. Terwijl de tweede de detectie van een persoon doet. Omdat via deze twee interrupts de ESP uit deepsleep gehaald wordt en zo de timer die gezet is na een meting gereset wordt, moet de ESP er voor zorgen dat na de interrupt de ESP weer in slaap gaat voor exact de juist tijd zodat de 5 minuten tijd tussen twee metingen behouden wordt. De manier waarop dit gedaan wordt, wordt uitgelegd aan de hand van een flowchart. In de flowchart is duidelijk te zien dat de initialisatie van de BME6800 enkel gedaan wordt als de 5min verlopen zijn. bij andere interupts (button of PIR) gaat hij direct in deepsleep. 

Als we dan verder kijken naar de logica. Dit is het deel waarbij de meting gedaan wordt en daarmee verder gewerkt wordt. Eerst en vooral wordt de interrupt die door de PIR sensor kan gegenereerd worden terug geactiveerd. Deze wordt gedeactiveerd na het detecteren van een persoon. Dit wordt gedaan omwille van het feit dat wanneer een persoon aanwezig is in het lokaal de esp niet constant uit deepsleep gehaald zou worden door de PIR. Na het activeren wordt een meeting gedaan en wordt de AIQ waarde en accuracy opgeslaan in het gegheugen. Daarna wordt een controle gedaan de hoeveelste meeting dit is. Is dit de 12de meeting dan zal de esp connectie maken met de broker en de laatste 12 metingen doorsturen. Zoniet checkt de esp of de laatste meeting een waarde heeft boven de 125 (slechte luchtkwaliteit). Als dit zo is stuurt de esp opnieuw de laatste 12 metingen door naar de broker. Indien ook een persoon aanwezig was in de vorige 5 min zal er een alarm afgaan in de vorm van een pieptoon, gegenereerd door een buzzer.
Hierna keert de flowchart terug naar de timing flowchart. De variable time_till_next_wakeup wordt terug op 5 min gezet, de actuele timestamp wordt opgeslaan, de esp deepsleep timer wordt ingesteld en de esp gaat in deepsleep wachtende op zijn volgende interrupt.
![AIQ categorieen](aiq.png)
![timing_flowchart](timing_flowchart.png)
![logica_flowchart](logica_flowchart.png)
