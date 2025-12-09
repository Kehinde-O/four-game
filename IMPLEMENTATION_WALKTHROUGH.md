# Connect Four Blazor Game - Implementation Walkthrough

## How Connect Four is Played

### Game Rules:
1. **Board**: 7 columns × 6 rows (42 total slots)
2. **Objective**: Be the first player to get 4 pieces in a row (horizontally, vertically, or diagonally)
3. **Gameplay**:
   - Players alternate turns
   - On your turn, choose a column and drop a piece
   - Pieces fall to the lowest available row in that column
   - You cannot place a piece in a full column
   - Game ends when:
     - A player gets 4 in a row (that player wins)
     - The board is full with no winner (tie)

### Current Game Status (from screenshot):
- ✅ **6 moves completed** (3 green pieces, 3 purple pieces)
- ✅ **Player 1's turn** (green pieces)
- ✅ Pieces are correctly placed in columns
- ✅ Statistics tracking is working (shows 6 moves)

---

## Implementation Walkthrough for Video

### 1. Project Setup (0:00 - 1:00)

**What to show:**
- Blazor Web App project structure
- `Program.cs` - Service registration
- Project configuration (.NET 8, Server render mode)

**Key Points:**
```csharp
// Program.cs - GameState registered as singleton
builder.Services.AddSingleton<GameState>();
```

**Why:** GameState needs to persist across component renders, so it's a singleton service.

---

### 2. GameState Class (1:00 - 2:30)

**File:** `GameState.cs`

**What to show:**
- Board representation (42-element list)
- Win detection logic (horizontal, vertical, diagonal)
- Piece placement logic (gravity simulation)

**Key Methods:**
- `PlayPiece(int column)` - Drops piece in column, returns landing row
- `CheckForWin()` - Checks all winning combinations
- `ResetBoard()` - Clears the board

**Key Points:**
- Board is 0-indexed, stored as flat array
- Win detection checks 69 possible winning combinations
- Pieces automatically fall to bottom of column

---

### 3. Board Component Structure (2:30 - 4:00)

**File:** `Components/Board.razor`

**What to show:**
- Component markup structure
- 42 board positions (7×6 grid)
- Column selection buttons (🔽)
- Statistics display area

**Key Sections:**
```razor
<!-- Column selection buttons -->
<nav>
    @for (byte i = 0; i < 7; i++)
    {
        var col = i;
        <span @onclick="() => PlayPiece(col)">🔽</span>
    }
</nav>

<!-- Game board (42 containers) -->
<div class="board">
    @for (var i = 0; i < 42; i++)
    {
        <span class="container"><span></span></span>
    }
</div>

<!-- Game pieces (rendered separately) -->
@for (var i = 0; i < 42; i++)
{
    <span class="@pieces[i]"></span>
}
```

**Why:** Board structure and pieces are separate for CSS positioning and animations.

---

### 4. Dependency Injection (4:00 - 4:30)

**What to show:**
```razor
@inject GameState State
```

**Key Points:**
- GameState is injected into the component
- Allows component to access shared game state
- Singleton ensures state persists across page refreshes

---

### 5. Component Parameters (4:30 - 5:30)

**What to show:**
```razor
[Parameter] public Color BoardColor { get; set; } = ColorTranslator.FromHtml("yellow");
[Parameter] public Color Player1Color { get; set; } = ColorTranslator.FromHtml("red");
[Parameter] public Color Player2Color { get; set; } = ColorTranslator.FromHtml("blue");
```

**In Home.razor:**
```razor
<Board @rendermode="InteractiveServer" 
       BoardColor="System.Drawing.Color.Black" 
       Player1Color="System.Drawing.Color.Green" 
       Player2Color="System.Drawing.Color.Purple" />
```

**Key Points:**
- Parameters allow customization from parent component
- Colors are converted to CSS variables
- Dynamic styling based on parameters

---

### 6. Game Logic Implementation (5:30 - 7:30)

**Method:** `PlayPiece(byte col)`

**What to show:**
```csharp
private void PlayPiece(byte col)
{
    errorMessage = string.Empty;
    try
    {
        var player = State.PlayerTurn;  // Gets current player (1 or 2)
        var turn = State.CurrentTurn;    // Gets turn number (0-41)
        var landingRow = State.PlayPiece(col);  // Drops piece, returns row
        
        // Apply CSS classes for piece appearance and animation
        pieces[turn] = $"player{player} col{col} drop{landingRow}";
        
        // Custom feature: Track moves
        totalMoves++;
        
        // Check for win
        var winState = State.CheckForWin();
        winnerMessage = winState switch
        {
            GameState.WinState.Player1_Wins => "Player 1 Wins!",
            GameState.WinState.Player2_Wins => "Player 2 Wins!",
            GameState.WinState.Tie => "It's a tie!",
            _ => ""
        };
        
        // Update statistics if game ended
        if (winState != GameState.WinState.No_Winner)
        {
            gamesPlayed++;
            // Update win counters...
        }
    }
    catch (ArgumentException ex)
    {
        errorMessage = ex.Message;  // "Column is full" or "Game is over"
    }
}
```

