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
- `src/TransactionList.jsx` — owns filter state (`filterType`, `filterCategory`) and delete confirmation state (`confirmId`); receives `transactions` and `onDelete` props; renders a filtered `<table>` and a custom modal for delete confirmation

**State ownership:**
- `transactions` (array of `{ id, description, amount, type, category, date }`) lives in `App`; `amount` is always a number
- Form and filter state are local to their respective components
- `confirmId` (the id of the transaction pending deletion, or `null`) is local to `TransactionList`

**Data flow**: `App` passes `transactions` to `Summary` and `TransactionList`, an `onAdd` callback to `TransactionForm`, and an `onDelete` callback to `TransactionList`. No memoization.

**Key implementation details:**
- `TransactionForm` calls `parseFloat(amount)` before passing to `onAdd` so the stored value is always a number
- Delete confirmation uses a custom in-page modal (not `window.confirm`); clicking the overlay dismisses it
- All styling lives in `src/App.css` — includes CSS animations (`fadeIn`, `modalIn`, `overlayIn`), hover transitions, and a blur backdrop for the modal

**Intentional issue** (course material — fix as exercise):
- "Freelance Work" seed entry is typed as `"expense"` but categorized as `"salary"` (logical mismatch)
