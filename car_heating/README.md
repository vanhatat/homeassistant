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

