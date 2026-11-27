# Tatar Cyrillic Keyboard "cvyshmu2"

A Tatar Cyrillic keyboard layout (`cyrl_cvyshmu2_tt`) in two formats:

| File | Format | Target platform |
|------|--------|-----------------|
| `cyrl_cvyshmu2_tt.xml` | Unexpected Keyboard XML | Android (Unexpected Keyboard) |
| `cyrl_cvyshmu2_tt` | XKB symbols | GNU/Linux (X11 / XWayland) |

## About the layout

`cyrl_cvyshmu2` is a custom Tatar layout based on the Russian ЙЦУКЕН arrangement,
extended with the letters required for Tatar (and shared by neighboring
languages): **җ ә ө ү қ ң һ**, plus **ґ** (Ukrainian) and the ruble sign **₽**.

## File 1: `cyrl_cvyshmu2_tt.xml` (Unexpected Keyboard, Android)

This is the native layout format of [Unexpected Keyboard](https://f-droid.org/packages/juloo.keyboard2/).

### Installation

1. Copy `cyrl_cvyshmu2_tt.xml` to the device, e.g.:
   ```
   /storage/emulated/0/Download/cyrl_cvyshmu2_tt.xml
   ```
2. Open **Unexpected Keyboard → Settings → Layouts → Add a layout from a file**
   (the exact path is: *Set layouts → Custom layout / +*), and select the file.
3. The layout appears as **cyrl_cvyshmu2_tt**.

### Gesture legend

| Gesture | Meaning | Example |
|---------|---------|---------|
| Tap | `key0` — the main letter | `җ` |
| Swipe up-left | `key1` — extra letter/symbol | `җ` → `ж` |
| Swipe up | `key2` — digit or symbol | `җ` → `1` |
| Swipe left | `key3` — symbol | `җ` → `!` |
| Swipe right | `key4` — extra letter/symbol | `к` → `қ` |
| Long press / slider | Uppercase of the main letter | `җ` → `Җ` |

`loc esc`, `loc tab`, `loc capslock`, `delete` are special action keys
(Esc, Tab, CapsLock, Delete), provided by the keyboard itself.

## File 2: `cyrl_cvyshmu2_tt` (XKB, GNU/Linux)

A four-level XKB symbols file equivalent to the mobile layout. The three letter
rows map 1:1 onto the physical `AD` / `AC` / `AB` rows, and the original top-row
swipes (digits and punctuation) are placed on the physical number row, the same
way the standard Russian ЙЦУКЕН layout works.

### Levels

| Level | Modifier | Content |
|-------|----------|---------|
| 1 | none | main letter (`key0`) |
| 2 | Shift | uppercase, incl. Җ Қ Ң Ә Ө Ү Һ Ґ |
| 3 | AltGr | swipe letters/symbols: ж ю щ қ w ц ё ъ ґ ₽ ` ; : ' " , . / \ |
| 4 | AltGr+Shift | uppercase of level 3, `?` etc. |

### Number row mapping

| Key | Base | Shift | AltGr | AltGr+Shift |
|-----|------|-------|-------|-------------|
| `` ` `` | `` ` `` | `~` | | |
| `1`–`0` | `1`–`0` | `!` `@` `#` `$` `%` `^` `&` `*` `(` `)` | | |
| `-` | `-` | `_` | `[` | `{` |
| `=` | `=` | `+` | `]` | `}` |
| `\` (above Enter) | `\` | `\|` | | |

Esc, Tab, CapsLock and Delete come from the physical keyboard (pulled in via
`include "pc(pc105)"`; AltGr is enabled by `include "level3(ralt_switch)"`).

### User installation (no root)

```sh
mkdir -p ~/.config/xkb/symbols
cp cyrl_cvyshmu2_tt ~/.config/xkb/symbols/
setxkbmap -I ~/.config/xkb -layout cyrl_cvyshmu2_tt
```

Revert with, for example: `setxkbmap -layout us`.

### System installation (with root)

```sh
cp cyrl_cvyshmu2_tt /usr/share/X11/xkb/symbols/
```

To make the layout selectable in the GNOME/KDE desktop switcher, add the
following block inside `<layoutList>` of `/usr/share/X11/xkb/rules/evdev.xml`:

```xml
<layout>
  <configItem>
    <name>cyrl_cvyshmu2_tt</name>
    <shortDescription>tt</shortDescription>
    <description>Tatar (cvyshmu2)</description>
    <languageList>
      <iso639Id>tat</iso639Id>
    </languageList>
  </configItem>
  <variantList/>
</layout>
```

### Sanity check

```sh
setxkbmap -I ~/.config/xkb -query          # layout should be listed
setxkbmap -I ~/.config/xkb -print          # xkb_symbols should contain cyrl_cvyshmu2_tt
```

## Notes

- Only the XKB file is needed on GNU/Linux; only the XML file is needed on
  Android. They describe the same layout.
- The layout was written strictly following xkeyboard-config conventions
  (standard key types `TWO_LEVEL`, `FOUR_LEVEL`, `FOUR_LEVEL_ALPHABETIC`), so it
  compiles with any modern xkeyboard-config.