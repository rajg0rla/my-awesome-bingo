# Copilot Instructions for Soc Ops Bingo Game

## Project Overview
**Soc Ops** is a social bingo game for in-person mixers. Players mark off squares when they find someone matching each prompt, with the goal of getting 5 in a row (horizontally, vertically, or diagonally).

**Tech Stack:** React 19 + TypeScript + Vite + Tailwind CSS v4 + Vitest

## Architecture Patterns

### Component Structure
- **App.tsx** - Top-level app switcher between `StartScreen` (game state: 'start') and `GameScreen` (game states: 'playing' or 'bingo')
- **useBingoGame hook** - Centralizes ALL game state (board, gameState, winningLine, modal) with localStorage persistence
- **UI Components** - `StartScreen`, `GameScreen`, `BingoBoard`, `BingoSquare`, `BingoModal` - receive props from hook
- **Game Logic** - Pure functions in `bingoLogic.ts` (generateBoard, toggleSquare, checkBingo) - NO React dependencies

**Key Rule:** Game logic stays in `utils/bingoLogic.ts` as pure functions. Components should be thin wrappers that call these utilities. This separation makes logic testable and reusable.

### State Management Pattern
Use `useBingoGame()` for all game state. It handles:
- localStorage persistence with version validation (STORAGE_VERSION = 1)
- Type-safe game state serialization/deserialization
- SSR safety checks (`typeof window !== 'undefined'`)
- Automatic saving on every state change via useEffect dependency array

**Important:** The hook uses `queueMicrotask` when checking for bingo after a square toggle to batch state updates properly.

### Data Types (src/types/index.ts)
```typescript
interface BingoSquareData { id: number; text: string; isMarked: boolean; isFreeSpace: boolean; }
interface BingoLine { type: 'row' | 'column' | 'diagonal'; index: number; squares: number[]; }
type GameState = 'start' | 'playing' | 'bingo';
```

## Key Implementation Details

### Board Generation
- Always 5x5 (25 squares total)
- Center square (index 12) is FREE_SPACE, always marked
- 24 questions randomly shuffled from `src/data/questions.ts`
- Questions pool must have ≥24 items with FREE_SPACE exported separately

### Winning Logic
- 5 marked squares in a row (5 types: 5 rows + 5 columns + 2 diagonals = 12 possible winning lines)
- Center free space counts toward winning lines
- `checkBingo()` returns first matching line or null
- On win: gameState → 'bingo', show BingoModal, highlight winning squares

### localStorage Persistence
- Key: 'bingo-game-state' (sync with useBingoGame.ts)
- Validates version, gameState enum, board structure, and BingoLine format before loading
- Clears invalid data with console warning
- Guards against SSR execution contexts

## Development Workflows

### Local Development
```bash
npm run dev          # Vite dev server (hot reload enabled)
npm run test         # Vitest (runs once, no watch)
npm run build        # TypeScript + Vite production build
npm run lint         # ESLint check
```

### Build & Deploy
- Push to `main` → GitHub Actions auto-deploys to GitHub Pages
- Base path: `process.env.VITE_REPO_NAME` determines app root (for GitHub Pages subfolder deployment)

## Testing Patterns

### Test Setup
- Use Vitest with globals enabled (describe, it, expect available without imports)
- jsdom environment for DOM testing
- Setup file: `src/test/setup.ts` (provides Testing Library globals)
- Tests should NOT watch mode (`watch: false` in config)

### Testing Utilities
- `@testing-library/react` for component testing (avoid shallow rendering)
- Pure function tests in `bingoLogic.test.ts` - mock Math.random for deterministic tests
- Use `beforeEach` for mockBoard setup
- Use `vi.spyOn()` for mocking when needed

## Code Conventions

### TypeScript
- Strict mode enabled (tsconfig.json references)
- Export types alongside implementations in utils
- Use type guards (see validateStoredData pattern) for localStorage deserialization

### React Patterns
- Functional components with hooks only
- Use `useCallback` for memoized callbacks (especially in custom hooks)
- Use `useMemo` for derived state (e.g., winningSquareIds from winningLine)
- Props interfaces named as `ComponentNameProps`

### Tailwind CSS v4
- Imported via `@tailwindcss/vite` plugin (NOT tailwindcss.config.ts)
- Use Tailwind utility classes directly in JSX
- No custom CSS in component files - Tailwind + index.css only
- Use `flex`, `grid` for layout; color scheme: grays with accent colors (amber, blue)

### File Organization
```
src/
  components/    # React components (UI only, no logic)
  data/          # Static data (questions list)
  hooks/         # Custom hooks (useBingoGame)
  types/         # Type definitions
  utils/         # Pure functions & game logic
  test/          # Test setup & utilities
```

## Common Tasks

### Adding New Questions
Edit `src/data/questions.ts` - must export default array of ≥24 strings plus `FREE_SPACE` constant.

### Modifying Game Logic
1. Add/update pure function in `bingoLogic.ts`
2. Export type alongside function
3. Add tests in `bingoLogic.test.ts` BEFORE modifying implementation
4. Update useBingoGame hook if logic changes state transitions

### Styling Changes
- Modify existing Tailwind classes in component files or add to `src/index.css`
- No CSS-in-JS or component-scoped styles
- Test responsive design - app should work on mobile (primary use case)

### Adding Modal/Dialog Features
Reference existing `BingoModal` pattern - use conditional rendering in App.tsx driven by boolean flags in game state.

## Edge Cases & Known Patterns

### Preventing Double-Click Bingo
`handleSquareClick` uses `queueMicrotask` to prevent race conditions when checking bingo immediately after toggle.

### localStorage Validation
Must validate stored data structure before hydrating - see `validateStoredData` type guard. Always clear on version mismatch.

### SSR Compatibility
All localStorage/window access wrapped in `typeof window !== 'undefined'` checks (for future SSR support).

## References
- Game logic: [bingoLogic.ts](../../src/utils/bingoLogic.ts)
- State management: [useBingoGame.ts](../../src/hooks/useBingoGame.ts)  
- Questions data: [questions.ts](../../src/data/questions.ts)
- Component entry: [App.tsx](../../src/App.tsx)
