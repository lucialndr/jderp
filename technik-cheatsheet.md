# Technik-Cheat-Sheet – model-un.de

Ein Spickzettel zu allem Technischen im Projekt: welche Komponenten benutzt werden, was sie sind, wie die Abläufe auf der Website funktionieren und wie Docker, Umgebungsvariablen und CORS zusammenspielen.

---

## 1. Das Projekt in einem Satz

Eine Webanwendung (auf Basis von **Nuxt**), die MUN-Konferenzen aus einer **PostgreSQL**-Datenbank anzeigt, filtern und auf einer **Karte** darstellen lässt, mit einem geschützten **Admin-Bereich** zum Pflegen der Daten – alles mit **Docker** containerisiert und über eine **CI/CD-Pipeline** automatisiert ausgeliefert.

---

## 2. Die Bausteine – was ist was?

| Komponente | Was ist das? | Rolle bei uns |
|---|---|---|
| **Vue 3** | JavaScript-Framework für Benutzeroberflächen, aufgebaut aus wiederverwendbaren **Komponenten**. | Alle sichtbaren Bausteine (Karten, Filter, Modal …). |
| **Nuxt 4** | Ein „Meta-Framework" auf Vue. Bringt Routing, Server-Rendering und einen eigenen Server mit. | Das Grundgerüst – Frontend **und** Backend in einem Projekt. |
| **Nitro** | Die Server-Engine, die in Nuxt eingebaut ist. | Liefert die Seiten aus und stellt die API bereit; erzeugt beim Build den `.output`-Ordner. |
| **Tailwind CSS v4** | CSS-Framework mit fertigen „Utility"-Klassen fürs Styling. | Das gesamte Design (aus den Figma-Entwürfen). |
| **Prisma** | Ein **ORM** – ein Werkzeug, das Datenbankzugriffe als typsichere JavaScript-Aufrufe statt als rohes SQL erlaubt. | Schreibt/liest Konferenzen & Admins; Datenmodell in `schema.prisma`. |
| **PostgreSQL** | Eine relationale Datenbank (Tabellen mit Zeilen/Spalten). | Speichert alle Konferenzen und den Admin-Account. |
| **Leaflet** | JavaScript-Bibliothek für interaktive Karten. | Die Kartenansicht mit den Konferenz-Markern. |
| **nuxt-auth-utils** | Nuxt-Modul für Login/Session per verschlüsseltem Cookie. | Schützt den Admin-Bereich. |
| **Docker** | Verpackt eine Anwendung samt allem, was sie braucht, in einen **Container** – läuft überall gleich. | App und Datenbank laufen als Container. |
| **Docker Compose** | Startet mehrere Container zusammen nach einer Konfigurationsdatei. | App + PostgreSQL mit einem Befehl. |
| **GitHub Actions** | Automatisierungs-Dienst von GitHub (CI/CD). | Baut, scannt und veröffentlicht das Image bei jedem Push. |
| **Trivy** | Sicherheits-Scanner für Container-Images. | Prüft das Image auf bekannte Schwachstellen. |
| **GHCR** | GitHub Container Registry – ein Speicher für Docker-Images. | Hier landet das fertige Image. |

---

## 3. Wichtige Begriffe kurz erklärt (Glossar)

