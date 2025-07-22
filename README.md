# GB34 (RP2040) ZMK Module

## Instructions

Add this repository to your existing `config/west.yml` as shown below. (No idea what that means? [Make a new repo from this template first](https://github.com/zmkfirmware/unified-zmk-config-template), and then you'll have something to edit.)

```yaml
manifest:
  remotes:
    - name: zmkfirmware
      url-base: https://github.com/zmkfirmware
  projects:
    - name: zmk
      remote: zmkfirmware
      revision: main
      import: app/west.yml
      # GB34 project name, url and branch
    - name: gb34-rp2040
      url: https://github.com/SeebM/zmk-config
      revision: GB34
  self:
    path: config
```

After updating this `west` manifest, copy [`boards/arm/niuniu_gb34_rp2040/niuniu_gb34_rp2040.keymap`](boards/arm/niuniu_gb34_rp2040/niuniu_gb34_rp2040.keymap) into your `config` folder for later modification. Don't forget to [edit your `build.yaml`](https://zmk.dev/docs/customization#building-additional-keyboards) to include the new board:

```yaml
---
include:
  - board: niuniu_gb34_rp2040
  # Including a settings_reset build is optional, but recommended.
  - board: niuniu_gb34_rp2040
    shield: settings_reset
```

## Common Questions/Problems

### How can I get to the bootloader?

1. Press and hold the BOOTSEL button between keys 10 and 11.
   - If the button is not working: look for a 2mm x 3mm rectangle with exactly eight pins on the underside of the PCB. This is the QSPI flash chip. Use metal tweezers to bridge its top-right and bottom-right pins. The bottom-right pin is marked with a tiny white silkscreen dot.
2. While holding the button down or making the equivalent connection with tweezers, plug the keyboard into USB.
   - If the keyboard is already plugged in, shorting the plated through-holes near the MCU will reset the microcontroller (which does the same thing).

### There was an error and the build didn't finish.

#### There is probably an error in your keymap.

[Clicking into the details of the action](https://docs.github.com/en/actions/quickstart#viewing-your-workflow-results) will show the error log. While very long, this log will often show the exact line number causing the problem in your keymap so it is worth reading closely. Check this against the last changes you made.

Keep in mind that each ZMK "keycode" is made up of a [behavior](https://zmk.dev/docs/features/keymaps#behaviors) which always has an `&` in front (e.g. `&kp` for a **k**ey **p**ress), followed by any details for that behavior.
Resetting to bootloader is just [`&bootloader`](https://zmk.dev/docs/behaviors/reset) while the letter `Z` is [`&kp Z`](https://zmk.dev/docs/behaviors/key-press). And "tap for A, hold for left Shift" is [`&mt LSHIFT A`](https://zmk.dev/docs/keymaps/behaviors/hold-tap)). Read [the docs](https://zmk.dev/docs/) closely.

## Further Troubleshooting Resources

- [ZMK's troubleshooting page](https://zmk.dev/docs/troubleshooting)
- Ask the ZMK experts in [ZMK's Discord](https://zmk.dev/community/discord/invite)
