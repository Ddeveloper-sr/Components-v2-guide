# Example: Media Showcase with Gallery + Files

Demonstrates `Media Gallery`, `File`, and `Container` together for a release announcement with screenshots and downloads.

## Full Code

```js
const {
  ContainerBuilder,
  TextDisplayBuilder,
  SeparatorBuilder,
  MediaGalleryBuilder,
  FileBuilder,
  AttachmentBuilder,
  MessageFlags,
} = require('discord.js');

async function sendReleaseAnnouncement(channel) {
  const screenshot1 = new AttachmentBuilder('./assets/v2-screenshot-1.png');
  const screenshot2 = new AttachmentBuilder('./assets/v2-screenshot-2.png');
  const changelogPdf = new AttachmentBuilder('./assets/changelog.pdf');

  const release = new ContainerBuilder()
    .setAccentColor(0xED4245) // Red accent for excitement
    .addTextDisplayComponents((t) =>
      t.setContent('# 🎉 Components V2 is Now Live!')
    )
    .addTextDisplayComponents((t) =>
      t.setContent('The biggest update to Discord messaging since embeds. Build richer, more beautiful messages than ever before.')
    )
    .addSeparatorComponents((s) => s.setDivider(true).setSpacing(2))
    // Media Gallery
    .addMediaGalleryComponents((gallery) =>
      gallery.addItems(
        (item) => item.setURL('attachment://v2-screenshot-1.png').setDescription('New Container + Section layout'),
        (item) => item.setURL('attachment://v2-screenshot-2.png').setDescription('Media Gallery in action')
      )
    )
    .addSeparatorComponents((s) => s)
    // Download section
    .addTextDisplayComponents((t) =>
      t.setContent('## 📥 Download the Full Changelog')
    )
    .addFileComponents((f) =>
      f.setURL('attachment://changelog.pdf')
    )
    .addTextDisplayComponents((t) =>
      t.setContent('-# Released today • Compatible with discord.js v14.18+')
    );

  await channel.send({
    components: [release],
    files: [screenshot1, screenshot2, changelogPdf],
    flags: MessageFlags.IsComponentsV2,
  });
}
```

## What Makes This Great

- Eye-catching header
- Visual proof with actual screenshots in a gallery
- Downloadable asset using `File`
- Clean separators
- Professional release-style formatting

## Tips for Production

- Host screenshots on a CDN and use external URLs for faster loads when possible
- Generate PDFs dynamically or host them
- Add interactive buttons (e.g., "Join Beta", "Read Docs") using `Section` accessories

This pattern works amazingly well for update posts, portfolio showcases, and product launches.
