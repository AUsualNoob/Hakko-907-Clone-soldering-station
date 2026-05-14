# Journal

---

## 3/16/2026 — Quick reminder for reviewer
*Time spent: 0.1h*

If you don't allow me to buy the soldering iron handle, I can buy it myself if you'd like. You already gave me a soldering iron and I don't wanna waste your money considering you are non-profit.

![](https://blueprint.hackclub.com/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6MTIyMzI3LCJwdXIiOiJibG9iX2lkIn19--c982a807350ab13435a072d6b9e03f5079dd9afe/gdgdf.PNG)

---

## 3/13/2026 — Big hole on 3D model
*Time spent: 0.1h*

That big hole is supposed to have a panel mount GX16-5 connector / aviation connector. It has a diameter of 18.5mm and I couldn't find a model for it. It isn't mounted onto the PCB but it has cups to put wires in it.

![](https://blueprint.hackclub.com/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6MTIwMjEwLCJwdXIiOiJibG9iX2lkIn19--f0792de748dc07f7c9655b44fac41c0a61d301ae/IMG_20260313_045212.png)

---

## 3/12/2026 — Bigger PCB, mounting holes, case changes
*Time spent: 5h*

I had to do some big changes. First, the PCB — I wanted to add mounting holes since this time I put some tolerance (my dumbass didn't add them in the macropad project) which made the PCB bigger. Thankfully it's less than 100x100mm (still 4 layers, meaning it might cost a bit more), but my PCB will be held in place now.

For the case, I already did some stuff wrong so I had to change it, then I made it bigger because of the mounting holes. Hopefully it's good enough. I think I'll submit after this.

![](https://blueprint.hackclub.com/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6MTE5OTk4LCJwdXIiOiJibG9iX2lkIn19--1f267192f7ccbd65aec78048dce9e2e7003d74f1/image.png)
![](https://blueprint.hackclub.com/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6MTE5OTk5LCJwdXIiOiJibG9iX2lkIn19--346c9f69ee514f173ba4b0d87a3b53ec0720a9c8/image.png)

---

## 3/1/2026 — Finished the UI
*Time spent: 6.5h*

It took me so long to make the UI, partially because I didn't know what Lopaka is and making UI was hell, and also because I suck at UI design. I took around 3 hours for the no-Lopaka design (looks bad, I scrapped it) and like 3 or 4 hours to finish the Lopaka design that actually looks good.

![](https://blueprint.hackclub.com/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6MTEzNTE2LCJwdXIiOiJibG9iX2lkIn19--75586e98ab015da4c51aa6b25c20bfb9ce017f57/image.png)

---

## 3/1/2026 — Finished the code
*Time spent: 2h*

I finished the code using the PID method. Both methods are stored on my side and if something goes wrong I can just switch until I tweak the code.

Here's a thorough explanation of how the PID method works:

**Error** — calculated by subtracting the wanted temperature by the current temperature.
```
Error = wantedTemp - currentTemp
```

**Proportional** — the error value is multiplied by a `kP` value, tweaked to ensure it works properly. Default: `2`. The product is going to be a part of the PWM value.
```
PWM = kP * Error
```

**Integral** — default of `0`. Usually if the error value gets smaller, the less power goes into the iron, so it usually stays at 10–20 degrees celsius less than the wanted value. This is where the integral comes in — it keeps getting added so you can reach the temperature. Multiplied by `kI`, default: `0.5`.
```
integral = integral + error
```

**Derivative** — the integral and error together usually overshoot, and that's where the derivative comes in. Multiplied by `kD`, default: `0.1`.
```
derivative = error - lastError
```

**Final output** — all put into one float variable called `pid`, then constrained to an integer PWM value.
```
pid = (kP * Error) + (kI * integral) + (kD * derivative)
```
```cpp
int pwm = constrain((int)pid, 0, 255);
```

btw the OLED art will be different, I'm just doing this for it to be quick. Also it's late at night and I'm too lazy to draw.

![](https://blueprint.hackclub.com/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6MTEyOTg3LCJwdXIiOiJibG9iX2lkIn19--e758ab1e82d055043309111d1a3ea7dc0af06d36/image.png)
![](https://blueprint.hackclub.com/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6MTEyOTg4LCJwdXIiOiJibG9iX2lkIn19--1afceceaffe8e22e6bbb245a9486377be05057a4/image.png)
![](https://blueprint.hackclub.com/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6MTEyOTg5LCJwdXIiOiJibG9iX2lkIn19--20199c39dbabb7b8b0f75690a2c22e4861a3c78a/image.png)
![](https://blueprint.hackclub.com/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6MTEyOTkwLCJwdXIiOiJibG9iX2lkIn19--1b9bd8b9bb5018b73a96bd568c00cfbf7814b974/image.png)

---

## 2/28/2026 — Finished part of the code
*Time spent: 3h*

I finished the code, however I may redo it considering I found out about the PID method. I'm using the bang-bang method (don't judge me, that's what Claude said when I told him to evaluate my code) and supposedly PID is better, so I'll make 2 versions — one for PID and one for bang-bang.

![](https://blueprint.hackclub.com/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6MTEyOTE2LCJwdXIiOiJibG9iX2lkIn19--4846304f83db74e1bccd1b07c33775e60a50a166/image.png)
![](https://blueprint.hackclub.com/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6MTEyOTE3LCJwdXIiOiJibG9iX2lkIn19--eb4ad0ebbe9730d5bc2a1618ceaff6bad0d1a3f9/image.png)
![](https://blueprint.hackclub.com/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6MTEyOTE4LCJwdXIiOiJibG9iX2lkIn19--41772820407e7ab7633a90711aa81fa525a8a9f3/image.png)

---

## 2/28/2026 — Finished the case
*Time spent: 0.7h*

Also my first time creating a case without a guide. I'm not the best at 3D design — I'll be really surprised if this works. If this works I'll actually be really happy.

![](https://blueprint.hackclub.com/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6MTEyNzQ4LCJwdXIiOiJibG9iX2lkIn19--155f79a49bba198410b6325d83ab60a4d89e7911/image.png)

---

## 2/28/2026 — Finished a part of the case
*Time spent: 2h*

I finished quite a big part of the case. All I need left is to make the top part with the holes it needs — I've already done it but I need to double check. Also I removed the switch from the PCB because I used the wrong footprint and it was too big. I want to keep it at less than 100mm so the PCB price will be cheap.

![](https://blueprint.hackclub.com/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6MTEyNzQwLCJwdXIiOiJibG9iX2lkIn19--c9d4e1e05a00b342260d2bae73d667193ef09d56/image.png)
![](https://blueprint.hackclub.com/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6MTEyNzQxLCJwdXIiOiJibG9iX2lkIn19--a424d41e1dd561bb9b64df6ccb0acc142eddb3a4/image.png)
![](https://blueprint.hackclub.com/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6MTEyNzQyLCJwdXIiOiJibG9iX2lkIn19--b65a639f60883eb6101489927f6522fd174db0a5/image.png)

---

## 2/27/2026 — Just finished routing the PCB
*Time spent: 3.5h*

Turns out it had to be a 4-layer PCB. At least I have a ground plane now. I haven't actually done a PCB without a guide before but I did so much damn research on this one. I tried very hard so it can actually work.

![](https://blueprint.hackclub.com/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6MTEyNDYxLCJwdXIiOiJibG9iX2lkIn19--efb67beb46258581d246d4941259d61f1ddec282/image.png)
![](https://blueprint.hackclub.com/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6MTEyNDY4LCJwdXIiOiJibG9iX2lkIn19--df6a16625f9640801f5397a505bcc37f6fc12dd1/image.png)
