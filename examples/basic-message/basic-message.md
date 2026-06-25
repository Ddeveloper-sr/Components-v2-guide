# Example: Basic Welcome Message

A simple, clean welcome message using Components V2 — perfect as a server join message or `/welcome` command response.

## Full Code (discord.js)

```js
const {
  ContainerBuilder,
  TextDisplayBuilder,
  SeparatorBuilder,
  SectionBuilder,
  ButtonStyle,
  MessageFlags,
} = require('discord.js');

async function sendWelcomeMessage(channel, member) {
  const welcome = new ContainerBuilder()
    .setAccentColor(0x57F287) // Green
    .addTextDisplayComponents((t) =>
      t.setContent(`# 👋 Welcome to the Server, ${member.user.username}!`)
    )
    .addSeparatorComponents((s) => s.setDivider(true).setSpacing(2))
    .addTextDisplayComponents((t) =>
      t.setContent(
        'We\'re excited to have you here.\n\n' +
        'Please read the <#rules> channel and introduce yourself in <#introductions>!'
      )
    )
    .addSeparatorComponents((s) => s)
    .addSectionComponents((section) =>
      section
        .addTextDisplayComponents(
          (t) => t.setContent('**Need help?**'),
          (t) => t.setContent('Feel free to ping any online moderator.')
        )
        .setButtonAccessory((btn) =>
          btn
            .setCustomId('get_help')
            .setLabel('Get Help')
            .setStyle(ButtonStyle.Primary)
            .setEmoji('🆘')
        )
    )
    .addTextDisplayComponents((t) =>
      t.setContent('-# This message was sent automatically • <t:' + Math.floor(Date.now()/1000) + ':R>')
    );

  await channel.send({
    components: [welcome],
    flags: MessageFlags.IsComponentsV2,
  });
}
```

## What This Demonstrates

- `Container` with accent color
- Multiple `TextDisplay` blocks
- `Separator` with divider + spacing
- `Section` with text + primary button accessory
- Subtle footer with timestamp

## Result Preview (Conceptual)

A clean green-accented card with welcome header, rules reminder, help section with button, and timestamp.

---

**Tip**: Replace the hardcoded channel mentions with actual IDs or use dynamic values from your server config.
