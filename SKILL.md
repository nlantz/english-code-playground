---
name: english-code-playground
description: A minimal, chat-native English-to-Python playground. Turn one short English idea into runnable Python, a concise execution result, and five levels of indented English pseudocode. Use when the user says "Playground:" or asks to launch the English Code Playground.
---

# English → Python Playground (MVP v0.2)

## Purpose

Turn an ordinary-English idea into an immediately explorable Python experiment **inside the user's existing chatbot**. This is an open playground, not a lesson plan. The user should never need to install software, clone a repository, obtain an API key, or configure a server. Use tools and native interface controls **already available** to the host. Don't promise capabilities the host does not support.

**Experience we are protecting:** small, quiet, and fast. The assistant does the heavy lifting. The user submits a short idea, sees code and a one-line result, slides between five English explanations, and edits the original idea to try again.

## Interaction contract

### On launch (no idea supplied)

Show **one small English input control**, with a few selectable starter ideas if native controls are available, and a **Generate & Explore** action. Start with an empty entry. Examples may include:
- `If a cat has yellow ears, brush only its ears.`
- `If a car is above the speed limit, issue a ticket.`
- `Every person has a name and an age.`

Submission should send a **short user message**, `Playground: <user idea>`. Do not send the entire skill instruction text or a giant generated prompt in the message. If the host only supports text, invite the user to send `Playground: ...`.

### On `Playground: <idea>` (or an unambiguous new idea)

1. Preserve the user's English text exactly for the retry field. Never silently reword their input.
2. Translate it into **minimal readable Python**. Prefer a runnable deterministic illustration. Do not add elaborate classes or scaffolding when a simple `if` will do.
3. If a real Python execution tool is available **and the code is safe to run**, actually run the exact displayed program and use the actual stdout/error. Real execution must be grounded in a real tool result, **never a prediction**. If a temporary test harness is needed, it must not alter the displayed program's behavior. Avoid external side effects, network calls, secrets, destructive operations, or executing untrusted code without suitable constraints.
4. If real execution is unavailable, give a compact prediction clearly marked **Simulated — not executed**. Don't invent tool success, environment information, or verification.
5. Based on the **actual displayed Python**, prepare **five separate English interpretations** before rendering the interface. Each interpretation must be accurate about what the program does, rather than just paraphrasing user intent. Keep control flow, conditions, loops, and nesting visible as indentation wherever possible.
6. Render the compact result interface specified below. Add one brief assumptions note *only if* translation required a substantive interpretation.

Do not require a second user action to execute the initial experiment. **Generating and running occur in one submission**. If runtime isn't available, the same flow uses a clearly labeled simulation.

## Result layout: freeze this UI

Keep the result in this order, with minimal chrome and no redundant explanations:

1. **Python:** a syntax-highlighted, copyable code block containing the entire example. This is a *display*, not an editor in MVP v0.2.
2. **Result:** a compact one-line stdout/value/error from that code. Mark it **Executed** only after real execution; otherwise **Simulated — not executed**. Longer output may be collapsible or truncated with an honest indicator. Show a small visualization *only when the program materially benefits from one*; no mandatory diagrams.
3. **English interpretation:** a locally responsive **five-step abstraction slider**, Natural → Precise → Logical → Technical → Literal. The host should switch between five **pre-generated** interpretation strings using local UI state, **not** send a fresh request to the model when the slider moves.
4. **Try again:** a small editable text field prefilled with the user's original English, followed by a **Try again** button. It submits only `Playground: <edited text>` as a new conversation turn. The preceding result doesn't have to update in place; the **next** assistant response renders a new, complete result.

**Do not** add separate callout cards for variables, flow diagrams, assumptions, status pills, extra buttons, an editor for Python, a history browser, or a curriculum. No repeated copy of the user's input above the results. Optimize for mobile as well as desktop.

If native widgets are unavailable, use a compact Markdown representation with one default indented English interpretation and offer other abstraction levels on request. Never display dead, non-functional controls or claim interactivity that isn't there.

## The five explanation levels

Create all five **from the Python** and store them as static local values before displaying the slider. Preserve line breaks, code-like indentation, and blank lines in the UI; use monospaced/preformatted presentation when needed to prevent whitespace from collapsing.

- **0 — Natural:** Human-friendly wording, still reflecting significant control flow.
- **1 — Precise:** Plain English with explicit state and condition.
- **2 — Logical:** Indented pseudocode with sequential operations and blocks.
- **3 — Technical:** Variables, comparisons, lists, and function operations named.
- **4 — Literal:** Closest faithful explanation of Python evaluation, operations, calls, and side effects.

Example skeleton (actual sentences should reflect the generated code):

```text
The cat's ears are yellow.
The brushed parts start empty.

If the ear color is yellow:
    Add the ears to the brushed parts.

Show which parts were brushed.
```

Important: the natural explanation must not claim physical brushing if the Python only `print`s a recommendation or records a string. A hypothetical `if` block must not be reported as always occurring. Preserve the relationship between a variable's initial value and the condition, and label substantive illustrative assumptions briefly.

## Rules / constraints

- **One user submission → one complete experiment.** The only routine follow-up UI is the prefilled retry field.
- **No generated wall-of-text prompt** from the Try again button. The launch skill already establishes what `Playground:` means.
- **No separate model calls for slider movement.** Pre-generate five interpretations with the current assistant turn.
- **Don't fake execution.** If no actual run took place, display `Simulated — not executed`, even for obvious output.
- **Never auto-repair user-supplied code** without permission. If the user asks about or submits code directly, explain errors and ask before replacing it.
- **Do not force persistence across chat turns.** Each experiment is a fresh native interface in the chatbot; previous responses remain in chat history.
- **No standalone web app, model API hookup, external server, or installation** as a prerequisite.
- Add features only on explicit request. The entire point of this version is to finish and validate the tiny interaction.
