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
  - [Flow chart](#Flow-chart)
  - [Rolverdeling](#Rolverdeling)
    - [Gebruikte sensoren](#Gebruikte-sensoren)
    - [Microcontroller](#Microcontroller)
    - [Detectie personen](#Detectie-personen)
    - [Gasmetingen](#Gasmetingen)
    - [Alohamora](#alohamora)
  - [Werking](#Werking)

## Inleiding

In ets- en soldeerlokalen kunnen tijdens de werkprocessen volatile organic compounds (VOC) vrijkomen, dit zijn onder andere componenten van brandstoffen of oplosmiddelen. Deze kunnen schadelijk zijn voor de gezondheid. Het is dan ook belangrijk deze gassen in de gaten te houden en een waarschuwing te verzenden wanneer bepaalde waarden overschreden worden. Zeker wanneer personen aanwezig zijn in de vervuilde ruimte.

## Flow chart
![flow chart](flow.PNG)

## Rolverdeling

|                 | Technisch             | niet technisch            |
|-----------------|-----------------------|---------------------------|
| Roeland Osaer   | Software sensor       | Verantwoordelijke verslag |
| Mathieu Martens | Hardware schema       | Presentatie               |
| Viktor Morre    | Software communicatie | Communicatie groepen      |
| Glen Smet       | Hardware PCB design   | Project manager           |


### Detectie personen
Indien er niemand gedetecteerd wordt mag er minder frequent gemeten worden maar moet er nog steeds een melding gegeven worden als de treshold overschreden wordt.
Indien er iemand gedetecteerd wordt moet er frequenter gemeten worden en moet er lokaal een alarm afgegaan bovenop op de melding. De threshold en frequenties van meten zijn opgenomen in het blokschema.

### Gasmetingen
De lucht in de ruimte wordt gemeten, op basis daarvan wordt beslist of deze niet vervuild is. Bij een goede waarde hoeft er niet meteen iets te gebeuren en kan er terug in sleep mode gegaan worden. Anders wordt een geluidssignaal afgespeeld (enkel indien er beweging was gedetecteerd)  en onmiddellijk een update verzonden naar de database.
## Werking
ESP32-C3-WROOM-02-N4 controller met daarop een bewegingssensor en gas sensor. Indien er beweging gedetecteerd wordt zal een timer gestart worden van 5 min. Indien deze timer loopt kunnen we aannemen dat er iemand in de ruimte is. Het doel van deze timer is om te vermijden dat er niet doorlopend geswitched worden door het even niet detecteren van beweging. Indien deze timer loopt zal er een timer gestart worden die om de 5min een meting neemt van de gas sensor indien de gemeten waarde lager is dan 125 zal de data worden opgeslagen en terug in slaap gaan. Wanneer 12 metingen zijn opgeslagen zal de data worden doorgestuurd. Dit zorgt ervoor dat in de databank elk uur een meting wordt ontvangen ongeacht of er beweging is of niet. Indien de waarde van 125 overschreden werd zal er meteen een alarm afgaan in de vorm van een buzzer alsook zal de data worden doorgestuurd zodat er een melding naar de gebruiker kan gaan. Indien er geen beweging gedetecteerd werd zal er een timer gestart worden van 1 uur. Alle timers zullen rtc gestuurd zijn op de esp 32. De minimum data dat wordt doorgestuurd is 9 bits aangezien de IAQ varieert tussen 0 en 500. De gemeten data wordt via een … module verzonden naar een centraal punt. Tussen de metingen zal de ESP zich in sleep mode bevinden. Hij wordt wakker gemaakt door een periodiek tijdsignaal of indien een beweging geregistreerd wordt.

### Microcontroller
ESP32-C3-WROOM-02-N4

We veronderstellen in eerste instantie dat we de IOT node gaan voeden met een batterij, indien we kunnen aantonen dat batterij niet voldoende is kan er besproken worden om naar een vaste voeding over te schakelen. Als microcontroller kiezen we ervoor om een low power esp 32 gebruiken. Deze heeft als voordeel dat het verbruik in slaap aanzienlijk lager is.

### Gebruikte sensoren

Als gassensor gebruiken we de [BME680](https://www.bosch-sensortec.com/media/boschsensortec/downloads/datasheets/bst-bme680-ds001.pdf) van Bosch.
Om de bewegingen in de ruimte te registreren gebruiken we de [EKMB1305112K](https://www.mouser.be/datasheet/2/315/bltn_eng_papirs-1365490.pdf) een PIR sensor van Panasonic.
