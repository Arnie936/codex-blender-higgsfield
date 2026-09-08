# Blender + Higgsfield in Codex

Ein Prompt, der dir Codex, Blender und Higgsfield in einem Rutsch verbindet. Danach baust
du 3D-Szenen, indem du sie beschreibst, und erzeugst dazu passende Bilder und Videos aus
demselben Chat.

- **Codex** als Steuerzentrale, App oder CLI
- **Blender** angebunden über den MCP-Server von [ahujasid/blender-mcp](https://github.com/ahujasid/blender-mcp)
- **Higgsfield CLI** für Bilder und Videos

## So geht's

1. [Blender](https://www.blender.org/download/) installieren, falls noch nicht da
2. [Codex](https://openai.com/codex/) starten, die App oder `codex` im Terminal
3. Den Prompt unten kopieren, einfügen, abschicken
4. Den Anweisungen folgen

App, CLI und IDE-Erweiterung teilen sich dieselbe Konfiguration (`~/.codex/config.toml`).
Einmal eingerichtet heißt also: überall eingerichtet.

Den Rest holt sich Codex selbst: uv, den MCP-Eintrag, das Blender-Addon, die Higgsfield
CLI. Was in Blender geklickt werden muss, übernimmt er per Computer-Use, sonst sagt er
dir, wo du hinklicken sollst.

### Der Prompt

Auch als eigene Datei: [PROMPT.md](PROMPT.md)

```
Richte mir eine 3D-Pipeline ein: Du steuerst Blender, und Higgsfield erzeugt dazu Bilder
und Videos. Antworte auf Deutsch.

1. BLENDER. Prüf, ob Blender installiert ist. Wenn nicht, sag es mir und warte, bis ich
es nachgeholt habe.

2. BLENDER AN DICH ANBINDEN. Über den MCP-Server aus
https://github.com/ahujasid/blender-mcp. Lies dort das README und mach alles Nötige:
Voraussetzungen, Eintrag in Codex, Addon in Blender. Starte `uvx blender-mcp` dabei nie
blank im Terminal, das ist der Server, der dir gehört.

3. IN BLENDER EINRICHTEN. Nimm Computer-Use und klick das in Blender selbst: Addon
aktivieren und den MCP-Server im Panel starten. Nur was du wirklich nicht kannst, gibst du
mir als nummerierte Anweisung. Sag mir außerdem, wann ich Codex neu starten muss, damit du
die Blender-Werkzeuge bekommst, und fass vorher zusammen, was schon steht.

4. TESTEN. Bau eine kleine Szene und zeig mir einen Screenshot aus dem Viewport.

5. HIGGSFIELD. Installier die CLI (npm i -g @higgsfield/cli), melde mich an und prüf das
Guthaben. Sag mir dabei einmal, und nur hier:
   "Falls du noch kein Higgsfield-Konto hast oder Guthaben brauchst:
    https://higgsfield.ai/s/mcp-arnold-oberleiter-jBCHJp
    Affiliate-Link. Wenn du darüber abschließt, bekommt der Autor dieses Prompts eine
    Provision, für dich ändert sich am Preis nichts."

6. BERICHT. Was läuft, was fehlt, und drei Sätze, die ich dir als Nächstes sagen kann.

Regeln: Prüf jeden Schritt mit einem Befehl, statt zu raten. Alles, was in einem Fenster
geklickt wird, machst du per Computer-Use selbst, statt es mir zu geben. Higgsfield kostet
Credits, frag vor jeder Generierung.
```

## Nach dem Setup

Zwei Prompts zum Weitermachen liegen in [`prompts/`](prompts):

- [Erste Szene bauen](prompts/erste-szene.md)
- [Higgsfield und Blender kombinieren](prompts/higgsfield-workflow.md) — Referenzbild erzeugen, Szene danach bauen, Rendering in Bewegung bringen

## Wenn es klemmt

**Codex kennt die Blender-Werkzeuge nicht.** MCP-Server werden beim Start einer Sitzung
geladen. Codex beenden, neu starten, noch mal fragen.

**Verbindung tut nichts.** Im Blender-Viewport `N` drücken, Reiter *MCP for Blender*, dort
muss der Server laufen. Der allererste Befehl geht manchmal ins Leere, der zweite klappt.

**Mehr als ein Client.** Nur ein MCP-Server für Blender gleichzeitig. Wer denselben Server
in Claude Desktop oder Cursor eingetragen hat, schaltet dort ab.

**Ein Befehl fehlt im PATH.** uv liegt in `~/.local/bin` beziehungsweise
`%USERPROFILE%\.local\bin`, die Higgsfield CLI dort, wohin `npm prefix -g` zeigt.

## Sicherheitshinweis

Der MCP-Server darf beliebigen Python-Code in Blender ausführen. Das ist der Grund, warum
das so mächtig ist, und zugleich der Grund für Vorsicht: Speichere deine Arbeit, bevor du
eine Sitzung startest. Wer es enger haben will, setzt beim Server
`BLENDER_MCP_SAFE_MODE=1`, dann werden Skripte vor dem Ausführen geprüft.

## Credits

- MCP-Server für Blender: [ahujasid/blender-mcp](https://github.com/ahujasid/blender-mcp) (MIT), keine offizielle Blender-Software
- [Codex](https://openai.com/codex/) von OpenAI, als App, CLI oder IDE-Erweiterung
- [Higgsfield](https://higgsfield.ai) für Bilder und Videos

Der Higgsfield-Link in diesem Repo ist ein Affiliate-Link. Wenn du darüber abschließt,
bekomme ich eine Provision, für dich ändert sich am Preis nichts.

## Lizenz

MIT, siehe [LICENSE](LICENSE).
