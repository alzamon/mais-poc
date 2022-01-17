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


## Planer videre
* Gjøre samme for `foto-ui-backend` og `bilde-api` som vi har gjort for `foto-ui`, for å prøve bruk av 
`nais.yaml`-filer for andre typer applikasjoner, og se dem snakke med hvertandre.
* Sette opp github-actions for en av applikasjonene?
* Kartlegge hvor enkelt/vanskelig det er å åpne "tunnel" til en MATS instans?
* Skal dette være en stor "lift and shift" eller skal vi si "nye tjenester rulles ut her og gradvis migreres også gamle"?
