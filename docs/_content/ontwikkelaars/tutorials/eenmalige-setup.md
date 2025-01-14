---
title: Eenmalige setup na het opstarten van de containers
weight: 90
---

Deze tutorial beschrijft de eenmalige configuratie van de referentie-
implementaties. De [autorisatieslides](./autorisatie.pptx) zoals gegeven op het
API-lab zijn ook beschikbaar.

Let op: deze setup hoef je slechts 1 keer uit te voeren, typisch als je voor
het eerst de containers opstart. Indien je dit toch een tweede keer uitvoert,
dan zal je zien dat de gegevens al ingevuld zijn of configuratie al bestaat.

## Wat zijn de vereisten voor deze tutorial?

* `docker` en `docker-compose` om lokaal op je (ontwikkelmachine) de
  componenten te hosten. Zie ['aan de slag'](../aan-de-slag) voor een
  uitgebreide beschrijving.

* Het handigste is om de containers in 1 command prompt te hebben draaien, en
  extra commando's in een tweede prompt ernaast uit te voeren. Zorg dat beide
  prompts zich in de juiste directory bevinden: `/pad/naar/gemma-zaken/infra`.

We nemen aan dat nu de containers draaien na het uitvoeren van
`docker-compose up` (of een variatie hierop).

## Aanmaken supergebruikers

Supergebruikers laten je toe om in de administratieve omgeving van de
componenten de nodige instellingen te maken. Deze moeten initieel in de
database aangemaakt worden.

Voer op de command prompt het `createsuperuser` commando uit voor elke
component:

```bash
docker-compose exec zrc_web src/manage.py createsuperuser
Username: bob
Email address: bob@example.com
Password:
Password (again):
Superuser created successfully.
```

De commando's zijn interactief, en wachtwoorden die je intikt _zie je niet_.

Doe dit ook voor de andere componenten:

```bash
docker-compose exec drc_web src/manage.py createsuperuser
```

```bash
docker-compose exec ztc_web src/manage.py createsuperuser
```

```bash
docker-compose exec brc_web src/manage.py createsuperuser
```

```bash
docker-compose exec nrc_web src/manage.py createsuperuser
```

```bash
docker-compose exec ac_web src/manage.py createsuperuser
```

## API-credentials genereren

