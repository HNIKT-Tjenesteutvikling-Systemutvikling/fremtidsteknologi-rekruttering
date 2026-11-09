# Rekrutteringsoppgave

## Om Oppgaven

Dette er en teknisk oppgave som skal løses, som en del av vår rekrutteringsprosess. Oppgaven består av tre deloppgaver som tester forskjellige aspekter av programmering og problemløsning.

## Slik kommer du i gang

1. Trykk **"Use this template"** → **"Create a new repository"** øverst på denne siden.
   - Velg din egen konto som eier
   - Velg **Private**
   - **Ikke** fork repoet – da kan løsningen din bli synlig for andre kandidater
2. Gå til **Settings → Collaborators → Add people** i ditt nye repo og legg til:
   - `geirvillyisaksen`
   - `Jorgenhnikt`
   - `VebjornHal`
   - `KristianAN`
3. Opprett en egen branch og jobb der (se retningslinjer under).
4. Lim inn lenken til repoet ditt på leveringssiden.

## Viktige retningslinjer

- **Git-branch**: Opprett en egen branch i repositoriet, og opprett en Pull Request (PR) mot `master` når du er klar til å levere.
- **Git-historikk**: Behold alle commits intakt (ikke squash).
- **Dokumentasjon**: Legg til kommentarer der det er nødvendig for å forklare logikk og valg.
- **Feilhåndtering**: Beskriv (enten i kode eller dokumentasjon) hvordan du håndterer feil og spesialtilfeller.
- **Uferdige løsninger**: Dersom du ikke rekker å fullføre alt, legg likevel inn commits som viser din fremgang og inkluder en forklaring på hva som gjenstår.
- **Pull Request**: Selv etter at du har opprettet en PR, kan du pushe nye endringer til din branch. Du kan dermed bruke tiden aktivt helt frem til innleveringsfristen.

## Oppgaver

### Generelt

Du står fritt i valg av programmeringsspråk til løsningen så lenge du styrer unna esoteriske språk som Piet og Brainfuck.
Om du er usikker på hva du kan bruke er for eksempel `Java`, `Scala`, `C#`, `F#`, `Python`, `TypeScript`, `JavaScript` og `Haskell` gode kandidater.
Bruker du eksterne biblioteker, begrunn hvorfor du valgte dem. Beskriv også hvordan vi setter opp kjøremiljøet og kjører koden din.

### Swagger

OpenAPI: https://rekruttering.azurewebsites.net/docs/

- **Oppgave 1.**

Vi har et API: https://rekruttering.azurewebsites.net, som genererer norske personnumre, der kun 1 av 5 er gyldige. Oppgaven er å konsumere dette endepunktet i en CLI-applikasjon (eller tilsvarende) og validere personnummeret som returneres. Dersom personnummeret ikke er gyldig, skal applikasjonen kalle endepunktet på nytt til den får et gyldig personnummer.

```shell
$ mytool generate pnr
$ 07042470850
```

1. Implementér en funksjon for validering av norske personnumre.
2. Hent ut personnummer fra APIet, bruk funksjonen for å kontrollere gyldigheten, og hent på nytt om det er ugyldig.
3. Returnér eller skriv ut det gyldige personnummeret.

https://www.matematikk.org/artikkel.html?tid=64296&within_tid=64294 har en bra beskrivelse av personnummer og validering av personnummer

- **Oppgave 2.**

APIet har også et endepunkt for å generere pasientdata. Dette krever et gyldig personnummer i requesten. Oppgaven er å opprette en ny kommando/funksjon i CLI-applikasjonen som genererer pasientdata basert på et gyldig personnummer fra Oppgave 1.

Brukseksempel:

```shell
$ mytool generate patient
$ 07042470850
$ {
  "lastFregDate": "2025-01-28T21:11:13.263297551",
  "registeredDate": "2025-01-28T21:11:13.263441153",
  "fnr": "07042470850",
  "name": {
    "firstName": "Gunnhild",
    "lastName": "Hoem"
  },
  "deceased": false,
  "maritalStatus": "Divorced",
  "education": "HighSchoolPreparatory",
  "addressType": "Norwegian",
  "address": {
    "districtCode": "",
    "districtName": "",
    "zipCode": "",
    "municipality": "",
    "municipalityNumber": "",
    "county": "",
    "street": ""
  },
  "owningCentre": "TESTNO",
  "tsCreated": "2025-01-28T21:11:13.265310977",
  "createdBy": "",
  "epikrise": "..."
}
```

