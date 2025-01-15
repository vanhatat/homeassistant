# Auton lämmityksen ajastus Home Assistantilla

Tämä toteutus tekee kutakuinkin seuraavaa:
* Käyttäjä syötää kellonajan, jolloin autolla on aikomus lähteä liikkeelle.
* Lasketaan sääennustuksen perusteella tarvittava lämmitysaika
* Lasketaan lämmityksen alkamisaika (lähtöaika - tarvittava lämmitysaika).
* Kolme tuntia ennen alkamisaikaa lasketaan lämmitysaika uudelleen sen hetkisen säätiedon perusteella. Asetetaan uusi lämmityksen alkuaika.
  * (Tämä vaihe on lähinnä hienosäätöä, jos vaikka toteutunut lämpötila poikkeaakin kovasti aiemmin tutkitusta ennusteesta. Voit siis jättää tämän stepin väliin etenkin aluksi.)
* Kun lämmityksen alkuaika koittaa:
  *  Laitetaan lämmitystolpan kytkin päälle
  *  Lähetetään ilmoitus kännykkään (ei pakollinen, mutta halusin itse varmistua että lämmitys meni päälle)

Idea on siis se, että lämmitys menee päälle, kun kello on seuraavan kerran syötetyssä ajassa, eli alle vuorokauden päästä. Oma toteutukseni jättää sekä automaation, että kytkimen päälle, jolloin automaatio laukeaa taas seuraavan kerran kun kun kello tulee lähtöaikaan. Kytkin jätetään päälle, koska meillä tuota pistorasiaa käytetään muuhunkin. Eli aina kun lämmitys halutaan ajastaa ja laitetaan auton lämmityspiuha kiinni, pitää kytkin laittaa pois päältä. Myös dashboardilla tästä muistutetaan, kuten alla oleva kuva osoittaa. Jos auton käyttö on päivittäistä, kannattaa varmaankin laittaa kytkin automaatiolla pois päältä automaattisesti lämmityksen jälkeen. Kannattaa varmaan tehdä vasta jonkin viiveen jälkeen, koska et välttämättä lähde juuri aiottuna lähtöaikana, jolloin lämmitys menisi virheellisesti pois päältä jo ennen lähtöä.

## Dashboard, lähtöajan asetus
Näkymä, kun kytkin on päällä:
![kuva](https://github.com/user-attachments/assets/f170eaa0-f0d4-47b6-b875-b4568f826c10)

Näkymä, kun kytkin ei ole päällä:
![kuva](https://github.com/user-attachments/assets/f5d8feeb-4658-4f19-a615-32aba4889294)

Jos kytkin on valmiiksi päällä, muistuttaa yo. näkymä käyttäjää, että kytkin pitää laittaa pois päältä, ennen kuin lämmitysjohto laitetaan pistorasiaan. Muutoin lämmitys menisi heti päälle.

Kortin koodi:
```
type: entities
entities:
  - entity: automation.aseta_auton_lammitys_paalle
    icon: mdi:car-electric
    name: Ajastin päällä
  - entity: input_datetime.auto_lahtoaika_klo
    name: Lähtöaika klo
  - type: conditional
    conditions:
      - entity: switch.katos_seina
        state: "on"
    row:
      type: section
      label: "HUOM: laita pistorasia pois päältä ennen ajastusta!"
  - type: conditional
    conditions:
      - entity: switch.katos_seina
        state: "on"
    row:
      entity: switch.katos_seina
      icon: mdi:toggle-switch-outline
      name: Pistorasia
  - type: conditional
    conditions:
      - entity: switch.katos_seina
        state: "on"
    row:
      type: section
      label: ""
  - entity: sensor.auto_lahtoaika
    name: Lähtöaika
    format: datetime
  - entity: input_number.auto_lammitys_ulkolampotila
    name: Lämpötilaennuste
    type: simple-entity
  - entity: sensor.auto_lammitysaika
    name: Lämmitysaika
  - entity: sensor.auto_lammitys_alkuaika
    name: Lämmitys alkaa
    format: datetime
state_color: true
show_header_toggle: false
title: Auton lämmitys

```
