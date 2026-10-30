# Calm Habits — Design Spec

Source of truth for the app's look and behaviour. Use these exact values. When something isn't covered here, choose the calmest, simplest option consistent with Apple's Human Interface Guidelines (style only; no Apple logos or trademarks).

**Feel:** calm, unhurried, trustworthy, like a deep breath. Quiet confidence, not excitement. No bright gradients, no heavy shadows, no crowded screens. Mostly neutral screens with **one focal colour per view** (Schill orange).

---

## 1. Colour tokens

### Light
| Token | Hex | Use |
|---|---|---|
| `background` | `#F6F6F2` | App background (soft off-white, olive tint) |
| `surface` | `#FFFFFF` | Cards, list groups, inputs |
| `border` | `#E4E5DC` | 1px card/input borders |
| `divider` | `#EEEFE6` | Row separators inside lists |
| `tint` | `#EEEFE6` | Icon chips, segmented-control track, note cards, missed days |
| `textPrimary` | `#262820` | Main text |
| `textSecondary` | `#66685A` | Captions, meta, section headers, inactive tabs (5.6:1 on white) |
| `iconMuted` | `#A3A592` | Chevrons, unchecked check-circle outline (decorative only) |
| `switchOff` | `#D8D9CE` | Switch track when off |
| `placeholder` | `#8A8C7C` | Input placeholder text |
| `accent` | `#E5762C` | **Schill orange.** Progress ring, completed checks, done calendar days, switch on, selected outlines. Never for small text. |
| `accentStrong` | `#B4541A` | Text links, active tab, primary button background (white text = 4.97:1), focus ring |
| `accentStrongPressed` | `#8F4214` | Link/button hover and pressed |
| `accentSoft` | `#F7DCC6` | Partial-day fills, today's in-progress dot |
| `accentWash` | `#FBEADC` | Selected chip background, active sidebar item |
| `warningBg` / `warningIcon` | `#FBF1DF` / `#8A6420` | Warning banners |
| `errorBg` / `errorText` | `#F6E4E0` / `#A4554A` | Error icons, destructive rows ("Reset all progress") |
| `overlay` | `rgba(38,40,32,0.28)` | Dim behind sheets |
| `frostedBar` | `rgba(246,246,242,0.78)` + blur 20 | Nav bar and tab bar |
| `frostedSheet` | `rgba(250,250,247,0.9)` + blur 30 | Bottom sheets |
| `barBorder` | `rgba(38,40,32,0.08)` | Hairline under/above frosted bars |

### Dark
| Token | Hex |
|---|---|
| `background` | `#141512` |
| `surface` | `#1D1F1A` |
| `border` | `#2C2E27` |
| `divider` | `#262821` |
| `tint` | `#262821` |
| `textPrimary` | `#EDEDE6` |
| `textSecondary` | `#A3A595` |
| `iconMuted` | `#5E6052` |
| `accent` | `#CE6A28` (Schill orange, ~10% dimmer) |
| `accentStrong` (links, active tab) | `#F0975A` |
| `accentSoft` | `#4A2F1C` |
| `frostedBar` | `rgba(20,21,18,0.78)` + blur 20 |
| `barBorder` | `rgba(237,237,230,0.08)` |

Theme follows the system by default; Settings can force Light or Dark.

---

## 2. Typography
Font: system font (`-apple-system` / SF Pro on iOS), fallback **Inter**.

| Style | Size | Weight | Line height | Letter spacing |
|---|---|---|---|---|
| Large title | 34 | 600 | 1.1 | -0.02em |
| Title | 22 | 600 | 1.2 | -0.02em |
| Headline (card titles) | 17 | 600 | 1.3 | 0 |
| Body | 17 | 400 | 1.4 | 0 |
| Subhead | 15 | 400 | 1.4 | 0 |
| Caption | 13 | 400 | 1.4 | 0 |
| Section header | 13 | 600, UPPERCASE | 1.4 | +0.02em, `textSecondary` |
| Tab label | 11 | 400 (600 when active) | — | 0 |

Hierarchy comes from size and weight, not colour. Support Dynamic Type.

---

