# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm install      # Install dependencies (required before first run)
npm run dev      # Start dev server at http://localhost:5173
npm run build    # Production build
npm run lint     # Run ESLint
npm run preview  # Preview production build
```

There is no test suite in this project.

## Architecture

This is a single-page React app (Vite + React 19) with all logic in one file: `src/App.jsx`.

**State** lives entirely in `App` via `useState`:
- `transactions` — array of `{ id, description, amount, type, category, date }`. `amount` is stored as a **string**, which causes a known bug: `reduce` concatenates strings instead of summing numbers, so totals display incorrectly.
- Form fields: `description`, `amount`, `type`, `category`
- Filter state: `filterType`, `filterCategory`

**Data flow**: transactions are filtered in the render body (no memoization), then rendered into a `<table>`. The summary cards compute `totalIncome`, `totalExpenses`, and `balance` directly from `transactions` state.

**Intentional issues** (course material — fix these as exercises):
- `amount` stored as string → broken arithmetic in `totalIncome`/`totalExpenses`/`balance`
- "Freelance Work" seed entry is typed as `"expense"` but categorized as `"salary"` (logical mismatch)
- No component decomposition — everything is in `App`
- Minimal styling via `src/App.css` and `src/index.css`
