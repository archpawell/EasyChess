# Antigravity Project Specification & Prompt: 2D Top-Down PvP Chess

## 1. Project Overview
Build a complete, responsive 2D Top-Down Player-vs-Player (local pass-and-play on a single computer) Chess browser game using HTML5, Tailwind CSS, and vanilla JavaScript. The layout must feature a large central 8x8 play field (`max-width: min(86vh, 880px)`, `88%` figure scale inside squares) flanked by a left tactical/customization sidebar and a right Turns Log & undo history sidebar.

## 2. Core Gameplay & Chess Engine Requirements
- **Local PvP (One Computer)**: Alternating turns between White and Black with a top status banner showing active player turn, check warnings, checkmate, stalemate, 50-move draw, insufficient material draw, mutual draw agreement, or resignation.
- **Full Legal Move Validation**:
  - Standard piece movement for Pawn, Rook, Knight, Bishop, Queen, and King.
  - **Castling** (Kingside `O-O` and Queenside `O-O-O`) verifying unmoved King/Rook, clear path, not currently in check, and not passing through attacked squares.
  - **En Passant** capture tracking immediately after a two-square pawn advance.
  - **Pawn Promotion Modal**: When a pawn reaches rank 8 or 1, present an interactive modal to promote into Queen, Rook, Bishop, or Knight rendered in the active skin and figure color.
  - **Self-Check Prevention**: Filter out any pseudo-legal move that leaves the active player's King in check.
- **Board Flip**: Allow flipping the 2D board orientation (`White at bottom` vs `Black at bottom`) at any time while keeping rank/file coordinates accurate.

## 3. Tactical Overlays (Toggleable Anytime)
- **Active Figure Movement (Blue Dots)**:
  - Optional checkbox toggle (enabled by default).
  - When an active player selects one of their figures, highlight valid empty destination cells with a centered **blue dot** (`#2563eb` with white contrast ring) and valid capture/en-passant cells with a **blue capture ring**.
- **Show dangerous turns (Blue & Red Dots)**:
  - Optional checkbox toggle (enabled by default).
  - Evaluates whether each legal destination square (including en passant exposure for double pawn pushes) leaves the moved piece under enemy attack, rendering a **blue dot** (`#2563eb`) for safe cells and a **red dot** (`#ef4444`) for dangerous cells.
- **Enemy Attack Zones (50% Transparent Red)**:
  - Optional checkbox toggle.
  - When enabled, compute all squares currently attacked/controlled by all enemy figures and overlay those cells with a **50% transparent red layer** (`rgba(239, 68, 68, 0.5)`).
- **Board Coordinates**: Optional toggle for rank (`1-8`) and file (`a-h`) labels with automatic contrast calculation against custom square colors.

## 4. Customizable Field Colors & Figure Colors (Full Color Scale + Presets)
- **Field Colors ("White" & "Black" Cells)**:
  - Provide HTML5 color scale pickers (`<input type="color">`), hex readouts, a Reset button, and 4 quick theme presets (Slate Minimal, Tournament Green, Classic Wood, Arctic Ice) to customize light and dark board cells live during gameplay.
- **Figure Colors ("White" & "Black" Figures)**:
  - Provide matching HTML5 color scale pickers (`<input type="color">`), hex readouts, a Reset button, and 4 quick presets (Classic Ivory & Slate, Pastel Pink & Royal Violet, Imperial Gold & Crimson, Sky Cyan & Emerald) to dynamically recolor both White and Black figures across all built-in and sprite-sheet skin packs.

## 5. Turns Log & Unlimited Turn Cancellation (Undo / Rewind)
- **Right-Side Turns Log**:
  - Record every move in Standard Algebraic Notation (SAN), paired by full move number (`1. e4 e5`).
- **Cancel Any Amount of Turns**:
  - **Start (`<<`)**: Cancel all turns and rewind to move 0.
  - **Undo Turn (`<`)**: Cancel the last half-move.
  - **Redo (`>`)**: Step forward through undone turns.
  - **Interactive History Jump**: Clicking any move in the Turns Log immediately restores the exact board state, castling rights, en passant target, and captured pieces at that point in time. Making a new move from a rewound state branches the timeline cleanly.

## 6. Lost / Captured Figures Display (50% Size & 50% Transparent)
- Display lost figures for each player in both the player bars and dedicated **"Lost by White"** / **"Lost by Black"** summary trays below the board.
- Render each lost figure at **50% size (`22px`) and 50% opacity (`opacity: 0.5`)** using the currently active figure skin pack and figure color scale, alongside live material advantage counters (`+1`, `+3`, etc.).

## 7. Optional Handicap Setup Mode
- Provide an **"Enter Handicap Edit Mode"** button that lets players click any non-King figure on the board from one or both sides to remove it (or click an emptied square to restore it) before starting the match.
- Allow choosing whether White or Black moves first in the handicap game, automatically syncing castling rights with the remaining rooks.

## 8. Switchable Custom Figure Skin Packs & Guinea Pig2 Pack
- Allow switching figure skin packs at any time—even mid-game—updating the board, captured trays, and promotion modal immediately:
  1. **Guinea Pig2 (Attached Pack — Default)**:
     - Supports slicing the 3x2 sprite sheet (`Gemini_Generated_Image_if56f5if56f5if56_2.jpeg`, ordered Top: `[Pawn, Rook, Knight]`, Bottom: `[Bishop, Queen, King]`) with edge flood-fill background removal and dynamic luminance tinting, paired with 1:1 matching vector illustrations of all 6 Guinea Pig2 characters:
       - **Pawn**: 3/4 right-facing plump guinea pig with center white blaze and grey cheek patches.
       - **Rook**: 3/4 right-facing guinea pig wearing a stone castle rook turret crown.
       - **Knight**: Piggyback cavalry guinea pig rider in a medieval visored helmet with plume mounted on a 3/4 left-facing steed guinea pig.
       - **Bishop**: Tall front-facing guinea pig wearing a bishop mitre hat and holding a diagonal scepter with both front paws.
       - **Queen**: Front-facing plump guinea pig wearing a 5-point ball-tipped crown and scalloped collar with an oval brooch.
       - **King**: Front-facing majestic guinea pig wearing a 3-peak diamond crown and full royal ermine-trimmed cape.
  2. **Classic Minimal**: Clean geometric chess silhouettes.
  3. **Geometric Tokens**: Top-down circular tokens with typography and dashed inner rings.
  4. **Modern Line**: Nordic minimalist symbol tokens.
  5. **Custom Skin Pack Editor Modal**:
     - Includes a **Quick 3x2 Sprite Sheet Auto-Slicer** (upload any 3x2 character sheet image, auto-remove white card background, and enable dynamic Figure Color tinting) plus 12 individual piece upload/SVG/URL slots persisted to `localStorage`.