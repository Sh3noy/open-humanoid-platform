# M1 bench joint: bill of materials

Prices are INR from the 2026-09-25 parts scout
(`/root/firstmate/data/m1-parts-list/report.md`, sandbox web tools only).
Evidence marks:

- **V** = read from a fetched product page on 2026-09-25.
- **S** = seen only in a search snippet; the shop page was not readable (403 or
  bot check).
- **E** = estimate; no source read.

Nothing was bought and no seller was contacted. **Re-check every price and
stock level at order time.** Anything not V is unverified.

## Parts to order

| # | Part | Key spec | Qty | Indian seller, price, date, evidence | Import fallback |
|---|---|---|---|---|---|
| 1 | Gimbal motor | Tiger/T-Motor GB2208, about 125-128 KV, high phase resistance. 12N14P (7 pole pairs) is typical for the class: **verify on the datasheet** | 1 | Robokits RKI-3511, **Rs 2,474**, 2026-09-25, **V**: https://robokits.co.in/multirotor-spare-parts/tiger-motor-esc-and-propeller/t-motor-gimbal/tiger-gimbal-motor-gb2208-125kv . Robu T-Motor GB2208 KV128, Rs 2,589, **S**: https://robu.in/product/t-motor-gb2208-kv128-gimbal-motor/ | T-Motor store: store.tmotor.com/product/gb2208-gimbal-type.html; generic 2208 gimbals on AliExpress (cheaper, unknown quality) |
| 2 | Magnetic encoder | MT6701 module, 14-bit; I2C/SSI/ABZ/PWM; magnet included on most modules (**confirm the listing says so**) | 1, plus 1 spare | Robu XJX-134 MT6701, price not read (403), **E Rs 300-500**: https://robu.in/product/xjx-134-mt6701-module/ ; robosap.in MT6701 listing, no price read | Alibaba module about USD 1.7, **S**: https://www.alibaba.com/product-detail/MT6701-Magnetic-Encoder-Magnetic-Induction-Angle_1601262043024.html ; Robu MT6835 21-bit module (higher spec, price not read): https://robu.in/product/mt6835-magnetic-encoder-module-pwm-spi/ |
| 3 | Controller + driver | ST B-G431B-ESC1: STM32G431CB, 3-phase driver, shunt current sense, onboard ST-LINK. Supply range "6-28 V" is from memory of the datasheet: **verify** | 1 | TME India, USD 54.76, about **Rs 4,600 (E, at about Rs 84/USD)**, 2026-09-25, **S**: https://www.tme.com/in/en/details/b-g431b-esc1/stm-development-kits/stmicroelectronics/ . element14 India listing, price not read: https://in.element14.com/stmicroelectronics/b-g431b-esc1/discovery-board-32bit-arm-cortex/dp/3247670 . Ignore Rajiv Electronics "Rs 50 + GST" (bad listing) | Mouser / DigiKey, scout snippets suggested about USD 17-25 but conflict with TME. Get one delivered quote including duty before deciding |
| 4 | Bench power supply | Current-limited 0-30 V / 5 A class, used at 12-24 V with a low limit | 1 | **Not priced.** See LAB.md | |
| 5 | Motor mount + magnet holder + fasteners | Printed bracket, M2/M3 screws, 22-24 AWG wire | 1 set | **E Rs 200-400**, not sourced | |
| 6 | Spare ST-LINK (optional) | Not needed, the ESC1 has one onboard | 0-1 | Robokits RKI-5071 Rs 120, **V**: https://robokits.co.in/programmers/stm32-stm08/st-link-v2 ; Robocraze Rs 149, **S** | |

Deferred, not part of M1: CAN-FD transceiver (MCP2562FD or TCAN1462 breakout).
M1 closes a torque loop and needs no CAN-FD. The G431 has FDCAN, so CAN-FD
later needs an external FD transceiver. Whether the ESC1's onboard CAN
transceiver is classic-only is unverified. Tanotis lists the MCP2562FD (page
404 on fetch, price not read).

## Totals

| Line | Rs | Evidence |
|---|---|---|
| Motor | 2,474 | V |
| Encoder (mid of 300-500) | 400 | E |
| ESC1 board | 4,600 | E (converted from S) |
| **Core kit, before PSU and mount** | **about 7,474** | mixed; only the motor is V |
| Mount and fasteners | 200-400 | E |
| Bench PSU | not priced | see LAB.md |

Budget line "Custom joint (motor, encoder, MCU, PCB)" in
`docs/v0-build-spec.md` is Rs 6,000. **The core kit is about Rs 1,500 over
it (about Rs 1,470, before PSU and mount).** Two readings from the scout:
(a) the budget line assumed a custom PCB, which the ESC1 replaces for M1;
(b) the ESC1 price varies by seller, and an import may land lower. The scout
found no verified cheaper option with a driver stage. Decision needed before
ordering: accept the overrun, or get a delivered Mouser/DigiKey quote first.

## Open items
- Verify the B-G431B-ESC1 supply range, current rating and CAN transceiver type
  against ST's datasheet.
- Read real prices for the encoder, and get a cart-level total for the ESC1.
- Confirm the motor pole-pair count and phase resistance from its datasheet.
