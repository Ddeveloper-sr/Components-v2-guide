# File Component

**Type**: `13`  
**Builder**: `FileBuilder`

The **File** component explicitly displays an uploaded attachment inside your V2 message.

In Components V2, attachments do **not** appear automatically at the bottom of the message. You must include a `File` component for each file you want visible.

## Why This Matters

- Gives you full control over where files appear in the layout
- Allows mixing files with text, galleries, sections, etc.
- Supports spoiler mode per file

## Basic Usage

```js
const { FileBuilder, AttachmentBuilder, MessageFlags } = require('discord.js');

const pdf = new AttachmentBuilder('./docs/manual.pdf', { name: 'manual.pdf' });

const fileComp = new FileBuilder()
  .setURL('attachment://manual.pdf')
  .setSpoiler(false);

await channel.send({
  components: [fileComp],
  files: [pdf],
  flags: MessageFlags.IsComponentsV2,
});
```

## Full Example — Documentation Package

```js
const {
  ContainerBuilder,
  TextDisplayBuilder,
  FileBuilder,
  AttachmentBuilder,
  MessageFlags,
} = require('discord.js');

const guide = new AttachmentBuilder('./assets/user-guide.pdf');
const changelog = new AttachmentBuilder('./assets/changelog.md');

const docCard = new ContainerBuilder()
  .setAccentColor(0x5865F2)
  .addTextDisplayComponents((t) =>
    t.setContent('## 📚 Documentation Bundle\nDownload the latest files below:')
  )
  .addFileComponents((f) => f.setURL('attachment://user-guide.pdf'))
  .addFileComponents((f) => f.setURL('attachment://changelog.md'))
  .addTextDisplayComponents((t) =>
    t.setContent('-# Both files are updated automatically on release')
  );

await channel.send({
  components: [docCard],
  files: [guide, changelog],
  flags: MessageFlags.IsComponentsV2,
});
```

## Raw JSON

```json
{
  "type": 13,
  "file": {
    "url": "attachment://report.pdf"
  },
  "spoiler": false
}
```

## Properties

| Property  | Description                     |
|-----------|---------------------------------|
| `url`     | Must use `attachment://filename` |
| `spoiler` | Blur the file preview           |
| `name`    | Optional override (usually ignored) |
| `size`    | Optional (API ignores it)       |

## Best Practices

- Always attach the file **and** reference it in a `File` component
- Use inside `Container` for grouped downloads
- Add descriptive text above the `File` component
- For images, prefer `Media Gallery` or `Thumbnail` — `File` is better for PDFs, zips, logs, etc.
- You can have multiple `File` components in one message

## Common Use Cases

- Release notes + downloadable PDF
- Log files for debugging
- Legal documents / terms
- Custom emoji packs or asset bundles

## Important Warning

If you attach files but **do not** include `File` components, the files will be uploaded but **invisible** to users in a V2 message.

---

**See also**:
- [Media Gallery](./media-gallery/README.md) — for images
- [Container](./container/README.md) — best place to put File components
