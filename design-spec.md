# Design Spec — Streak Concept 2

Source: Figma `Ba8jlZmjd5LT146gOU67gz`, section `27:4017` "Streak - Concept 2".

## Phone canvas
- Size: **360 × 814** (Android safe area). Screens may scroll vertically inside the phone frame.
- Top bar (status + nav) lives inside the phone, not the desktop wrapper.

## Design tokens

### Colors
| Token | Value |
|---|---|
| Background / Primary | `#161616` |
| Background / Secondary | `#242424` |
| Popup background | `rgba(36,36,36,0.88)` (with 5px backdrop blur, 72% black overlay) |
| Text / Primary | `#FFFFFF` |
| Text / Secondary | `#B9B9B9` |
| Text / Disabled | `#6e6e6e` |
| Text / Inverted | `#161616` |
| Accent / Interactive (green) | `#6eff46` |
| Accent / Lime gradient start | `#C2EF43` |
| Accent / Lime gradient end | `#6EFF46` |
| Stroke / Glass light | `rgba(255,255,255,0.7)` |
| Glass fill 10 | `rgba(255,255,255,0.1)` |
| Glass fill 05 | `rgba(255,255,255,0.05)` |

### Reward button (radial gradient)
white → mint → lime → cream → pink. Used on all "primary action" buttons that look like a glossy lime-pink-pearl.
```
radial-gradient(ellipse at left bottom,
  #FFFFFF 20%, #D2FFC5 40%, #A4FF8B 60%, #D2CFC1 75%, #FF9FF7 90%);
```

### Typography
- **Display**: `Russo One` (used for all titles, balance numbers, button labels)
- **Body**: `Montserrat Medium` (paragraph text, captions)

| Style | Size | LH | Tracking | Family |
|---|---|---|---|---|
| Headline | 24 | 28 | 1 | Russo One |
| Title | 20 | 24 | 1 | Russo One |
| Title Small CAPS | 16 | 20 | 2 | Russo One |
| Body Large Tight | 16 | 20 | 1 | Russo One |
| Body Basic Bold | 14 | 16 | 1 | Russo One |
| Body Paragraph | 14 | 20 | 1 | Montserrat |
| Caption | 12 | 14 | 0.5 | Montserrat |
| Button M-L | 12 | 16 | 6 (CAPS) | Russo One |
| Button S Bold | 10 | 12 | 8 (CAPS) | Russo One |

---

## Screens

### Screen 1 — `Internal_1` (node `27:4190`)
**Wallet, 3-day streak active, button: "Активировать буст"**

Layout (top-down):
1. Status bar (Android, 24px) — `09:30 PM` left, signal icons right.
2. Navigation bar (64px) — back arrow, "Кошелек" title, help "?" icon.
3. Caption text — "Здесь ваш кошелёк. Все заработанные деньги поступят на этот счёт."
4. **All Minutes glass card** — title "Заработай до $1500", row: "Получай 3 награды ежедневно" + value "30 / 365 дней", lime progress bar (~28%).
5. **Balance card** (`#242424`, radius 32) — "≈$955.84" + info "i", "Balance" subtitle, then 3 token rows:
   - DOPPY · 1 ~ $2.63 · 144 · ~$378.72
   - BNH · 1 ~ $10.64 · 6 · ~$63.84
   - LUMY · — · 2 · ~$462.28
6. **Streak widget** (`rgba(255,255,255,0.05)`, radius 24) — title "Разблокируй Вывод DOPPY", body "Заходи в ленту каждый день и собирай по 3 награды — 3 дня подряд, и вывод разблокирован!", row of 3 progress items: 🔥 "1 день" / 🔥 "2 день" / ⭕ "3 день" (third is empty circle outline `#6e6e6e`).
7. **Offer card** inside widget — body "Активируй буст заработка / и разблокируй вывод сразу", small white pill button "АКТИВИРОВАТЬ" (CAPS, dark text).

**Click target → goes to Screen 2:** the "АКТИВИРОВАТЬ" button.

### Screen 2 — `Internal_2` (node `27:4231`)
**7-day streak with image, no button**

Same header & balance section as Screen 1. Streak widget changes:
- Two coins image (DOPPY token pair) at the top of the widget, ~180×115px.
- Title: "Получи $7 в DOPPY"
- Body: "Заходи в ленту каждый день и собирай по 3 награды — 7 дней подряд!"
- 7 progress items in a row: 🔥1 🔥2 🔥3 ⭕4 ⭕5 ⭕6 ⭕7 (3 fire emojis, 4 empty circles)
- No button.

**Click target → goes to Screen 3:** the streak widget body (or the row of days).

