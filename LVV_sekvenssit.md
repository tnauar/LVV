# Lupa- ja valvontaviraston (LVV) prosessikaaviot

Tämä dokumentti kuvaa Lupa- ja valvontaviraston (LVV) keskeiset asianhallintaprosessit sekvenssikaavioina. Kaaviot on toteutettu Mermaid-syntaksilla, ja ne renderöityvät suoraan GitHubin Markdown-näkymässä.

## Sisällysluettelo
1. [Rekisteriä koskeviin tietopyyntöihin vastaaminen](#1-rekisteriä-koskeviin-tietopyyntöihin-vastaaminen)
2. [Lupapäätösten valmistelu ja ratkaiseminen](#2-lupapäätösten-valmistelu-ja-ratkaiseminen)

---

## 1. Rekisteriä koskeviin tietopyyntöihin vastaaminen

Prosessi noudattaa julkisuuslain määräaikoja. Tavanomainen julkinen tieto toimitetaan kahden viikon kuluessa ja erityistoimenpiteitä vaativa laajempi pyyntö viimeistään kuukauden kuluessa.

```mermaid
sequenceDiagram
    autonumber
    actor Pyyntaja as Tietopyytäjä
    participant Kirjaamo as LVV Kirjaamo / Asiointi
    participant Kasittelija as LVV Asiankäsittelijä
    participant Rekisteri as LVV Rekisteri (esim. Soteri, Astori)

    Pyyntaja->>Kirjaamo: Lähettää tietopyynnön (lomake, sähköposti, posti)
    Kirjaamo-->>Pyyntaja: Vastaanottokuittaus ja diaarinumero
    Kirjaamo->>Kasittelija: Kirjaa ja ohjaa pyynnön käsittelyyn

    alt Pyyntö on liian yleisluontoinen
        Kasittelija->>Pyyntaja: Pyytää täsmennystä (avustaa yksilöinnissä)
        Pyyntaja->>Kasittelija: Toimittaa tarkennetun pyynnön
    end

    Kasittelija->>Kasittelija: Tarkistaa julkisuuden, salassapidon ja käyttötarkoituksen
    
    opt Tieto on maksullinen (esim. virallinen rekisteriote)
        Kasittelija->>Pyyntaja: Ilmoittaa arvioidusta maksusta etukäteen
        Pyyntaja->>Kasittelija: Hyväksyy maksun
    end

    Kasittelija->>Rekisteri: Tekee haun yksilöidyillä hakuehdoilla
    Rekisteri-->>Kasittelija: Palauttaa rekisteritiedot / asiakirjat

    alt Tiedot voidaan luovuttaa (kokonaan tai osittain)
        Kasittelija->>Kasittelija: Poistaa salassa pidettävät tiedot (peittäminen)
        Kasittelija-->>Pyyntaja: Toimittaa tiedot (turvaviesti / posti) 2 vk – 1 kk kuluessa
        opt Osa tiedoista salataan
            Kasittelija-->>Pyyntaja: Ilmoittaa kieltäytymisen perusteen (oikeus valituskelpoiseen päätökseen)
        end
    else Tietoja ei voida luovuttaa (salassapito)
        Kasittelija-->>Pyyntaja: Antaa kieltäytymisilmoituksen
        opt Pyyntäjä vaatii virallisen päätöksen
            Pyyntaja->>Kasittelija: Pyytää valituskelpoista päätöstä
            Kasittelija-->>Pyyntaja: Toimittaa hallintopäätöksen ja valitusosoituksen
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
    Esittelija->>Ratkaisija: Esittelee asian ja toimittaa päätösluonnoksen

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