1. Lag en kommando/funksjon som henter (eller gjenbruker) et gyldig personnummer fra Oppgave 1.
2. Bruk dette gyldige personnummeret i payloaden til API-kallet for å generere pasientdata.
3. Skriv ut eller returnér resultatet.

- **Oppgave 3.**

Pasientdataene fra Oppgave 2 inneholder også feltet `epikrise`. Det er en fritekst som kan inneholde personopplysninger om pasienten, blant annet:

- fornavn og etternavn (`firstName`, `lastName`)
- adresse (`street`, `zipCode`, `municipality`, `municipalityNumber`)
- navnet til pasientens pårørende (mor og far)

Navnene til mor og far står **bare** i fritekst. De finnes ikke som egne felt i pasientdataene.

Eksempel (forkortet):

```json
{
  "fnr": "07042470850",
  "name": {
    "firstName": "Gunnhild",
    "lastName": "Hoem"
  },
  "address": {
    "municipality": "Bergen",
    "municipalityNumber": "4601",
    "zipCode": "5013",
    "street": "Strandgaten 12"
  },
  "epikrise": "Gunnhild Hoem, bosatt i Strandgaten 12, 5013 Bergen, ble innlagt med brystsmerter. Pasienten ble fulgt opp av mor Ingrid Hoem og far Per Hoem under oppholdet. ..."
}
```

Oppgaven er å utvide CLI-applikasjonen slik at pasientdata kan hentes ut **pseudonymisert**:

1. Hent pasientdata på samme måte som i Oppgave 2.
2. Pseudonymiser hele pasientobjektet. Det gjelder de strukturerte feltene (`name`, `address`, `fnr`) og personopplysninger i `epikrise`: navn, adresser, fødselsnumre, telefonnumre og e-postadresser, både for pasienten og pårørende. Datoer skal beholdes. Samme opprinnelige verdi skal få samme erstatning overalt i objektet.
3. Velg selv om opplysningene skal erstattes med plassholdere (for eksempel `[FORNAVN_1]`) eller med realistiske, syntetiske verdier. Begrunn valget.
4. Pseudonymiseringen skal kunne reverseres. Lagre koblingen mellom opprinnelige verdier og erstatningene i en nøkkel som holdes atskilt fra de pseudonymiserte dataene. Lag også en kommando/funksjon som gjenoppretter originalen ved hjelp av nøkkelen. Resultatet skal være identisk med de opprinnelige pasientdataene.
5. Beskriv hvilke typer personopplysninger løsningen din fanger opp, og hvilke den kan gå glipp av.

Brukseksempel (kommandoer og flagg velger du selv):

```shell
$ mytool generate patient --pseudonymize --key ~/patient-key.json > ~/patient-pseudo.json
$ mytool reidentify ~/patient-pseudo.json --key ~/patient-key.json
```

> **Merk:** Løsningen skal ikke bruke LLM-er (store språkmodeller) til å finne eller erstatte personopplysninger. Vi ser etter smarte løsninger som ikke krever store mengder GPU.

## Innlevering

Lever løsningen din via Pull Request med følgende:

- Din tilnærming til oppgavene
- Begrunnelse for valgte løsninger
- Utfordringer du møtte underveis
- Kjente problemer eller begrensninger
- Potensielle forbedringer gitt mer tid
- Hvordan teste koden

### Andre OS og distros

Husk det kan være en ide å ha en enkel måte for oss å teste koden.

- [`shell script`](https://github.com/bobbyiliev/introduction-to-bash-scripting)
- [`makefile`](https://makefiletutorial.com/)
- [`Github action`](https://github.com/features/actions)

### Hva Vi Ser Etter

**Vi er mest interessert i**:

- Hvordan du strukturerer løsningen
- Hvordan du håndterer edge-cases og feilhåndtering
- Hvordan du tester koden din
- Din evne til å kommunisere tekniske beslutninger

Valg av programmeringsspråk og teknologi er mindre viktig enn oversiktlighet, dokumentasjon og kvalitet.

Lykke til!
Vi gleder oss til å se din løsning!