### Screen 3 — `Internal_3` (node `27:4269`)
**7-day streak fully filled, button "Забрать награду"**

Same as Screen 2 but **no top image**, all 7 days are 🔥, and an "Забрать награду" gradient button appears.

**Click target → opens Popup 1:** the "Забрать награду" button.

### Screen 4 — `Internal_4` (node `27:4309`)
**Cycle complete: top widget gets a reward button too**

- All Minutes card now contains a "Забрать награду" gradient button at the bottom.
- Streak widget = Screen 2 layout (with coins image) + 7 fully-filled fire days + "Забрать награду" button.

**Click target → opens Popup 2:** the lower "Забрать награду" button (and optionally the upper one).

### Popup 1 — `Popup_Overlay_1` (node `27:4180`)
Overlay: 5px backdrop blur over the underlying screen, dark 72% black scrim.
Card (`#242424cc`, radius 32, 280×~316 inside 328-wide):
- DOPPY token glossy image (180×180 box, contained).
- Title (Russo One 20): "Поздравляем! Ты разблокировал вывод DOPPY"
- Subtitle (Montserrat 14): "Оставайся активным и получай более крутые награды"
- Reward gradient button: "ЗАБРАТЬ НАГРАДУ"
- Close "X" icon top-right.

**Click target → goes to Screen 4:** button OR cross.

### Popup 2 — `Popup_Overlay_2` (node `27:4170`)
Same shell as Popup 1.
- Money-bag image (silver bag with $ + DOPPY coins on top).
- Title: "Поздравляем! / Ты получил награду — / 7 DOPPY!"
- Same subtitle.
- "ЗАБРАТЬ НАГРАДУ" gradient button.

**Click target → goes to Screen 7:** button OR cross.

### Screen 7 — `Refund_2btn` (node `27:5469`)
**Stats / progress overview, 24-day streak, two buttons**

Top 194px is **black** (Android safe area / "from previous screen"). Rest is the content area.
1. Back arrow only (no title).
2. Hero: "Получи [$1500 lime pill] / за 365 дней в приложении" + subtitle "Смотри ленту, лови 3 награды в день / и получи бонус в монете DOPPY". Behind the title there's a faint coins-pile background.
3. **Big lime progress bar** spanning the width, with a status pill that "rides" the bar reading "Твой стрик: / 24 дня" (dark pill with white border).
4. **Milestones row** (5 items, evenly spaced): 3 дня — Разблокировка вывода (50% opacity, gray arrow); 7 дней — $7; 30 дней — $30; 180 дней — $500; 365 дней — $1500 (this last in green). Triangle markers above each.
5. **Buttons**: "СМОТРЕТЬ ВИДЕО" (gradient reward button) + "УСЛОВИЯ" (transparent with green outline + green text).

**Click target → goes to Screen 8:** "СМОТРЕТЬ ВИДЕО" button.

### Screen 8 — `Refund_1btn` (node `27:5648`)
**Stats / progress overview, 30-day streak, one button**

Same as Screen 7 but:
- Status pill says "Твой стрик: 30 дней"
- Progress bar fills further (to ~30 day position).
- The "30 дней / $30" milestone marker is **active** (green arrow + green text).
- Single button "ЗАБРАТЬ НАГРАДУ" (gradient).

**Click target → restarts cycle to Screen 1:** "ЗАБРАТЬ НАГРАДУ" button.

---

## Navigation map

```
Screen 1 ─[АКТИВИРОВАТЬ]─►
Screen 2 ─[tap streak/days]─►
Screen 3 ─[ЗАБРАТЬ НАГРАДУ]─►
Popup 1  ─[ЗАБРАТЬ НАГРАДУ or X]─►
Screen 4 ─[ЗАБРАТЬ НАГРАДУ]─►
Popup 2  ─[ЗАБРАТЬ НАГРАДУ or X]─►
Screen 7 ─[СМОТРЕТЬ ВИДЕО]─►
Screen 8 ─[ЗАБРАТЬ НАГРАДУ]─► back to Screen 1 (cycle)
```

---

## Text strings — RU / EN

### Header strings (used across screens)
| key | RU | EN |
|---|---|---|
| nav.wallet | Кошелек | Wallet |
| caption.walletDescription | Здесь ваш кошелёк. Все заработанные деньги поступят на этот счёт. | This is your wallet. All your earnings will land here. |

### All-minutes widget
| key | RU | EN |
|---|---|---|
| minutes.title | Заработай до $1500 | Earn up to $1500 |
| minutes.body | Получай 3 награды ежедневно | Collect 3 rewards every day |
| minutes.value | 30 | 30 |
| minutes.unit | / 365 дней | / 365 days |
| minutes.cta | ЗАБРАТЬ НАГРАДУ | CLAIM REWARD |

