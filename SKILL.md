---
name: english-code-playground
description: Interactive English-to-Python learning playground. Use on launch or messages beginning Playground:.
---

# Playground v0.6

Read the complete [fixed renderer](https://raw.githubusercontent.com/nlantz/english-code-playground/main/comparison.html). This file is the UI, not inspiration for a new UI. Populate its JSON experiment-data payload, preserving layout, CSS, paired grid rows, semantic colors, and slider behavior. Use the host's supported inline interactive rendering mechanism; adapt only the controls needed for host integration. Do not print the file as source or ask users to open a website.

## Launch

Show only an editable English input, optional starter ideas, and Generate & Explore. Submit a new chat turn containing only `Playground: <exact input>`. No introduction or tutorial afterward.

## Experiment

1. Generate one minimal complete Python program illustrating the idea. Include example calls and prints in that same program.
2. Actually run the exact program using a safe Python tool if available. Capture stdout or errors. Never claim execution without a tool result. If unavailable, predict and label simulated. Avoid external side effects. Ask permission before repairing user-supplied code.
3. Produce the JSON payload below. Replace only the example payload in the fixed renderer. Render it inline, then add a native editable retry field prefilled with the exact idea and a Try again button submitting only `Playground: <edited input>`. The slider is local, not a new model call.
4. Return the interface, not explanatory prose.

## Data contract

```json
{
  "idea": "original user text",
  "python": [[["x", "state"], [" = ", "plain"], ["1", "value"]]],
  "english": [
    [[["Start with ", "plain"], ["x", "state"], [" at ", "plain"], ["1", "value"]]],
    [[["Set x to one.", "plain"]]],
    [[["Set x = 1.", "plain"]]],
    [[["Assign 1 to x.", "plain"]]],
    [[["Bind x to integer 1.", "plain"]]]
  ],
  "result": {"status": "simulated", "output": ""},
  "assumptions": ""
}
```

Each physical source line is an array of [text, role] spans. Join spans exactly to recover source. Use roles control (purple), state (blue), value (green), action (orange), plain (neutral). Pre-generate five English levels: Natural, Precise, Logical, Technical, Literal. Each has exactly the Python row count, blank rows and leading indentation. Explain each source line separately, accurately. Use short phrases and meaningful semantic colors in all five levels. The renderer checks alignment. Escape less-than signs in JSON embedded in HTML script elements using the JSON Unicode escape for U+003C. Result status is executed, simulated, or error (actual execution error only). Assumptions are brief or empty.

## Acceptance check

One inline comparison with separate, continuous Python and English panels, shared horizontal rows, 18px monospace, matching colors and a local five-position slider. Result underneath, then retry field. No separate code block, summary, five-level prose list or example-execution section. If HTML rendering is unavailable, use a native interactive shared-row grid with the same data and slider. If interactive UI is unavailable entirely, use a static aligned two-column representation and briefly disclose that limitation. Do not pretend a static control works. No installations, API keys, or external servers.

Repeat this contract on every Playground turn.
