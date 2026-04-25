---
description: Blazor component patterns and best practices for Soc Ops. Apply when creating or modifying .razor components.
applyTo: "SocOps/Components/**/*.razor"
---

# Blazor Component Conventions

## Component Structure

Always organize your components following this pattern:

```razor
@* Props and routing *@
@page "/optional-route"
@implements IAsyncDisposable

@* Markup *@
<div class="container">
    <h1>@Title</h1>
    <button @onclick="HandleClick">Action</button>
</div>

@* Code-behind *@
@code {
    @* Parameters *@
    [Parameter] public string Title { get; set; } = "Default";
    [Parameter] public EventCallback<string> OnUpdate { get; set; }

    @* Injected services *@
    [Inject] public BingoGameService GameService { get; set; } = null!;

    @* Private state *@
    private bool isLoading;
    private List<string> items = [];

    @* Lifecycle methods *@
    protected override async Task OnInitializedAsync()
    {
        // Initialize component, load data
        await GameService.InitializeAsync();
    }

    protected override async Task OnParametersSetAsync()
    {
        // React to parameter changes
    }

    @* Event handlers *@
    private async Task HandleClick()
    {
        // Use InvokeAsync for state updates
        await InvokeAsync(async () =>
        {
            await GameService.UpdateStateAsync();
            StateHasChanged();
        });
    }

    @* Cleanup *@
    async ValueTask IAsyncDisposable.DisposeAsync()
    {
        // Unsubscribe from events, cleanup
    }
}
```

## Key Patterns

### Parameters and Callbacks
```razor
[Parameter] public required string Id { get; set; }
[Parameter] public EventCallback OnClose { get; set; }
[Parameter(CaptureUnmatchedValues = true)] public Dictionary<string, object>? Attributes { get; set; }
```

### Service Injection & Events
```csharp
[Inject] public BingoGameService GameService { get; set; } = null!;

protected override async Task OnInitializedAsync()
{
    GameService.OnStateChanged += StateChangedCallback;
    await GameService.InitAsync();
}

private void StateChangedCallback()
{
    InvokeAsync(StateHasChanged);
}

async ValueTask IAsyncDisposable.DisposeAsync()
{
    GameService.OnStateChanged -= StateChangedCallback;
}
```

### Conditional Rendering
```razor
@if (isLoading)
{
    <p>Loading...</p>
}
else if (items.Any())
{
    @foreach (var item in items)
    {
        <div>@item</div>
    }
}
else
{
    <p>No items found.</p>
}
```

## Naming Conventions

- **Component files**: PascalCase matching type name (`BingoBoard.razor` contains `BingoBoard` component)
- **Parameters**: PascalCase (`[Parameter] public string GameId`)
- **Event handlers**: Prefix with verb or `Handle` (`HandleClick`, `OnKeyDown`)
- **Private methods/fields**: camelCase (`_state`, `handleUpdate()`)

## CSS and Styling

- Add scoped CSS in `ComponentName.razor.css`
- Use utility classes from `wwwroot/css/app.css` first
- Avoid inline styles; use semantic class names
- Example:
  ```css
  /* BingoSquare.razor.css */
  .square {
    @apply p-2 border rounded cursor-pointer transition;
  }
  .square.marked {
    @apply bg-marked text-white;
  }
  ```

## State Management Best Practices

1. **Prop drilling**: Pass simple data via Parameters
2. **Complex state**: Use injected service (`BingoGameService`)
3. **Local state**: Keep UI state local (isExpanded, isAnimate) in @code block
4. **Avoid mutable Parameter objects**: Clone before modifying
5. **Trigger StateHasChanged()** only after async operations that modify state

## Performance Considerations

- Use `@key` directive in loops to reduce DOM churn:
  ```razor
  @foreach (var item in items)
  {
      <BingoSquare @key="item.Id" Data="item" />
  }
  ```
- Minimize StateHasChanged() calls; batch updates
- Consider `ComponentBase.SetParametersAsync()` for complex param validation
- Lazy-load heavy components with `@if` conditions

## Testing Patterns

- **Component testing**: Use bUnit for unit tests
- **Service mocking**: Mock injected services in tests
- **Event callbacks**: Verify EventCallback invocations
- Example: See `.solutions/` for reference implementations

## When to Extract a Component

Extract when:
- Markup is reused 2+ times
- Component has distinct responsibility
- Prop interface is clean (< 5 parameters)

Don't extract if it increases complexity (e.g., tight coupling, prop drilling).
