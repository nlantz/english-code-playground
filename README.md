# English → Python Playground

A tiny AI-native coding playground that runs **inside your existing chatbot**. Type an idea in ordinary English; the chatbot turns it into Python, runs it when possible, and shows the meaning at five levels of abstraction. No separate application or API key.

## Launch

Give your chatbot this repository URL and say:

> Read the full `SKILL.md` and `comparison.html` in this repository, then launch the English → Python Playground by populating the fixed renderer's JSON data and displaying it in your native interactive UI. Do not redesign the layout.

If it cannot read the URL, upload or paste [`SKILL.md`](SKILL.md) and [`comparison.html`](comparison.html) instead. The latter is the fixed renderer, not a separate application the user needs to launch.

## What you get

1. **Abstraction slider**, from natural English to literal Python explanation.
2. **Side-by-side Python and English**, in separate editor-style panels, with larger text, matching semantic colors, and line-by-line alignment.
3. **One-line execution result**, actually executed if a safe Python tool is available; otherwise explicitly labeled a simulation.
4. **Try again**, with your original English still editable.

No Git clone, local setup, API credentials, course flow, or long chat prompts. The chatbot handles translation. Native controls depend on the host; text-only chatbots can offer the same content without a slider.

## Quick test

After loading `SKILL.md`, submit:

`Playground: If a cat has yellow ears, brush only its ears.`

Edit the retry field and try another sentence. Changing the abstraction slider should not start a new chat turn.

## Status

**MVP v0.6.** Chat-native, Python only. The Python is displayed, not edited inline. Each submission generates a fresh result in the conversation. Additional languages and code editing are intentionally out of scope.
