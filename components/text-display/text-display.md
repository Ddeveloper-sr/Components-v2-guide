# Text Display Component

**Type**: `10`  
**Builder**: `TextDisplayBuilder`

The **Text Display** is the fundamental content block in Components V2. It replaces the top-level `content` field of a message.

## When to Use

- Any time you need to show formatted text
- Inside `Container`, `Section`, or as a top-level component
- Supports full Discord markdown (bold, italic, lists, code blocks, mentions, emojis, etc.)

## Basic Usage (discord.js)

```js
const { TextDisplayBuilder, MessageFlags } = require('discord.js');

const welcome = new TextDisplayBuilder()
  .setContent('# Hello World!\n**Bold** *italic* __underline__ ~~strikethrough~~\n`inline code`');

await channel.send({
  components: [welcome],
  flags: MessageFlags.IsComponentsV2,
});
```

## Advanced Example — Multiple Text Displays + Mentions

```js
const header = new TextDisplayBuilder().setContent('## 📢 Announcement');
const body = new TextDisplayBuilder().setContent(
  'Hey <@&123456789012345678>, the new update is live!\n\n' +
  'Check the <#987654321098765432> channel for details.'
);
const footer = new TextDisplayBuilder().setContent('-# Posted by the team • <t:1710000000:R>');

await channel.send({
  components: [header, body, footer],
  flags: MessageFlags.IsComponentsV2,
  allowedMentions: { roles: ['123456789012345678'] }, // control pings
});
```

## Raw JSON Structure

```json
{
  "type": 10,
  "content": "# Heading\nSome **bold** text here."
}
```

## Important Notes

- `content` supports up to the normal message character limit when combined
- Mentions work and respect `allowed_mentions`
- You can have **multiple** `TextDisplay` components in one message (great for structured layouts)
- Inside a `Section`, you can add **up to 3** `TextDisplay` components

## Best Practices

- Use `#`, `##`, `###` for headings instead of large bold text
- Keep individual `TextDisplay` blocks focused (one idea per block)
- Use `-#` for subtle footer/disclaimer text
- Combine with `Separator` for visual breathing room

## Common Pitfalls

- Forgetting the `IsComponentsV2` flag → text won't appear
- Trying to use `content` at the top level together with V2 components → ignored
- Very long single `TextDisplay` → can hit limits; split into multiple

---

**See also**:
- [Container](../container/README.md) — wrap text beautifully
- [Section](../section/README.md) — pair text with buttons/thumbnails
- [Separator](../separator/README.md) — add spacing between text blocks
