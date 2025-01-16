# Auton lämmityksen ajastus Home Assistantilla

Tämä toteutus tekee kutakuinkin seuraavaa:
* Käyttäjä syötää kellonajan, jolloin autolla on aikomus lähteä liikkeelle.
* Lasketaan sääennustuksen perusteella tarvittava lämmitysaika
* Lasketaan lämmityksen alkamisaika (lähtöaika - tarvittava lämmitysaika).
* Kolme tuntia ennen alkamisaikaa lasketaan lämmitysaika uudelleen sen hetkisen säätiedon perusteella. Asetetaan uusi lämmityksen alkuaika.
  * (Tämä vaihe on lähinnä hienosäätöä, jos vaikka toteutunut lämpötila poikkeaakin kovasti aiemmin tutkitusta ennusteesta. Voit siis jättää tämän stepin väliin etenkin aluksi.)
* Kun lämmityksen alkuaika koittaa:
  *  Laitetaan lämmitystolpan pistorasia päälle
  *  Lähetetään ilmoitus kännykkään (ei pakollinen, mutta halusin itse varmistua että lämmitys meni päälle)

Idea on siis se, että lämmitys menee päälle, kun kello on seuraavan kerran syötetyssä ajassa, eli alle vuorokauden päästä. Oma toteutukseni jättää sekä automaation, että pistorasian päälle, jolloin automaatio laukeaa taas seuraavan kerran kun kun kello tulee lähtöaikaan. Pistorasia jätetään päälle, koska meillä tuota pistorasiaa käytetään muuhunkin. Eli aina kun lämmitys halutaan ajastaa ja laitetaan auton lämmityspiuha kiinni, pitää pistorasia laittaa pois päältä. Myös dashboardilla tästä muistutetaan, kuten alla oleva kuva osoittaa. Jos auton käyttö on päivittäistä, kannattaa varmaankin laittaa pistorasia automaatiolla pois päältä automaattisesti lämmityksen jälkeen. Kannattaa varmaan tehdä vasta jonkin viiveen jälkeen, koska et välttämättä lähde juuri aiottuna lähtöaikana, jolloin lämmitys menisi virheellisesti pois päältä jo ennen lähtöä.

## Dashboard, lähtöajan asetus
Näkymä, kun pistorasia on päällä:

![Näyttökuva 2025-01-15 213950](https://github.com/user-attachments/assets/cab7d831-cb78-4c79-8623-145ee52a051a)

Näkymä, kun pistorasia ei ole päällä:

![Näyttökuva 2025-01-15 213914](https://github.com/user-attachments/assets/a214d74b-ced1-4fbf-86aa-7f27cf23b0cc)

### Kenttien/entiteettien selitykset (entiteetin nimi suluissa)
* **_Ajastin päällä (automation.aseta_auton_lammitys_paalle)_**:
  * Auton lämmityksen kytkennän automaatio päälle/pois
* **_Lähtöaika klo (input_datetime.auto_lahtoaika_klo)_**:
  * Kellonaika, jolloin autolla aiotaan lähteä liikenteeseen.
* **_Pistorasia (switch.katos_seina)_**: pistorasia päälle/pois. Pistorasia pitää olla pois, kun lämmitysjohto laitetaan seinään.
* **_Lähtöaika (sensor.auto_lahtoaika)_**: seuraava lähtöaika, eli seuraava hetki, kun kello on sama kuin _Lähtöaika klo_. Tämän perusteella automaatiot/skriptit laskevat lämmityksen alkamisajan. Tätä ei olisi pakko näyttää käyttäjälle, mutta itse pidän sen näkyvissä varmuuden vuoksi (voin varmistua, että hetki on laskettu oikein).
* **_Lämpötilaennuste (input_number.auto_lammitys_ulkolampotila)_**: ennusteen mukainen lämpötila lähtöaikana. Tämän perusteella automaatiot/skriptit laskevat tarvittavan lämmitysajan. Tämä päivitetään kolme tuntia ennen lähtöaikaa sen hetkisen säätiedon perusteella. Tässä käytetään _Meteorologisk institutt (Met.no)_-integraatiota, kun ei ole sopivaa anturia käytettävissä. On toiminut ihan riittävän tarkasti omaan tarpeeseen.

Jos pistorasia on valmiiksi päällä, muistuttaa yo. näkymä käyttäjää, että pistorasia pitää laittaa pois päältä, ennen kuin lämmitysjohto laitetaan pistorasiaan. Muutoin lämmitys menisi heti päälle.

Kortin koodi Home Assistantissa:
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
