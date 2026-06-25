# Best Practices for Discord Components V2

Follow these guidelines to create professional, accessible, performant, and future-proof messages.

## 1. Structure & Readability

- **Start with a Container** for almost every non-trivial message
- Use `TextDisplay` with proper markdown hierarchy (`#`, `##`, `###`, `-#`)
- Add `Separator` generously — visual breathing room is key
- Limit depth: Avoid deeply nested containers unless necessary
- One primary action per `Section` (use button accessory)

## 2. Performance & Limits

- **Keep total components ≤ 40** (count nested ones)
- **Total text** across all TextDisplays should ideally stay under 3000–3500 characters
- Prefer external image URLs (CDN) over attachments when possible for speed
- Reuse similar layouts across commands for consistency

## 3. Accessibility

- Always provide `description` on `Thumbnail`, `MediaGalleryItem`, and images
- Use clear, descriptive button labels (avoid just emojis)
- Respect `allowed_mentions` — don't spam pings
- Use `-#` for secondary/disclaimer text (visually smaller)

## 4. Theming & Consistency

- Pick **2–3 accent colors** max for your entire bot and stick to them
- Use the same header style across similar commands
- For multi-card messages, give each `Container` a distinct but related accent color
- Dark mode friendly: Avoid very light colors on light backgrounds

## 5. Interactive Components

- **Primary action** → Button as `Section` accessory (cleanest)
- **Multiple choices or filters** → `ActionRow` with Select Menu inside `Container`
- Always give `custom_id` meaningful names (e.g., `profile_view_123456`)
- Handle interactions gracefully and edit the original message when possible

## 6. Files & Media

- **Images** → `Media Gallery` (grids) or `Thumbnail` (single, with text)
- **Documents / PDFs / archives** → `File` component (never forget to reference them!)
- Always attach files **and** include the corresponding `File` / `MediaGalleryItem`

## 7. Common Anti-Patterns to Avoid

| Anti-Pattern                        | Why It's Bad                          | Better Alternative |
|-------------------------------------|---------------------------------------|--------------------|
| Using `content` + V2 components     | Ignored or causes issues              | Pure V2 components |
| No `IsComponentsV2` flag            | Components don't render               | Always include flag |
| 10+ TextDisplays without separators | Looks cramped                         | Use Separators + Containers |
| Forgetting to reference attachments | Files invisible to users              | Always add File/Media component |
| Mixing many different accent colors | Visually chaotic                      | Consistent theming |
| Putting selects in Section accessory| Not supported                         | Use ActionRow in Container |

## 8. Testing Recommendations

- Test on both desktop and mobile Discord clients
- Test with the flag on and off to compare
- Have non-developer friends review the visual result
- Check interaction flows end-to-end

## 9. Future-Proofing

- Prefer `LabelBuilder` in all new modals
- Use the builder classes (they are updated faster than raw JSON)
- Keep an eye on official Discord developer updates — new component types are added regularly
- Document your custom component patterns internally

## Quick Reference: When to Use What

| Goal                              | Recommended Components                     |
|-----------------------------------|--------------------------------------------|
| Simple text message               | 1–3 `TextDisplay` + optional `Separator`   |
| Card with accent + content        | `Container` + `TextDisplay` + `Separator`  |
| Text + action button              | `Section` (text + button accessory)        |
| Profile / feature with image      | `Section` + `Thumbnail`                    |
| Image grid                        | `Media Gallery`                            |
| Downloadable files                | `File` inside `Container`                  |
| Dashboard / stats                 | `Container` + multiple `Section` + `ActionRow` |
| Complex form                      | Modal with `Label` + inputs/selects        |

---

Following these practices will make your bot's messages look **native**, **modern**, and **highly usable** — exactly what Discord users expect in 2026 and beyond.

Happy building! If you have a great pattern to share, consider contributing back to the community.
