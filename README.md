# Thought Orbits

Radial cause-and-effect maps for thinking through a topic.

Put one topic in the center. Its **causes** orbit on the left and its **effects** on the right, in rings by degree (1°, 2°, 3°…). Push any idea up or down to run a scenario, then compare what the map expects with what you have actually seen.

The first map is **RAM prices**, seeded with sourced 2026 data on memory prices and smartphone shipments.

## Using it

- **Tap an idea** to edit it: name, notes, how it connects to its parent, strength, scenario push, and what you have seen in reality.
- **Add a cause / Add an effect** grows the next ring out.
- **Link across** connects ideas in different branches. These hidden links are often where the insight is.
- **Spacing** slider (top left) spreads the rings apart so labels are easier to read.
- **Insights** shows the strongest chains, the reality check, and any feedback loops.
- **Copy outline** gives a plain-text version to paste into a chat for deeper analysis.

Edits save in your browser, per map, on that device only. They do not change the files in this repository until you export and commit them.

## Reading the map

| Mark | Meaning |
|---|---|
| ▲ amber fill | Model expects it to rise |
| ▼ blue fill | Model expects it to fall |
| ► on a line | What pushes what |
| − on a line | Pushes the opposite way |
| Thick ring | Scenario input you pushed |
| Red dashed ring | Model and reality disagree |
| `seen ▲ +23% YoY` | What you observed, with a figure |

## How the model calculates

Each arrow passes on `strength × 20%` of the change and flips it on a − arrow. Effects that meet add up and are capped at 100%. Feedback loops are detected and listed but not run, so treat results as the first wave of effects.

## Start a new analysis

1. Open the page and press **New map**. A blank draft opens and saves on your device.
2. Build it out, then press **Export file** and copy the JSON.
3. Save it in this repository as `maps/<your-topic>.json` (lowercase, hyphens).
4. Add it to `maps/index.json`:
   ```json
   {"slug": "your-topic", "title": "Your topic"}
   ```
5. Commit. It now appears in the map picker, and opens directly at `?map=your-topic`.

You can also start from `maps/template.json`, or write the JSON by hand.

## Map file format

```json
{
  "title": "RAM prices",
  "description": "One line on what the map covers",
  "updated": "2026-10-13",
  "nodes": [
    {"id": "c", "label": "RAM prices", "parent": null},
    {
      "id": "ai",
      "label": "AI data center buildout",
      "parent": "c",
      "flow": "in",
      "sign": 1,
      "w": 5,
      "shock": 1,
      "obs": "up",
      "fig": "Mobile DRAM +78–83% QoQ in 2Q26",
      "src": "TrendForce, May 2026",
      "url": "https://…",
      "note": "Why it connects"
    }
  ],
  "links": [
    {"a": "ai", "b": "hbm", "sign": 1, "w": 5, "note": "a pushes b"}
  ]
}
```

| Field | Values |
|---|---|
| `parent` | `null` for the center, otherwise the id of the idea one ring in |
| `flow` | `"in"` = this idea drives its parent (a cause), `"out"` = the parent drives this idea (an effect) |
| `sign` | `1` = moves the same way, `-1` = moves the opposite way |
| `w` | Strength 1 to 5 |
| `shock` | Scenario push: `1` up, `-1` down, `0` none |
| `obs` | What you have seen: `"up"`, `"down"`, `"flat"` or `""` |
| `fig`, `src`, `url` | The observed figure, its source, and a link |

## Running locally

The page loads map files with `fetch`, so open it through a local server rather than double-clicking the file:

```sh
python3 -m http.server 8000
# then open http://localhost:8000
```