- **Frontend:** Was im Browser läuft und sichtbar ist (Vue-Komponenten).
- **Backend:** Die Server-Logik, die Daten verarbeitet (bei uns die Nuxt-Server-API / Nitro).
- **SSR (Server-Side Rendering):** Die Seite wird auf dem Server fertig zu HTML gebaut und ausgeliefert → schnell sichtbar, gut auffindbar.
- **Hydration:** Nachdem das fertige HTML da ist, „belebt" Vue es im Browser, sodass Klicks/Interaktionen funktionieren.
- **API / REST-Endpunkt:** Eine Adresse auf dem Server, die Daten liefert oder entgegennimmt (z. B. `GET /api/conferences`).
- **ORM:** Schicht zwischen Code und Datenbank, die SQL abnimmt (hier Prisma).
- **Image vs. Container:** Das **Image** ist der „Bauplan" (eingefroren), der **Container** ist die laufende Instanz davon.
- **Reverse Proxy:** Ein vorgeschalteter Server (z. B. Caddy/nginx), der Anfragen annimmt und an die App weiterleitet.
- **TLS / HTTPS:** Verschlüsselte Verbindung im Browser (das Schloss-Symbol).
- **CORS:** Browser-Schutzregel, die festlegt, welche fremden Webseiten die API aufrufen dürfen.
- **Umgebungsvariablen (ENV):** Einstellungen, die *außerhalb* des Codes gesetzt werden (z. B. Passwörter, DB-Adresse).
- **Session / Cookie:** Kleiner, verschlüsselter Wert im Browser, der nach dem Login „merkt", dass man angemeldet ist.
- **Hash:** Einweg-Verschlüsselung eines Passworts – aus dem Hash lässt sich das Passwort nicht zurückrechnen.
- **CI/CD:** Automatisches Bauen/Testen/Ausliefern bei jeder Code-Änderung.

---

## 4. Wie die Website technisch abläuft

### Seitenaufruf
1. Browser fragt eine Seite an.
2. **Nitro** (Nuxt-Server) rendert die Seite mit Vue zu fertigem HTML (**SSR**) und schickt sie zurück.
3. Im Browser „hydriert" Vue die Seite → sie wird interaktiv.

### Konferenzen anzeigen
```
Browser  →  GET /api/conferences  →  Nitro  →  Prisma  →  PostgreSQL
Browser  ←        JSON-Daten       ←  Nitro  ←  Prisma  ←  PostgreSQL
```
Die Liste wird gerendert, jede Konferenz als Karte.

### Filtern
Läuft **clientseitig** auf den bereits geladenen Daten (Zielgruppe, Sprache, Bundesland, Zeitraum). Der gewählte Filter wird zusätzlich in die **URL** geschrieben, damit man Links teilen/neu laden kann.

### Kartenansicht
**Leaflet** lädt die Kartenkacheln von einem externen Anbieter (z. B. OpenStreetMap) und setzt für jede Konferenz einen **Marker** anhand der gespeicherten Koordinaten (`lat`/`lng`).

### Merkliste
Wird **nur im Browser** gespeichert (`localStorage`) – verlässt das Gerät nicht und braucht keinen Server.

### Admin-Bereich (geschützt)
1. `/admin` aufrufen → Login.
2. `POST /api/auth/login` prüft das Passwort gegen den **Hash** in der DB und setzt ein **Session-Cookie**.
3. Angemeldet kann man Konferenzen anlegen/ändern/löschen → `POST` / `PUT` / `DELETE /api/conferences/...` schreiben über Prisma in die DB.
4. Beim Speichern wird die Adresse serverseitig über einen **Geocoding-Dienst** in Koordinaten umgewandelt.

---

## 5. Datenfluss / Schichten

```
[ Browser ]  ⇄  [ Nuxt:  Frontend (Vue)  +  Server-API (Nitro) ]  ⇄  [ Prisma ]  ⇄  [ PostgreSQL ]
```

- Frontend und Backend liegen im selben Nuxt-Projekt – **kein separates Backend** nötig.
- **Datenbankzugriff und Passwörter bleiben serverseitig** – nichts Geheimes landet im Browser.

---

## 6. Die Datenbank

Zwei Tabellen (vom Prisma-Schema erzeugt):

- **`AdminUser`** – `id`, `email`, `passwordHash` (mit *scrypt* gehasht), `createdAt`.
- **`Conference`** – u. a. `name`, `shortName`, `city`, `state`, `startDate`/`endDate`, `targetAudience`, `language`, `maxParticipants`, `availableSpots`, `description`, `website`, `organizer`, `lat`/`lng`, `committees`, `email`, `isFeatured`, `registrationDeadline`.

Zwei **Enums** (feste Auswahlwerte):
- `TargetAudience`: `schueler`, `studierende`, `beide`
- `ConferenceLanguage`: `de`, `en`, `de_en`

Plus **Indizes** auf `city`, `startDate`, `state`, `targetAudience` für schnelles Filtern/Sortieren.

---

