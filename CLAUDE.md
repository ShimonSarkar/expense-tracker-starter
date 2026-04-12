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

This is a single-page React app (Vite + React 19) decomposed into four components:

- `src/App.jsx` — root component; holds `transactions` state and passes it down
- `src/Summary.jsx` — computes and displays `totalIncome`, `totalExpenses`, and `balance` from `transactions` prop
- `src/TransactionForm.jsx` — owns its own form state (`description`, `amount`, `type`, `category`); calls `onAdd(transaction)` prop on submit
- `src/TransactionList.jsx` — owns its own filter state (`filterType`, `filterCategory`); receives `transactions` prop and renders a filtered `<table>`

**State ownership:**
- `transactions` (array of `{ id, description, amount, type, category, date }`) lives in `App`; `amount` is a number
- Form and filter state are local to their respective components

**Data flow**: `App` passes `transactions` to `Summary` and `TransactionList`, and an `onAdd` callback to `TransactionForm`. No memoization.

**Intentional issue** (course material — fix as exercise):
- "Freelance Work" seed entry is typed as `"expense"` but categorized as `"salary"` (logical mismatch)
- Minimal styling via `src/App.css` and `src/index.css`
