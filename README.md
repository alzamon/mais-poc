# mais-poc
Midlertidig repo for POC Mattilsynet i gke med Navs Nais. 

## Hva har vi gjort foreløpig?
* Satt opp gke-cluster med naiserator-operatoren
og videre oppsett.
* Rullet ut to eksempel frontend applikasjoner.
* Enabled Google Artifact Registry og lastet opp 
foto-ui docker image.
* Tilpasset eksempel frontend applikasjon til en 
ny applikasjon [foto-ui-mais.yaml](./foto-ui-mais.yaml).

## Bumps on the road
* Vi har ingresser med path og strip 
```    ingress:
      routes:
        - host: foto.utv.k8s.mattilsynet.no
          path: /api
          strip: /api
```
Må finne ut hvordan vi evt. løser same-origin issue.
* Ønsker gjennomgang av access policy setup for å la tjenester snakke sammen
* Litt vanskelig å vite hva fra [nais dokumentasjonen](https://doc.nais.io) som fungerer i vårt POC-cluster.

## Planer videre
* Gjøre samme for `foto-ui-backend` og `bilde-api` som vi har gjort for `foto-ui`, for å prøve bruk av 
`nais.yaml`-filer for andre typer applikasjoner, og se dem snakke med hvertandre.
* Sette opp github-actions for en av applikasjonene?
* Kartlegge hvor enkelt/vanskelig det er å åpne "tunnel" til en MATS instans?
* Skal dette være en stor "lift and shift" eller skal vi si "nye tjenester rulles ut her og gradvis migreres også gamle"?
* Gå gjennom infrastruktur stacken vår å se hva vi kan rydde bort etter migrering
* Tror det vil være av interesse å utfase keycloak og bruke Azure AD og [Access Policy](https://doc.nais.io/security/auth/azure-ad/access-policy/index.html) isteden.
* Vi bruker også unleash