Gebruik de [tokentool](https://ref.tst.vng.cloud/tokens/) om een _Client ID_
en _Secret_ te genereren, of verzin deze zelf. Deze credentials moet je straks
opvoeren.

## Configuratie in de administratieve interface

Het IP-adres uit de ['aan de slag'](../aan-de-slag) voorbereiding is hier
nodig om de componenten via de browser aan te spreken.

### ZRC

1. Open in je browser `http://<zrc-ip>:8000/admin/` en log in met je 
   gebruikersnaam en wachtwoord uit de vorige stap.

2. Navigeer naar **Sites** > **Sites** en klik `example.com` aan.

3. Wijzig 'Domeinnaam' naar `<zrc-ip>:8000` en wijzig 'Weergavenaam' naar `ZRC`

4. Sla de wijzigingen op

5. Navigeer terug naar de **Voorpagina**

6. Navigeer naar **VNG_API_COMMON** > **JWT secrets** en klik rechtsboven
   op **JWT Secret toevoegen**

7. Vul bij **Identifier** het _Client ID_ in en bij **Secret** het _Secret_.
   Beide komen uit de stap [api-credentials genereren](#api-credentials-genereren).

   Deze credentials laten toe om met de API van het ZRC te communiceren.

8. Ga een stap terug naar **VNG_API_COMMON** en navigeer naar
   **API credentials**. Klik rechtsboven op **API Credential toevoegen**.

   Deze credentials worden gebruikt als het ZRC met een _andere_ API moet
   communiceren (zoals een NRC).

9. Configureer credentials voor het NRC:

    * Vul bij **Api root** het adres van het NRC in: `http://<nrc-ip>:8004/api/v1`
    * Vul bij **Client id** het _Client ID_ in
    * Vul bij **Secret** het _Secret_ in

    Klik vervolgens op **Opslaan en nieuwe toevoegen**

10. Configureer credentials voor het DRC:

    * Vul bij **Api root** het adres van het DRC in: `http://<drc-ip>:8001/api/v1`
    * Vul bij **Client id** het _Client ID_ in
    * Vul bij **Secret** het _Secret_ in

    Klik vervolgens op **Opslaan en nieuwe toevoegen**

11. Configureer credentials voor het ZTC:

    * Vul bij **Api root** het adres van het ZTC in: `http://<ztc-ip>:8002/api/v1`
    * Vul bij **Client id** het _Client ID_ in
    * Vul bij **Secret** het _Secret_ in

    Klik vervolgens op **Opslaan**

12. Configureer credentials voor het AC:

    * Vul bij **Api root** het adres van het ZTC in: `http://<ac-ip>:8005/api/v1`
    * Vul bij **Client id** het _Client ID_ in
    * Vul bij **Secret** het _Secret_ in

    Klik vervolgens op **Opslaan**

13. Ga terug naar de **Voorpagina**

14. Navigeer naar **NOTIFICATIES** > **Notificatiescomponentconfiguratie**

15. Wijzig de **Api root** naar `http://<nrc-ip>:8004/api/v1` - dit is je eigen,
    lokale NRC. Vul opnieuw je **Client id** en **Secret** in.
    
16. Navigeer naar **AUTHORIZATIONS** > **Autorisatiecomponentconfiguratie**

17. Wijzig de **Api root** naar `http://<nrc-ip>:8005/api/v1` - dit is je eigen,
    lokale AC. Vul opnieuw je **Client id** en **Secret** in.

### DRC

1. Open in je browser `http://<drc-ip>:8001/admin/` en log in met je 
   gebruikersnaam en wachtwoord uit de vorige stap.

2. Navigeer naar **Sites** > **Sites** en klik `example.com` aan.

3. Wijzig 'Domeinnaam' naar `<drc-ip>:8001` en wijzig 'Weergavenaam' naar `DRC`

4. Sla de wijzigingen op

5. Navigeer terug naar de **Voorpagina**

6. Navigeer naar **VNG_API_COMMON** > **JWT secrets** en klik rechtsboven
   op **JWT Secret toevoegen**

7. Vul bij **Identifier** het _Client ID_ in en bij **Secret** het _Secret_.
   Beide komen uit de stap [api-credentials genereren](#api-credentials-genereren).

   Deze credentials laten toe om met de API van het DRC te communiceren.

8. Ga een stap terug naar **VNG_API_COMMON** en navigeer naar
   **API credentials**. Klik rechtsboven op **API Credential toevoegen**.

   Deze credentials worden gebruikt als het DRC met een _andere_ API moet
   communiceren (zoals een NRC).

9. Configureer credentials voor het NRC:

    * Vul bij **Api root** het adres van het NRC in: `http://<nrc-ip>:8004/api/v1`
    * Vul bij **Client id** het _Client ID_ in
    * Vul bij **Secret** het _Secret_ in

    Klik vervolgens op **Opslaan en nieuwe toevoegen**

10. Configureer credentials voor het ZRC:

    * Vul bij **Api root** het adres van het ZRC in: `http://<zrc-ip>:8000/api/v1`
    * Vul bij **Client id** het _Client ID_ in
    * Vul bij **Secret** het _Secret_ in

    Klik vervolgens op **Opslaan en nieuwe toevoegen**

11. Configureer credentials voor het ZTC:

    * Vul bij **Api root** het adres van het ZTC in: `http://<ztc-ip>:8002/api/v1`
    * Vul bij **Client id** het _Client ID_ in
    * Vul bij **Secret** het _Secret_ in

    Klik vervolgens op **Opslaan**
    
12. Configureer credentials voor het AC:

    * Vul bij **Api root** het adres van het ZTC in: `http://<ac-ip>:8005/api/v1`
    * Vul bij **Client id** het _Client ID_ in
    * Vul bij **Secret** het _Secret_ in

    Klik vervolgens op **Opslaan**

13. Ga terug naar de **Voorpagina**

14. Navigeer naar **NOTIFICATIES** > **Notificatiescomponentconfiguratie**

15. Wijzig de **Api root** naar `http://<nrc-ip>:8004/api/v1` - dit is je eigen,
    lokale NRC. Vul opnieuw je **Client id** en **Secret** in.
    
16. Navigeer naar **AUTHORIZATIONS** > **Autorisatiecomponentconfiguratie**

17. Wijzig de **Api root** naar `http://<nrc-ip>:8005/api/v1` - dit is je eigen,
    lokale AC. Vul opnieuw je **Client id** en **Secret** in.

### ZTC

1. Open in je browser `http://<ztc-ip>:8002/admin/` en log in met je 
   gebruikersnaam en wachtwoord uit de vorige stap.

2. Navigeer naar **MISCELLANEOUS** > **Sites** en klik `example.com` aan.

3. Wijzig 'Domeinnaam' naar `<ztc-ip>:8002` en wijzig 'Weergavenaam' naar `ZTC`

4. Sla de wijzigingen op

5. Navigeer terug naar de **Voorpagina**

6. Navigeer naar **MISCELLANEOUS** > **JWT secrets** en klik rechtsboven
   op **JWT Secret toevoegen**

7. Vul bij **Identifier** het _Client ID_ in en bij **Secret** het _Secret_.
   Beide komen uit de stap [api-credentials genereren](#api-credentials-genereren).

   Deze credentials laten toe om met de API van het ZTC te communiceren.
   
8. Ga een stap terug naar **MISCELLANEOUS** en navigeer naar
   **API credentials**. Klik rechtsboven op **API Credential toevoegen**.

   Deze credentials worden gebruikt als het ZTC met een _andere_ API moet
   communiceren (zoals een AC).
   
9. Configureer credentials voor het AC:

    * Vul bij **Api root** het adres van het ZTC in: `http://<ac-ip>:8005/api/v1`
    * Vul bij **Client id** het _Client ID_ in
    * Vul bij **Secret** het _Secret_ in

    Klik vervolgens op **Opslaan**
  
10. Navigeer naar **MISCELLANEOUS** > **Autorisatiecomponentconfiguratie**

11. Wijzig de **Api root** naar `http://<nrc-ip>:8005/api/v1` - dit is je eigen,
    lokale AC. Vul opnieuw je **Client id** en **Secret** in.   

### NRC

1. Open in je browser `http://<nrc-ip>:8004/admin/` en log in met je 
   gebruikersnaam en wachtwoord uit de vorige stap.

2. Navigeer naar **VNG_API_COMMON** > **JWT secrets** en klik rechtsboven
   op **JWT Secret toevoegen**

7. Vul bij **Identifier** het _Client ID_ in en bij **Secret** het _Secret_.
   Beide komen uit de stap [api-credentials genereren](#api-credentials-genereren).

   Deze credentials laten toe om met de API van het NRC te communiceren.

### AC

1. Open in je browser `http://<ac-ip>:8005/admin/` en log in met je 
   gebruikersnaam en wachtwoord uit de vorige stap.

2. Navigeer naar **Sites** > **Sites** en klik `example.com` aan.

3. Wijzig 'Domeinnaam' naar `<ac-ip>:8005` en wijzig 'Weergavenaam' naar `AC`

4. Sla de wijzigingen op

5. Navigeer terug naar de **Voorpagina**

6. Navigeer naar **VNG_API_COMMON** > **JWT secrets** en klik rechtsboven
   op **JWT Secret toevoegen**

7. Vul bij **Identifier** het _Client ID_ in en bij **Secret** het _Secret_.
   Beide komen uit de stap [api-credentials genereren](#api-credentials-genereren).

   Deze credentials laten toe om met de API van het AC te communiceren.

8. Ga een stap terug naar **VNG_API_COMMON** en navigeer naar
   **API credentials**. Klik rechtsboven op **API Credential toevoegen**.

   Deze credentials worden gebruikt als het AC met een _andere_ API moet
   communiceren (zoals een NRC).

9. Configureer credentials voor het NRC:

    * Vul bij **Api root** het adres van het NRC in: `http://<nrc-ip>:8004/api/v1`
    * Vul bij **Client id** het _Client ID_ in
    * Vul bij **Secret** het _Secret_ in

    Klik vervolgens op **Opslaan en nieuwe toevoegen**

13. Ga terug naar de **Voorpagina**

14. Navigeer naar **NOTIFICATIES** > **Notificatiescomponentconfiguratie**

15. Wijzig de **Api root** naar `http://<nrc-ip>:8004/api/v1` - dit is je eigen,
    lokale NRC. Vul opnieuw je **Client id** en **Secret** in.
    
16. Navigeer naar **AUTHORIZATIONS** > **Autorisatiecomponentconfiguratie**

17. Wijzig de **Api root** naar `http://<nrc-ip>:8005/api/v1` - dit is je eigen,
    lokale AC. Vul opnieuw je **Client id** en **Secret** in.
    
18. Ga terug naar de **Voorpagina**

19. Navigate to **AUTHORIZATIONS** > **Applicaties** 
    Configure access of all consumers to all providers. For testing purposes
    a superuser creation is allowed:
    
    * Fill in **Client ids** with a comma separated list of client identifiers. 
    * Fill in **Label** with the name of the consumer
    * Tick **Heeft alle autorisaties** flag which grants superuser rights.  
    
    If you don't want to have a superuser access choose specific rights in the 
    section **Autorisaties**.     

## Registratie van de kanalen

Het ZRC, DRC en AC moeten hun notificatiekanaal registeren. Dit doe je op de
command prompt:

```bash
docker-compose exec zrc_web src/manage.py register_kanaal
# Registered kanaal 'zaken' with http://<nrc-ip>:8004/api/v1
```

```bash
docker-compose exec drc_web src/manage.py register_kanaal
# Registered kanaal 'documenten' with http://<nrc-ip>:8004/api/v1
```

```bash
docker-compose exec ac_web src/manage.py register_kanaal
# Registered kanaal 'autorisaties' with http://<nrc-ip>:8004/api/v1
```


[token-generator]: https://ref.tst.vng.cloud/tokens/
