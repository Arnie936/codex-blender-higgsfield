# Higgsfield und Blender kombinieren

Der eigentliche Grund für die beiden Werkzeuge im selben Chat: Erst ein Referenzbild
erzeugen, dann die 3D-Szene danach bauen, am Ende das Rendering in Bewegung bringen.
Alles ab der Linie kopieren und in Codex einfügen.

---

Wir arbeiten in drei Etappen. Higgsfield kostet Credits, also frag mich vor jeder
Generierung, ob ich sie will, und zeig mir vorher die Kosten mit
`higgsfield generate cost`.

**Etappe 1 — Referenz**

Ich sage dir, was ich bauen will. Mach daraus einen Bildprompt und erzeuge ein
Referenzbild:

```
higgsfield generate cost gpt_image_2 --prompt "<dein prompt>"
higgsfield generate create gpt_image_2 --prompt "<dein prompt>"
higgsfield generate wait <job_id>
```

Lade das Ergebnis in den Ordner `referenzen/` und zeig es mir. Passt es nicht, ändere den
Prompt und frag, ob du es noch einmal versuchen sollst.

**Etappe 2 — Nachbau in Blender**

Lies das Referenzbild und leite daraus ab: Formen, Proportionen, Blickwinkel,
Lichtrichtung, Farbstimmung. Sag mir, was du erkennst, bevor du anfängst.

Dann bau die Szene über die Blender-Werkzeuge nach. Nach jedem größeren Schritt einen
`get_viewport_screenshot` und den Vergleich zum Referenzbild in einem Satz: was passt
schon, was noch nicht.

Prüf mit `get_polyhaven_status`, ob du Poly Haven nutzen kannst, und hol dir dort HDRIs
und Texturen, statt Materialien von Hand zusammenzubauen.

**Etappe 3 — Rendering weiterverarbeiten**

Wenn die Szene steht, rendere ein Standbild aus der Kamera und speichere es. Danach kann
Higgsfield daraus ein Video machen, wenn ich das will:

```
higgsfield upload create <pfad/zum/render.png>
higgsfield model list --video
higgsfield generate cost <video_model> --prompt "<bewegung>" --image <upload_id>
higgsfield generate create <video_model> --prompt "<bewegung>" --image <upload_id>
```

Schlag mir eine Kamerabewegung vor, die zur Szene passt, zeig die Kosten und warte auf
mein OK.

Regeln für alle Etappen:

- Erfinde keine Modellnamen. Wenn du unsicher bist, welche Modelle verfügbar sind, ruf
  `higgsfield model list` auf und richte dich danach.
- Nenne mir vor jeder Generierung die Kosten und nach jeder das Restguthaben
  (`higgsfield account status`).
- Bilder und Videos gehören in eigene Ordner, nicht wild in den Projektordner.
