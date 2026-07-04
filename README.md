## GIFT Checker

**English** ｜ [日本語](README.ja.md)

**Syntax checking, practice, and HTML export tool for Moodle GIFT-format text (Japanese / English UI)**  

[![Version](https://img.shields.io/badge/version-0.32-blue)](https://tools.nakaix.com/gift-checker/)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)

🔗 [Open the tool](https://tools.nakaix.com/gift-checker/) | [GitHub](https://github.com/TadashiNakai/gift-checker)

### Overview

#### Preflight your GIFT questions before importing them into Moodle

GIFT Checker is a single-file HTML tool for checking question files written in Moodle's GIFT format **before importing them into Moodle**.

Just paste your GIFT text into the browser to find possible syntax issues, answer the questions as if taking a real quiz, and check how feedback and explanations will appear. You can also export a question sheet + answer key as a single HTML file.

All processing is completed inside your browser, and your question data is never sent to an external server (only when TeX math rendering is enabled, the MathJax display library is loaded from a CDN). The interface can be switched between Japanese and English.

#### When to use it

- You want to check notation mistakes or missing escapes before importing GIFT into Moodle
- You want to catch import problems caused by control characters such as `=`, `~`, `{}`, and `#`
- You want to answer the questions yourself and review the feedback before showing them to students
- You want to inspect Moodle-exported GIFT containing `[moodle]`, `[html]`, `\n`, and similar markers in the browser
- You want to check whether questions containing images, audio, or video are displayed as intended
- You want to check how questions containing TeX math (`\( … \)`, `$$ … $$`) are displayed, or convert the math to images for a Moodle without TeX support
- You want not only to import questions into Moodle, but also to create a printable/distributable question-and-answer HTML file

#### Main things you can do

- Check GIFT syntax (issues are separated into errors and warnings) and jump to the flagged lines
- Apply one-click fix suggestions (escaping, full-width conversion, duplicate-line removal)
- Preview and practice-simulate all 9 GIFT question types
- Render TeX math (`\( … \)`, `$$ … $$`, `\[ … \]`) — optional
- Export a GIFT file with the math converted to images, for a Moodle without TeX support
- Show an outline pane next to the editor (jump to a question from its title)
- Check correct, incorrect, and partial-credit judgments, feedback, and overall explanations
- Save a question sheet + answer key as a single HTML file
- Save a practice-results summary as HTML
- Display and export GIFT containing images, audio, and video
- Handle Moodle-exported markers and escape forms such as `[moodle]`, `[html]`, `\n`, and escaped characters
- Run entirely in the browser as a single file; question data is never sent externally (MathJax is loaded from a CDN only when TeX rendering is on)

#### Note

GIFT Checker is not intended to be a perfect reimplementation of Moodle's own import process.  
It is a helper tool for **pre-import checking, test-running, and formatting teaching materials** before actually importing them into Moodle. Please confirm the final behavior in your own Moodle environment.

### Main features

#### Syntax checking

Issues are shown in two categories: **errors** (problems that cannot be imported into Moodle; you cannot proceed to the question list until they are fixed) and **warnings** (suspicious spots; you can proceed anyway after reviewing them).

Detected as errors:

- A stray `}` with no matching `{`
- A `{` that is never closed

Detected as warnings:

- Lines inside an answer block that do not begin with `=` or `~`
- Unescaped control characters (`= ~ { }`) inside answer text
- Unescaped `= ~` inside answer feedback (after `#`), which would otherwise be misread as new answers
- Duplicated answers / accepted alternatives
- A single question containing multiple `{…}` blocks (in standard GIFT, the second and later blocks are not treated as gaps)
- Multiple-answer questions whose positive grades (`%…%`) do not sum to 100% (which causes an import error in Moodle)

Fixing support:

- Shows the line number of each issue; clicking it jumps to the corresponding line in the editor
- A "Fix suggestion" popup opens at the same time, letting you apply fixes with one click: escaping control characters (e.g. `\=`), converting them to full-width characters (e.g. ＝), applying all fixes on the line at once, or deleting a duplicated line
- If the line has changed since detection, the fix is not applied, to prevent corrupting the text
- When there are only warnings and no errors, a "Review the list anyway →" button lets you proceed

#### Question list

- Parses the GIFT text and shows a question-list table
- Lists each question's type, title, beginning of the question text, and whether an overall explanation is present
- The beginning of the question text is shown as plain text, with HTML tags and entity-encoded tags (e.g. `&amp;lt;font...&amp;gt;`) stripped
- Click a row to start answering from any question
- The list screen lets you set the following run options:
  - Shuffle question order
  - Shuffle answer options
  - Show option hints
  - Show the question text and options on the answer-check screen
  - Reflect HTML tags in the question display and HTML output (raw HTML mode; `&lt;...&gt;` / `&amp;lt;...&amp;gt;` forms are also restored)
  - Render TeX math (a page reload is required after changing this)

#### Supported GIFT question types (all 9)

| Type | Description |
|---|---|
| Multiple choice (MC) | `=` correct / `~` incorrect, with `%nn%` weighting |
| Multiple answers | Checkboxes, with partial credit |
| True/False | True/False buttons, two feedback messages |
| Short answer | Accepted alternatives, case-insensitive, weighted partial credit |
| Matching | Pull-down selection, per-pair correctness, and distractors (extra wrong options with no corresponding sub-question) |
| Missing word | A single in-sentence blank |
| Numerical | Tolerance, ranges (`low..high`), and multiple answers |
| Essay | No auto-grading; shows a sample answer / explanation |
| Description | No answer; shown as descriptive text |

In addition, `$CATEGORY:`, `::title::`, `#### overall explanation`, `// comment lines`, escapes such as `\=`, and the `\n` (newline) escape are supported. Format specifiers such as `[moodle]` / `[html]` that Moodle adds in front of each answer (right after `=` / `~`) or its feedback are automatically removed as metadata that is not meant to be displayed or graded.

#### Practice simulation

- Question navigation buttons (`[ 1 ] [ 2 ] … [ n ]`) let you jump freely to any question
- Step buttons `[ |◀ ] [ ◀ ] [ ▶ ] [ ▶| ]` (first / previous / next / last) are placed before the numbered buttons for quick movement between questions (the relevant buttons are automatically disabled at the first and last question)
- When shuffled, each button shows the original parse-order number (so the order is still clear)
- Judges correct / incorrect / partially correct answers
- Shows feedback for the selected option and the overall explanation for the question
- The "Show the question text and options on the answer-check screen" option displays them together
- "← Back to question" on the answer-check screen returns to the question screen for the same question (with your selection/input preserved) so you can change your answer
- The "previous" button on the question screen changes its label by context: if the previous question has been answered, it reads "← Back to previous result" (jumping to that question's latest grading), otherwise "← Back to previous question" — making its role distinct from `◀` (which simply moves to the previous question text)
- Re-answering and re-checking overwrites only that question's score with the latest selection; the total always reflects each question's final answer

#### Results summary / review mode

- After finishing all questions (or at any point), the score screen shows the correctness of every question at a glance (unanswered is shown as "–")
- Essay and Description are treated as not auto-graded and are excluded from the denominator (points) of the score
- The navigation buttons change their underline color by answer status — green for correct, red for incorrect, gray for not-auto-graded (Essay / Description) — for quick scanning
- Re-answering updates the colors accordingly when the result changes
- Click a row to move to the detailed review (review mode)
- Review mode shows the question text, options (color-coded for correctness), and explanation
- Hovering over an option shows its individual feedback as a tooltip
- The **"⬇ Save results as HTML"** button exports a single HTML file (`result-YYYYMMDD-hhmmss.html`) containing the score rate, the summary table, and details of "incorrect questions" and "questions that were not auto-graded (Essay / Description)"

#### HTML output for printing / distribution

- The **"⬇ Save questions and answers as HTML"** button exports the question sheet + answer key as a single HTML file (`quiz-YYYYMMDD-hhmmss.html`)
- Questions are placed in the first half and answers in the second half (a compact layout intended for printing)
- Options use circled numbers (①–㊿); matching shows the left items as circled letters (Ⓐ Ⓑ …) and the right candidates as circled numbers, and answers show the pairing explicitly in the form `Ⓐ→③` …
- Reflects the list screen's shuffle settings (question order / options)
- Images are embedded as data URLs, so the file is self-contained (no extra files needed)
- Images in the saved HTML keep their aspect ratio and are scaled down only when they exceed the body width
- Audio and video, when present, are embedded as the corresponding media elements
- When the "Reflect HTML tags as-is" option on the list screen is on, HTML tags are reflected in the question display and HTML output (script, `on*` attributes, `javascript:` URLs, etc. are removed)
- When TeX rendering is on, MathJax is embedded in the saved HTML as well, so formulas are rendered (a network connection is needed for display; the saved results-summary HTML behaves the same way)

#### Rich-text input

- The input area is a `contenteditable` rich-text box
- When the browser window is large, the input area becomes wider and taller
- Keeps a minimum size while also supporting manual vertical resizing
- The toolbar can apply bold (B), italic (I), underline (U), superscript, and subscript
- Pasting from Word etc. preserves formatting; you can also select text and apply formatting manually with the toolbar buttons
- Questions with formatting are exported with an automatically added `[html]` marker
- Formatting is also reflected in the preview screen
- Image sizes inside the editor respect the original `style` / `width` / `height` so that they appear close to how they look on the question display screen
- The **≡ menu** at the right end of the toolbar toggles the outline view, loads the sample (with an overwrite confirmation if the editor already contains text), and exports GIFT with TeX converted to images
- **Outline view**: shows a pane of question titles next to the editor (on by default; the state is remembered). Clicking an item jumps to the corresponding line in the editor, and the pane's height follows the editor

#### Handling of images, audio, and video

##### Images

- Supports pasting images from the clipboard (PNG, JPEG, etc.)
- Image data is managed in memory internally and represented as a placeholder within the GIFT text
- Questions containing images automatically get a `[html]` marker
- Images appear in the preview, practice, and HTML output
- On GIFT export, images are embedded in `<img src="data:image/...">` form
- Handles the `src\=` / `data\:` style escapes seen in Moodle GIFT exports
- The saved HTML keeps the image aspect ratio and does not impose an excessive height limit

##### Audio

- Supports the `<audio src="data:audio/mp3;base64,..."></audio>` form
- Supports the `<audio><source src="data:audio/mp3;base64,..."></audio>` form
- Handles the `src\=` / `data\:` escapes originating from Moodle GIFT
- Displayed as audio with playback controls in the question display and HTML output

##### Video

- Supports the `<video src="data:video/mp4;base64,..."></video>` form
- Supports the `<video><source src="data:video/mp4;base64,..."></video>` form
- Preserves `video/mp4` base64 embedding in the HTML output as well
- Displayed as video with playback controls in the question display and HTML output

#### Reflecting HTML tags (raw HTML mode)

Turning on the **"Reflect HTML tags as-is"** option on the list screen passes HTML-like markup inside question text and options to the browser for rendering.  
Even when content from Moodle XML or GIFT appears as `&lt;font&gt;`, or as double-encoded `&amp;lt;font&amp;gt;`, it is restored before being processed for display.

Example: `&lt;font color="#FF0000"&gt;①②&lt;/font&gt;`  
With this option on, ①② is shown in red on the question display screen and in the HTML output.

In raw HTML mode, formatting tags such as nested `<font>`, `<span style="...">`, `<sup>` / `<sub>`, and `<b>` / `<i>` / `<u>` are rendered by leaving them to the browser's HTML parser rather than splitting them tag by tag.  
For safety, however, `script`, `style`, `on*` event attributes such as `onclick`, and `javascript:` URLs are removed or disabled.

Only the "beginning of the question text" in the question list is, for readability, always shown as plain text with HTML tags stripped — regardless of whether raw HTML mode is on or off.

If a tag is left unclosed, the browser will correct it, but formatting may apply to an unintended range. Turning this on only for GIFT files that need it is recommended.

#### Rendering TeX math (MathJax)

Turning on the **"Render TeX math"** toggle on the list screen renders formulas wrapped in `\( … \)`, `$$ … $$`, or `\[ … \]` with MathJax (the same notation as Moodle's editor).

- Off by default. The setting is remembered in localStorage, and changes take effect after a page reload (a prompt is shown when you switch it)
- Only while it is on, the MathJax display library is loaded from a CDN; question data is never sent externally
- Inside the input editor, TeX stays as raw code so you can view and edit it directly
- Formulas are rendered in the preview, practice, and review screens, and also embedded in the saved HTML (questions + answers, results summary)
- A sample question containing TeX math has been added

#### Exporting GIFT with TeX as images

**"Export GIFT with TeX as images"** in the ≡ menu on the input screen exports a GIFT file in which the formulas in the text are converted to images (base64 PNG).

- Lets you import questions with math into a Moodle whose TeX filter is disabled
- Formulas are rendered as high-resolution images and embedded as data URLs with an `[html]` marker, just like ordinary images
- Requires TeX rendering to be on (the menu item is disabled while it is off)
- Shows the number of formulas to convert before exporting (the file may become large)

#### Exporting in GIFT format

- The **"⬇ Download as GIFT"** button downloads a `quiz-YYYYMMDD-hhmmss.gift` file
- Blocks containing formatting tags, `<font>`, images, audio, or video automatically get a `[html]` marker

#### Japanese / English switching

- The **"日本語 / English"** toggle in the header switches the UI language
- On first use the language is auto-detected from the browser settings (Japanese for a Japanese environment, English otherwise)
- A manually chosen language is remembered in localStorage and preferred from then on
- Sample questions and HTML output also follow the selected language
- The currently displayed screen — question list, practice, results summary, review mode, etc. — is re-rendered the moment you switch, so labels and graded-result text reflect the current language immediately

#### Other

- Load GIFT files by dragging and dropping them onto the editor (with binary detection)
- Loading an existing GIFT file that contains formatting, images, audio, or video restores them in the editor as far as possible
- Toggle settings (shuffle question order / shuffle options / show hints / show question text / reflect HTML tags), plus the TeX rendering setting and the outline-view state, are remembered in the browser (localStorage)
- The version number is shown in the header, and a footer with links to the tool, GitHub, and the author has been added at the bottom of the page
- Fully browser-contained, single file; question data is never sent externally (MathJax is loaded from a CDN only when TeX rendering is on)

### How to use

1. [Open the tool](https://tools.nakaix.com/gift-checker/) (or open `index.html` in your browser)
2. Paste GIFT-format text into the input area, or drag and drop a file
3. Click **"📋 Check question list"**
4. Any issues are shown separated into errors (cannot be imported) and warnings (suspicious spots). Clicking an issue jumps to the line and lets you apply a fix suggestion with one click. Errors block you until fixed; with warnings only, use "Review the list anyway →" to proceed
5. Choose settings on the list screen (shuffle, hints, raw HTML, TeX rendering, etc.) and operate as needed
6. Use **"▶ Start from the beginning"** or click a row to begin the practice simulation
7. Use **"⬇ Save questions and answers as HTML"** to export the question sheet + answer key HTML
8. Use **"⬇ Download as GIFT"** to save a GIFT file
9. After practicing, use **"📊 View results summary"** to review correctness, and if needed **"⬇ Save results as HTML"** to export the results

### How it differs from other tools

Many tools that handle GIFT focus on "generating questions or converting to other formats." GIFT Checker is distinctive in that, at the **text stage before importing into Moodle**, it provides (1) practice simulation for all 9 question types, (2) HTML output of a question sheet + answer key, and (3) in-browser checking of materials that include images, audio, video, and TeX math, and (4) one-click fixes for syntax issues and conversion of TeX math to images — all in a single-file browser tool. Everything runs entirely locally with no external transmission.

### GIFT format references

- [Moodle official documentation (GIFT format)](https://docs.moodle.org/en/GIFT_format)

### File structure

```text
index.html     # GIFT Checker itself (single file)
README.md      # README (English, canonical)
README.ja.md   # README (Japanese)
```

### Version history

| Version | Date | Changes |
|---|---:|---|
| v0.32 | 2026-07-04 | Added TeX/LaTeX math rendering (MathJax; off by default; switched with a toggle on the list screen and applied after a reload. TeX stays as raw code in the input editor, rendering is also embedded in the saved HTML, and a sample question with math was added). Added "Export GIFT with TeX as images", which embeds formulas converted to images (base64 PNG) for a Moodle without TeX support. Split the syntax check into errors (cannot be imported; block proceeding to the list until fixed) and warnings (can proceed anyway), and added a `{` `}` matching check and a 100% grade-sum check for multiple-answer questions. Added a "Fix suggestion" popup that jumps to the flagged line and applies escaping, full-width conversion, or duplicate-line removal with one click. Added an outline view next to the editor (question titles; click to jump; state remembered) and a ≡ toolbar menu (outline view / load sample / TeX image export). Added the version number to the header and a footer at the bottom of the page. Also improved stability and safety: fixed the syntax check reporting wrong line numbers or failing on GIFT containing `\n` escapes, the "delete this line" fix being applicable even after the target line had changed, outline jumps drifting when untitled questions were present, and stale-language text remaining when returning to past grading results after switching the language. Hardened the HTML sanitization for the question display and raw HTML mode (more thorough removal of script-executing constructs). Changed saved-file timestamps to the `YYYYMMDD-hhmmss` format (hyphen between date and time). |
| v0.29 | 2026-06-24 | Overhauled Japanese / English switching. The currently displayed dynamic screen (question list, question, result, results summary, review mode) is now re-rendered on switch, so labels and graded-result text no longer remain in the old language. Graded results are re-graded from the stored response and regenerated in the current language (correctness judgments are unchanged). Fixed a bug where the True/False selection judgment was inverted in English mode. The results-summary row tooltip, the separator used to list options, and the parentheses around weights/explanations now also follow the language. |
| v0.28 | 2026-06-24 | Alongside a pre-release review, improved internal robustness — e.g. replacing the disabled-state check of the navigation step buttons from a label-string dependency to an explicit direction (no change to appearance or behavior). |
| v0.27 | 2026-06-24 | Made the "previous" button on the question screen switch its label by context: "← Back to previous result" if the previous question is answered, "← Back to previous question" if not. Clarified its difference in role from the `◀` step button. |
| v0.26 | 2026-06-24 | Added step buttons before the numbered navigation buttons: first `\|◀`, previous `◀`, next `▶`, last `▶\|`. The relevant buttons are automatically disabled at the first and last question. |
| v0.25 | 2026-06-24 | Extended the removal of Moodle-origin format specifiers to option feedback (after `#`), so that a leading `[moodle]` etc. in feedback such as `#[moodle]Correct: …` no longer remains in the display. |
| v0.24 | 2026-06-24 | Handle GIFT's `\n` newline escape as an actual newline (`\\n` is kept as a literal `\n`). Also, in raw HTML mode, newlines are now treated as whitespace like a browser (not converted to `<br>`), so Moodle-origin `[html]` content containing block elements such as `<p>` / `<ul>` / `<li>` displays as intended. |
| v0.23 | 2026-06-24 | Added support for matching distractors (extra wrong options with no left item / no corresponding sub-question), including them in the answer-candidate pull-down and in the right-candidate pool of the HTML output. Grading is unchanged; choosing a distractor counts as incorrect. |
| v0.22 | 2026-06-24 | Automatically remove the format specifiers `[moodle]` / `[html]` / `[plain]` / `[markdown]` that Moodle-exported GIFT adds in front of each answer (right after `=` / `~`) from the text of options, matching items, and numerical answers. Ordinary brackets such as `[2,3]` are preserved. |
| v0.21 | 2026-06-23 | Changed the "beginning of the question text" in the question list to display as plain text, stripping HTML tags and double-encoded tags such as `&amp;lt;font...&amp;gt;`. Regardless of whether raw HTML mode is on or off, tag strings are no longer shown in the list. |
| v0.20 | 2026-06-23 | Even when raw HTML mode is on, render formatting tags such as bold, italic, underline, superscript, and subscript (`<b>`, `<i>`, `<u>`, `<sup>`, `<sub>`) as real HTML. Entity-encoded formatting tags are also restored, reconciling arbitrary-HTML reflection with formatting handling. |
| v0.19 | 2026-06-23 | In raw HTML mode, restore entity-encoded HTML tags such as `&lt;font&gt;` to real tags before rendering. Even for the `&lt;...&gt;` form originating from Moodle XML / GIFT, nested `<font>` and tags inside `<sup>` can be left to the browser. |
| v0.18 | 2026-06-23 | In raw HTML mode, pass the whole sanitized HTML string to the browser rather than splitting and reassembling tags individually. Improved handling of structures, such as nested `<font>`, that are prone to breaking under the tool's simple tag handling. |
| v0.17 | 2026-06-22 | Improved rendering of the `<font face="..." size="...">` form output by Moodle so that its attributes are preserved on display. |
| v0.16 | 2026-06-22 | Added a "Enable safe HTML tags" checkbox to the list screen, allowing safe HTML tags such as `font` to be enabled via a whitelist in the question display and HTML output. Changed the policy to suppress dangerous tags, attributes, and CSS. |
| v0.15 | 2026-06-22 | To bring image sizes in the rich-text box closer to the question display, applied the `style` / `width` / `height` from image tags to the in-editor display as well. |
| v0.14 | 2026-06-22 | Made the main area and rich-text box responsive so the input area can be used wider and taller when the browser window is large. |
| v0.13 | 2026-06-22 | For images in the rich-text box, removed the excessive shrinking from `max-height:4em` and switched to aspect-ratio-preserving display. |
| v0.12 | 2026-06-22 | Removed the fixed vertical size limit from image display in the saved HTML. Prioritized aspect-ratio preservation, scaling down only when exceeding the body width. |
| v0.11 | 2026-06-22 | Fixed aspect-ratio distortion of images in the saved HTML. Simplified the header/footer and removed the `Implementation` label. |
| v0.10 | 2026-06-22 | Added support for `audio` / `video` with `<source>`. Added support for `data:video/mp4;base64,...` video embedding. |
| v0.09 | 2026-06-22 | Added support for `audio/mp3` base64-embedded audio. Handle the Moodle-GIFT-origin `src\=` / `data\:` escapes for audio as well. |
| v0.08 | 2026-06-22 | Correct Moodle-GIFT-origin image escapes (`src\=` / `data\:`, etc.) and improve restoration/display of data-URL images. |
| v0.07 | 2026-06-21 | Added Japanese / English switching, refreshed sample questions, a print-oriented HTML output feature, additional syntax checks (unescaped characters in feedback, duplicate options), and copy-with-image support. |
| v0.06 | 2026-06-17 | Added rich-text input (bold, italic, underline, superscript, subscript), image pasting, and a GIFT export feature. |
| v0.05 | 2025-06-15 | Initial release. Supports all 9 GIFT question types. |

### License

MIT License  
Concept & Design: [@TadashiNakai](https://x.com/TadashiNakai)
