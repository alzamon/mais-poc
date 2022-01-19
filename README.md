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
* Issue med envsubst i [nginx-unpriviledged](https://hub.docker.com/r/nginxinc/nginx-unprivileged). Vi bruker den for å skille hvilken keycloak-instans vi går på i forskjellige miljøer.

Se også [bumps fra tidligere møter](./bumps_vi_har_diskutert).

## Planer videre
* Gjøre samme for `foto-ui-backend` og `bilde-api` som vi har gjort for `foto-ui`, for å prøve bruk av 
`nais.yaml`-filer for andre typer applikasjoner, og se dem snakke med hverandre.
* Sette opp github-actions for en av applikasjonene?
* Kartlegge hvor enkelt/vanskelig det er å åpne "tunnel" til en MATS instans?
* Skal dette være en stor "lift and shift" eller skal vi si "nye tjenester rulles ut her og gradvis migreres også gamle"?
* Gå gjennom infrastruktur stacken vår å se hva vi kan rydde bort etter migrering
* Tror det vil være av interesse å utfase keycloak og bruke Azure AD og [Access Policy](https://doc.nais.io/security/auth/azure-ad/access-policy/index.html) isteden.
* Hva må vi sette opp i Azure AD for å kunne sette opp frontend [the nais way](https://github.com/navikt/permitteringsportal/blob/main/src/app/api/client.ts)
* Vi bruker også unleash
* Teste stackdriver for logging
* prometheus og grafana inn i clusteret
* Teste Artifact Registry mer for å vite mer om det er noe å satse på
