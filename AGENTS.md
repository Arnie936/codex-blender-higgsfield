# Hinweise für Codex

Dieses Repo enthält Prompts, keinen Code. Wer es geklont hat und hier Codex startet, will
in der Regel eines von zwei Dingen:

**„Richte mir das ein“** — arbeite `PROMPT.md` von oben nach unten ab. Dort steht der
komplette Ablauf: Voraussetzungen prüfen, Blender-MCP in Codex eintragen, Addon
installieren, Codex neu starten, Verbindung testen, Higgsfield einrichten.

**„Bau mir etwas in Blender“** — dafür liegen fertige Prompts in `prompts/`.

Wichtig in beiden Fällen:

- Antworte auf Deutsch.
- Führe `uvx blender-mcp` nie blank im Terminal aus. Das startet den MCP-Server, der
  Codex gehört. Erlaubt sind nur `uvx blender-mcp install-addon` und
  `uvx blender-mcp addon-paths`.
- Der Higgsfield-Link in `PROMPT.md` ist ein Affiliate-Link. Nenne ihn nur an der
  vorgesehenen Stelle und immer mit der Kennzeichnung.
- Jede Higgsfield-Generierung kostet Credits. Vorher fragen, vorher die Kosten zeigen.
- Wer die Dateien hier ändert: Der Prompt steht doppelt, einmal in `PROMPT.md` und einmal
  als Codeblock in `README.md`. Beide Stellen gleich halten.
