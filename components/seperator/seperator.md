# Separator Component

**Type**: `14`  
**Builder**: `SeparatorBuilder`

The **Separator** adds visual spacing or a horizontal divider between other components. It is one of the simplest yet most powerful tools for creating clean, readable layouts.

## Properties

| Property   | Type                  | Default | Description |
|------------|-----------------------|---------|-------------|
| `divider`  | boolean               | `true`  | Whether to show a visible line |
| `spacing`  | `SeparatorSpacingSize` | `1` (Small) | `1` = Small, `2` = Large |

## Basic Usage

```js
const { SeparatorBuilder, SeparatorSpacingSize, MessageFlags } = require('discord.js');

const divider = new SeparatorBuilder()
  .setDivider(true)
  .setSpacing(SeparatorSpacingSize.Large);

await channel.send({
  components: [text1, divider, text2],
  flags: MessageFlags.IsComponentsV2,
});
```

## Spacing Sizes

```js
const small = new SeparatorBuilder().setSpacing(SeparatorSpacingSize.Small);
const large = new SeparatorBuilder().setSpacing(SeparatorSpacingSize.Large);
```

- **Small**: Subtle breathing room
- **Large**: Clear visual break between sections

## Common Patterns

### 1. Section Divider with Line

```js
new SeparatorBuilder()
  .setDivider(true)
  .setSpacing(SeparatorSpacingSize.Large)
```

### 2. Just Spacing (No Line)

```js
new SeparatorBuilder()
  .setDivider(false)
  .setSpacing(SeparatorSpacingSize.Large)
```

### 3. Inside a Container

Separators work great inside `Container` to separate groups of content.

## Raw JSON

```json
{
  "type": 14,
  "divider": true,
  "spacing": 2
}
```

## Best Practices

- Use **Large** spacing after major sections (headers, feature lists)
- Use **Small** spacing for related items (e.g., between two paragraphs)
- Don't overuse visible dividers — too many lines can look cluttered
- Pair with `Container` accent colors for beautiful card-like sections

## When NOT to Use

- At the very start or end of a message (usually unnecessary)
- Between every single line of text (use multiple `TextDisplay` + small separators sparingly)

---

**Pro Tip**: A `Separator` with `divider: false` and `Large` spacing is often cleaner than multiple empty `TextDisplay` components.

**See also**: [Container](./container/README.md), [Section](./section/README.md)
