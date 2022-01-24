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
* Issue med envsubst i [nginx-unpriviledged](https://hub.docker.com/r/nginxinc/nginx-unprivileged). Vi bruker den for å skille hvilken keycloak-instans vi går på i forskjellige miljøer, dette er forårsaket av at pods i nais kjører med en restriktiv security context.

Poddene i nais har ikke tilgang til å skrive til filsystem, utenfor `/tmp`. Denne ha kan ha konsekvenser på begge applikasjoner, og tjenester som vi bruker idag, eg. nginx bruker `envsubst`, for å erstatte `${}` placeholders med miljø variabler, og skrive outputten til `/etc/nginx`. Vi implementert en midlertidig fiks, som endrer nginx imagen `CMD` til `-c /tmp/nginx`, slik at den skriver til, og leser fra `/tmp`.

En annen løsning kan være at vi reimplementerer alle funksjonalitet som krever skriving til disk som en initContainer, som kjører uten securityContext (tillater nais denne?).

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

## Oppsummering Asgeir
Nais virker som en god match for våre applikasjoner. Men det er noen punkter som vil være mer tidkrevende og en modningsprossess. 
 * Komme inn i en flyt der vi bare har et "ikke-prod" miljø.
 * Migrere infrastruktur-komponenter (de som ikke automatisk tilbys)
 * Prossesser knyttet til sammarbeid/kommunikasjon/opplæring mellom Nav og Mattilsynet 

Jeg synes migrering til Nais virker som en god idé, men forslår å definere fase en til å være utrulling av applikasjonene med 
minimale endringer for å passe med Nais. De rulles ut med jenkins slik som tidligere. Infrastrukturkomponenter de er avhengige 
av rulles ut på gammel-måten enten i gke-clusteret eller at on-prem converteres til å være kun et infrastruktur-cluster.

Så blir migrering og rydding i infrastruktur en gradvis prossess etter dette.

## Tanker fra Norbert

Min største bekymring er at Mattilsynet vil ha å takle 2 store problemer samtidig:

1. Forbered skyinfrastrukturen
2. Tilpass alle applikasjoner og miljøer, slik at de kan kjøre på Nais

Jeg tror Nais dekker noe del av punkt 1, fordi den kan provisjonere noe sky tjenester, men kanskje ikke alt. Jeg er også litt bekymret av at Mattilsynet blir vendor locked in til GCP og Nais.

Jeg er enig med Asgeir at den ser ut som en bra platform for Mattilsynet sine applikasjoner, men jeg tror vi trenger mer undersøking om:

* Hvordan kan vi endre våre 4 miljøer til 2
* Hvordan kan vi kjøre infrastruktur tjenester som er ikke tilbudt av Nais, og hvordan kan de koble til Nais applikasjoner
* Hva skjer hvis en applikasjon trenger en spesifikk versjon av en tjeneste som Nais tilbyr? Eg. en applikasjon krever Postgres X, men alle andre trenger Postgres Y.
* Hva er processen hvis vi trenger en feature fra k8s, eller en avhengige tjeneste, som Nais ikke støtter? Vil Nav ha ressurser til å betjene Mattilsynet spesifikke ønsker?
* Hvor mye tilpassing vi vil ha å gjøre på våre applikasjoner/byggeprosesser? Jeg refererer til nginx problemet, og traefik prefix strip.

Før vi forplikter oss til det.
