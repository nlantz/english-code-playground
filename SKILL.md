---
name: english-code-playground
description: A minimal, chat-native English-to-Python playground. Turn one short English idea into runnable Python, a concise execution result, and five levels of line-aligned English pseudocode. Use when the user says "Playground:" or asks to launch the English Code Playground.
---

# English → Python Playground (MVP v0.4)

## Purpose

Turn an ordinary-English idea into an immediately explorable Python experiment **inside the user's existing chatbot**. This is an open playground, not a lesson plan. The user should never need to install software, clone a repository, obtain an API key, or configure a server. Use tools and native interface controls **already available** to the host. Don't promise capabilities the host does not support.

**Experience we are protecting:** small, quiet, and fast. The assistant does the heavy lifting. The user submits a short idea, sees Python and English side by side with corresponding lines aligned, a one-line result, and slides between five English explanations, and edits the original idea to try again.

## Interaction contract

### On launch (no idea supplied)

Show **one small English input control**, with a few selectable starter ideas if native controls are available, and a **Generate & Explore** action. Start with an empty entry. Examples may include:
- `If a cat has yellow ears, brush only its ears.`
- `If a car is above the speed limit, issue a ticket.`
- `Every person has a name and an age.`

**The launch response consists only of the input interface.** No introductory or trailing prose, separate list of the five levels, setup reassurance, repository link or usage summary. The interface itself is the explanation; omit even redundant captions. For a text-only host, use one concise input invitation.

Submission should send a **short user message**, `Playground: <user idea>`. Do not send the entire skill instruction text or a giant generated prompt in the message. If the host only supports text, invite the user to send `Playground: ...`.

### On `Playground: <idea>` (or an unambiguous new idea)

1. Preserve the user's English text exactly for the retry field. Never silently reword their input.
2. Translate it into **minimal readable Python**. Prefer a runnable deterministic illustration. Do not add elaborate classes or scaffolding when a simple `if` will do.
3. If a real Python execution tool is available **and the code is safe to run**, actually run the exact displayed program and use the actual stdout/error. Real execution must be grounded in a real tool result, **never a prediction**. If a temporary test harness is needed, it must not alter the displayed program's behavior. Avoid external side effects, network calls, secrets, destructive operations, or executing untrusted code without suitable constraints.
4. If real execution is unavailable, give a compact prediction clearly marked **Simulated — not executed**. Don't invent tool success, environment information, or verification.
5. Based on the **actual displayed Python**, prepare **five separate arrays of English lines** before rendering the interface, one per abstraction level. Split the exact Python into physical source lines, including empty lines. Each array must contain exactly the same number of entries as Python has source lines, in the same order. Each English entry describes its corresponding Python line only, begins at the same indentation depth (copy its leading whitespace), and is empty when its Python line is empty. Each interpretation must faithfully describe the corresponding program operation. Do not merge multiple statements or redistribute explanations across rows.
6. Render the compact result interface specified below. Add one brief assumptions note *only if* translation required a substantive interpretation.

Do not require a second user action to execute the initial experiment. **Generating and running occur in one submission**. If runtime isn't available, the same flow uses a clearly labeled simulation.

## Result layout: freeze this UI

Render a single compact result interface, without any surrounding narration. Keep this order:

1. **Abstraction slider:** a locally responsive five-step slider, Natural → Precise → Logical → Technical → Literal. Show only the current level and the slider, with short endpoint labels if useful. Use five pre-generated line arrays and local UI state; never send a fresh model request when the slider moves.
2. **Side-by-side comparison:** one shared two-column layout headed **Python** and **English**. Create one synchronized row per physical Python source line, including blank lines. Place the original code line on the left and its corresponding English entry at the current slider level on the right. These are two views of the same program, not independently flowing text panels. Use a shared row layout (a table or grid with shared row sizing), top-aligned cells, and a consistent monospaced font, line height, and whitespace-preserving rendering for both sides. Preserve actual indentation spaces and line breaks using preformatted rendering; ordinary paragraph rendering collapses whitespace and is unacceptable. Python remains fixed when the slider changes. Only the English column changes. Both columns must be visually distinct editor-style boxes with separate rounded borders, subtle backgrounds, equal top padding, and a small gutter between them. They must still share the same row geometry. Use readable monospace text at normal-to-large body size (target 17–18 px on desktop, at least 16 px where possible), not small captions. Apply syntax and semantic highlighting to both columns, as specified below. A copy affordance is optional.
3. **Result:** compact stdout/value/error below the comparison. Mark **Executed** only after real execution, otherwise **Simulated — not executed**. Longer output may be truncated with an honest indicator. Add a small visualization only if it materially helps.
4. **Try again:** a small editable text field prefilled with the user's exact English, followed by a **Try again** button. Submit only `Playground: <edited text>` as a new turn. The next response renders a fresh complete result.

