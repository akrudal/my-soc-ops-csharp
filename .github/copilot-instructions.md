# Soc Ops Workspace Guidelines

## ✓ Development Checklist (Before Commit)

- [ ] `dotnet build SocOps/SocOps.csproj` passes with no errors
- [ ] Code follows C# PascalCase for public members, camelCase for private
- [ ] No unused variables, imports, or dead code
- [ ] Component structure follows `.github/instructions/blazor-components.instructions.md`

---

## Quick Start

```bash
dotnet build SocOps/SocOps.csproj    # Build
dotnet run --project SocOps/SocOps.csproj  # Dev server (http://localhost:5166)
```

## Code Style

**C#**: PascalCase for public members, camelCase for private. Enable `<Nullable>enable</Nullable>`.

**Blazor**: Components in `Components/` (reusable), pages in `Pages/` (routable). Use service injection for complex state.

**Naming**: `ComponentName.razor`, method handlers prefixed with verb (`HandleClick`, `OnStateChanged`).

## Architecture

| Folder | Purpose |
|--------|---------|
| `Components/` | Reusable Blazor components (BingoBoard, BingoSquare, GameScreen, etc.) |
| `Models/` | Data structures (BingoSquareData, GameState, BingoLine) |
| `Services/` | Business logic (BingoGameService state, BingoLogicService rules) |
| `Data/` | Static configuration (Questions.cs) |
| `Pages/` | Routable pages (Home.razor) |
| `wwwroot/` | Static assets & CSS utilities |

## State Management

- `BingoGameService`: Holds game state, triggers `OnStateChanged` events
- Components subscribe to service events and call `StateHasChanged()`
- Persist to localStorage via JSInterop
- Use dependency injection to inject services into components

## Styling

Custom CSS utility classes in `wwwroot/css/app.css` (Tailwind-like approach):

```
Layout: .flex, .grid, .grid-cols-5, .items-center
Spacing: .p-4, .mb-2, .mx-auto, .gap-2
Colors: .bg-accent (primary), .bg-marked (selected), .text-gray-700
Type: .text-lg, .font-semibold, .text-center
```

See `.github/instructions/css-utilities.instructions.md` and `.frontend-design.instructions.md` for detailed patterns.

## Key Files & Patterns

- **Components**: See `SocOps/Components/BingoBoard.razor` for structure & service injection
- **Services**: See `SocOps/Services/BingoGameService.cs` for event-driven state pattern
- **Styling**: See `wwwroot/css/app.css` for utility classes

## Troubleshooting

| Issue | Fix |
|-------|-----|
| Port conflict | Modify `SocOps/Properties/launchSettings.json` |
| Build fails | Run `dotnet clean` then `dotnet build` |
| HMR not working | Refresh browser if dev server is running |
| State lost | Check localStorage in DevTools (Application tab) |

## Documentation & Resources

- **Lab Guide**: `workshop/` folder (offline) or [online docs](https://dotnet-presentations.github.io/vscode-github-copilot-agent-lab/docs/)
- **Contributing**: See `CONTRIBUTING.md` (CLA, CoC)
- **Component Patterns**: `.github/instructions/blazor-components.instructions.md` (applies to `SocOps/Components/**/*.razor`)
- **Available Agents**: `tdd`, `quiz-master`, `ui-review`, `pixel-jam`
