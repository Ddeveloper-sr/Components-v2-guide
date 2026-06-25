# Modals in the Components V2 Era

While most of the new layout components are for **messages**, Discord also updated how **modals** work.

## Key Changes

- `TextInput` components should now be wrapped in a `Label` component (type 18)
- `ActionRow` + `TextInput` is deprecated for modals (still works but not recommended)
- `Label` provides better accessibility with a visible label + optional description

## Basic Modal with Label + Text Input

```js
const {
  ModalBuilder,
  TextInputBuilder,
  TextInputStyle,
  LabelBuilder,
} = require('discord.js');

const modal = new ModalBuilder()
  .setCustomId('feedback_modal')
  .setTitle('Send Feedback');

const label = new LabelBuilder()
  .setLabel('Your Feedback')
  .setDescription('Please be as detailed as possible')
  .setComponent(
    new TextInputBuilder()
      .setCustomId('feedback_input')
      .setStyle(TextInputStyle.Paragraph)
      .setPlaceholder('What did you like or dislike?')
      .setMaxLength(2000)
  );

modal.addLabelComponents(label);

await interaction.showModal(modal);
```

## Other Components Supported in Labels (Modals)

You can now put these inside `Label`:

- `TextInput`
- `StringSelectMenu`
- `UserSelectMenu`
- `RoleSelectMenu`
- `MentionableSelectMenu`
- `ChannelSelectMenu`
- `FileUpload` (new!)
- Checkbox / Radio groups (newer additions)

## Example: Select Menu in a Modal

```js
const label = new LabelBuilder()
  .setLabel('Preferred Contact Method')
  .setComponent(
    new StringSelectMenuBuilder()
      .setCustomId('contact_method')
      .setPlaceholder('How should we reach you?')
      .addOptions([
        { label: 'Email', value: 'email' },
        { label: 'Discord DM', value: 'dm' },
        { label: 'No preference', value: 'any' },
      ])
  );
```

## Why Use Label?

- Better visual hierarchy
- Supports description text under the label
- Future-proofs your modals for new component types
- Improved screen reader support

## Raw Structure (Simplified)

```json
{
  "type": 18,           // Label
  "label": "Your Name",
  "description": "Optional helper text",
  "component": {
    "type": 4,          // Text Input
    "custom_id": "name",
    "style": 1
  }
}
```

## Best Practices

- Always use `LabelBuilder` for new modals
- Keep labels short (max 45 characters)
- Use descriptions (max 100 chars) for extra guidance
- For complex forms, use multiple Labels inside one modal

---

**Note**: Message components (V2 layouts) and modals are separate systems. You cannot put a `Container` inside a modal.

**See also**: Official Discord modal documentation and the new Label component reference.
