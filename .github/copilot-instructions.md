# SocOps — Copilot Workspace Instructions

## Pre-commit Checklist

Before committing any changes, you MUST run and pass all of the following:

```bash
dotnet build SocOps/SocOps.csproj   # 0 errors required
dotnet test                          # all tests must pass (when tests exist)
```

No linter is configured — enforce C# conventions manually (see below).

## Project

**SocOps** is a SOC-themed Social Bingo game — Blazor WebAssembly (.NET 10). Players find colleagues matching questions to mark squares in a 5×5 grid.

```bash
dotnet run --project SocOps/SocOps.csproj  # dev server → http://localhost:5166
```

## Architecture

| Path | Role |
|---|---|
| `Components/` | Blazor UI — `BingoBoard`, `BingoSquare`, `BingoModal`, `GameScreen`, `StartScreen` |
| `Services/BingoGameService.cs` | Single source of truth; persists to localStorage via JSInterop |
| `Services/BingoLogicService.cs` | Pure static logic — board generation, win detection |
| `Models/` | Data only — `BingoSquareData`, `GameState` (enum), `BingoLine` |
| `Data/Questions.cs` | SOC question pool + `FREE_SPACE` constant |
| `wwwroot/css/app.css` | Custom Tailwind-like utility classes |

**Key rules:** Center square (index 12) is always the free space. Components subscribe to `BingoGameService.OnStateChanged` and call `StateHasChanged`. Fire-and-forget saves: `_ = SaveGameStateAsync()`.

## Conventions

- **C#**: PascalCase public, `_camelCase` private; nullable enabled — no `!` suppression.
- **Styling**: Custom CSS utilities only — do NOT add Tailwind or other frameworks. See `.github/instructions/css-utilities.instructions.md`.
- **UI design**: Follow `.github/instructions/frontend-design.instructions.md` for new components.
- **Workshop**: Lab guides in `workshop/`; reference solutions in `.solutions/step-XX-*/`.
