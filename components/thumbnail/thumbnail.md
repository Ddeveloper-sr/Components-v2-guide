# Thumbnail Component

**Type**: `11`  
**Builder**: `ThumbnailBuilder`

**Thumbnail** is a small image used **exclusively as an accessory** inside a `Section` component.

It is the V2 equivalent of the classic embed thumbnail — perfect for avatars, icons, small previews next to text.

## Usage (Inside Section)

```js
const { SectionBuilder, ThumbnailBuilder, AttachmentBuilder, MessageFlags } = require('discord.js');

const avatar = new AttachmentBuilder('./assets/user-avatar.png');

const profileSection = new SectionBuilder()
  .addTextDisplayComponents(
    (t) => t.setContent('**@GrokTheBuilder**'),
    (t) => t.setContent('xAI • Building the future of AI'),
    (t) => t.setContent('📍 San Francisco Bay Area')
  )
  .setThumbnailAccessory((thumb) =>
    thumb
      .setURL('attachment://user-avatar.png')
      .setDescription('Profile picture of Grok')
      .setSpoiler(false)
  );

await channel.send({
  components: [profileSection],
  files: [avatar],
  flags: MessageFlags.IsComponentsV2,
});
```

## Raw JSON (as accessory)

```json
{
  "type": 9,
  "components": [ { "type": 10, "content": "..." } ],
  "accessory": {
    "type": 11,
    "media": { "url": "attachment://thumb.png" },
    "description": "Thumbnail description",
    "spoiler": false
  }
}
```

## Properties

| Property      | Description                          | Max Length |
|---------------|--------------------------------------|------------|
| `url`         | `attachment://` or external URL      | -          |
| `description` | Alt text / hover tooltip             | 1024 chars |
| `spoiler`     | Blur the image                       | -          |

## Best Practices

- Keep thumbnails **small and square** (Discord handles cropping)
- Always provide `description` for accessibility
- Use for **avatars, logos, small product shots**
- For larger images or grids → use `Media Gallery` instead
- You can use external URLs (good for CDNs)

## Thumbnail vs Media Gallery

| Use Case                    | Recommended Component     |
|-----------------------------|---------------------------|
| Single small image + text   | `Thumbnail` in `Section`  |
| Multiple images in grid     | `Media Gallery`           |
| Large hero image            | `Media Gallery` (1 item) or external in Text |
| File downloads (PDF, zip)   | `File`                    |

---

**See also**:
- [Section](./section/README.md) — required parent for Thumbnail
- [Media Gallery](./media-gallery/README.md)