## 7. Authentifizierung (Admin)

- **Erster Aufruf von `/admin`:** Selbst-Registrierung legt den `AdminUser` an. Danach ist die Registrierung **gesperrt** (es darf nur einen geben).
- **Login:** Passwort wird gegen den `passwordHash` geprüft; bei Erfolg setzt `nuxt-auth-utils` ein verschlüsseltes **Session-Cookie** (signiert mit `NUXT_SESSION_PASSWORD`).
- **Geschützte Aktionen** (Anlegen/Ändern/Löschen) prüfen die Session, bevor sie ausgeführt werden.
- Endpunkte: `register.post`, `login.post`, `logout.post`, `status.get` unter `server/api/auth/`.

---

## 8. Docker – wie funktioniert das?

### Image vs. Container
- **Image** = eingefrorener Bauplan der App (alles drin, was sie zum Laufen braucht).
- **Container** = ein laufendes Exemplar dieses Images.

### Dockerfile (Multi-Stage)
Zwei Stufen, damit das Endergebnis klein und sicher ist:
1. **Build-Stage:** installiert Abhängigkeiten, erzeugt den Prisma-Client (`prisma generate`) und baut die App (`nuxt build` → `.output`).
2. **Runtime-Stage:** schlankes Image, das **nur** das gebaute `.output` + den Prisma-Client enthält und mit `node .output/server/index.mjs` startet. (npm wird hier entfernt → kleineres, sichereres Image.)

Die App lauscht auf `HOST=0.0.0.0` und `PORT=3000` – sie macht **kein eigenes HTTPS** (das übernimmt später der Reverse Proxy).

### docker-compose
Startet **zwei Container** zusammen:
- `app` – die Nuxt-Anwendung
- `postgres` – die Datenbank

Sie sind über ein internes Netzwerk verbunden; die App erreicht die DB über den **Hostnamen `postgres`** (nicht `localhost`!). Die Daten liegen in einem **Volume** (`postgres_data`) und überleben einen Neustart.

### Die wichtigsten Befehle
```bash
docker compose up -d --build   # App + DB bauen und starten
docker compose up -d postgres  # nur die Datenbank (für lokale Entwicklung)
docker compose logs -f app     # Logs der App ansehen
docker compose down            # stoppen (Daten bleiben im Volume)
docker compose down -v         # stoppen + Daten löschen
docker compose pull            # neueres Image holen (bei image: statt build:)
```

---

## 9. CI/CD-Pipeline (GitHub Actions)

Bei jedem **Push auf `main`** läuft automatisch:
```
git push  →  Docker-Image bauen  →  Trivy-Security-Scan  →  Push nach GHCR
```
- Findet **Trivy** offene Critical/High-Schwachstellen, wird **nichts veröffentlicht** (Sicherheits-Gate).
- Die Scan-Ergebnisse landen zusätzlich im **Security-Tab** des Repos.
- Definiert in `.github/workflows/`.

---

## 10. Umgebungsvariablen (ENV)

**Warum?** Damit alles, was sich zwischen Umgebungen unterscheidet (DB-Adresse, Secrets), **außerhalb des Codes** liegt – kein Passwort landet im Repository.

| Variable | Bedeutung |
|---|---|
| `DATABASE_URL` | Adresse + Zugangsdaten der PostgreSQL-DB |
| `NUXT_SESSION_PASSWORD` | Geheimer Schlüssel zum Verschlüsseln des Session-Cookies (min. 32 Zeichen) |
| `NUXT_CORS_ORIGINS` | Erlaubte fremde Origins für CORS (leer = aus) |

- **Lokal:** in einer Datei `.env` (nicht im Git, durch `.gitignore` ausgeschlossen).
- **In Produktion:** auf dem Server / bei Vercel / im Hosting als ENV gesetzt.
- In Nuxt werden `NUXT_*`-Variablen automatisch auf die `runtimeConfig` in `nuxt.config.ts` abgebildet.

> Merksatz: Im Container ist `DATABASE_URL` = `…@postgres:5432/…`, vom eigenen Rechner aus `…@localhost:5432/…`.

---

## 11. CORS – was ist das und wie bei uns?

