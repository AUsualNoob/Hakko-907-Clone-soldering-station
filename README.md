# Hakko 907 Soldering Station

![CAD render](https://github.com/user-attachments/assets/163ff10d-e818-4b90-a387-73bb7ed6d63e)

## Features

- Arduino Nano based control
- Compatible with Hakko 907 / 907C style handles
- Temperature monitoring
- Adjustable temperature control

---

## PCB

### Schematic

![Schematic](https://github.com/user-attachments/assets/abeda465-e69d-4d6b-9c29-141b1890cead)

Took me a long while. Research and part searching is hard man.

### Tracing

![PCB trace](https://github.com/user-attachments/assets/ac25b324-75d4-4819-83f2-7b77c31b50a6)

I had to make it 4 layer for my sake, I wouldn't be able to make it 2 layer.

---

## Firmware

There are 2 versions of the firmware.

### Bang-Bang (BB)

Works by having the transistor at either max PWM value or 0 PWM value for increasing and decreasing temperature.

### PID (Proportional-Integral-Derivative)

Adjusts the PWM value depending on how far the current temperature is from the wanted temperature.

It all starts with an error value.

#### Error

The error value is calculated by subtracting the wanted temperature by the current temperature.

```
Error = wantedTemp - currentTemp
```

#### Proportional

That error value is multiplied by a `kP` value. The `kP` value is tweaked to ensure it works properly, it has a default of `2`. The product of the error value and the `kP` value is going to be a part of the PWM value.

```
PWM = kP * Error
```

#### Integral

The integral value has a default of `0`. Usually if the error value gets smaller, the less power goes into the iron, so it usually stays at 10–20 degrees celsius less than the wanted value. This is where the integral comes in — it keeps getting added so you can reach the temperature. It's then multiplied by `kI`, which is also something that should be tweaked, it has a default of `0.5`.

```
integral = integral + error
```

#### Derivative

However the integral value and the error value together usually overshoots, and that's where the derivative comes in. It's then multiplied by `kD`, which has a default value of `0.1` and should also be tweaked.

```
derivative = error - lastError
```

#### Final Output

In the end, it's all put into one float variable, called `pid`, which is then constrained to then be an integer pwm value.

```
pid = (kP * Error) + (kI * integral) + (kD * derivative)
```

```cpp
int pwm = constrain((int)pid, 0, 255);
```

---

## CAD

![CAD render](https://github.com/user-attachments/assets/163ff10d-e818-4b90-a387-73bb7ed6d63e)

Fusion is hard man.

---

## Finished Product

<img width="1699" height="931" alt="image" src="https://github.com/user-attachments/assets/b44783df-4a06-4c3d-bc0d-b559bb12ee50" />

<img width="1324" height="852" alt="image" src="https://github.com/user-attachments/assets/d3ea94b2-884b-4f66-b9e8-846b1e8f6a5b" />

<img width="1449" height="819" alt="image" src="https://github.com/user-attachments/assets/985e671e-db0a-4637-bce9-f0976217c735" />

<img width="809" height="233" alt="image" src="https://github.com/user-attachments/assets/dcebe23e-f05d-4910-b076-d771b0b2ec63" />

---

## BOM

| Qty | Part | Purpose | Cost (USD) | Distributor |
|-----|------|---------|------------|-------------|
| 2x | [5mm LEDS](https://www.ram-e-shop.com/shop/led-gg-led-5mm-green-color-5940) | Knowing when it's on or off | 0.019$ | Ram Electronics |
| 1x | [Arduino Nano v3.x](https://www.ram-e-shop.com/shop/kit-arduino-nano-ch340-arduino-nano-328-with-ch340-chip-clone-8338) | Brain of the device | $5.00 | Ram Electronics |
| 1x | [1000µF 50V capacitor](https://www.ram-e-shop.com/shop/c-1000u50v-capacitor-1000uf-50v-6104) | Passive component | $0.10 | Ram Electronics |
| 1x | [100µF 50V capacitor](https://www.ram-e-shop.com/shop/c-100u50v-capacitor-100uf-50v-6073) | Passive component | $0.03 | Ram Electronics |
| 1x | [1N5822](https://www.amazon.eg/-/en/5pcs-1N5822-5822-1N582-Diode/dp/B0DWY2S9S8) | Passive component | $3.00 | Amazon |
| 1x | [1.3" 128x64 I2C OLED display](https://www.ram-e-shop.com/shop/oled-1-3-4pin-oled-1-3-4pin-lcd-display-module-i2c-iic-communicate-7794) | Displaying temperature | $1.75 | Ram Electronics |
| 1x | [1N5240B zener diode](https://www.amazon.eg/-/en/gp/product/B0CWPCPZN2) | Zener diode | $1.50 | Amazon |
| 1x | [RUEF300 polyfuse](https://www.amazon.eg/-/en/gp/product/B0FNLD9RPG) | Protection passive component | $2.00 | Amazon |
| 1x | [GX16-5 connector](https://www.ram-e-shop.com/shop/gx16-5pin-gx16-5-pole-metal-male-female-panel-chassis-connector-7025) | Connector for soldering iron | $1.00 | Ram Electronics |
| 1x | [DC barrel jack](https://www.amazon.eg/-/en/gp/product/B0D4N6JF7F) | Input plug for power | $1.00 | Amazon |
| 1x | [100µH inductor](https://www.amazon.eg/-/en/gp/product/B0FF9X6THF) | Noise reduction | $1.50 | Amazon |
| 1x | [IRF4905 MOSFET](https://www.amazon.eg/-/en/IRF4905-P-Channel-HEXFET-MOSFET-74A/dp/B09H13NQD4) | P-channel MOSFET for reverse polarity protection | $2.00 | Amazon |
| 1x | [IRLZ44N MOSFET](https://www.ram-e-shop.com/shop/irlz44-irlz44n-logic-level-gate-n-channel-mosfet-6633) | N-channel MOSFET for heater control | $1.00 | Ram Electronics |
| 1x | [100k resistor](https://www.ram-e-shop.com/shop/carbon-resistance-1-4w-price-per-4-resistors-9506#attr=314) | Passive component | $0.40 | Ram Electronics |
| 1x | [10k resistor](https://www.ram-e-shop.com/shop/carbon-resistance-1-4w-price-per-4-resistors-9506#attr=314) | Passive component | $0.02 | Ram Electronics |
| 1x | [470Ω resistor](https://www.ram-e-shop.com/shop/carbon-resistance-1-4w-price-per-4-resistors-9506#attr=307) | Passive component | $0.02 | Ram Electronics |
| 1x | [3.3k resistor](https://www.amazon.eg/-/en/Resistor-Through-Electronics-Circuit-Projects/dp/B0GYQ2GYK1) | Passive component | $1.50 | Amazon |
| 1x | [2.7k resistor](https://www.ram-e-shop.com/shop/carbon-resistance-1-4w-price-per-4-resistors-9506#attr=307) | Passive component | $0.02 | Ram Electronics |
| 2x | [2.2k resistor](https://www.amazon.eg/-/en/Resistor-Through-Electronics-Circuit-Projects/dp/B0GYQ2GYK1) | Passive component | $1.00 | Amazon |
| 1x | [Rotary encoder](https://www.ram-e-shop.com/shop/pot-ec11-rotary-5pin-20mm-ec11-rotary-encoder-with-push-button-switch-5pin-20mm-silver-9624) | Controlling temperature | $0.50 | Ram Electronics |
| 1x | [LM2576](https://www.amazon.eg/-/en/gp/product/B0FHHC669J) | Step down converter for the Arduino | $2.00 | Amazon |
| 1x | [LM358 op-amp](https://www.ram-e-shop.com/shop/lm358-copy-lm358p-copy-6130) | OPAMP for amplifying | $0.10 | Ram Electronics |
| 1x | [Soldering Iron Handle](https://www.amazon.eg/-/en/Home-Soldering-Desoldering-Solder-Station/dp/B0GCSY3SBH/ref=sr_1_7?crid=164RY93DEOGSH&dib=eyJ2IjoiMSJ9.oCii5B0EJOsOawhvu71j2wZhxK530PdLNBRV6zdfw3OTkg1bqy9Yza85gD998wNN5HrHseDyYSVGOqD3hhtNVk6EGwazw0Ifr6qm7G5Nc9aeKXNLEXccIUOAhkQdMeVWgLRELA1Q0lunMn9dBY4rNqdgxpqJ6kCvdFOx1OawHm3lGXM1f2BZOPYWBCD0mLDAcphNL7xB0_6o7BtSDkXERMccpYQZjLgv-jea5Jj_LPzZnS8gFeBS01P8wHm20pf7cbIObiJ8NbOkE0pC-3s5_JlGeBRfxMo3UMOcEvQLn_c.uLxslEId-HzDGxANhADrrzYBeEtk4ObHqa4aIK8KpVg&dib_tag=se&keywords=soldering+iron+handle&qid=1778771214&sprefix=soldering+iron+handl%2Caps%2C188&sr=8-7) | The handle for the device | 8.20$ | Amazon |
| 1x | [DC Power Adaptor](https://www.amazon.eg/-/en/gp/product/B0FXBHBNHM/ref=ewc_pr_img_7?smid=A2DMKAT7ZKLGQN&psc=1) | Powering the device | 12.41$ | Amazon |
| 5x | PCB | Electrical connections and main body | 7$ | JLCPCB |


**Subtotal: ~$53.1**

**Shipping and Tax: 30$**

**Total: ~83.1$**
