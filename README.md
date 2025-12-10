# Connect Four Game - Blazor Implementation

A complete Connect Four game built with Blazor following the Microsoft Learn tutorial, with an additional statistics tracking feature.

## Assignment Requirements

✅ **Completed all tutorial units** - All 8 units from the "Build Connect Four game with Blazor" module have been implemented:
- Unit 1: Introduction
- Unit 2: Blazor basics
- Unit 3: Exercise - Blazor (Board component creation)
- Unit 4: Game logic concepts
- Unit 5: Exercise - Game logic (Full game implementation)
- Unit 6: Exercise - Customization using parameters (Color customization)
- Unit 7: Module assessment
- Unit 8: Summary

✅ **Additional Custom Feature** - Statistics Tracking System:
- **Move Counter**: Tracks total moves made across all games
- **Games Played Counter**: Tracks number of completed games
- **Player Win Tracking**: Separate counters for Player 1 wins, Player 2 wins, and ties
- **Session Persistence**: Statistics persist across game resets (within the same session)
- **Visual Display**: Statistics are displayed in a styled panel above the game board

## Features

### Core Game Features
- ✅ 7×6 game board (42 positions)
- ✅ Two-player gameplay (alternating turns)
- ✅ Win detection (horizontal, vertical, diagonal)
- ✅ Tie detection (full board with no winner)
- ✅ Error handling (full columns, game over states)
- ✅ Reset game functionality
- ✅ Piece drop animations
- ✅ Color customization (Board, Player 1, Player 2 colors)

### Additional Feature: Statistics Tracking
The statistics panel displays:
- **Moves**: Total number of pieces placed across all games
- **Games Played**: Number of completed games
- **Player 1 Wins**: Count of Player 1 victories
- **Player 2 Wins**: Count of Player 2 victories
- **Ties**: Number of tied games

Statistics are updated automatically when games end and persist across game resets within the same browser session.

## Technology Stack

- **.NET 8.0**
- **Blazor Server** (InteractiveServer render mode)
- **C#** for game logic
- **Razor Components** for UI
- **CSS** for styling and animations
- **System.Drawing.Common** for color customization

## Project Structure

```
ConnectFour/
├── Components/
│   ├── Pages/
│   │   └── Home.razor          # Main page with Board component
│   ├── Layout/
│   │   ├── MainLayout.razor     # Main layout
│   │   └── NavMenu.razor        # Navigation menu
│   └── Board.razor              # Main game board component
│   └── Board.razor.css          # Board styling and animations
├── GameState.cs                 # Game logic and win detection
├── Program.cs                   # Service registration
└── README.md                    # This file
```

## How to Run

1. **Prerequisites**: .NET 8.0 SDK installed
2. **Clone the repository**:
   ```bash
   git clone https://github.com/Kehinde-O/four-game.git
   cd four-game
   ```
3. **Restore dependencies**:
   ```bash
   dotnet restore
   ```
4. **Run the application**:
   ```bash
   dotnet run
   ```
5. **Open in browser**: Navigate to `https://localhost:7224` or `http://localhost:5273`

## How to Play

1. Click the 🔽 button above a column to drop a piece
2. Players alternate turns automatically
3. First player to get 4 pieces in a row (horizontal, vertical, or diagonal) wins
4. Click "Reset the game" to start a new game (statistics persist)
5. Watch the statistics panel update as you play!

## Customization

The game board colors can be customized in `Home.razor`:

```razor
<Board @rendermode="InteractiveServer" 
       BoardColor="System.Drawing.Color.Black" 
       Player1Color="System.Drawing.Color.Green" 
       Player2Color="System.Drawing.Color.Purple" />
```

## Additional Feature Implementation Details

The statistics tracking feature is implemented in `Board.razor`:

- **Fields**: `totalMoves`, `gamesPlayed`, `player1Wins`, `player2Wins`, `ties`
- **Update Logic**: Statistics increment during gameplay and when games end
- **Display**: Statistics are shown in a styled container with individual stat items
- **Persistence**: Statistics persist across game resets within the same browser session

## Video Demonstration

A video demonstration of the additional statistics tracking feature is available (to be submitted in Canvas).

## Assignment Submission Checklist

- ✅ All tutorial units completed
- ✅ Additional feature implemented (Statistics Tracking)
- ✅ Application uploaded to GitHub repository
- ⏳ Video demonstration (to be recorded and submitted)

## License

This project is part of a CSE 325 assignment at Brigham Young University-Idaho.

