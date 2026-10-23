# Vermietung – Web-App (Version 1)

Läuft als Web-App (PWA) auf iPhone und Android, gehostet über GitHub Pages unter `/vermietung/`. Im Netz liegt nur der App-Code, die Daten bleiben verschlüsselt auf dem Gerät.

## Dateien
- `index.html` (komplette App), `sw.js` (Offline-Cache, nur App-Dateien, eigener Bereich `/vermietung/`), `manifest.webmanifest`, Icons.

## Sicherheit (wie Finanzplanung)
- Daten: AES-256-GCM mit zufälligem Datenschlüssel (DEK), gespeichert in localStorage (`vm.web.v3`).
- Der DEK liegt nur eingepackt im Tresor, mit mehreren Schlössern:
  - `rec`: Wiederherstellungscode (24 Zeichen, PBKDF2-SHA256 150.000 Runden). Immer vorhanden.
  - `prf`: Face ID / Fingerabdruck über Passkey mit WebAuthn-PRF (HKDF-SHA256).
  - `gate` + `dev`: Falls kein PRF: Passkey als Sperre plus nicht exportierbarer Geräteschlüssel in IndexedDB (`vermietung`).
  - nur `dev`: ohne Passkey, öffnet direkt.
- Content-Security-Policy: `connect-src 'none'`, keine externen Quellen.
- Sicherung = kompletter Tresor als JSON-Datei über das Teilen-Menü. Erinnerung nach 7 Tagen.
- Sperre nach mehr als 1 Minute im Hintergrund (bei Face ID).
- Import: `Vermietung-Daten.json` (aus der Excel) oder Backups der Scriptable-Version (`VMB1.…` mit Backup-Passwort).
