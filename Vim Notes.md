## Working with Code Blocks

One of Vim's biggest strengths is editing *text objects*. Rather than selecting text with the mouse, you tell Vim what kind of object you want to operate on.

### Text Objects

These commands only work when the cursor is **inside** the object.

### Curly Braces

| Command | Description |
|---------|-------------|
| `vi{` | Select inside `{}` |
| `va{` | Select around `{}` (includes braces) |
| `ci{` | Change inside `{}` |
| `ca{` | Change around `{}` (includes braces) |
| `di{` | Delete inside `{}` |
| `da{` | Delete around `{}` |
| `yi{` | Yank inside `{}` |

### Other Useful Text Objects

| Command | Description |
|---------|-------------|
| `ci(` | Change inside parentheses |
| `ci[` | Change inside square brackets |
| `ci"` | Change inside double quotes |
| `ci'` | Change inside single quotes |
| `ciw` | Change the current word |

### Matching Delimiters

| Command | Description |
|---------|-------------|
| `%` | Jump to the matching `{}`, `()`, or `[]` |

This is useful when you're outside a block or need to quickly move between opening and closing delimiters.

---

## Navigating Nested Blocks

Vim also has motions for navigating nested code blocks.

| Command | Description |
|---------|-------------|
| `[{` | Jump to the beginning of the current/enclosing `{}` block |
| `]}` | Jump to the end of the current/enclosing `{}` block |
| `]{` | Jump to the next opening `{` |
| `[}` | Jump to the previous closing `}` |

Repeated `[{` climbs outward through nested blocks.

Example:

```php
public function lookup()
{
    if ($foo) {
        while ($bar) {
            return [];
            // Cursor here
        }
    }
}
```

Pressing `[{` repeatedly moves the cursor to:

1. `while`
2. `if`
3. `function`

---

## Rule of Thumb

- If you're **inside** a block, use a **text object** (`ci{`, `vi{`, etc.).
- If you're **outside** the block, use a **motion** (`%`, `[{`, etc.) to get there first.

### My Current Workflow

When replacing an entire block of code:

1. Search for the block (`/`)
2. Navigate with `n` / `N`
3. Use `%` to jump between matching braces if needed
4. Once inside the correct block, use `ci{` to replace the contents

As I become more comfortable with Vim, the goal is to rely less on searching and more on text objects and motions.