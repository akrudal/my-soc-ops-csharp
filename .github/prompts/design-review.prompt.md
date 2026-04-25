---
name: design-review
description: Analyze visual design and UX of components/pages. Provides feedback on typography, colors, spacing, animations, and accessibility.
---

# Design Review Prompt

## Purpose

Conduct a structured visual design and user experience review of Blazor components or pages in the Soc Ops project.

## How to Use

Type `/design-review` and provide:

1. **Component or Page**: Name of the `.razor` file to review (e.g., `StartScreen.razor`)
2. **Context**: What is it supposed to do? Who are the users?
3. **Goals**: What should it feel like? (e.g., "welcoming", "engaging", "professional")
4. **Specific areas**: Optional—focus on typography, color, spacing, animations, interactions

Example:
```
/design-review

Component: BingoBoard.razor
Context: Players mark squares during active game session
Goals: Feel playful, easy to tap, visually exciting feedback
Focus on: Click animations, contrast for marked squares, visual hierarchy
```

## Review Checklist

The review will assess:

### Typography
- [ ] Font choices (avoid generic: Arial, Inter, Roboto)
- [ ] Font sizes create hierarchy
- [ ] Line height aids readability
- [ ] Weights support emphasis

### Color & Theme
- [ ] Palette is cohesive (not scattered)
- [ ] Contrast passes WCAG AA (4.5:1 for text)
- [ ] Accent colors guide attention
- [ ] Light/dark mode consistency

### Spacing & Layout
- [ ] Padding and margins are consistent
- [ ] White space breathes (not cramped)
- [ ] Component density matches use case
- [ ] Grid/flexbox structure is clean

### Motion & Interaction
- [ ] Animations feel responsive (not slow)
- [ ] Micro-interactions delight without annoying
- [ ] Transitions are smooth (30-300ms typically)
- [ ] Disabled/loading states are clear

### Accessibility
- [ ] Sufficient color contrast
- [ ] Touch targets ≥ 44x44px
- [ ] Keyboard navigation works
- [ ] Focus indicators visible

### Context-Specific Polish
- [ ] Design matches project aesthetic
- [ ] Matches Soc Ops playful tone (if appropriate)
- [ ] Uses project CSS utilities efficiently
- [ ] Avoids "AI slop" patterns

## Output

The review will provide:

1. **Summary**: Overall design assessment
2. **Strengths**: What's working well
3. **Opportunities**: Areas to improve
4. **Suggestions**: Specific changes (code examples if needed)
5. **Resources**: Links to design guidance, examples in codebase

## Follow-Up Actions

After review, typically:
- Implement visual changes suggested
- Request second review if major changes
- Update project CSS utilities if new patterns emerge
- Document design decisions in component comments

## Related Resources

- **Design Skill**: `.github/instructions/frontend-design.instructions.md` — Creative vision guidance
- **CSS Utilities**: `.github/instructions/css-utilities.instructions.md` — Available classes
- **Component Patterns**: `.github/instructions/blazor-components.instructions.md`
- **Example Components**: Examine `SocOps/Components/` for design patterns already in use
