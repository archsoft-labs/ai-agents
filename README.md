# ai-agents

A curated collection of agents and skills for automating document processing workflows.

> Also available in: [Português (Brasil)](README.pt-br.md)

---

## Agents

Agents are self-contained instruction sets that orchestrate one or more skills to complete a full task in a single pipeline. They are independent of any specific AI coding tool.

| Agent | Description |
|---|---|
| [html-translator](agents/html-translator.md) | Extracts clean content from one or more HTML files and translates the result into a target language in a single automated pipeline — no confirmation required between steps. |

---

## Skills

Skills are focused, reusable instruction sets that perform a single well-defined operation. They can be triggered independently or composed by agents, and are independent of any specific AI coding tool.

| Skill | Description |
|---|---|
| [html-extract-content](skills/html-extract-content/skill.md) | Extracts the main content from one or more HTML files, discards navigation and layout noise, handles image renaming, and produces one clean A4-formatted HTML file per input. |
| [html-translate](skills/html-translate/skill.md) | Translates a clean HTML file into any target language — accepting locale codes, full names in any language, or colloquial references — and produces one A4-formatted translated HTML file per input. |

---

## How it Works

**Skills** are the building blocks. Each skill handles one responsibility and can be invoked directly when the user needs only that operation.

**Agents** chain skills together. When the user wants to perform multiple operations in sequence — for example, extract content and then translate it — an agent coordinates the pipeline automatically, removing the need for manual hand-off between steps.

---

## Author

Ricardo de Luna Galdino