## 3. Spacing, shape, depth
- **Grid:** 8pt. Allowed values: 4, 8, 16, 24, 32, 48 (12 and 20 only where noted).
- **Screen side margins:** 20.
- **Between cards:** 16. **Between sections:** 24.
- **Card padding:** 20 (16 for compact stat cards).
- **List row:** min height 52 (settings) / 64 (habits); horizontal padding 16.
- **Radius:** cards 20, buttons 14, inputs 10, icon chips 10, segmented track 10 / segment 8, sheet top corners 20, circles fully round.
- **Touch targets:** at least 44×44.
- **Shadow (only one):** `0 4px 20px rgba(0,0,0,0.05)`, on the main card per screen. Others use the 1px border only.
- **Icons:** line icons (lucide), stroke 1.75, size 20 in lists, 26 in tab bar. No emoji.

---

## 4. Motion
- Duration 200–300ms. Easing: iOS-style spring / `cubic-bezier(.2,.8,.2,1)`.
- Sheets slide up 40px + fade in (300ms); overlay fades in (250ms).
- Check button scales to 0.92 on press; fill fades in.
- Progress ring animates its stroke when progress changes.
- Large title fades out between 20 and 50px of scroll, while the compact centred title fades into the frosted nav bar and its hairline border appears.
- Skeletons shimmer (1.4s loop).
- **Reduced motion:** turn off all of the above (instant changes, static skeletons).
- Light haptic when a habit is completed.

---

## 5. Components
- **Card:** surface, 1px border, radius 20, padding 20.
- **Grouped list:** one card containing rows separated by `divider`; section header above it (13 caps, 16 left inset).
- **Habit row:** 36px icon chip (tint bg, `textSecondary` icon) · name (17) + caption (13) · 44px check button. Tapping the row (not the check) opens Habit detail.
- **Check button:** 28px circle. Off = 1.5px `iconMuted` outline. On = `accent` fill with white check (stroke 2.5).
- **Progress ring:** 88px, stroke 8, track `tint`, progress `accent`, round caps, starts at 12 o'clock; % (17 semibold) in the centre.
- **Week strip:** 7 columns, 28px dots. Done = `accent`; partial = `accentSoft`; today = 1.5px `accentStrong` ring + bold orange letter; future = 1.5px `border` ring.
- **Calendar:** 7-col grid, gap 6, square cells radius 10. Done = `accent`; missed = `tint`; today = `accentStrong` ring + semibold; future = `border` ring. Legend below.
- **Primary button:** min height 52, radius 14, `accentStrong` bg, white 17 semibold. Disabled: `border` bg, `textSecondary` text.
- **Secondary button:** min height 44, surface bg, 1px border, `accentStrong` text.
- **Text button / link:** `accentStrong`, 17.
- **Switch:** 51×31 track, 27px white knob with soft shadow; on = `accent`, off = `switchOff`. Role `switch`.
- **Segmented control:** `tint` track, padding 2; selected segment white, soft shadow, 600 weight. Role `radiogroup`/`radio`.
- **Input:** min height 48, radius 10, 1px border, surface bg, 17 text; focus = 2px `accent` ring.
- **Frosted nav bar:** height 56; left spacer, centred compact title (17 semibold, hidden until scrolled), right 44px circular + button (surface, border, orange icon).
- **Tab bar:** height 84, frosted, hairline top border; 3 tabs (Today, Insights, Settings); active = `accentStrong` + 600.
- **Sheet:** frosted, top radius 20, 36×5 grabber, header row: Cancel (left link) · title (centre, 17 semibold).
- **Empty / error block:** centred 56px circle icon (tint bg, or `accentWash` with orange icon for empty-with-action, or `errorBg` with `errorText` icon for errors) → title 17 semibold → caption 15 `textSecondary` → optional button.
- **Warning banner:** `warningBg`, radius 14, padding 12×16, icon `warningIcon`, title 15 semibold + caption 13.

---

## 6. Screens

