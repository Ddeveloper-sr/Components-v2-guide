# Migration Guide: From Legacy Embeds to Components V2

Many bots still use the classic `EmbedBuilder`. This guide shows how to modernize them.

## Quick Comparison

| Legacy Embed Feature       | Components V2 Equivalent                     | Benefit |
|----------------------------|----------------------------------------------|--------|
| `setTitle()` + `setDescription()` | `TextDisplay` with `#` / `##` + body text   | More flexible markdown |
| `setColor()`               | `Container.setAccentColor()`                 | Cleaner look |
| `setThumbnail()`           | `Section` + `Thumbnail` accessory            | Better layout control |
| `addFields()`              | Multiple `TextDisplay` or `Section`          | Unlimited + better formatting |
| `setImage()`               | `Media Gallery` (1 item) or `File`           | Grid support + positioning |
| `setFooter()` + timestamp  | `TextDisplay` with `-#` at the bottom        | Consistent styling |
| Multiple embeds            | Multiple `Container` components              | Much more powerful grouping |

## Before (Legacy Embed)

```js
const embed = new EmbedBuilder()
  .setTitle('Server Stats')
  .setDescription('Current status of the server')
  .setColor(0x5865F2)
  .setThumbnail(user.displayAvatarURL())
  .addFields(
    { name: 'Members', value: '12,847', inline: true },
    { name: 'Online', value: '3,291', inline: true }
  )
  .setFooter({ text: 'Updated just now' })
  .setTimestamp();

await channel.send({ embeds: [embed] });
```

## After (Components V2) — Recommended

```js
const {
  ContainerBuilder,
  TextDisplayBuilder,
  SeparatorBuilder,
  SectionBuilder,
  MessageFlags,
} = require('discord.js');

const stats = new ContainerBuilder()
  .setAccentColor(0x5865F2)
  .addTextDisplayComponents((t) => t.setContent('# Server Stats'))
  .addTextDisplayComponents((t) => t.setContent('Current status of the server'))
  .addSeparatorComponents((s) => s.setDivider(true))
  .addSectionComponents((section) =>
    section
      .addTextDisplayComponents(
        (t) => t.setContent('**👥 Members**: 12,847'),
        (t) => t.setContent('**🟢 Online**: 3,291')
      )
      .setThumbnailAccessory((thumb) =>
        thumb.setURL(user.displayAvatarURL({ extension: 'png', size: 128 }))
      )
  )
  .addSeparatorComponents((s) => s)
  .addTextDisplayComponents((t) =>
    t.setContent('-# Updated just now • <t:' + Math.floor(Date.now() / 1000) + ':R>')
  );

await channel.send({
  components: [stats],
  flags: MessageFlags.IsComponentsV2,
});
```

## Migration Checklist

1. Replace every `EmbedBuilder` with one or more `ContainerBuilder`
2. Convert titles/descriptions → `TextDisplay` with markdown headings
3. Move fields into `TextDisplay` or `Section` (much more readable)
4. Convert `setThumbnail` → `Section.setThumbnailAccessory`
5. Convert `setImage` → `MediaGallery` (even for single images)
6. Convert footer/timestamp → bottom `TextDisplay` with `-#`
7. Add `flags: MessageFlags.IsComponentsV2`
8. Remove `embeds: [...]` from the send call
9. Test thoroughly — V2 messages cannot be mixed with legacy content/embeds

## When to Keep Legacy Embeds

- You need to support very old Discord clients (rare)
- Quick & dirty prototyping
- Messages that must work without the V2 flag for some reason

For almost all new development and major updates, **Components V2 is strongly recommended**.

---

**Pro Tip**: You can gradually migrate command-by-command. Start with your most important `/info` or announcement commands.
