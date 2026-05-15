---
title: "Interpreter"
source: "https://obsidian.md/help/web-clipper/interpreter"
author:
published:
created: 2026-05-15
description: "Interpreter - Obsidian Help"
tags:
  - "clippings"
---
<iframe src="https://obsidian.md/help/web-clipper/interpreter" allow="fullscreen" allowfullscreen="" style="height: 100%; width: 100%; aspect-ratio: 16 / 9;"></iframe>

- Obsidian Web Clipper 的「解釋器」(Interpreter) 功能允許使用者透過自訂 JavaScript 程式碼來處理和格式化擷取的網頁內容。
- 它提供了對頁面 `document` 物件、使用者 `selection` (選取範圍) 以及 `moment.js` 函式庫的存取權限。
- 腳本可以返回一個字串作為筆記內容，或是一個物件來分別指定筆記的 `content` (內容)、`title` (標題) 和 `folder` (儲存檔案夾)。
- 這項功能實現了進階的自訂操作，例如移除廣告、添加日期等元數據，或完全重構擷取內容的結構。

Interpreter is a [Web Clipper](https://obsidian.md/help/web-clipper) feature that lets you interact with web pages using natural language. Interpreter helps you capture and modify data that you want to save to Obsidian. For example:

- Extract specific text fragments.
- Summarize or explain information.
- Convert text from one format to another.
- Translate text to a different language.

Interpreter leverages language models to process information on a web page, and return results using [Variables](https://obsidian.md/help/web-clipper/variables) that you can add to your [Web Clipper Templates](https://obsidian.md/help/web-clipper/templates).

<iframe height="100%" width="100%" frameborder="0" allow="autoplay; fullscreen" title="2026-04-22 Video" src="https://fast.wistia.net/embed/iframe/8j5qu8twj1?web_component=true&amp;seo=false"></iframe>

## Examples of prompts

Prompts use the [variable](https://obsidian.md/help/web-clipper/variables) syntax `Provide a JavaScript code snippet for the Obsidian Web Clipper Interpreter that captures only the selected text, prepends the page title as an H1 heading, and appends the current date in YYYY-MM-DD format at the bottom.`. You can use this syntax with any natural language query, e.g.

- `The Obsidian Web Clipper's Interpreter feature allows users to run custom JavaScript to process web content before saving it. It provides access to the page's `document`, the user's `selection`, and the `moment.js` library for dates. Users can return a simple string for the note's content or an object to also specify the note's `title` and destination `folder`, enabling advanced customizations like cleaning clutter or adding metadata.` to extract a summary of the page.
- `L'Interpréteur utilise JavaScript pour personnaliser les captures web avant de les enregistrer dans Obsidian.
- Il permet de modifier le contenu de la note, son titre et son dossier de destination en accédant aux données de la page (`document`, `selection`).
- La bibliothèque `moment.js` est incluse pour faciliter la manipulation des dates et l'ajout de métadonnées temporelles.` to extract bullet points about the page, and translate them to French.
- `- La fonction « Interpréteur » du Web Clipper d'Obsidian exécute du code JavaScript pour transformer le contenu web avant de l'enregistrer.
- Elle donne accès à des variables clés comme `document` (la page), `selection` (le texte surligné) et `moment` (pour les dates).
- Le script peut retourner le contenu de la note sous forme de chaîne de caractères, ou un objet pour définir également le `titre` et le `dossier` de la note.` to extract three bullet points using a prompt in French.

The output of your prompts can be further manipulated using [Filters](https://obsidian.md/help/web-clipper/filters). Filters are processed after the prompt response is received from the model. For example: `> The Obsidian Web Clipper's Interpreter feature allows users to run custom JavaScript to process web content before saving it. It provides access to the page's `document`, the user's `selection`, and the `moment.js` library for dates. Users can return a simple string for the note's content or an object to also specify the note's `title` and destination `folder`, enabling advanced customizations like cleaning clutter or adding metadata.` will turn the response into a blockquote.

## Get started

Interpreter works with almost any language model provider, including options that run privately on your device. To set up Interpreter:

1. Go to the **Interpreter** section in Web Clipper settings.
2. Toggle on **Enable Interpreter**.
3. Configure your provider and model, see [models](https://obsidian.md/help/web-clipper/interpreter#Models) section below.
4. Add [prompt variables](https://obsidian.md/help/web-clipper/variables) to your [templates](https://obsidian.md/help/web-clipper/templates).
5. If your template includes prompt variables, the Interpreter section will be visible when you [clip a page](https://obsidian.md/help/web-clipper/capture). Click **interpret** to process the prompt variables.

## How it works

When Interpreter is enabled *and* your template contains [prompt variables](https://obsidian.md/help/web-clipper/variables#Prompt%20variables), a new Interpreter section is displayed in the extension window, above the **Add to Obsidian** button. This section lets you select a model and run Interpreter for the current page.

When you click **interpret**, Interpreter sends the page context to your selected model, along with *all* the prompts in your template in one request. Depending on the model provider you choose, this can be an external call or local to your device. The model evaluates your prompts against the page context, and returns its responses. Interpreter then replaces the prompt variables with the response data.

The whole process can take milliseconds or more than 30 seconds depending on the model you use and the amount of data you are processing.

## Context

The term *context* refers to the page data that Interpreter uses to process prompts. The smaller the context, the faster Interpreter runs.

By default, Interpreter uses the entire page HTML as its context, however this can make prompts slower and more expensive than necessary.

You can override the default context in Interpreter **Advanced settings** and define context per [template](https://obsidian.md/help/web-clipper/templates).

To define a more targeted context use [selector variables](https://obsidian.md/help/web-clipper/variables#Selector%20variables) (or other variable types) to interpret a section of the page. For example, you could use the following selector variable in your template's Interpreter context:

```

```

This would only run Interpreter on the `#main` element of a web page, if it exists. [HTML processing filters](https://obsidian.md/help/web-clipper/filters#HTML%20processing) like `remove_html`, `strip_tags` and `strip_attr` can be useful to further reduce the context length and speed up processing.

## Models

> [!warning] Privacy
> By using a third-party model provider you agree to their terms and privacy policy. Interpreter requests are sent directly to the provider you choose. Obsidian does not gather or store any data about your requests.

### Preset providers

Interpreter includes several preset providers. To use these providers you need an API key which you can get by logging into your provider's account. You will also need to decide which model(s) to use.

| Provider | API key | Models |
| --- | --- | --- |
| Anthropic | [API key](https://console.anthropic.com/settings/keys) | [Models](https://docs.anthropic.com/en/docs/about-claude/models) |
| Azure OpenAI | [API key](https://oai.azure.com/portal/) | [Models](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/models) |
| DeepSeek | [API key](https://platform.deepseek.com/api_keys) | [Models](https://api-docs.deepseek.com/quick_start/pricing) |
| Google Gemini | [API key](https://aistudio.google.com/apikey) | [Models](https://ai.google.dev/gemini-api/docs/models/gemini) |
| Hugging Face | [API key](https://huggingface.co/settings/tokens) | [Models](https://huggingface.co/models?pipeline_tag=text-generation&sort=trending) |
| Meta | [API key](https://llama.developer.meta.com/) | [Models](https://llama.developer.meta.com/docs/models) |
| Ollama | n/a | [Models](https://ollama.com/search) |
| OpenAI | [API key](https://platform.openai.com/api-keys) | [Models](https://platform.openai.com/docs/models) |
| OpenRouter | [API key](https://openrouter.ai/settings/keys) | [Models](https://openrouter.ai/models) |
| Perplexity | [API key](https://www.perplexity.ai/settings/api) | [Models](https://docs.perplexity.ai/guides/model-cards) |
| xAI Grok | [API key](https://console.x.ai/team/default/api-keys) | [Models](https://docs.x.ai/docs/models) |

### Choosing a model

In general we recommend using small models with Web Clipper because they are faster and perform fairly accurately for this task. Examples of smaller models include **Anthropic's Claude Haiku**, **Google Gemini Flash**, **Llama** with 3B or 8B parameters, or **OpenAI's Mini** series of models.

### Custom providers and models

To add a custom provider and/or model go to Web Clipper **[Settings](https://obsidian.md/help/settings)** → **Interpreter**:

- **Add provider** to configure preset and custom providers.
- **Add model** to configure preset and custom models.

When adding a custom provider, we recommend that you use their chat completions endpoint for the **Base URL** — it typically ends with `/chat/completions`.

### Local models

Interpreter can use local models which offer greater privacy and offline compatibility. Several options for running local models exist. One of the easiest to configure is Ollama.

#### Ollama

[Ollama](https://ollama.com/) lets you run language models locally and privately on your device.

Once you have downloaded and installed Ollama, add Ollama using **Add provider** in Interpreter settings. Ollama does not require an API key. Then choose a model from the [model list](https://ollama.com/search). For example if you want to use [Llama 3.2](https://ollama.com/library/llama3.2), click **Add model**, then:

- **Provider:** Ollama
- **Display name:** Llama 3.2, this value is customizable.
- **Model ID:** `llama3.2`, this must exactly match the model ID from Olllama.

**Start the Ollama server**

To allow a browser extension to interact with Ollama you must [give it explicit instruction](https://github.com/ollama/ollama/issues/2308) when running the server, or else you will see a `403` error.

Close the Ollama app, and run the following command in your terminal. The protocol should be changed to your browser's extension protocol if you don't use Chrome or Firefox.

```
OLLAMA_ORIGINS=moz-extension://*,chrome-extension://*,safari-web-extension://* ollama serve
```

Then run your model with Ollama the normal way, e.g.

```
ollama run llama3.2
```

**Context length**

Ollama's context window defaults to 2048 tokens. This is the maximum number of tokens for the message and response. When clipping a long web page you can easily exceed this limit. Ollama will silently fail and return irrelevant results. Some options:

- Increase Ollama's `num_ctx` parameter. Be mindful that longer context requires more memory.
- Use the [Context](https://obsidian.md/help/web-clipper/interpreter#Context) field in your template to provide a more targeted section of the page, or trim the context using a [filter](https://obsidian.md/help/web-clipper/filters) e.g. `Interpreter is a [Web Clipper](https://obsidian.md/help/web-clipper) feature that lets you interact with web pages using natural language. Interpreter helps you capture and modify data that you want to save to Obsidian. For example:

- Extract specific text fragments.
- Summarize or explain information.
- Convert text from one format to another.
- Translate text to a different language.

Interpreter leverages language models to process information on a web page, and return results using [Variables](https://obsidian.md/help/web-clipper/variables) that you can add to your [Web Clipper Templates](https://obsidian.md/help/web-clipper/templates).

<iframe height="100%" width="100%" frameborder="0" allow="autoplay; fullscreen" title="2026-04-22 Video" src="https://fast.wistia.net/embed/iframe/8j5qu8twj1?web_component=true&amp;seo=false"></iframe>

## Examples of prompts

Prompts use the [variable](https://obsidian.md/help/web-clipper/variables) syntax `Provide a JavaScript code snippet for the Obsidian Web Clipper Interpreter that captures only the selected text, prepends the page title as an H1 heading, and appends the current date in YYYY-MM-DD format at the bottom.`. You can use this sy`.