**Key Points:**
- Error handling for invalid moves
- CSS class string format: `"player{player} col{col} drop{row}"`
- Win detection happens after each move
- Statistics update automatically

---

### 7. CSS Styling & Animations (7:30 - 9:00)

**File:** `Components/Board.razor.css`

**What to show:**
- Board layout (flexbox)
- Piece positioning (absolute positioning)
- Drop animations (6 different keyframe animations)
- Color variables (CSS custom properties)

**Key CSS:**
```css
/* Board background color */
:root {
    --board-bg: yellow;
    --player1: red;
    --player2: blue;
}

/* Piece drop animations */
.drop1 { animation: drop1 1s; }
.drop2 { animation: drop2 1.5s; }
/* ... up to drop6 */

/* Column positioning */
.col0 { left: calc(0em + 9px); }
.col1 { left: calc(4em + 9px); }
/* ... up to col6 */
```

**Key Points:**
- Each row has different animation duration (realistic gravity)
- Pieces positioned absolutely within containers
- CSS variables allow dynamic color changes

---

### 8. Custom Feature: Statistics Tracking (9:00 - 10:30)

**What to show:**
```csharp
// Statistics fields
private int totalMoves = 0;
private int gamesPlayed = 0;
private int player1Wins = 0;
private int player2Wins = 0;
private int ties = 0;
```

**In markup:**
```razor
<div class="stats-container">
    <div class="stat-item">
        <strong>Moves:</strong> <span class="stat-value">@totalMoves</span>
    </div>
    <!-- More statistics... -->
</div>
```

**Key Points:**
- Statistics increment during gameplay
- Persist across game resets (within session)
- Visual display with styled cards
- This is the **additional custom feature** beyond the tutorial

---

### 9. Interactive Render Mode (10:30 - 11:00)

**What to show:**
```razor
<Board @rendermode="InteractiveServer" ... />
```

**Key Points:**
- Required for event handling (@onclick)
- Server-side rendering with SignalR connection
- Real-time updates without page refresh

---

### 10. Reset Functionality (11:00 - 11:30)

**Method:** `ResetGame()`

**What to show:**
```csharp
private void ResetGame()
{
    State.ResetBoard();        // Clear game state
    winnerMessage = string.Empty;
    errorMessage = string.Empty;
    pieces = new string[42];   // Clear visual pieces
    // Statistics persist (not reset)
}
```

**Key Points:**
- Resets board and UI
- Statistics remain (session tracking)
- Button only appears when game ends

---

## Demo Flow for Video

1. **Show empty board** - Explain structure
2. **Make first move** - Show piece dropping animation
3. **Alternate players** - Show turn indicator changing
4. **Show statistics updating** - Move counter increments
5. **Try invalid move** - Show error message (full column)
6. **Complete a game** - Show win detection
7. **Show statistics** - All counters updated
8. **Reset game** - Show new game, statistics persist
9. **Show customization** - Different colors

---

## Technical Highlights

### Blazor Concepts Demonstrated:
- ✅ Razor components (.razor files)
- ✅ Component parameters ([Parameter] attribute)
- ✅ Dependency injection (@inject)
- ✅ Event handling (@onclick)
- ✅ CSS isolation (.razor.css)
- ✅ Interactive render modes
- ✅ State management (singleton service)

### Game Features:
- ✅ Full Connect Four game logic
- ✅ Win detection (all directions)
- ✅ Error handling
- ✅ Visual animations
- ✅ Color customization
- ✅ **Statistics tracking (custom feature)**

---

## File Structure Summary

```
ConnectFour/
├── Program.cs                    # Service registration
├── GameState.cs                  # Game logic & win detection
├── Components/
│   ├── Pages/
│   │   └── Home.razor           # Main page with Board component
│   └── Board.razor              # Main game component
│   └── Board.razor.css          # Styling & animations
```

---

## Key Takeaways

1. **Blazor allows C# for web development** - No JavaScript needed
2. **Component-based architecture** - Reusable, maintainable code
3. **Server-side interactivity** - Real-time updates via SignalR
4. **CSS isolation** - Component-specific styles
5. **Dependency injection** - Clean separation of concerns
6. **Custom features** - Easy to extend functionality

---

## Video Script Tips

- Start with the running application
- Show the game working first (hook viewers)
- Then dive into code structure
- Explain each file's purpose
- Highlight the custom statistics feature
- End with a complete game demonstration

