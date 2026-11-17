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