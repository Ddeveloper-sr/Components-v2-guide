# Container Component

**Type**: `17`  
**Builder**: `ContainerBuilder`

The **Container** is the star of Components V2. It acts as a visual "card" that groups related components together with an optional colored accent bar on the left.

Think of it as a modern, flexible replacement for the classic Discord embed.

## Why Containers Are Amazing

- Optional **accent color** (like embed color but cleaner)
- Supports **spoiler** mode
- Can contain almost any other V2 component (Text, Section, Media Gallery, Separator, Action Row, File, even nested Containers in some cases)
- Creates beautiful grouped layouts

## Basic Container

```js
const { ContainerBuilder, TextDisplayBuilder, MessageFlags } = require('discord.js');

const card = new ContainerBuilder()
  .setAccentColor(0x5865F2) // Discord Blurple
  .addTextDisplayComponents((t) =>
    t.setContent('# 📌 Pinned Announcement\n\nThis message uses the new V2 system.')
  );

await channel.send({
  components: [card],
  flags: MessageFlags.IsComponentsV2,
});
```

## Full Featured Container Example

```js
const {
  ContainerBuilder,
  TextDisplayBuilder,
  SeparatorBuilder,
  SectionBuilder,
  ButtonStyle,
  MessageFlags,
} = require('discord.js');

const dashboard = new ContainerBuilder()
  .setAccentColor(0xFEE75C) // Yellow
  .setSpoiler(false)
  // Header
  .addTextDisplayComponents((t) =>
    t.setContent('## 📊 Server Dashboard\n*Last updated just now*')
  )
  // Divider
  .addSeparatorComponents((s) => s.setDivider(true).setSpacing(2))
  // Stats Section
  .addSectionComponents((sec) =>
    sec
      .addTextDisplayComponents(
        (t) => t.setContent('**👥 Members**: 12,847'),
        (t) => t.setContent('**💬 Messages Today**: 3,291'),
        (t) => t.setContent('**🎮 Active Games**: 47')
      )
      .setButtonAccessory((b) =>
        b.setCustomId('refresh_stats').setLabel('Refresh').setStyle(ButtonStyle.Secondary)
      )
  )
  // Another separator
  .addSeparatorComponents((s) => s)
  // Footer text
  .addTextDisplayComponents((t) =>
    t.setContent('-# Data provided by internal API • Auto-refreshes every 60s')
  );

await channel.send({
  components: [dashboard],
  flags: MessageFlags.IsComponentsV2,
});
```

## Raw JSON

```json
{
  "type": 17,
  "accent_color": 5865F2,
  "spoiler": false,
  "components": [
    { "type": 10, "content": "..." },
    { "type": 14, "divider": true },
    { "type": 9, "components": [...], "accessory": {...} }
  ]
}
```

## Properties

| Property       | Description                              | Example          |
|----------------|------------------------------------------|------------------|
| `accent_color` | Left border color (0x000000 – 0xFFFFFF) | `0x57F287`       |
| `spoiler`      | Blur the entire container                | `true` / `false` |
| `components`   | Array of child components                | Text, Section, etc. |

## Nesting & Composition

You can put almost anything inside a Container:

- Multiple `TextDisplay`
- `Separator`
- `Section` (with buttons or thumbnails)
- `Media Gallery`
- `File`
- `Action Row` (for buttons/selects at the bottom)

**Note**: Deeply nested Containers are possible but keep it reasonable for readability.

## Best Practices

- Use **one primary accent color** per container for visual consistency
- Start with a header `TextDisplay` using `#` or `##`
- End with subtle `-#` footer text
- Use `Separator` generously inside for breathing room
- For multiple cards, send multiple top-level Containers

## Common Patterns

1. **Dashboard / Stats Card** (as above)
2. **Feature Announcement Card**
3. **Media + Description Card** (Container > Media Gallery + Text)
4. **Form-like Layout** (Container > Sections with buttons)

---

**Pro Tip**: Combine `Container` + `Section` + `Separator` to recreate almost any embed layout — but much more beautifully and flexibly.

**See also**: [Section](./section/README.md), [Separator](./separator/README.md), [Media Gallery](./media-gallery/README.md)
