# Getting Started with Discord Components V2

This guide will get you from zero to sending your first beautiful Components V2 message in **under 5 minutes**.

## Prerequisites

- Node.js 18+
- A Discord bot token with `Message Content` intent (for testing)
- discord.js v14.18 or newer (recommended)

```bash
npm install discord.js
```

## Step 1: Basic Bot Setup

Create `index.js`:

```js
const { Client, GatewayIntentBits, MessageFlags } = require('discord.js');

const client = new Client({
  intents: [
    GatewayIntentBits.Guilds,
    GatewayIntentBits.GuildMessages,
    GatewayIntentBits.MessageContent,
  ],
});

client.once('ready', () => {
  console.log(`✅ Bot ready as ${client.user.tag}`);
});

client.on('messageCreate', async (message) => {
  if (message.content === '!v2demo' && !message.author.bot) {
    // We will add V2 components here in the next steps
  }
});

client.login('YOUR_BOT_TOKEN');
```

## Step 2: Your First Text Display (Replaces `content`)

Add this inside the `messageCreate` handler:

```js
const { TextDisplayBuilder } = require('discord.js');

const text = new TextDisplayBuilder()
  .setContent('Hello from **Components V2**! 🎉\nThis replaces the old `content` field.');

await message.reply({
  components: [text],
  flags: MessageFlags.IsComponentsV2,
});
```

**Result**: A clean message with formatted text. No embed needed!

## Step 3: Add a Separator

```js
const { TextDisplayBuilder, SeparatorBuilder, SeparatorSpacingSize, MessageFlags } = require('discord.js');

const text1 = new TextDisplayBuilder().setContent('**Section 1**');
const separator = new SeparatorBuilder()
  .setDivider(true)
  .setSpacing(SeparatorSpacingSize.Large);
const text2 = new TextDisplayBuilder().setContent('**Section 2** — more space above!');

await message.reply({
  components: [text1, separator, text2],
  flags: MessageFlags.IsComponentsV2,
});
```

## Step 4: Create a Container (The "Card")

```js
const { ContainerBuilder, TextDisplayBuilder, MessageFlags } = require('discord.js');

const container = new ContainerBuilder()
  .setAccentColor(0x5865F2) // Blurple
  .addTextDisplayComponents(
    (t) => t.setContent('# Welcome to V2!\nThis is inside a styled container.')
  );

await message.reply({
  components: [container],
  flags: MessageFlags.IsComponentsV2,
});
```

## Step 5: Full Mini Example — Profile Card

Combine everything:

```js
const {
  ContainerBuilder,
  TextDisplayBuilder,
  SeparatorBuilder,
  SectionBuilder,
  ButtonStyle,
  MessageFlags,
} = require('discord.js');

const profile = new ContainerBuilder()
  .setAccentColor(0x57F287) // Green
  .addTextDisplayComponents((t) =>
    t.setContent('## 👤 User Profile\n**@CoolDev** • Level 42')
  )
  .addSeparatorComponents((s) => s.setDivider(true))
  .addSectionComponents((section) =>
    section
      .addTextDisplayComponents(
        (t) => t.setContent('**Bio:** Passionate about building awesome Discord bots!'),
        (t) => t.setContent('📍 Earth • 🗓️ Joined 2023')
      )
      .setButtonAccessory((btn) =>
        btn
          .setCustomId('view_badges')
          .setLabel('View Badges')
          .setStyle(ButtonStyle.Primary)
      )
  );

await message.reply({
  components: [profile],
  flags: MessageFlags.IsComponentsV2,
});
```

Run your bot, type `!v2demo` in a channel, and enjoy your first modern Discord message!

---

## Next Steps

- Explore each component in detail:
  - [Text Display](../components/text-display/README.md)
  - [Container](../components/container/README.md)
  - [Section](../components/section/README.md)
- See [Full Examples](../examples/)
- Learn [Best Practices](../best-practices/README.md)
- Read the [Migration Guide](../migration-from-embeds/README.md) if you have existing embed code

---

**Pro Tip**: Always test with the `IsComponentsV2` flag. You can temporarily remove it to compare with legacy behavior.

Happy building! 🚀
