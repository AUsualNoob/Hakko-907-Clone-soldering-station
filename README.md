<img width="681" height="550" alt="image" src="https://github.com/user-attachments/assets/f6286f3b-8d2a-4b29-ad59-001733849733" /># Hakko 907 soldering station

## Features

- Arduino Nano based control
- Compatible with Hakko 907 / 907C style handles
- Temperature monitoring
- Adjustable temperature control

## PCB

### Schematic
<img width="363" height="467" alt="image" src="https://github.com/user-attachments/assets/abeda465-e69d-4d6b-9c29-141b1890cead" />
Took me a long while, Research and part searching is hard man.
### Tracing
<img width="681" height="550" alt="image" src="https://github.com/user-attachments/assets/ac25b324-75d4-4819-83f2-7b77c31b50a6" />
I had to make it 4 layer for my sake, I wouldn't be able to make it 2 layer.

## Firmware

There are 2 versions of the firmware, There's a BB version (Bang-Bang)
which works by having the transistor at either max PWM value or 0 PWM value for increasing and decreasing temperature
And the PID version (Proportial-Integral-Derivative)
Which adjusts the PWM value depending on how far the current temperature is, This explanation was pasted from my journal in hackclub blueprint
so basically, it all starts with an error value
the error value is calculated by subtracting the wanted temperature by the current temperature
(Error = wantedTemp - currentTemp)
then, that Error value is multiplied by a kp value, the kp value is tweaked to ensure it works properly, it has a default of 2
the product of the Error value and the kp value is going to be a part of the PWM value
(PWM = kp * Error)
then there's the integral value, the integral value has a default of 0, it's calculated like this
(integral = integral + error)
usually if the Error value gets smaller, the less power goes into the iron, so it usually stays at 10 - 20 degrees celsius less than the wanted value, this is where the integral comes in, it keeps getting added so you can reach the temperature
then it's multiplied by ki, which is also something that should be tweaked, it has a default of 0.5
however the integral value and the error value together usally overshoots, and thats where the derivative comes in
derivative is calculated like this
(derivative = error - lastError)
its then multiplied by kd, which has a default value of 0.1 and should also be tweaked.
in the end, it's all put into one float variable, called pid, which is this
(pid = (kp * Error) + (ki * integral) + (kd * derivative))
which is then constrained to then be an integer pwm value
(int pwm = constrain((int)pid, 0, 255))

## CAD
<img width="504" height="390" alt="image" src="https://github.com/user-attachments/assets/163ff10d-e818-4b90-a387-73bb7ed6d63e" />
Fusion is hard man.

# BOM

1x Arduino Nano

1x 1000µF 50V capacitor

1x 100µF 50V capacitor

2x 5mm LEDs

1x 1N5822 diode

1x 1N5240B zener diode

1x 1.3" 128x64 I2C OLED display

1x Polyfuse

1x GX16-5 connector

1x DC barrel jack

1x Inductor (for LM2576)

1x IRF4905 MOSFET

1x IRLZ44N MOSFET

1x 100k resistor

1x 10k resistor

1x 470Ω resistor

1x 3.3k resistor

1x 2.7k resistor

2x 2.2k resistors

1x Rotary encoder

1x LM2576HVS-5 buck converter IC

1x LM358 op-amp
