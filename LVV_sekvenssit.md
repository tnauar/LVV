# Lupa- ja valvontaviraston (LVV) prosessikaaviot

Tämä dokumentti kuvaa Lupa- ja valvontaviraston (LVV) keskeiset prosessit sekvenssikaavioina.

## Sisällysluettelo
1. [Rekisteriä koskeviin tietopyyntöihin vastaaminen](#1-rekisteriä-koskeviin-tietopyyntöihin-vastaaminen)
2. [Lupapäätösten valmistelu ja ratkaiseminen](#2-lupapäätösten-valmistelu-ja-ratkaiseminen)

---

## 1. Rekisteriä koskeviin tietopyyntöihin vastaaminen

Prosessi noudattaa julkisuuslain määräaikoja. Tavanomainen julkinen tieto toimitetaan kahden viikon kuluessa ja erityistoimenpiteitä vaativa laajempi pyyntö viimeistään kuukauden kuluessa.

```mermaid
sequenceDiagram
    autonumber
    actor Pyytaja as Tietopyytäjä
    participant Asiointi as LVV Asiointi / Kirjaamo
    participant Kasittelija as LVV Asiankäsittelijä
    participant Rekisteri as Soteri

    Pyytaja->>Asiointi: Lähettää tietopyynnön (lomake, sähköposti, posti)
    Asiointi-->>Pyytaja: Vastaanottokuittaus
    Asiointi->>Kasittelija: Kirjaa ja ohjaa pyynnön käsittelyyn

    alt Pyyntö on liian yleisluontoinen
        Kasittelija->>Pyytaja: Pyytää täsmennystä ja avustaa tarvittaessa
        Pyytaja->>Kasittelija: Toimittaa tarkennetun pyynnön
    end

    Kasittelija->>Kasittelija: Tarkistaa julkisuuden, salassapidon ja käyttötarkoituksen
    
    opt Tieto on maksullinen
        Kasittelija->>Pyytaja: Ilmoittaa arvioidusta maksusta etukäteen
        Pyytaja->>Kasittelija: Hyväksyy maksun
    end

    Kasittelija->>Rekisteri: Tekee haun yksilöidyillä hakuehdoilla
    Rekisteri-->>Kasittelija: Palauttaa rekisteritiedot

    alt Tiedot voidaan luovuttaa (kokonaan tai osittain)
        Kasittelija->>Kasittelija: Poistaa salassa pidettävät tiedot
        Kasittelija-->>Pyytaja: Toimittaa tiedot 2 vk – 1 kk kuluessa
        opt Osa tiedoista salataan
            Kasittelija-->>Pyytaja: Ilmoittaa kieltäytymisen perusteen
        end
    else Tietoja ei voida luovuttaa (salassapito)
        Kasittelija-->>Pyytaja: Antaa kieltäytymisilmoituksen
        opt Pyyntäjä vaatii virallisen päätöksen
            Pyytaja->>Kasittelija: Pyytää valituskelpoista päätöstä
            Kasittelija-->>Pyytaja: Toimittaa hallintopäätöksen ja valitusosoituksen
        end
    end
```

### Keskeiset vaiheet
* **Vireilletulo ja kirjaaminen:** Pyyntö saapuu kirjaamoon tai sähköisen asioinnin kautta, kirjataan diaariin ja ohjataan käsittelijälle.
* **Yksilöinti:** Käsittelijä auttaa pyytäjää täsmentämään pyyntöä, jos haluttua asiakirjaa tai rekisteritietoa ei voida heti paikallistaa.
* **Julkisuus- ja maksullisuusharkinta:** Käsittelijä tarkistaa salassapitoperusteet ja ilmoittaa mahdollisesta maksuasetuksen mukaisesta maksusta etukäteen.
* **Luovutus tai päätös:** Tiedot toimitetaan pyytäjälle tai kieltäytymisestä annetaan pyydettäessä valituskelpoinen hallintopäätös.

---

## 2. Lupapäätösten valmistelu ja ratkaiseminen

Lupahakemusten käsittely noudattaa hallintolakia. Päätöksenteossa sovelletaan esittelymenettelyä, jossa esittelijä valmistelee päätösehdotuksen ja ratkaisija hyväksyy sen.

```mermaid
sequenceDiagram
    autonumber
    actor Hakija as Luvan hakija
    participant Asiointi as Sähköinen asiointi / Kirjaamo
    participant Esittelija as LVV Esittelijä (Ylitarkastaja)
    participant Sidosryhmat as Lausunnonantajat / Asianosaiset
    participant Ratkaisija as LVV Ratkaisija (Johtaja / Päällikkö)
    participant Rekisteri as LVV Rekisteri & Laskutus

    Hakija->>Asiointi: Lähettää lupahakemuksen ja liitteet
    Asiointi-->>Hakija: Vastaanottokuittaus ja diaarinumero
    Asiointi->>Esittelija: Kirjaa ja jakaa hakemuksen valmisteluun

    opt Hakemus on puutteellinen
        Esittelija->>Hakija: Lähettää täydennyspyynnön (määräaika)
        Hakija->>Esittelija: Toimittaa puuttuvat selvitykset
    end

    opt Lausunnot, kuuleminen tai tarkastus tarpeen
        Esittelija->>Sidosryhmat: Pyytää lausunnot / varaa tilaisuuden kuulemiseen
        Sidosryhmat-->>Esittelija: Toimittavat lausunnot ja muistutukset
        opt Hakijan kuuleminen vastineista
            Esittelija->>Hakija: Varaa tilaisuuden antaa selitys
            Hakija->>Esittelija: Toimittaa vastineen
        end
    end

    Esittelija->>Esittelija: Arvioi luvan edellytykset ja laatii päätösesityksen
    alt Esittelija hyväksyy päätösesityksen
        Esittelija->>Esittelija: Tekee ja allekirjoittaa lupapäätöksen
    else Esittelija esittelee asian 
        Esittelija->>Ratkaisija: Esittelee asian ja toimittaa päätösluonnoksen
    end

    alt Ratkaisija hyväksyy päätösesityksen
        Ratkaisija->>Ratkaisija: Tekee ja allekirjoittaa lupapäätöksen
    else Ratkaisija pyytää muutoksia
        Ratkaisija-->>Esittelija: Palauttaa jatkovalmisteluun
        Esittelija->>Ratkaisija: Esittelee korjatun päätösesityksen
        Ratkaisija->>Ratkaisija: Tekee ja allekirjoittaa lupapäätöksen
    end

    Ratkaisija->>Rekisteri: Tallentaa päätöksen rekisteriin ja käynnistää laskutuksen
    Ratkaisija-->>Hakija: Antaa päätöksen tiedoksi ja liittää valitusosoituksen
    opt Tiedoksianto muille tahoille
        Ratkaisija-->>Sidosryhmat: Lähettää päätöksen tiedoksi lausunnonantajille
    end
```

### Keskeiset vaiheet
* **Vireilletulo ja täydennys:** Hakemus kirjataan diaariin. Puutteelliseen hakemukseen pyydetään täydennystä määräajassa.
* **Kuuleminen ja lausunnot:** Tarvittaessa kuullaan asianosaisia ja pyydetään lausunnot muilta viranomaisilta. Hakijalle varataan tilaisuus antaa vastine.
* **Esittely ja ratkaisu:** Esittelijä laatii päätösesityksen, jonka ratkaisija tarkastaa ja allekirjoittaa.
* **Tiedoksianto ja laskutus:** Päätös valitusosoituksineen lähetetään tiedoksi, tallennetaan rekisteriin ja käynnistetään käsittelymaksun laskutus.