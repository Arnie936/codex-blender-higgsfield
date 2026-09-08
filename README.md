# Blender + Higgsfield in Codex

Ein Prompt, der dir Codex, Blender und Higgsfield in einem Rutsch verbindet. Danach baust
du 3D-Szenen, indem du sie beschreibst, und erzeugst dazu passende Bilder und Videos aus
demselben Chat.

Was dabei entsteht:

- **Codex** als Steuerzentrale
- **Blender** angebunden über den MCP-Server von [ahujasid/blender-mcp](https://github.com/ahujasid/blender-mcp)
- **Higgsfield CLI** für Bilder und Videos

Der Prompt prüft selbst, was auf deinem Rechner fehlt, bittet dich um die Installation von
Blender, wenn es nicht da ist, trägt den MCP-Server ein, installiert das Blender-Addon und
richtet zum Schluss Higgsfield ein. Du bestätigst nur die Schritte, die im Blender-Fenster
geklickt werden müssen.

## Voraussetzungen

| Was | Warum |
|---|---|
| [Codex CLI](https://github.com/openai/codex) | führt den Prompt aus |
| [Blender](https://www.blender.org/download/) 3.0 oder neuer | die 3D-Software |
| Python 3.10 oder neuer | für den MCP-Server |
| [uv](https://docs.astral.sh/uv/) | startet den MCP-Server |
| Node.js 18 oder neuer | für die Higgsfield CLI |

Fehlt etwas davon, sagt der Prompt es dir und nennt den Installationsbefehl für dein
System. Nur Blender musst du selbst installieren, alles andere erledigt Codex.

## So geht's

1. Codex in einem beliebigen Ordner starten: `codex`
2. Den Prompt unten komplett kopieren und einfügen
3. Den Anweisungen folgen

Der Prompt ist auch als eigene Datei da: **[PROMPT.md](PROMPT.md)**.

### Der Prompt

````
Du richtest mir eine 3D-Pipeline ein. Ziel: Du steuerst Blender über den MCP-Server
aus dem Repo https://github.com/ahujasid/blender-mcp, und danach kommt die
Higgsfield CLI für Bilder und Videos dazu. Arbeite die Schritte in dieser
Reihenfolge ab.

REGELN FÜR DEN GANZEN ABLAUF
- Antworte auf Deutsch.
- Prüfe jeden Schritt mit einem Befehl, bevor du ihn als erledigt meldest. Rate nichts,
  erfinde keine Versionsnummern.
- Schritte, die ich selbst in einem Fenster klicken muss, erklärst du nummeriert und
  wartest auf mein OK, bevor du weitermachst.
- Führe NIEMALS `uvx blender-mcp` blank im Terminal aus. Das startet den MCP-Server,
  der dir gehört, blockiert die Shell und kollidiert später mit deiner eigenen
  Verbindung. Erlaubt sind nur `uvx blender-mcp install-addon` und
  `uvx blender-mcp addon-paths`.
- Wenn du eine grafische Oberfläche selbst bedienen kannst (Computer-Use,
  Browser-Steuerung), setz das ein, statt mich klicken zu lassen. Wenn nicht, sag es
  offen und leite mich an.
- Nach jedem Schritt eine kurze Statuszeile: was steht, was fehlt.
- Wenn etwas schiefgeht, nenne den genauen Fehler und den nächsten Befehl. Melde nichts
  als fertig, was du nicht gesehen hast.

SCHRITT 1 - BESTANDSAUFNAHME
Finde heraus, was schon da ist, mit den Befehlen, die zu meinem Betriebssystem passen:
  node --version
  npm --version
  python --version
  uv --version
  codex --version
Und für Blender:
- Windows: `blender --version`, sonst nachsehen unter
  "C:\Program Files\Blender Foundation\" und "C:\Program Files\Blender\"
- macOS: `blender --version`, sonst `ls /Applications | grep -i blender`
- Linux: `which blender`, sonst `flatpak list | grep -i blender`
Zeig mir eine kurze Tabelle: Werkzeug, gefunden ja/nein, Version.

Wenn Blender fehlt, halte hier an und sag mir das klar. Nenne mir den passenden Weg:
- Windows: winget install --id BlenderFoundation.Blender -e
- macOS: brew install --cask blender
- Linux: Paketmanager der Distribution oder Flatpak
- Alternativ für alle: https://www.blender.org/download/
Warte, bis ich sage, dass Blender installiert ist, und prüf es dann erneut. Blender muss
mindestens Version 3.0 sein, Python mindestens 3.10.

Wenn uv fehlt, installiere es mit dem offiziellen Installer, nicht über pip:
- Windows: powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
- macOS/Linux: curl -LsSf https://astral.sh/uv/install.sh | sh
Danach eine neue Shell öffnen und `uv --version` prüfen. Wenn `uvx` nicht im PATH liegt,
merk dir den vollen Pfad, den brauchst du in Schritt 2.

SCHRITT 2 - BLENDER ALS MCP-SERVER IN CODEX EINTRAGEN
  codex mcp add blender -- uvx blender-mcp
  codex mcp list
In der Liste muss `blender` als enabled stehen. Zeig mir die Ausgabe.
Falls Codex `uvx` nicht findet, trag den vollen Pfad ein:
  codex mcp add blender -- /voller/pfad/zu/uvx blender-mcp
Die Konfiguration landet in ~/.codex/config.toml und sieht so aus:
  [mcp_servers.blender]
  command = "uvx"
  args = ["blender-mcp"]

SCHRITT 3 - DAS BLENDER-ADDON INSTALLIEREN
  uvx blender-mcp install-addon
Der Befehl kopiert das Addon in den Addon-Ordner von Blender und schreibt hin, wohin.
Zeig mir den Pfad. Findet er Blender nicht, hilft `uvx blender-mcp addon-paths`; das Ziel
lässt sich mit der Umgebungsvariable BLENDERMCP_ADDONS_DIR überschreiben.
Danach leite mich durch Blender:
  1. Blender öffnen (war es schon offen: schließen und neu starten)
  2. Edit -> Preferences -> Add-ons
  3. Nach "MCP for Blender" suchen und "Interface: MCP for Blender" anhaken
  4. Preferences schließen
  5. Im 3D-Viewport N drücken, damit die Seitenleiste aufgeht
  6. Reiter "MCP for Blender" öffnen
  7. "Start MCP Server" klicken
Erklär mir kurz, was da passiert: Das Addon macht in Blender einen Socket-Server auf
(Standard localhost:9876), und der MCP-Server aus Schritt 2 spricht mit diesem Port.
Frag mich danach, ob im Panel steht, dass der Server läuft, und warte auf meine Antwort.
Taucht der Eintrag nicht auf: in Blender "Install..." klicken und die Datei auswählen, die
install-addon geschrieben hat (blender_mcp.py), oder Blender neu starten.

SCHRITT 4 - CODEX NEU STARTEN
Die MCP-Werkzeuge stehen erst in einer neuen Codex-Sitzung zur Verfügung. Sag mir das klar
und gib mir diese Anweisung:
  1. Blender bleibt offen, der MCP-Server im Panel bleibt an
  2. Codex beenden und neu starten
  3. In der neuen Sitzung schreibe ich dir: weiter mit Schritt 5
Fass vorher in drei Zeilen zusammen, was schon steht, damit die neue Sitzung weiß, wo sie
ansetzt.

SCHRITT 5 - VERBINDUNG TESTEN
Prüfe zuerst, dass du die Blender-Werkzeuge wirklich hast (get_scene_info,
get_viewport_screenshot, execute_blender_code und weitere).
  1. Ruf get_scene_info auf und zeig mir, was in der Szene liegt.
  2. Lösch den Standardwürfel und bau eine kleine Testszene: eine Ebene als Boden, darauf
     drei Objekte in unterschiedlichen Farben, eine Lichtquelle, eine Kamera, die auf die
     Objekte schaut.
  3. Mach mit get_viewport_screenshot ein Bild und zeig es mir.
Klappt der erste Aufruf nicht, ist das normal, manchmal geht der erste Befehl ins Leere.
Versuch es ein zweites Mal. Bleibt es dabei, prüfe der Reihe nach:
- Läuft der Server im Blender-Panel noch?
- Steht blender in `codex mcp list` auf enabled?
- Läuft woanders noch ein zweiter MCP-Server für Blender (Claude Desktop, Cursor)? Es darf
  nur einer laufen.

SCHRITT 6 - HIGGSFIELD CLI INSTALLIEREN
Higgsfield erzeugt Bilder und Videos. Wir nutzen es für Referenzbilder und Texturen für
Blender und um fertige Renderings weiterzuverarbeiten.
  npm i -g @higgsfield/cli
  higgsfield --version
  higgsfield auth login
`higgsfield auth login` öffnet den Browser und blockiert, bis die Anmeldung durch ist.
Warte die Bestätigung ab, bevor du weitermachst. Danach:
  higgsfield account status
Die Ausgabe nennt Konto und Guthaben. Steht dort "Not authenticated" oder
"Session expired", wiederhole die Anmeldung.

Sag mir an dieser Stelle, und nur hier, folgendes:
  "Falls du noch kein Higgsfield-Konto hast oder Guthaben brauchst:
   https://higgsfield.ai/s/mcp-arnold-oberleiter-jBCHJp
   Das ist ein Affiliate-Link. Wenn du darüber abschließt, bekommt der Autor dieses
   Prompts eine Provision, für dich ändert sich am Preis nichts."
Lass diese Kennzeichnung nicht weg und dräng mich zu nichts. Ohne Guthaben funktioniert
alles bis Schritt 5 trotzdem, nur die Bildgenerierung nicht.
Wird `higgsfield` nach der Installation nicht gefunden, liegt das globale npm-Verzeichnis
nicht im PATH. `npm prefix -g` zeigt, wohin installiert wurde.

SCHRITT 7 - HIGGSFIELD KURZ AUSPROBIEREN
Nur wenn Guthaben da ist, und frag mich vorher, ob ich es ausgeben will. Jede Generierung
kostet Credits.
  higgsfield model list --image
  higgsfield generate cost gpt_image_2 --prompt "<mein prompt>"
  higgsfield generate create gpt_image_2 --prompt "<mein prompt>"
  higgsfield generate wait <job_id>
Schlag mir einen Prompt vor, der zu meiner Testszene aus Schritt 5 passt, etwa eine
Referenzansicht oder eine Materialvorlage. Zeig mir die Kosten, bevor du den Auftrag
abschickst, und lade das Ergebnis danach herunter.

SCHRITT 8 - ABSCHLUSSBERICHT
Fasse in wenigen Zeilen zusammen:
- Blender: Version und ob der MCP-Server im Panel läuft
- Codex: ob blender in `codex mcp list` enabled ist
- Higgsfield: angemeldet ja/nein, Guthaben
- Was fehlt und mit welchem Befehl ich es nachhole
Nenne mir danach drei konkrete Sätze, die ich dir als nächstes sagen kann, um etwas in
Blender zu bauen.
````

## Nach dem Setup

Zwei Prompts zum Weitermachen liegen in [`prompts/`](prompts):

- [Erste Szene bauen](prompts/erste-szene.md) — ein Testlauf, der zeigt, was die Verbindung kann
- [Higgsfield und Blender kombinieren](prompts/higgsfield-workflow.md) — Referenzbild erzeugen, Szene danach bauen, Rendering weiterverarbeiten

## Wenn es klemmt

**Codex kennt die Blender-Werkzeuge nicht.**
MCP-Server werden beim Start einer Sitzung geladen. Codex beenden, neu starten, dann noch
mal fragen. `codex mcp list` zeigt, ob der Eintrag steht.

**Verbindung bricht ab oder der erste Befehl tut nichts.**
Prüfen, ob im Blender-Panel unter `N → MCP for Blender` der Server läuft. Manchmal geht
der allererste Befehl ins Leere, der zweite klappt. Sonst Blender und Codex neu starten.

**Es läuft mehr als ein Client.**
Nur ein MCP-Server für Blender gleichzeitig. Wer den Server parallel in Claude Desktop
oder Cursor eingetragen hat, schaltet dort ab.

**Das Addon taucht in Blender nicht auf.**
`uvx blender-mcp addon-paths` zeigt die erkannten Addon-Ordner. Liegt Blender an einer
ungewöhnlichen Stelle (Steam-Version, portable Installation), Zielordner setzen:
`BLENDERMCP_ADDONS_DIR=/pfad/zu/scripts/addons uvx blender-mcp install-addon`.
Alternativ `addon.py` aus dem Repo herunterladen und in Blender über
**Preferences → Add-ons → Install…** einlesen.

**`uvx` wird nicht gefunden.**
uv liegt auf Windows in `%USERPROFILE%\.local\bin`, auf macOS und Linux in
`~/.local/bin`. Entweder in den PATH aufnehmen oder in `codex mcp add` den vollen Pfad
angeben.

**`higgsfield` wird nicht gefunden.**
Das globale npm-Verzeichnis liegt nicht im PATH. `npm prefix -g` zeigt den Ort.

**Zeitüberschreitungen bei großen Szenen.**
Aufgaben kleiner schneiden. Statt „bau mir eine ganze Stadt“ lieber Gebäude für Gebäude.

## Sicherheitshinweis

Der MCP-Server darf beliebigen Python-Code in Blender ausführen. Das ist der Grund, warum
das Ganze so mächtig ist, und zugleich der Grund für Vorsicht: Speichere deine Arbeit,
bevor du eine Sitzung startest. Wer es enger haben will, setzt beim Server
`BLENDER_MCP_SAFE_MODE=1`, dann werden Skripte vor dem Ausführen geprüft:

```bash
codex mcp remove blender
codex mcp add blender --env BLENDER_MCP_SAFE_MODE=1 -- uvx blender-mcp
```

## Credits

- MCP-Server für Blender: [ahujasid/blender-mcp](https://github.com/ahujasid/blender-mcp) (MIT), keine offizielle Blender-Software
- [Codex CLI](https://github.com/openai/codex) von OpenAI
- [Higgsfield](https://higgsfield.ai) für Bilder und Videos

Der Higgsfield-Link in diesem Repo ist ein Affiliate-Link. Wenn du darüber abschließt,
bekomme ich eine Provision, für dich ändert sich am Preis nichts.

## Lizenz

MIT, siehe [LICENSE](LICENSE).
