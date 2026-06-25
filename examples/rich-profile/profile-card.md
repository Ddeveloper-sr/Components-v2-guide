# Example: Rich User Profile Card

A beautiful, self-contained user profile using `Container` + `Section` + `Thumbnail`.

## Full Code

```js
const {
  ContainerBuilder,
  TextDisplayBuilder,
  SeparatorBuilder,
  SectionBuilder,
  ButtonStyle,
  AttachmentBuilder,
  MessageFlags,
} = require('discord.js');

async function sendProfileCard(channel, user) {
  // You would normally fetch real user data / avatar here
  const avatar = new AttachmentBuilder('https://i.pravatar.cc/128?img=47', { name: 'avatar.png' });

  const profile = new ContainerBuilder()
    .setAccentColor(0x5865F2)
    .addTextDisplayComponents((t) =>
      t.setContent('## 👤 User Profile')
    )
    .addSeparatorComponents((s) => s.setDivider(true))
    .addSectionComponents((section) =>
      section
        .addTextDisplayComponents(
          (t) => t.setContent(`**${user.username}**`),
          (t) => t.setContent('Level 42 • 128,450 XP'),
          (t) => t.setContent('📍 Online • Last seen just now')
        )
        .setThumbnailAccessory((thumb) =>
          thumb
            .setURL('attachment://avatar.png')
            .setDescription(`${user.username}'s avatar`)
        )
    )
    .addSeparatorComponents((s) => s)
    .addTextDisplayComponents((t) =>
      t.setContent('**Bio**\nPassionate developer and Discord enthusiast. Always happy to help!')
    )
    .addSeparatorComponents((s) => s.setDivider(false).setSpacing(1))
    .addSectionComponents((section) =>
      section
        .addTextDisplayComponents(
          (t) => t.setContent('**Badges** • 🏆 Early Supporter • 💎 Nitro Booster')
        )
        .setButtonAccessory((btn) =>
          btn
            .setCustomId(`view_full_profile_${user.id}`)
            .setLabel('View Full Profile')
            .setStyle(ButtonStyle.Secondary)
        )
    );

  await channel.send({
    components: [profile],
    files: [avatar],
    flags: MessageFlags.IsComponentsV2,
  });
}
```

## Highlights

- Clean header + separator
- `Section` with thumbnail avatar on the right
- Bio section
- Badges + action button
- Professional color scheme

## Extension Ideas

- Add more `Section` rows for "Recent Activity", "Mutual Servers", etc.
- Make the button open a modal or another V2 message with more details
- Fetch real avatar with `user.displayAvatarURL({ extension: 'png', size: 128 })`

Perfect for `/profile` slash commands or user info buttons.
