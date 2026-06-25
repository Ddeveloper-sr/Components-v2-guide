# Media Gallery Component

**Type**: `12`  
**Builder**: `MediaGalleryBuilder` + `MediaGalleryItemBuilder`

The **Media Gallery** displays a responsive grid of **1 to 10 images** (or videos in some cases). It is the modern replacement for multiple image embeds.

Perfect for:

- Screenshots / art showcases
- Before/after comparisons
- Product images
- Meme collections
- Portfolio grids

## Basic Gallery (2 images)

```js
const { MediaGalleryBuilder, AttachmentBuilder, MessageFlags } = require('discord.js');

const file1 = new AttachmentBuilder('./screenshots/feature1.png');
const file2 = new AttachmentBuilder('./screenshots/feature2.png');

const gallery = new MediaGalleryBuilder()
  .addItems(
    (item) => item
      .setURL('attachment://feature1.png')
      .setDescription('New dashboard UI'),
    (item) => item
      .setURL('attachment://feature2.png')
      .setDescription('Mobile view')
      .setSpoiler(false)
  );

await channel.send({
  components: [gallery],
  files: [file1, file2],
  flags: MessageFlags.IsComponentsV2,
});
```

## Full Example with External URLs + Spoilers

```js
const { MediaGalleryBuilder, MessageFlags } = require('discord.js');

const gallery = new MediaGalleryBuilder()
  .addItems(
    (item) => item.setURL('https://picsum.photos/id/1015/800/600').setDescription('Mountain landscape'),
    (item) => item.setURL('https://picsum.photos/id/1016/800/600').setDescription('Forest path').setSpoiler(true),
    (item) => item.setURL('https://picsum.photos/id/102/800/600').setDescription('City skyline')
  );

await channel.send({
  components: [gallery],
  flags: MessageFlags.IsComponentsV2,
});
```

## Raw JSON

```json
{
  "type": 12,
  "items": [
    {
      "media": { "url": "attachment://image.png" },
      "description": "Alt text here",
      "spoiler": false
    }
  ]
}
```

## Limits

- **Minimum**: 1 item
- **Maximum**: 10 items
- Each item supports `description` (alt text, max 1024 chars)
- `spoiler` per item

## Best Practices

- Always provide meaningful `description` (accessibility + hover text)
- Mix local attachments + external URLs as needed
- Use `spoiler: true` for sensitive/preview images
- For single images, consider using `Section` + `Thumbnail` instead (more compact)
- Combine with `Container` for labeled galleries

## Inside a Container

```js
const container = new ContainerBuilder()
  .setAccentColor(0xED4245)
  .addTextDisplayComponents((t) => t.setContent('## 🖼️ Gallery'))
  .addMediaGalleryComponents((g) =>
    g.addItems(/* ... */)
  );
```

## When to Use Gallery vs Multiple Thumbnails

- **Gallery**: Grid layout, 2+ images, standalone visual focus
- **Thumbnail in Section**: Single small image next to text (profile, feature list)

---

**See also**:
- [File](./file/README.md) — for non-image attachments
- [Thumbnail](./thumbnail/README.md) — single image accessory
- [Container](./container/README.md) — wrap your gallery
