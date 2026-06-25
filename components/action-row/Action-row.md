# Action Row & Interactive Components in V2

**Type**: `1` (Action Row)  
**Builders**: `ActionRowBuilder`, plus all the select and button builders

Even in Components V2, **interactive components** (buttons and select menus) still live inside `ActionRow` components.

However, you now have more placement options:
- Top-level `ActionRow` (like before)
- Inside `Container`
- As **accessory** inside `Section` (only Buttons and Thumbnails allowed as accessories)

## Supported Interactive Components

| Component           | Type | Can be in ActionRow | Can be Section Accessory | Notes |
|---------------------|------|---------------------|---------------------------|-------|
| Button              | 2    | Yes                 | Yes                       | Primary action |
| String Select       | 3    | Yes                 | No                        | Dropdown |
| User Select         | 5    | Yes                 | No                        | User picker |
| Role Select         | 6    | Yes                 | No                        | Role picker |
| Mentionable Select  | 7    | Yes                 | No                        | Users + Roles |
| Channel Select      | 8    | Yes                 | No                        | Channel picker |
| Text Input          | 4    | No (modals only)    | No                        | Use `Label` in modals |

## Example: Buttons in Action Row (Top Level)

```js
const { ActionRowBuilder, ButtonBuilder, ButtonStyle, MessageFlags } = require('discord.js');

const row = new ActionRowBuilder().addComponents(
  new ButtonBuilder()
    .setCustomId('primary_action')
    .setLabel('Confirm')
    .setStyle(ButtonStyle.Primary),
  new ButtonBuilder()
    .setCustomId('secondary')
    .setLabel('Cancel')
    .setStyle(ButtonStyle.Secondary)
);

await channel.send({
  components: [row],
  flags: MessageFlags.IsComponentsV2,
});
```

## Example: Button as Section Accessory (Recommended for V2)

See the [Section component guide](./section/README.md) — this is the preferred modern pattern.

## Example: Select Menu inside Container

```js
const {
  ContainerBuilder,
  TextDisplayBuilder,
  ActionRowBuilder,
  StringSelectMenuBuilder,
  MessageFlags,
} = require('discord.js');

const container = new ContainerBuilder()
  .setAccentColor(0x5865F2)
  .addTextDisplayComponents((t) => t.setContent('## Choose your role'))
  .addActionRowComponents((row) =>
    row.addComponents(
      new StringSelectMenuBuilder()
        .setCustomId('role_select')
        .setPlaceholder('Select a role...')
        .addOptions([
          { label: 'Developer', value: 'dev', emoji: '💻' },
          { label: 'Designer', value: 'design', emoji: '🎨' },
          { label: 'Moderator', value: 'mod', emoji: '🛡️' },
        ])
    )
  );

await channel.send({
  components: [container],
  flags: MessageFlags.IsComponentsV2,
});
```

## Raw JSON for Action Row with Button

```json
{
  "type": 1,
  "components": [
    {
      "type": 2,
      "style": 1,
      "label": "Click Me",
      "custom_id": "btn_1"
    }
  ]
}
```

## Best Practices for V2

1. **Prefer Section + Button accessory** for single primary actions (cleaner UI)
2. Use top-level or Container-wrapped `ActionRow` for multiple buttons or selects
3. Keep `ActionRow` near the bottom of a Container when possible
4. You can have multiple `ActionRow` components (max 5 buttons per row, or 1 select)
5. `custom_id` is still required for all interactive components that send interactions

## Limits Reminder

- Max 5 components per Action Row
- Max 40 components total in message

---

**See also**:
- [Section](./section/README.md) — best place for single buttons
- [Container](./container/README.md) — great for grouping selects + text
- Official docs for all select menu types
