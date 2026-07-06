# Trello-Style Kanban Board

A single-file kanban board with drag-and-drop cards and persistent storage, built in vanilla HTML, CSS, and JavaScript.

## Features

- **Card creation** — add cards to any lane via the "+ Add a card" control; add new lanes via "+ Add another lane"
- **Movement** — drag and drop cards between lanes (or reorder within a lane) using the native HTML5 Drag and Drop API
- **Persistence** — board state is saved to `localStorage` on every change and reloaded automatically, so it survives a page refresh
- Inline lane renaming and card/lane deletion

## How to Run

No build step or server needed. Just open `kanban_board_standalone.html` directly in any browser.

```
open kanban_board_standalone.html
```

(or double-click the file / drag it into a browser window)

> Note: if opened via `file://` in some browsers, `localStorage` can behave inconsistently. For guaranteed behavior, you can also serve it locally:
> ```
> python3 -m http.server 8000
> ```
> then visit `http://localhost:8000/kanban_board_standalone.html`

## Implementation Notes

- **State shape**: a single JS object holding `lanes` (ordered array of `{id, title, cardIds}`) and `cards` (a map of `cardId -> {id, text, createdAt, num}`). Lanes reference cards by ID, so moving a card is just removing its ID from one lane's array and inserting it into another's.
- **Drag and drop**: cards are marked `draggable`, with `dragstart` capturing the card ID (via `dataTransfer.setData`) and `dragover`/`drop` on each lane's card list handling the reordering logic, including calculating drop position from cursor Y-coordinate so a card can land at a specific spot, not just get appended.
- **Persistence**: the entire state object is serialized to JSON and written to `localStorage` on every change (debounced slightly), then parsed back out on load. If no saved state exists, a small set of default lanes/cards is shown.
- **No frameworks**: built without React/Tailwind/WebSockets — plain DOM APIs and CSS, since the app's state and interactions are simple enough not to need a framework.

## Technologies Used

HTML, CSS, JavaScript, HTML5 Drag and Drop API, localStorage