**CORS** (Cross-Origin Resource Sharing) ist eine Schutzregel des Browsers: Standardmäßig darf JavaScript von Website A **nicht** einfach die API von Website B aufrufen.

- Bei uns liegen Frontend und API auf **derselben Domain** → im Normalbetrieb ist **kein** CORS nötig.
- Soll die API trotzdem von einer **anderen** Domain genutzt werden, sind die erlaubten Adressen über `NUXT_CORS_ORIGINS` konfigurierbar. Eine kleine Server-Middleware (`server/middleware/cors.ts`) setzt dann die passenden Header.

---

## 12. Reverse Proxy & TLS

- Die App spricht intern nur **HTTP** auf `:3000` und kümmert sich **nicht** um Zertifikate.
- Im öffentlichen Betrieb steht ein **Reverse Proxy** (z. B. **Caddy**) davor: Der nimmt `https://…` an, **terminiert das TLS** (Verschlüsselung) und leitet die Anfrage intern an Port 3000 weiter.
- Vorteil: Die App bleibt einfach, der Proxy übernimmt HTTPS, Zertifikate und Domain.

---

## 13. Sicherheit auf einen Blick

- Logik & DB-Zugriff laufen **serverseitig** – keine Secrets im Browser.
- Passwörter werden nur als **Hash** (scrypt) gespeichert.
- Login über verschlüsseltes **Session-Cookie**.
- Konfiguration ausschließlich über **ENV** – nichts Geheimes im Code.
- Image wird automatisch mit **Trivy** auf Schwachstellen geprüft.

---

## 14. Schnellreferenz: Befehle

```bash
# Entwicklung
npm install                 # Abhängigkeiten installieren
npm run dev                 # Dev-Server mit Hot-Reload (localhost:3000)
npm run build               # Produktions-Build erzeugen
npm run preview             # Build lokal starten

# Prisma (Datenbank)
npx prisma generate         # Prisma-Client aus dem Schema erzeugen
npx prisma db push          # Schema in die DB schreiben (Tabellen anlegen)
npx prisma db seed          # Beispieldaten laden
npx prisma studio           # grafischer DB-Editor (localhost:5555)

# Docker
docker compose up -d --build
docker compose logs -f app
docker compose down

# Datenbank sichern / einspielen
docker compose exec postgres pg_dump -U mun_user mun_database > dump.sql
cat dump.sql | docker compose exec -T postgres psql -U mun_user -d mun_database

# Secret erzeugen
openssl rand -base64 32     # Wert für NUXT_SESSION_PASSWORD
```

---

## 15. Wo liegt was? (Repo-Struktur)

```
app/                 Frontend (Vue): components/, pages/, composables/, layouts/, assets/
server/              Server-API (Nitro): api/ (conferences, auth, health), middleware/, utils/
prisma/              schema.prisma (Datenmodell), seed.ts (Beispieldaten)
public/              statische Dateien (Bilder, favicon)
shared/              geteilte TypeScript-Typen
Dockerfile           Multi-Stage-Bauplan des Images
docker-compose.yml   App + PostgreSQL
nuxt.config.ts       Nuxt-Konfiguration (inkl. runtimeConfig)
.github/workflows/   CI/CD-Pipeline
```

---

### Die 30-Sekunden-Zusammenfassung für die Präsentation

> „Die Seite ist eine **Nuxt-Anwendung** (Vue im Frontend, ein eingebauter Server für die API). Die Konferenzen liegen in einer **PostgreSQL-Datenbank**, auf die wir typsicher mit **Prisma** zugreifen. Beim Aufruf rendert der Server die Seite vor, im Browser wird sie interaktiv; Filter laufen clientseitig, die Karte über **Leaflet**. Der Admin-Bereich ist per **Session** geschützt. Alles läuft in **Docker-Containern** (App + DB), konfiguriert über **Umgebungsvariablen**, und wird über eine **GitHub-Actions-Pipeline** automatisch gebaut, mit **Trivy** auf Sicherheit geprüft und in die **Container-Registry** veröffentlicht. Im Betrieb steht ein **Reverse Proxy** davor, der HTTPS übernimmt."