### Appearance and matching colors

- Render two visibly separate, continuous panels labeled Python and English. Use quiet, neutral surfaces, rounded outlines, and generous but equal internal padding. Do not put each individual source line in its own card.
- Use the same readable monospace font, size, and line spacing for both panels. Aim for 17–18 px body text and 1.5–1.6 line height. Use the host's regular or large text size when exact sizing is unavailable. Never shrink the text to squeeze both columns onto a narrow screen.
- Color-code meaningful tokens in **both** panels using a shared, restrained semantic palette: purple for control flow (if, else, for, while and their English equivalents); blue for variables and named state; green for literal strings, numbers and corresponding English values; amber/orange for actions and function calls; neutral foreground for punctuation and connective prose. Adapt hues to light/dark themes and maintain readable contrast. In English, color only the corresponding words or phrases, not whole unrelated sentences. For example, Python `if` and English "If" share purple, `ear_color` and "ears" share blue, `"yellow"` and "yellow" share green, and `append` and "Record" share amber. Do not depend on color alone to communicate meaning.
- Pre-generate semantic spans for each line of the Python and for each of the five English levels. Preserve every character and leading space when composing the spans. The Python text must remain exactly the code executed. Use inline colored spans or host-native equivalent, not screenshots or decorative colored badges. Do not use ordinary Markdown paragraph rendering that collapses indentation.
- A practical layout is a two-column shared grid with one row per line pair and a continuous border/background behind each column (or a grid with shared row sizing/subgrid). Keep the two panels independent visually, but not in vertical layout. If the native host cannot draw continuous panel outlines with shared rows, use the closest supported two-panel appearance and retain shared row alignment over decorative borders. If custom coloring or type sizing is unsupported, use native syntax highlighting or readable preformatted text rather than faking the feature.

### Alignment is a functional requirement

- Use shared rows, not two independent code blocks, text paragraphs, or independently wrapping panels. If either cell wraps, both cells occupy the same row height so the next pair starts at the same vertical position.
- Preserve blank rows on both sides. Match each English line's leading spaces to its corresponding Python line, even at the Natural level. Keep descriptions short enough to reduce wrapping.
- For continuation lines, provide a short corresponding fragment or leave the English cell empty when there is nothing useful to say; never move the explanation to another row. Prefer generated Python with simple physical lines to begin with.
- On narrow screens, prefer a horizontally scrollable shared comparison to stacking the two columns. If scrolling is unsupported, use a compact two-column table with shared wrapping rows. Keep the line-by-line relationship intact.
- Before rendering, check that each level has the same row count as Python, that blank rows match, and that indentation matches. Changing the slider must not shift the logical correspondence.

**Do not** add separate cards for variables, flow diagrams, assumptions, status pills, extra buttons, a Python editor, a history browser, or a curriculum. Do not repeat the user input above the results. Show substantive assumptions in one short note when necessary. Do not print a separate list of abstraction levels or setup requirements.

If native widgets are unavailable, use a two-column Markdown table with one row per source line and whitespace-preserving code spans or preformatted cells as supported. If custom color and panel styling are unavailable, do not promise them. Offer other levels on request. Never display non-functional controls or claim interactivity that is not there.

## The five explanation levels

Create all five **from the Python**, as arrays of aligned lines, before displaying the slider. Every level uses the same row structure; only the vocabulary and detail change. Never convert a level into a prose paragraph, even at Natural or Literal.

- **0 — Natural:** Everyday words that describe each line's operation.
- **1 — Precise:** Plain English with explicit state and conditions.
- **2 — Logical:** Pseudocode with named operations and blocks.
- **3 — Technical:** Variables, comparisons, lists, and function operations named.
- **4 — Literal:** Faithful Python mechanics, evaluation, calls, and side effects.

Example of the required row alignment at the Natural level (illustrative only; render as a shared two-column interface, not as prose):

```text
Python                              English
ear_color = "yellow"                The cat's ears are yellow.
brushed_parts = []                  Start with no brushed parts.

if ear_color == "yellow":           If the ears are yellow:
    brushed_parts.append("ears")        Record the ears as brushed.

print(brushed_parts)                Show the brushed parts.
```

At the Literal level, the same rows might read:

```text
Assign the string "yellow" to ear_color.
Bind brushed_parts to a new empty list.

If ear_color == "yellow" evaluates to True:
    Call append("ears") on brushed_parts.

Call print() with brushed_parts.
```

All five levels must use the same number of entries and the same indentation pattern. This example has seven rows including two empty rows.

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
