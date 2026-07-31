# ai-agents

Uma coleção de agentes e skills para automatizar fluxos de processamento de documentos.

> Disponível também em: [English](README.md)

---

## Agentes

Agentes são conjuntos de instruções autocontidos que orquestram uma ou mais skills para concluir uma tarefa completa em um único pipeline. São independentes de qualquer ferramenta de IA específica.

| Agente | Descrição |
|---|---|
| [html-translator](agents/html-translator.md) | Extrai o conteúdo limpo de um ou mais arquivos HTML e traduz o resultado para um idioma de destino em um pipeline automatizado — sem confirmações entre as etapas. |

---

## Skills

Skills são conjuntos de instruções focados e reutilizáveis que executam uma única operação bem definida. Podem ser acionadas de forma independente ou compostas por agentes, e são independentes de qualquer ferramenta de IA específica.

| Skill | Descrição |
|---|---|
| [html-extract-content](skills/html-extract-content/skill.md) | Extrai o conteúdo principal de um ou mais arquivos HTML, descarta navegação e elementos de layout, renomeia imagens e produz um arquivo HTML limpo no formato A4 por entrada. |
| [html-translate](skills/html-translate/skill.md) | Traduz um arquivo HTML limpo para qualquer idioma de destino — aceitando códigos de localidade, nomes completos em qualquer idioma ou referências coloquiais — e produz um arquivo HTML traduzido no formato A4 por entrada. |

---

## Como funciona

**Skills** são os blocos fundamentais. Cada skill cuida de uma única responsabilidade e pode ser invocada diretamente quando o usuário precisa apenas daquela operação.

**Agentes** encadeiam skills. Quando o usuário quer executar múltiplas operações em sequência — por exemplo, extrair o conteúdo e depois traduzir — o agente coordena o pipeline automaticamente, eliminando a necessidade de intervenção manual entre as etapas.

---

## Autor

Ricardo de Luna Galdino
