# Installation instructions

## For development

Configure keycloak's `LOGOUT_REDIRECT_URL` and `KC_*` variables in the `docker/.env` file.

An example realm and users export is provided in the `keycloak exports` folder.

You can launch the application with `docker compose up`.

## For production

There are no clear instruction on running this in production, you can only inspect [the official documentation](https://superset.apache.org/docs/configuration/configuring-superset/) for basic configuration options. Even so, I will give a short, nonexhaustive guide for it.

There is a `docker-compose-non-dev.yml` that was used for the production deployment. Superset also [ships for helm](https://superset.apache.org/docs/installation/kubernetes/) if you're interested in that.

### Deploying the `docker-compose-non-dev.yml`

To verify nothing is terribly wrong, use the command `docker compose -f docker-compose-non-dev.yml up --build --force-recreate`(`--build --force-recreate` not required but useful). If everything went fine, you should be able to get to a login screen at `http://localhost:8088`. You can stop the compose now, reconfigure and try again.

### Configuration

You will mainly be interested in `docker/.env`.

First, configure access to an external database labeled as `# database configurations (do not modify)` in the envfile. If this works, continue to the next step.

Configure the `KC_*` variables in the envfile to connect to keycloak. I strongly encourage you to import the realm and users from `keycloak exports` and use the `pokadm` keycloak user after a password reset. You should change the `Root URL` and `Web origins` in the keycloak client of the superset application, else you will see `Invalid login. Please try again.`.

This time, you should be able to log into superset, and you should see some example dashboard and be able to access administration in the top right.

After this, you should configure proper keycloak access. If that works, you're done.

#### Additionally

You likely need to change everything that contains the word secret, do a fuzzy search. This includes `SUPERSET_SECRET_KEY` in the envfile.

Consider setting a more restrictive CORS policy in `CORS_OPTIONS` in `docker/pythonpath_dev/superset_config.py`.

DO NOT delete the db service in the compose file, as it contains the example database.

Set `MAPBOX_API_KEY` in `.env` which you can generate on [this site](https://account.mapbox.com/access-tokens/), but it requires credit card details, ask your supervisor.

# Český transkript

# Instrukce pro instalaci

## Pro vývoj

Nastavte proměnné `LOGOUT_REDIRECT_URL` a `KC_*` Keycloaku v souboru `docker/.env`.

Příkladem exportu realm a uživatelů je poskytován v složce keycloak exports.

Aplikaci můžete spustit příkazem `docker compose up`.

## Pro produkční prostředí

Neexistují žádné jasně dobré instrukce pro běh v produkčním prostředí, ale můžete se podívat na o[oficiální dokumentaci](https://superset.apache.org/docs/configuration/configuring-superset/) pro základní možnosti konfigurace. Taky vám dám stručný, neúplný návod.

Existuje soubor `docker-compose-non-dev.yml`, který býval použit pro produkční nasazení. Superset také umožňuje nasazení pomocí [helmu](https://superset.apache.org/docs/installation/kubernetes/), pokud vás něco takového zajímá.

### Nasazení souboru docker-compose-non-dev.yml

Chcete-li ověřit, že nic není hrubě špatné, použijte příkaz `docker compose -f docker-compose-non-dev.yml up --build --force-recreate` (`--build --force-recreate` není vyžadováno, ale je to užitečné). Pokud všechno šlo dobře, měli byste být schopni dostat se na přihlašovací stránku na adrese http://localhost:8088. Nyní můžete zastavit compose, změnit konfiguraci a zkusit znovu.

### Konfigurace

Bude vás především zajímat soubor `docker/.env`.

Nejprve nastavte přístup k externí databázi označené jako `# database configurations (do not modify)` v envfilu. Pokud to funguje, pokračujte dalším krokem.

Nastavte proměnné `KC_*` v souboru env pro připojení k Keycloaku. Silně doporučuji importovat realm a uživatele z `keycloak exports` a použít uživatele Keycloaku `pokadm` po resetu hesla. Změňte `Root URL` a `Web origins` pro keycloak client supersetu, jinak by superset měl vracet `Invalid login. Please try again`.
Tentokrát byste měli být schopni se přihlásit do Supersetu a měli byste vidět některé ukázkové dashboardy a mít přístup k administraci v pravém horním rohu.

Po tomto kroku byste měli nastavit řádný přístup Keycloaku ve vašem vlastním realmu. Pokud to funguje, jste hotovi.

### Dále

Pravděpodobně budete muset změnit všechno, co obsahuje slovo secret, udělejte si pro jistotu fuzzy search. Ale jedno takové místo je `SUPERSET_SECRET_KEY` v souboru env.

Zvažte nastavení více restriktivní politiky CORS v `CORS_OPTIONS` v `docker/pythonpath_dev/superset_config.py`.

NEMAŽTE službu db v souboru compose, protože obsahuje ukázkovou databázi.

Poskytněte `MAPBOX_API_KEY` v `.env`, který lze vygenerovat na [této stránce](https://account.mapbox.com/access-tokens/), ale vyžaduje to poskytnutí kreditní karty, kontaktujte svého nadřízeného.
