# Section Component

**Type**: `9`  
**Builder**: `SectionBuilder`

A **Section** lets you display up to **3 lines of text** alongside an **accessory** — either a **Button** or a **Thumbnail**.

This is perfect for feature lists, profile snippets, call-to-action blocks, or media + description cards.

## Structure

```
┌─────────────────────────────────────────────┐
│  Text line 1                                │  ← Accessory
│  Text line 2                                │     (Button or
│  Text line 3                                │      Thumbnail)
└─────────────────────────────────────────────┘
```

## Basic Usage with Button Accessory

```js
const { SectionBuilder, ButtonStyle, MessageFlags } = require('discord.js');

const section = new SectionBuilder()
  .addTextDisplayComponents(
    (t) => t.setContent('**🚀 New Feature Released**'),
    (t) => t.setContent('Check out the latest update in our changelog.'),
    (t) => t.setContent('-# Available to all servers')
  )
  .setButtonAccessory((button) =>
    button
      .setCustomId('view_changelog')
      .setLabel('View Changelog')
      .setStyle(ButtonStyle.Primary)
      .setEmoji('📜')
  );

await channel.send({
  components: [section],
  flags: MessageFlags.IsComponentsV2,
});
```

## Usage with Thumbnail Accessory

```js
const { SectionBuilder, AttachmentBuilder, MessageFlags } = require('discord.js');

const file = new AttachmentBuilder('./assets/avatar.png');

const section = new SectionBuilder()
  .addTextDisplayComponents(
    (t) => t.setContent('**@CoolUser**'),
    (t) => t.setContent('Level 87 • 12,450 XP'),
    (t) => t.setContent('Top contributor this month!')
  )
  .setThumbnailAccessory((thumb) =>
    thumb
      .setURL('attachment://avatar.png')
      .setDescription('User avatar')
      .setSpoiler(false)
  );

await channel.send({
  components: [section],
  files: [file],
  flags: MessageFlags.IsComponentsV2,
});
```

## Raw JSON Example (with Button)

```json
{
  "type": 9,
  "components": [
    { "type": 10, "content": "**Title**" },
    { "type": 10, "content": "Description here" }
  ],
  "accessory": {
    "type": 2,
    "style": 1,
    "label": "Click Me",
    "custom_id": "btn_123"
  }
}
```

## Limits & Rules

- Maximum **3** `TextDisplay` components inside a Section
- Accessory can be **Button** (most common) or **Thumbnail**
- Cannot put Action Row or other complex layouts inside Section

## Best Practices

- Use for **"hero"** style content: important info + primary action
- Keep text concise — 1-2 lines ideal
- Use emoji in the first line for visual pop
- Pair multiple Sections inside a `Container` for beautiful lists

## Common Use Cases

- Feature highlights with "Learn More" button
- User / server profile summaries
- Product / item cards with thumbnail + buy button
- Status updates with action

---

**See also**:
- [Thumbnail](./thumbnail/README.md)
- [Container](./container/README.md) — wrap multiple Sections
- [Button in Action Row](./action-row/README.md)
