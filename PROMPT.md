# Setup-Prompt

Alles ab der Linie in Codex einfügen und abschicken. Codex arbeitet die Schritte
der Reihe nach ab und fragt nach, wo du klicken musst.

---

Du richtest mir eine 3D-Pipeline ein. Ziel: Du steuerst Blender über den MCP-Server
aus dem Repo https://github.com/ahujasid/blender-mcp, und danach kommt die
Higgsfield CLI für Bilder und Videos dazu. Arbeite die Schritte in dieser
Reihenfolge ab.

**Regeln für den ganzen Ablauf**

- Antworte auf Deutsch.
- Prüfe jeden Schritt mit einem Befehl, bevor du ihn als erledigt meldest. Rate nichts,
  erfinde keine Versionsnummern.
- Schritte, die ich selbst in einem Fenster klicken muss, erklärst du nummeriert und
  wartest auf mein OK, bevor du weitermachst.
- Führe **niemals** `uvx blender-mcp` blank im Terminal aus. Das startet den MCP-Server,
  der dir gehört, blockiert die Shell und kollidiert später mit deiner eigenen
  Verbindung. Erlaubt sind nur die Unterbefehle `uvx blender-mcp install-addon` und
  `uvx blender-mcp addon-paths`.
- Wenn du eine grafische Oberfläche selbst bedienen kannst (Computer-Use,
  Browser-Steuerung), setz das ein, statt mich klicken zu lassen. Wenn nicht, sag es
  offen und leite mich an.
- Nach jedem Schritt eine kurze Statuszeile: was steht, was fehlt.
- Wenn etwas schiefgeht, nenne den genauen Fehler und den nächsten Befehl. Melde nichts
  als fertig, was du nicht gesehen hast.

---

## Schritt 1 — Bestandsaufnahme

Finde heraus, was schon da ist. Nutze die Befehle, die zu meinem Betriebssystem passen:

```
node --version
npm --version
python --version
uv --version
codex --version
```

Und für Blender, je nach System:

- Windows: `blender --version`, sonst nachsehen unter
  `C:\Program Files\Blender Foundation\` und `C:\Program Files\Blender\`
- macOS: `blender --version`, sonst `ls /Applications | grep -i blender`
- Linux: `which blender`, sonst `flatpak list | grep -i blender`

Zeig mir eine kurze Tabelle: Werkzeug, gefunden ja/nein, Version.

**Wenn Blender fehlt**, halte hier an und sag mir das klar. Nenne mir den passenden Weg:

- Windows: `winget install --id BlenderFoundation.Blender -e`
- macOS: `brew install --cask blender`
- Linux: über den Paketmanager der Distribution oder Flatpak
- Alternativ für alle: Download unter https://www.blender.org/download/

Warte, bis ich sage, dass Blender installiert ist, und prüf es dann erneut. Blender
muss mindestens Version 3.0 sein, Python mindestens 3.10.

**Wenn `uv` fehlt**, installiere es mit dem offiziellen Installer, nicht über pip:

- Windows: `powershell -c "irm https://astral.sh/uv/install.ps1 | iex"`
- macOS/Linux: `curl -LsSf https://astral.sh/uv/install.sh | sh`

Danach eine neue Shell öffnen und `uv --version` prüfen. Wenn `uvx` nicht im PATH liegt,
merk dir den vollen Pfad, den brauchst du in Schritt 2.

---

## Schritt 2 — Blender als MCP-Server in Codex eintragen

```
codex mcp add blender -- uvx blender-mcp
codex mcp list
```

In der Liste muss `blender` als *enabled* stehen. Zeig mir die Ausgabe.

Falls `uvx` für Codex nicht auffindbar ist, trag stattdessen den vollen Pfad ein, zum
Beispiel:

```
codex mcp add blender -- /voller/pfad/zu/uvx blender-mcp
```

Zur Kontrolle liegt die Konfiguration in `~/.codex/config.toml` und sieht so aus:

```toml
[mcp_servers.blender]
command = "uvx"
args = ["blender-mcp"]
```

---

## Schritt 3 — Das Blender-Addon installieren

```
uvx blender-mcp install-addon
```

Der Befehl kopiert das Addon in den Addon-Ordner von Blender und schreibt hin, wohin.
Zeig mir den Pfad. Findet er Blender nicht, hilft `uvx blender-mcp addon-paths`, um die
erkannten Ordner zu sehen; das Ziel lässt sich mit der Umgebungsvariable
`BLENDERMCP_ADDONS_DIR` überschreiben.

Danach leite mich durch Blender:

1. Blender öffnen (falls es schon offen war: schließen und neu starten)
2. **Edit → Preferences → Add-ons**
3. Nach `MCP for Blender` suchen und **Interface: MCP for Blender** anhaken
4. Preferences schließen
5. Im 3D-Viewport `N` drücken, damit die Seitenleiste aufgeht
6. Reiter **MCP for Blender** öffnen
7. **Start MCP Server** klicken

