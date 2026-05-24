# Skill Builder

A coloring-book browser app for the Czechitas data-analysis workshop. Toggle SQL-review rules → watch a simulated agent's verdicts change live.

![Screenshot](screenshot.png)

**Live demo:** https://ishalkin.github.io/czechitas-skill-builder/

**Sibling tools:**
- https://ishalkin.github.io/czechitas-stack-trace/ — same five layers, abstracted into a stack-trace x-ray
- https://ishalkin.github.io/czechitas-pixel-agent/ — the same five layers as a pixel-art office walkthrough

## What it does

Workshop participants click checkboxes representing skill rules ("refuse DROP", "avoid `_OLD` tables"). The center panel shows the resulting `SKILL.md` file. The right panel shows how a simulated agent would respond to four SQL queries — verdicts change in real time as rules toggle.

The app teaches the connection rule-file → agent-behavior by making it visible. Tier 1 (always visible) covers the wow moment in 4 clicks. Tier 2 (collapsed `▾ Pokročilé ☆`) reveals trigger phrases, procedural checks, references, format export, triggering test, token cost, and composite rules.

## How we use it in the workshop

Day 1 — Tanya opens the URL on the projector and tells participants:

> "Это раскраска. Поиграйте 5 минут, пощёлкайте — посмотрите как меняется поведение справа когда вы кликаете чекбоксы слева."

After 5 minutes of play, the framing flips:

> "то что вы только что меняли — это `skill.md`. Тот же файл вы кладёте в `.opencode/skills/` и ваш агент в Snowflake начинает себя так вести."

## Local development

No build step. No dependencies. Just open the file:

```bash
python -m http.server 8000
# → http://localhost:8000
```

To run the embedded self-tests:

```
http://localhost:8000/?test=1
```

A successful run shows "ALL TESTS PASSED (39)" in navy.

## Architecture

Single `index.html` with everything inlined: HTML, CSS, JS, and the Rosie SVG. The app's state lives in one object; any input change triggers a full re-render of the markdown preview and playground.

The simulation is deterministic: rules have tags, tests have triggers (lists of tags), and a rule fires on a test when its tag is in the test's triggers. Verdict = max severity among triggered rules. This is intentionally simple — for a workshop demo, predictable beats clever.

## License

MIT.
