# Zettelspiel PWA

Single-File-HTML-Partyspiel, paketiert als installierbare iOS-PWA. Funktioniert offline nach erstem Online-Aufruf.

## Dateien im Ordner

- `index.html` — die App (Spiellogik unverändert)
- `manifest.webmanifest` — App-Metadaten für Home-Screen-Installation
- `sw.js` — Service Worker (Cache-First, Versions-Cleanup, Runtime-Caching für Google Fonts)
- `icon-192.png`, `icon-512.png` — Home-Screen-Icons

## Auf iPhone installieren (6 Schritte)

### 1. GitHub-Repo erstellen

Auf [github.com/new](https://github.com/new):
- Repository name: `zettelspiel-pwa`
- **Public** auswählen (nötig, weil GitHub Free Pages nur auf public Repos erlaubt)
- "Create repository"

### 2. Dateien hochladen

Im neuen Repo: "uploading an existing file" anklicken → diese 5 Dateien per Drag&Drop reinziehen:
- `index.html`
- `manifest.webmanifest`
- `sw.js`
- `icon-192.png`
- `icon-512.png`

(Die `README.md` ist optional — kann mit hoch oder weggelassen werden.)

Dann unten "Commit changes".

### 3. GitHub Pages aktivieren

Repo → **Settings** → in der linken Sidebar **Pages** → unter "Build and deployment":
- Source: **Deploy from a branch**
- Branch: **main**, Folder: **/ (root)**
- Save

### 4. URL abwarten

Nach 1–2 Minuten erscheint oben auf der Pages-Seite:
> Your site is live at `https://<dein-username>.github.io/zettelspiel-pwa/`

Diese URL aufs iPhone schicken (z. B. iMessage, Mail, AirDrop).

### 5. Auf iPhone in Safari öffnen und installieren

**Wichtig: Safari benutzen, nicht Chrome.** Nur Safari hat "Zum Home-Bildschirm".

1. URL in Safari öffnen → einmal komplett laden lassen (damit Service Worker und Fonts gecacht werden)
2. Teilen-Icon unten antippen (Quadrat mit Pfeil nach oben)
3. Im Menü nach unten scrollen → "Zum Home-Bildschirm"
4. Name "Zettelspiel" bestätigen → "Hinzufügen"

App-Icon mit Z erscheint auf dem Home-Bildschirm.

### 6. Offline testen

Flugmodus an → App vom Home-Bildschirm starten → muss laden und voll funktionieren. Beim ersten Start nach Installation einmal online sein, damit alles im Cache landet.

## Privacy-Hinweis

Das Repo ist technisch public (GitHub-Free-Limitation), aber niemand findet es ohne den Link. GitHub-Pages-URLs werden von Suchmaschinen nur indiziert, wenn sie irgendwo verlinkt sind — und das Spiel speichert keine sensiblen Daten, nur lokalen State im Browser. Wenn du echte Zugangsbeschränkung willst, brauchst du GitHub Pro (privates Repo + Pages) oder einen anderen Host.

## Updates ausspielen

Wenn du `index.html` änderst und hochlädst, holt sich der Service Worker das neue File erst beim nächsten Reload. Damit alte Caches gelöscht werden, **erhöhe die Version** in `sw.js`:

```js
const CACHE_NAME = 'zettelspiel-v2';  // war v1
```

Dann hochladen → beim nächsten Öffnen löscht der SW die alte Version automatisch.

## Lokal testen (vor Upload)

```bash
cd zettelspiel-pwa
python3 -m http.server 8000
```

Browser: http://localhost:8000 — DevTools → Application → Service Workers sollte `sw.js` als "activated" zeigen.