Erklär mir kurz, was da passiert: Das Addon macht in Blender einen Socket-Server auf
(Standard localhost:9876), und der MCP-Server aus Schritt 2 spricht mit diesem Port.

Frag mich danach, ob im Panel steht, dass der Server läuft. Warte auf meine Antwort.

Wenn der Eintrag in den Add-ons nicht auftaucht: in Blender **Install…** klicken und die
Datei auswählen, die `install-addon` geschrieben hat (`blender_mcp.py`), oder Blender neu
starten.

---

## Schritt 4 — Codex neu starten

Die MCP-Werkzeuge stehen erst in einer neuen Codex-Sitzung zur Verfügung. Sag mir das
klar und gib mir diese Anweisung:

1. Blender bleibt offen, der MCP-Server im Panel bleibt an
2. Codex beenden und neu starten
3. In der neuen Sitzung schreibe ich dir: **weiter mit Schritt 5**

Fass vorher in drei Zeilen zusammen, was bereits steht, damit die neue Sitzung weiß,
wo sie ansetzt.

---

## Schritt 5 — Verbindung testen

In der neuen Sitzung: Prüfe zuerst, dass du die Blender-Werkzeuge tatsächlich hast
(`get_scene_info`, `get_viewport_screenshot`, `execute_blender_code` und weitere).

1. Ruf `get_scene_info` auf und zeig mir, was in der Szene liegt.
2. Lösch den Standardwürfel und bau eine kleine Testszene: eine Ebene als Boden, darauf
   drei Objekte in unterschiedlichen Farben, eine Lichtquelle, eine Kamera, die auf die
   Objekte schaut.
3. Mach mit `get_viewport_screenshot` ein Bild und zeig es mir.

Klappt der erste Aufruf nicht, ist das normal: manchmal geht der erste Befehl ins Leere.
Versuch es ein zweites Mal. Bleibt es dabei, prüfe der Reihe nach:

- Läuft der Server im Blender-Panel noch?
- Steht `blender` in `codex mcp list` auf enabled?
- Läuft irgendwo sonst noch ein zweiter MCP-Server für Blender (Claude Desktop, Cursor)?
  Es darf nur einer laufen.

---

## Schritt 6 — Higgsfield CLI installieren

Higgsfield erzeugt Bilder und Videos. Wir nutzen es, um Referenzbilder und Texturen für
Blender zu bekommen und um fertige Renderings weiterzuverarbeiten.

```
npm i -g @higgsfield/cli
higgsfield --version
higgsfield auth login
```

`higgsfield auth login` öffnet den Browser und blockiert, bis die Anmeldung durch ist.
Warte die Bestätigung ab, bevor du weitermachst. Danach:

```
higgsfield account status
```

Die Ausgabe nennt Konto und Guthaben. Steht dort `Not authenticated` oder
`Session expired`, wiederhole die Anmeldung.

**Sag mir an dieser Stelle, und nur hier, folgendes:**

> Falls du noch kein Higgsfield-Konto hast oder Guthaben brauchst:
> https://higgsfield.ai/s/mcp-arnold-oberleiter-jBCHJp
> Das ist ein Affiliate-Link. Wenn du darüber abschließt, bekommt der Autor dieses
> Prompts eine Provision, für dich ändert sich am Preis nichts.

Lass diese Kennzeichnung nicht weg und dräng mich zu nichts. Ohne Guthaben funktioniert
alles bis Schritt 5 trotzdem, nur die Bildgenerierung nicht.

Wird `higgsfield` nach der Installation nicht gefunden, liegt das globale npm-Verzeichnis
nicht im PATH. `npm prefix -g` zeigt, wohin installiert wurde.

---

## Schritt 7 — Higgsfield kurz ausprobieren

Nur wenn Guthaben da ist, und frag mich vorher, ob ich es ausgeben will. Jede Generierung
kostet Credits.

```
higgsfield model list --image
higgsfield generate cost gpt_image_2 --prompt "<mein prompt>"
higgsfield generate create gpt_image_2 --prompt "<mein prompt>"
higgsfield generate wait <job_id>
```

Schlag mir einen Prompt vor, der zu meiner Testszene aus Schritt 5 passt, zum Beispiel
eine Referenzansicht oder eine Materialvorlage. Zeig mir die Kosten, bevor du den Auftrag
abschickst, und lade das Ergebnis danach herunter.

---

## Schritt 8 — Abschlussbericht

Fasse in wenigen Zeilen zusammen:

- Blender: Version und ob der MCP-Server im Panel läuft
- Codex: ob `blender` in `codex mcp list` enabled ist
- Higgsfield: angemeldet ja/nein, Guthaben
- Was fehlt und mit welchem Befehl ich es nachhole

Nenne mir danach drei konkrete Sätze, die ich dir als nächstes sagen kann, um etwas in
Blender zu bauen.
