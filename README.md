# Egebjerggaard Markstyring

Dansk, selvhostet markstyringsapp til overblik over marker, gødningsplaner, væksttilsyn og medarbejderadgang. Appen bruger kun Python-standardbiblioteket og SQLite; der skal ikke installeres pakker eller oprettes en cloudkonto.

## Start appen

1. Installér Python 3.10 eller nyere.
2. På Windows dobbeltklikker du på `start-markstyring.bat`. Serveren åbner derefter appen i browseren. Alternativt åbner du en terminal i projektmappen og kører `py server.py`.
3. Hvis browseren ikke åbner automatisk, gå til [http://127.0.0.1:8000](http://127.0.0.1:8000).
4. Lad serverens terminalvindue være åbent, mens du bruger appen. Stop serveren med `Ctrl+C`.
5. Første gang opretter du direktørkontoen og vælger selv en adgangskode på mindst 12 tegn.

Åbn ikke `index.html` direkte: siden kræver serveren for at hente og gemme data. Hvis du alligevel gør det, viser siden en vejledning til at starte appen.

Databasen ligger som standard i `data/markstyring.sqlite3` ved siden af programmet. Sikkerhedskopiér denne fil regelmæssigt. Placeringen kan ændres med miljøvariablen `FARM_APP_DB`, adressen med `HOST` (standard `127.0.0.1`) og porten med `PORT`.

## Medarbejdere og roller

Direktøren kan oprette personlige medarbejderkonti og aktivere eller deaktivere adgang. Medarbejdere kan oprette og redigere mark-, gødnings- og tilsynsdata. Sletning og brugeradministration er forbeholdt direktøren. Brugerne skifter selv adgangskode under **Medarbejdere → Sikkerhed**.

Gå ikke på kompromis med adgangskoder. Appen gemmer adgangskoder som PBKDF2-SHA256-hashes med individuelt tilfældigt salt; den gemmer ikke klartekst-adgangskoder.

## Vigtigt om anvendelse

Appen binder som standard kun til `127.0.0.1`, så den er kun tilgængelig på den computer, hvor serveren kører. Hvis medarbejdere skal logge ind fra deres egne computere eller telefoner, skal appen sættes op på en betroet fælles server bag en HTTPS-reverse-proxy, med firewall, sikkerhedskopiering og passende driftsansvar. Sæt `COOKIE_SECURE=1`, når reverse-proxyen leverer HTTPS, så login-cookies kun sendes over en krypteret forbindelse. Hold app-serverens port privat bag proxyen; bind den ikke direkte til det offentlige internet. `HOST` kan ændres, hvis proxyen kører i et separat, afskærmet netværk.

Gødningsmål, produktvalg og agronomiske forslag skal kontrolleres mod gældende regler, jordprøver og faglig rådgivning. Appen beregner eller godkender ikke lovpligtige kvælstofkvoter.

## Test

Kør de automatiserede backend- og adgangstests med `py -m unittest test_server.py`.
