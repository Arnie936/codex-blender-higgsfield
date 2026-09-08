# Erste Szene bauen

Für die Sitzung nach dem Setup. Blender ist offen, der MCP-Server im Panel läuft.
Alles ab der Linie kopieren und in Codex einfügen.

---

Bau mir eine Szene in Blender und zeig mir zwischendurch, was passiert.

Vorher: Ruf `get_scene_info` auf und sag mir, was gerade in der Datei liegt. Wenn dort
noch etwas steht, das ich behalten könnte, frag nach, bevor du löschst.

Die Szene:

- ein Innenraum mit Boden, Rückwand und Seitenwand
- ein Schreibtisch mit Stuhl
- auf dem Tisch ein Laptop, eine Tasse und eine kleine Pflanze
- eine große Lichtquelle von links wie ein Fenster, dazu ein schwaches Fülllicht
- eine Kamera in Augenhöhe, leicht schräg auf den Tisch

Arbeite in dieser Reihenfolge und mach nach jedem Block einen Screenshot mit
`get_viewport_screenshot`, damit ich mitsehe:

1. Raum und Grundformen
2. Möbel
3. Objekte auf dem Tisch
4. Materialien und Farben
5. Licht und Kamera

Regeln:

- Benenne jedes Objekt sinnvoll, keine Namen wie `Cube.003`.
- Halte die Maße plausibel: Tischhöhe etwa 75 cm, Laptop etwa 32 cm breit.
- Wenn ein Schritt fehlschlägt, zeig mir den Fehler und repariere ihn, bevor du
  weitermachst.
- Frag mich nach jedem Screenshot, ob es so passt, und ändere auf Zuruf.

Wenn du Assets brauchst, prüf mit `get_polyhaven_status`, ob Poly Haven verfügbar ist, und
hol dir von dort Texturen oder HDRIs, statt alles von Hand zu bauen.

Am Ende: Speicher die Datei als `erste-szene.blend` in meinem aktuellen Ordner und sag
mir, welche Objekte in der Szene liegen.