### Balance card
| key | RU | EN |
|---|---|---|
| balance.label | Balance | Balance |
| balance.value | ≈$955.84 | ≈$955.84 |
| token.doppy | DOPPY | DOPPY |
| token.bnh | BNH | BNH |
| token.lumy | LUMY | LUMY |
| token.doppyRate | 1 ~ $2.63 | 1 ~ $2.63 |
| token.bnhRate | 1 ~ $10.64 | 1 ~ $10.64 |

### Streak widget — Screen 1
| key | RU | EN |
|---|---|---|
| streak1.title | Разблокируй Вывод DOPPY | Unlock DOPPY withdrawals |
| streak1.body | Заходи в ленту каждый день и собирай по 3 награды — 3 дня подряд, и вывод разблокирован! | Open the feed every day and earn 3 rewards — 3 days in a row, and withdrawals unlock! |
| streak1.day | день | day |
| streak1.offerTitle | Активируй буст заработка\nи разблокируй вывод сразу | Activate the earnings boost\nand unlock withdrawals right away |
| streak1.offerCta | АКТИВИРОВАТЬ | ACTIVATE |

### Streak widget — Screens 2–4
| key | RU | EN |
|---|---|---|
| streak2.title | Получи $7 в DOPPY | Get $7 in DOPPY |
| streak2.body | Заходи в ленту каждый день и собирай по 3 награды — 7 дней подряд! | Open the feed daily and earn 3 rewards — 7 days in a row! |
| streak2.cta | ЗАБРАТЬ НАГРАДУ | CLAIM REWARD |

### Popup 1
| key | RU | EN |
|---|---|---|
| popup1.title | Поздравляем! Ты разблокировал вывод DOPPY | Congrats! You've unlocked DOPPY withdrawals |
| popup1.subtitle | Оставайся активным и получай более крутые награды | Stay active and earn even cooler rewards |
| popup1.cta | ЗАБРАТЬ НАГРАДУ | CLAIM REWARD |

### Popup 2
| key | RU | EN |
|---|---|---|
| popup2.title | Поздравляем!\nТы получил награду —\n7 DOPPY! | Congrats!\nYou've earned a reward —\n7 DOPPY! |
| popup2.subtitle | Оставайся активным и получай более крутые награды | Stay active and earn even cooler rewards |
| popup2.cta | ЗАБРАТЬ НАГРАДУ | CLAIM REWARD |

### Refund screens (7 + 8)
| key | RU | EN |
|---|---|---|
| refund.heroPrefix | Получи | Earn |
| refund.heroPill | $1500 | $1500 |
| refund.heroSuffix | за 365 дней в приложении | in 365 days in the app |
| refund.subtitle | Смотри ленту, лови 3 награды в день\nи получи бонус в монете DOPPY | Watch the feed, grab 3 rewards a day\nand earn your bonus in DOPPY |
| refund.streakLabel | Твой стрик: | Your streak: |
| refund.day | день | day |
| refund.daysFew | дня | days |
| refund.daysMany | дней | days |
| refund.milestone3 | Разблокировка\nвывода | Unlock\nwithdrawals |
| refund.milestone7 | $7 | $7 |
| refund.milestone30 | $30 | $30 |
| refund.milestone180 | $500 | $500 |
| refund.milestone365 | $1500 | $1500 |
| refund.btnVideo | СМОТРЕТЬ ВИДЕО | WATCH VIDEO |
| refund.btnTerms | УСЛОВИЯ | TERMS |
| refund.btnClaim | ЗАБРАТЬ НАГРАДУ | CLAIM REWARD |

---

## Asset inventory (`/prototype/assets/`)
- `top_bg.png` — purple/pink gradient banner behind nav bar (160h)
- `fire_emoji.png` — 🔥 fire emoji used for streak progress (32×32)
- `coin_doppy.png` — DOPPY token (round, green letter D)
- `coin_cheel.png` + `coin_cheel_bg.png` — BNH alien token
- `coin_lumy.png` — LUMY rainbow token
- `popup_doppy.png` — large DOPPY trophy (popup 1)
- `popup_money_bag.png` — silver $ bag with DOPPY coins (popup 2)
- `refund_coins_bg.png` — soft coins pile behind refund hero
- `progress_bar_filled.png` — pre-rendered green bar (we re-create with CSS)
- `arrow_marker.png` / `arrow_marker_active.png` — small triangle markers above milestones (we re-create with CSS)
