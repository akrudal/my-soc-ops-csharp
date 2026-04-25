---
name: Component Generator
description: "Scaffold new Blazor components with boilerplate, patterns, and optional routing. Use when: creating new .razor components, building interactive UI, adding pages to the app"
applyTo: "SocOps/Components/**/*.razor,SocOps/Pages/**/*.razor"
---

# Component Generator Agent

## Goal

Generate production-ready Blazor components following project conventions with minimal boilerplate.

## Workflow

1. **Gather Requirements**
   - Component name and purpose
   - Parameters (props)
   - Event callbacks needed
   - Service dependencies
   - Routing (if page)

2. **Generate Boilerplate**
   - Scaffold full `.razor` file with structure
   - Create optional `.razor.css` for scoped styles
   - Include service injection pattern
   - Add lifecycle hooks as needed

3. **Apply Conventions**
   - Follow naming from `blazor-components.instructions.md`
   - Use utility classes from `css-utilities.instructions.md`
   - Include JSDoc comments for parameters
   - Add @key directive for loops

4. **Output**
   - Ready-to-use component file
   - Optional styles file
   - Usage example in parent component

## Component Template

When generating, scaffold this structure:

```razor
@* ComponentName.razor *@
@page "/optional-route"
@implements IAsyncDisposable

<div class="container">
    <h1>@Title</h1>
    <!-- Component content -->
</div>

@code {
    [Parameter]
    public required string Title { get; set; }

    [Inject]
    public required BingoGameService GameService { get; set; }

    protected override async Task OnInitializedAsync()
    {
        // Initialize
    }

    async ValueTask IAsyncDisposable.DisposeAsync()
    {
        // Cleanup
    }
}
```

## Example Prompts

### Create Interactive Component
```
Generate a ScoreDisplay component that:
- Shows current score and round number
- Has parameters for Score (int) and Round (int)
- Updates when OnScoreChanged event fires
- Uses the GameState model from Models/
- Includes a reset button that triggers OnReset callback
```

### Create Page Component
```
Create a Settings page at /settings that allows users to:
- Toggle sound effects
- Change difficulty level
- View game statistics

Include form inputs, labels, and bind to GameService state.
```

### Create Reusable UI
```
Build a ConfirmationModal component with:
- Title and message parameters
- OnConfirm and OnCancel callbacks
- Primary/secondary button styling from CSS utilities
- Keyboard support (Enter to confirm, Esc to cancel)
```

## Key References

- Conventions: `.github/instructions/blazor-components.instructions.md`
- CSS Utilities: `.github/instructions/css-utilities.instructions.md`
- Design Guidance: `.github/instructions/frontend-design.instructions.md`
- Example Components: `SocOps/Components/` (BingoBoard.razor, GameScreen.razor, etc.)