### Today (tab)
1. Frosted nav bar (compact title on scroll, + opens Add sheet).
2. Caption date ("Friday, 30 October") + large title "Today".
3. Progress card (shadowed): ring + "2 of 4 done" (22 semibold) + "A steady day. Small steps still count."
4. "HABITS" grouped list of habit rows.
5. "This week" card: header + "4 of 5 days" caption + week strip.
6. "How does today feel?" card (tint bg): caption "A quick check-in. No wrong answers." + 3-option selector: Calm / Okay / Heavy (selected = white with 1.5px `accentStrong` ring).
7. Bottom padding clears the tab bar.

### Habit detail (pushed)
- Nav: "‹ Today" back link (left), "Edit" (right), frosted.
- Large title = habit name; caption "Every day · 7:30 am".
- 2-col stat cards: Current streak (flame icon in `accent`, "12 days") · This month ("26 / 30").
- Calendar card: month name + "Best: 21 days", weekday letters, grid, legend (Walked / Missed).
- "WHY THIS MATTERS" note card (tint bg), body 17.
- Primary "Done for today" + text button "Skip today".

### Settings (tab)
- Large title "Settings".
- **Reminders:** Daily reminder (switch) · Reminder time (value + chevron) · Gentle nudges (switch, caption "One quiet reminder in the evening").
- **Appearance:** Theme segmented control: System / Light / Dark.
- **Your data:** Sync across devices (switch) · Export habits (chevron) · Reset all progress (`errorText`, confirm first).
- Footer caption: "Version 1.0 · Your habits stay on your devices."

### Add habit sheet (modal)
- Grabber, Cancel, title "New habit".
- NAME input (placeholder "e.g. Stretch for five minutes").
- ICON picker: 4 tiles (sun, book, water drop, moon); selected = `accentWash` bg, orange icon, 1.5px `accent` ring.
- HOW OFTEN segmented: Daily / Weekdays / Custom.
- Card: Remind me (switch); when on, reveals Time row (value in a tint pill).
- Primary "Add habit", disabled until the name is non-empty.

### Insights (tab)
Overview of all habits using the same stat card, calendar and week-strip components. Keep it calm: no dense charts.

### Wide screens (≥768px)
- Sidebar (min 220) replaces the tab bar: app name caption + nav items (44 high, radius 10; active = `accentWash` bg, `accentStrongPressed` text).
- Content max width 1040; progress and week cards in an auto-fit grid (min 280); habits as cards in an auto-fill grid (min 240).
- The header row has the large title on the left and a primary "New habit" button on the right.
- Below 768px, the sidebar stacks above the content and everything drops to one column.

---

## 7. States (every screen)
| Screen | Loading | Empty | Error |
|---|---|---|---|
| Today | Skeleton ring + 2 text bars + 3 row bars | Sprout icon · "Nothing to tend to yet" · "Start with one small habit." · **Add a habit** (primary) | Cloud-off icon · "Couldn't load today" · "Check your connection and try again." · **Try again** (secondary) |
| Habit detail | Skeleton title + calendar grid | Calendar icon · "No history yet" · "Your first check-in will show up here." | Alert icon · "This habit couldn't be found" · "It may have been removed on another device." · **Back to Today** (link) |
| Settings | Skeleton rows with switch shapes | Bell icon · "No reminders set" · "Pick a time and we'll nudge you gently." · **Set a time** (secondary) | Warning banner "Changes weren't saved" · "We'll keep trying in the background." · **Retry** on the affected row |

Skeleton colour: `tint`, shimmer to `background`.

---

## 8. Sample data (seed on first launch)
| Habit | Icon | Caption |
|---|---|---|
| Morning walk | sun | Daily · 12-day streak |
| Read 20 pages | book | Daily · 5-day streak |
| Drink water | water drop | 6 of 8 glasses |
| Wind down by 10 | moon | Weeknights |

---

## 9. Accessibility
- WCAG AA: body text at least 4.5:1. Never put small text in `accent` (#E5762C); use `accentStrong`.
- Every icon-only button has an accessibility label (e.g. "Add habit", "Mark done: Morning walk").
- Correct roles: switch, radio/radiogroup, button, link; check buttons expose pressed state.
- Progress ring and calendar have text equivalents ("2 of 4 habits done"; "Walked 26 of 30 days").
- Visible focus ring: 2px `accentStrong`, offset 2.
- Respect reduced motion and Dynamic Type.
