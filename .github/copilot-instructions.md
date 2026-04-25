# Soc Ops Workspace Guidelines

**Soc Ops** is a Social Bingo game built with Blazor WebAssembly (.NET 10). Players find people matching trivia questions to mark squares on a 5x5 board and race to get 5 in a row. This is also a hands-on workshop exploring Copilot agents, custom instructions, and multi-agent development.

## Build and Test

```bash
# From workspace root
dotnet build SocOps/SocOps.csproj              # Build the project
dotnet run --project SocOps/SocOps.csproj     # Run dev server (http://localhost:5166)
```

> **Dev Container**: If working in GitHub Codespaces, the `.devcontainer/devcontainer.json` automatically restores dependencies on startup.

## Code Style and Conventions

### C# Guidelines
- **Naming**: Use PascalCase for public members, camelCase for private fields and parameters
- **Components**: Place reusable UI in `Components/`, routable pages in `Pages/`
- **Services**: Business logic in `Services/` organized by domain (GameService, LogicService)
- **Models**: Data structures in `Models/`, static data in `Data/`
- **Nullable**: Project enables `<Nullable>enable</Nullable>`; prefer nullable annotations

### Blazor Components
- **.razor files**: Combine markup, C# code-behind, and scoped CSS
- **Event handling**: Use `@onXyz` directives; for complex state, delegate to a service
- **Props**: Use `[Parameter]` and `[Parameter(CaptureUnmatchedValues = true)]`
- **Lifecycle**: `OnInitialedAsync()`, `OnParametersSetAsync()` for setup; `Dispose()` for cleanup

Example structure:
```razor
@* Markup *@
<button @onclick="HandleClick">Click me</button>

@code {
    [Parameter] public string Message { get; set; }

    private async Task HandleClick() 
    { 
        // Handle event
    }
}
```

## Architecture

```
SocOps/
├── Components/                # Reusable Blazor components
│   ├── BingoBoard.razor      # 5x5 game board display
│   ├── BingoSquare.razor     # Individual square with toggle state
│   ├── GameScreen.razor      # Active game UI
│   ├── BingoModal.razor      # Modal overlay for win/loss
│   └── StartScreen.razor     # Initial game setup
├── Models/                    # Data structures
│   ├── BingoSquareData.cs    # Square state (marked, question, answer)
│   ├── GameState.cs          # Overall game state (score, board, timer)
│   └── BingoLine.cs          # Winning line detection
├── Services/                  # Business logic & state management
│   ├── BingoGameService.cs   # State management, event-driven updates
│   └── BingoLogicService.cs  # Game rules, win detection, shuffling
├── Data/                      # Static configuration
│   └── Questions.cs          # Bingo question bank
├── Pages/                     # Routable pages
│   ├── Home.razor            # Landing/game entry
│   ├── Counter.razor         # Demo page
│   └── NotFound.razor        # 404 handler
├── Layout/                    # Layout components
│   ├── MainLayout.razor      # Root layout
│   └── NavMenu.razor         # Navigation
└── wwwroot/                   # Static assets
    ├── css/
    │   └── app.css           # Utility classes (see CSS Styling)
    ├── index.html            # Entry point
    └── lib/                   # Third-party libraries
```

## State Management Pattern

- **BingoGameService**: Holds game state (board, score, game state enum)
- **Event-driven updates**: Components subscribe to `OnStateChanged` event
- **Persistence**: Use JSInterop to save/restore state from localStorage
- **Initialization**: Services injected via dependency injection in components

## Styling

This project uses custom CSS utility classes (Tailwind-like approach) in `wwwroot/css/app.css`:
- **Layout**: `.flex`, `.grid`, `.grid-cols-5`, `.items-center`, `.justify-center`
- **Spacing**: `.p-4`, `.mb-2`, `.mx-auto`, `.gap-2`
- **Colors**: `.bg-accent` (primary), `.bg-marked` (selected), `.text-gray-700`
- **Typography**: `.text-lg`, `.font-semibold`, `.text-center`

For comprehensive style patterns and new component design, see `.github/instructions/css-utilities.instructions.md` and `.github/instructions/frontend-design.instructions.md`.

## Related Documentation

- **Lab Guide**: `workshop/` (offline guide) or view [online step.html](https://dotnet-presentations.github.io/vscode-github-copilot-agent-lab/docs/)
  - 00-Overview: Project goals and workshop structure
  - 01-Setup: Dev environment (this checklist)
  - 02-Design: Blazor component design patterns
  - 03-Quiz Master: Custom quiz master agent
  - 04-Multi-Agent: Orchestrating multiple agents together
- **Contributing**: `CONTRIBUTING.md` (CLA, code of conduct)
- **Solutions**: `.solutions/` folder contains step-by-step implementations for reference

## Available Agents

Pre-built specialized agents for common workflows:

| Agent | Purpose |
|-------|---------|
| `tdd` | Full TDD cycle orchestration (Red → Green → Refactor) |
| `quiz-master` | Interactive quiz master for game content |
| `ui-review` | Visual design review and feedback |
| `pixel-jam` | Rapid UI prototyping and polish |

Invoke these with `/create-agent` or in the Agents panel.

## Development Workflow Tips

1. **Before committing**: Ensure `dotnet build` passes with no errors
2. **Component testing**: Manually test in the running dev server (HMR enabled)
3. **State debugging**: Inspect localStorage and browser console for state changes
4. **Styling**: Check existing utilities in `app.css` before adding new CSS
5. **Questions/Content**: Modify game questions in `Data/Questions.cs`

## Troubleshooting

- **Port conflict**: If port 5166 is in use, modify `SocOps/Properties/launchSettings.json` to change port
- **Build fails**: Run `dotnet clean` then `dotnet build SocOps/SocOps.csproj`
- **HMR not working**: Ensure dev server is running; refresh browser if needed
- **localStorage issues**: Check browser DevTools Application → LocalStorage for game state
