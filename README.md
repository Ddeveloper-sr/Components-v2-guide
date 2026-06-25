# Discord Components V2 Complete Guide & Tutorial

> **A comprehensive, beginner-to-advanced tutorial for Discord's Components V2 system.**  
> Build beautiful, interactive, embed-free messages using the new layout and content components.  
> Includes discord.js examples, raw JSON references, and best practices.

---

## What is Components V2?

**Components V2** (also called Display Components or the new message components system) is Discord's modern way to build rich, structured messages **entirely with components**.

Instead of relying on the legacy `content` + `embeds` model, you now compose messages using:

- **Layout components** (Container, Section, Separator, Action Row)
- **Content components** (Text Display, Media Gallery, File, Thumbnail)
- **Interactive components** (Buttons, Select Menus) placed inside layouts

### Why Switch to V2?

| Feature                  | Legacy (Embeds + Content)      | Components V2                          | Winner      |
|--------------------------|--------------------------------|----------------------------------------|-------------|
| Layout control           | Limited (embed fields)         | Full control with nesting & containers | V2          |
| Visual consistency       | Embeds look dated              | Modern cards, galleries, dividers      | V2          |
| Accessibility            | Varies                         | Better structured & labeled            | V2          |
| File / Media handling    | Auto-attached                  | Explicit via `File` / `MediaGallery`   | V2          |
| Character limits         | Strict per embed               | More flexible (up to ~4000 total text) | V2          |
| Future-proof             | Legacy support only            | Actively developed by Discord          | V2          |

**Key Rule**: When you send a message with the `IS_COMPONENTS_V2` flag, **you cannot use `content` or `embeds`** at the same time.

---

## Enabling Components V2

You **must** include the flag on every message that uses V2 components.

### discord.js (Recommended)

```js
const { MessageFlags } = require('discord.js');

await channel.send({
  components: [ /* your V2 components here */ ],
  flags: MessageFlags.IsComponentsV2,
});
```

### Raw API / Other Libraries

```json
{
  "flags": 32768,           // 1 << 15 = IS_COMPONENTS_V2
  "components": [ ... ]
}
```

> **Note**: Some libraries (like discord.js) automatically add the flag when you use `withComponents` or certain builders in newer versions. Always verify.

---

## Important Limits & Rules

- **Maximum 40 components** per message (counting nested ones)
- **Total text across all `TextDisplay`** components should stay under ~4000 characters for best compatibility
- Attachments **must** be referenced using `attachment://filename` in `File`, `MediaGalleryItem`, or `Thumbnail`
- Polls and stickers are disabled on V2 messages
- You can still edit legacy messages, but new messages should prefer V2
- `id` field (optional 32-bit integer) can be used for identifying components programmatically (different from `custom_id`)

---

## Component Overview

### Layout Components
- [Container](https://github.com/Ddeveloper-sr/Components-v2-guide/blob/main/components%2Fcontainer%2FContainer.md) — The main "card" wrapper with optional accent color
- [Section](./components/section/README.md) — Text + accessory (button or thumbnail) side-by-side
- [Separator](./components/separator/README.md) — Visual divider or spacing
- [Action Row](./components/action-row/README.md) — Holds interactive components (buttons/selects)

### Content Components
- [Text Display](./components/text-display/README.md) — Markdown text (replaces message `content`)
- [Media Gallery](./components/media-gallery/README.md) — Grid of 1–10 images/videos
- [File](./components/file/README.md) — Explicitly display an attached file
- [Thumbnail](./components/thumbnail/README.md) — Small image used as accessory in Sections

### Interactive Components (used inside layouts)
- Buttons, String Select, User Select, Role Select, Channel Select, Mentionable Select
- See [Action Row](./components/action-row/README.md) for placement rules

### Modals (still use some V2 patterns)
- [Modals with Label + Components](./modals/README.md)

---

## Getting Started

Jump into the [Getting Started guide](./getting-started/README.md) for your first V2 message in under 5 minutes.

---

## Full Examples

Ready-to-run examples:

- [Basic Message](./examples/basic-message/README.md)
- [Rich User Profile Card](./examples/rich-profile/README.md)
- [Media Showcase with Gallery + Files](./examples/media-showcase/README.md)

---

## Migration from Legacy Embeds

See the dedicated [Migration Guide](./migration-from-embeds/README.md) — convert your old embed-heavy commands to beautiful V2 layouts.

---

## Best Practices & Pro Tips

Read [Best Practices](./best-practices/README.md) for performance, accessibility, theming, and common pitfalls.

---

## Resources & Official Docs

- Official Discord Reference: https://docs.discord.com/developers/components/reference
- discord.js Guide: https://discordjs.guide/
- This guide's source: Feel free to contribute improvements!

---

**Made with ❤️ for the Discord developer community.**  
Questions? Open an issue or check the examples folder.

---

*Last updated: June 2026 | Compatible with discord.js v14.18+ and Discord API v10+*
