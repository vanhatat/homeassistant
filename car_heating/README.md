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

Idea on siis se, että lämmitys menee päälle, kun kello on seuraavan kerran syötetyssä ajassa, eli alle vuorokauden päästä. Oma toteutukseni jättää sekä automaation, että kytkimen päälle, jolloin automaatio laukeaa taas seuraavan kerran kun kun kello tulee lähtöaikaan. Kytkin jätetään päälle, koska meillä tuota pistorasiaa käytetään muuhunkin. Eli aina kun lämmitys halutaan ajastaa ja laitetaan auton lämmityspiuha kiinni, pitää kytkin laittaa pois päältä. Myös dashboardilla tästä muistutetaan, kuten alla oleva kuva osoittaa. Jos auton käyttö on päivittäistä, kannattaa varmaankin laittaa kytkin automaatiolla pois päältä automaattisesti lämmityksen jälkeen. Kannattaa varmaan tehdä vasta jonkin viiveen jälkeen, koska et välttämättä lähde juuri aiottuna lähtöaikana (jolloin lämmitys menisi virheellisesti pois päältä jo ennen lähtöä).


