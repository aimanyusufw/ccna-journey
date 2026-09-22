# Day 4 - Intro to the Cisco CLI

> [!info] Course context
> Jeremy's IT Lab — **Free CCNA | Intro to the CLI | Day 4 | CCNA 200-301 Complete Course**
> Video: [YouTube](https://www.youtube.com/watch?v=IYbtai7Nu2g)
> Finally, hands-on: configuring Cisco devices through the **Cisco IOS CLI**. ⚠️ Note: Cisco's **IOS** is _not_ related to Apple's iOS!

## CLI vs. GUI

- **CLI** (Command-Line Interface) = the interface used to configure Cisco routers, switches, firewalls. Most network engineers prefer it.
- **GUI** (Graphical User Interface) exists too (e.g., Cisco ASDM for firewalls), but this course uses CLI only.
- **Cisco IOS** = the operating system on Cisco devices (like Windows/macOS).

## Connecting to a Cisco Device

- First-time configuration requires connecting via the **console port** (remote access methods come later).
- Cisco Catalyst switches typically have **two console ports**: an **RJ-45** port and a **USB Mini-B** port.
- Cable needed: a **rollover cable** — one end RJ-45, other end **DB9** (serial). Most modern laptops need a **DB9→USB adapter**.

> [!tip] Rollover (console) cable wiring
> Unlike crossover, pins are _reversed end to end_: **pin 1↔8, 2↔7, 3↔6, 4↔5, 5↔4, 6↔3, 7↔2, 8↔1**.

### Terminal Emulator + Default Serial Settings

Use a **terminal emulator** (e.g., **PuTTY** at putty.org), connect via **Serial**. Cisco default settings (match device defaults — won't need changing):

| Setting           | Default      |
| ----------------- | ------------ |
| Speed (baud rate) | **9600** bps |
| Data bits         | **8**        |
| Stop bits         | **1**        |
| Parity            | **none**     |
| Flow control      | **none**     |

On first boot you'll be asked about the "initial configuration dialog" — answer **no**, then press Enter.

## CLI Modes

| Mode                        | Prompt                   | Capabilities                                                                                                         |
| --------------------------- | ------------------------ | -------------------------------------------------------------------------------------------------------------------- |
| **User EXEC** ("user mode") | `Router>` (greater-than) | Very limited — can look at some things, change _nothing_.                                                            |
| **Privileged EXEC**         | `Router#` (# / hashtag)  | Full access to _view_ config, restart device, change the clock, save config. Cannot change the configuration itself. |
| **Global Configuration**    | `Router(config)#`        | Where you actually change the device configuration.                                                                  |

Enter privileged EXEC with **`enable`**; enter global config with **`configure terminal` / `conf t`**. Use **`exit`** to step back down a mode.

## CLI Shortcuts & Help

- **`?`** — lists available commands in the current mode.
- **`?`** after partial text (no space) — lists commands _starting with_ that text.
- **`?`** with a space — shows what you can type _next_ (e.g., `enable password ?`). `LINE` in caps = type your own value.
- **`<cr>`** = no further options — just press Enter.
- **`Tab`** — auto-completes a partial command (if unambiguous).
- **Abbreviations** — type the shortest unique form: `en` = enable, `conf t` = configure terminal, `sh` = show, `run` = running-config, `do` = run privileged command from config mode.
- If your abbreviation is ambiguous (e.g., just `e` — both `enable` and `exit`), you get an "ambiguous command" error.

> [!warning] Case sensitivity
> Passwords are **case-sensitive** — `CCNA` ≠ `ccna`.

## Protecting Privileged EXEC Mode

### `enable password` (plain text — insecure)

- Configures a password for the `enable` command, stored in **plain text** in the config.
- Anyone who reads the config knows the password — a security risk.

### `service password-encryption` (weak encryption)

- Encrypts all plain-text passwords (enable + others) shown in the config.
- Passwords show as jumbles like `08026F6028` with a leading **7** = Cisco's proprietary type-7 encryption (weak — easily cracked online).
- ⚠️ The **password itself doesn't change** — only how it's displayed.

### `enable secret` (strong — preferred) 🏆

- Same function as `enable password` but uses **MD5 (type 5)** encryption — much stronger. Always encrypted, regardless of `service password-encryption`.
- If **both** are configured, **`enable secret` wins** and `enable password` is ignored.
- **Always use `enable secret`.**

If you type the wrong password **3 times**, you're denied access for "bad secrets."

## Configuration Files

- **running-config** — the current, _active_ configuration. Live-edited as you type commands.
- **startup-config** — the config _loaded on restart_.
- View: `show running-config`, `show startup-config` (before first save: "startup-config is not present").

### Saving the Configuration (Privileged EXEC mode)

Three equivalent commands to save running-config → startup-config:

1. **`write`**
2. **`write memory`**
3. **`copy running-config startup-config`**

## Cancelling Commands

Prepend **`no`** to remove/disable any configured command, e.g., `no service password-encryption`.

> [!example] `no service password-encryption` effects
>
> - **Current passwords**: remain encrypted, NOT decrypted.
> - **Future passwords**: stored in clear text.
> - **`enable secret`**: unaffected — always encrypted.

## `do` command

From global (or sub) configuration mode you can run privileged EXEC commands by prefixing **`do`**, e.g.:

```
Router(config)# do show running-config
Router(config)# do sh ip interface brief
```

## Command Cheat Sheet

| Command                                                         | Function                                               |
| --------------------------------------------------------------- | ------------------------------------------------------ |
| `enable`                                                        | Enter privileged EXEC mode                             |
| `disable`                                                       | Return to user EXEC mode                               |
| `configure terminal` / `conf t`                                 | Enter global configuration mode                        |
| `enable password <pw>`                                          | Protect privileged EXEC with plain-text password       |
| `enable secret <pw>`                                            | Protect privileged EXEC with MD5-encrypted password ✅ |
| `service password-encryption`                                   | Type-7 encrypt the enable (and other) passwords        |
| `show running-config`                                           | View active config                                     |
| `show startup-config`                                           | View saved config                                      |
| `write` / `write memory` / `copy running-config startup-config` | Save running-config to startup-config                  |
| `no <command>`                                                  | Remove/cancel a configured command                     |
| `do <privileged cmd>`                                           | Run a privileged EXEC command from config mode         |

## CLI Modes Recap

- `Router>` → **user EXEC** (`>`)
- `Router#` → **privileged EXEC** (`#`)
- `Router(config)#` → **global configuration** (`(config)#`)

## Quiz Recaps (from the video)

1. Cable to connect to an RJ-45 console port = **A: rollover cable** (crossover is Device-to-Device Ethernet; USB is the _other_ console port).
2. `enable` password rejected = **C: Caps Lock is on** (service password-encryption only affects _display_, not the password itself; passwords are case-sensitive).
3. Most secure way to protect privileged EXEC = **A: enable secret** (MD5 always encrypted; type-7 encryption is weak).
4. Both `enable password` and `enable secret` configured → **C: you enter the enable secret only** (it always takes precedence; never both).
5. Full version of `conf t` = **B: configure terminal**.
