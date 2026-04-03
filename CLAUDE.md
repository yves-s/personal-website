# CLAUDE.md – personal-web Project Instructions

> Dieses Dokument wird von Claude Code automatisch gelesen.
> Projektspezifische Konfiguration (Stack, Build-Commands, Pfade, Pipeline-Verbindung) liegt in `project.json`.

---

## Projekt

**personal-web** – Persoenliche Website / Landing Page von Yves Schleich. Statische HTML-Seite, gehostet auf Vercel (yvesschleich.com). Deutsche Sprache.

---

## Konventionen

### Git
- **Branches:** `feature/{ticket-id}-{kurzbeschreibung}`, `fix/...`, `chore/...`
- **Commits:** Conventional Commits (`feat:`, `fix:`, `chore:`, `docs:`, `test:`)
- **Sprache:** Commit Messages auf Englisch

### Code
- **Sprache:** HTML, CSS (inline), vanilla JavaScript
- **Hosting:** Vercel (statisch, vercel.json fuer Security Headers)
- **Fonts:** Google Fonts (DM Sans, JetBrains Mono)
- **SEO:** Structured Data (JSON-LD), Open Graph, robots.txt, sitemap.xml

### Dateien
- Keine Dateien löschen ohne explizite Anweisung

---

## Autonomer Modus

Dieses Repo nutzt ein Multi-Agent-System. Ob lokal oder auf dem Server:

1. **Arbeite autonom** — keine interaktiven Fragen, keine manuellen Bestätigungen
2. **Plane selbst** — kein Planner-Agent, keine Spec-Datei. Lies betroffene Dateien direkt und gib Agents konkrete Instruktionen
3. **Wenn unklar:** Konservative Lösung wählen, nicht raten
4. **Commit + PR** am Ende des Workflows → Board-Status "in_review"
5. **Merge erst nach Freigabe** — User sagt "passt"/"ship it" oder `/ship`

## Ticket-Workflow (Just Ship Board)

> Nur aktiv wenn `pipeline.workspace_id` und `pipeline.project_id` in `project.json` gesetzt sind. Ohne Pipeline-Config werden diese Schritte übersprungen.

Falls Pipeline konfiguriert ist, sind Status-Updates **PFLICHT**:

| Workflow-Schritt | Board-Status | Wann |
|---|---|---|
| `/ticket` — Ticket schreiben | — | Erstellt ein neues Ticket im Board |
| `/develop` — Ticket implementieren | **`in_progress`** | Sofort nach Ticket-Auswahl, VOR dem Coding |
| `/ship` — PR mergen & abschließen | **`done`** | Nach erfolgreichem Merge |

**Board-API-Credentials auflösen** — bei JEDEM API-Call (Tickets lesen, erstellen, updaten, Status ändern) dieses Snippet verwenden:
```bash
# 1. workspace_id aus project.json lesen
WS_ID=$(node -e "process.stdout.write(require('./project.json').pipeline?.workspace_id || '')")
# 2. Credentials via read-workspace auflösen (IMMER --id, NIEMALS --slug)
WS_JSON=$(bash .claude/scripts/write-config.sh read-workspace --id "$WS_ID")
# 3. board_url und api_key aus dem JSON extrahieren
BOARD_URL=$(echo "$WS_JSON" | node -e "process.stdout.write(JSON.parse(require('fs').readFileSync('/dev/stdin','utf-8')).board_url)")
API_KEY=$(echo "$WS_JSON" | node -e "process.stdout.write(JSON.parse(require('fs').readFileSync('/dev/stdin','utf-8')).api_key)")
# 4. API-Call
curl -s -H "X-Pipeline-Key: $API_KEY" "$BOARD_URL/api/tickets/{N}"
```

**WICHTIG:**
- `api_key` und `board_url` stehen **NICHT** in `project.json` — sie liegen in `~/.just-ship/config.json`
- **NIEMALS** `cat ~/.just-ship/config.json` ausgeben oder manuell nach Workspaces suchen
- **IMMER** `read-workspace --id` mit der UUID aus `project.json` verwenden, **NIEMALS** `--slug`

**Überspringe KEINEN dieser Schritte.** Falls ein Update fehlschlägt, versuche es erneut oder informiere den User.

---

## Architektur

```
/
├── index.html          # Haupt-Landing-Page
├── impressum.html      # Impressum
├── datenschutz.html    # Datenschutzerklaerung
├── vercel.json         # Vercel Config (Security Headers)
├── robots.txt          # Robots
├── sitemap.xml         # Sitemap
├── og-image.png        # Open Graph Bild
├── favicon.ico/.svg    # Favicons
└── yves-schleich.jpg   # Profilbild
```

---

## Sicherheit

- Keine API Keys, Tokens oder Secrets im Code
- Input Validation auf allen Endpoints

---

## Konversationelle Trigger

**"passt"**, **"done"**, **"fertig"**, **"klappt"**, **"sieht gut aus"** → automatisch `/ship` ausführen

**Wichtig:** `/ship` läuft **vollständig autonom** — keine Rückfragen bei Commit, Push, PR oder Merge. Der User hat seine Freigabe bereits gegeben.
