---
name: log-sheet-digitizer
description: Digitizes photographed industrial log sheets into reviewable structured reports.
---

# Log Sheet Digitizer

## Instructions

Use this skill when the user asks to digitize, extract, inspect, summarize, or share a photographed production log, melting log, inspection sheet, handwritten form, table, or similar field document.

When an image is attached, visually inspect the image first. Extract only what is visible. Do not invent missing cells. If text is unclear, mark it as unreadable or low confidence.

Call the `run_js` tool using `index.html` and a JSON string for `data` with the following fields:

- `action`: "save_report"
- `document_title`: Short document title. Use the printed title if visible.
- `captured_at_iso`: Current date/time if available from the conversation, otherwise empty string.
- `summary`: 1-3 sentence summary of what the sheet appears to contain.
- `header_fields`: Array of objects with:
  - `label`: Field label.
  - `value`: Extracted value.
  - `confidence`: "high", "medium", or "low".
  - `raw_text`: Optional raw visible text.
- `table_name`: Name for the main table.
- `columns`: Array of column names.
- `rows`: Array of row arrays. Preserve column order. Use empty string for blank cells and "[unreadable]" for unclear cells.
- `key_findings`: Array of concise observations, anomalies, or important extracted facts.
- `needs_review`: Array of fields, rows, or cells that need human review.
- `source_quality`: Array of image quality notes such as rotated image, glare, blur, crop, handwriting, or low contrast.

After `run_js` succeeds, provide the formatted report returned by the tool.

If the user explicitly asks to share, send, forward, WhatsApp, or prepare for WhatsApp, also call the `run_intent` tool with:

- `intent`: "share_text"
- `parameters`: A JSON string with:
  - `title`: "Log Sheet Report"
  - `text`: The formatted report returned by `run_js`

## Rules

- Preserve uncertainty. Never guess a number, heat number, timing, or grade.
- Keep the final answer practical and short.
- If there is no image, ask the user to attach a log sheet photo.
- For WhatsApp, use only the share action. Do not claim to read WhatsApp chats or send without user confirmation.
