---
layout: page
title: "An Intro to the FTC Strafer Drive Base"
permalink: /notes/ftc_strafer_intro/
---

These notes walk you through the FTC robot you're about to work on. By the end, you should be able to:

1. Name every major part of the robot and say what it does.
2. Explain how mecanum wheels let the robot slide sideways.
3. Check that all the wiring is correct **before** anyone turns anything on.
4. Pair the **Driver Hub** to the **Control Hub** and tell the software where the motors are plugged in.
5. Write a Blocks program that drives the robot, and then peek at what the same program looks like in Java.

Read along and try the things in **bold** as you come to them. If something doesn't match what you see on the actual robot, stop and ask — the robot in front of you is always the source of truth.

## Safety

These notes assume you're in a supervised lab — still, a handful of habits keep people and boards out of trouble:

1. **Keep the battery unplugged until "Hub Setup."** The pack is **12 volts** — not enough to cause a serious shock, but **<span style="color:#b91c1c;">reverse polarity</span>** (power wired the wrong way) *can* <span style="color:#b91c1c;">damage the Control Hub</span>. Leave the XT30 **<span style="color:#b45309;">disconnected</span>** until that section explicitly says to connect it.
2. **Match red and black every time.** Before you mate power, confirm **<span style="color:#dc2626;">red</span> to <span style="color:#dc2626;">red</span>** and **<span style="color:#1c1917;">black</span> to <span style="color:#1c1917;">black</span>.** The XT30 shape helps, but it is still possible to force a mistake — don't rush the check.
3. **<span style="color:#b45309;">Stay clear of pinch points and wheels</span>** when the robot might move. That means gears, shafts, belts, and mecanum rollers — and **long hair or loose sleeves** tied back or clear before you run an OpMode.
4. **Watch your fingers and toes.** FTC robots run on **much bigger motors** and **heavier drivetrains** than typical **FLL** bots. A stray hand near a spinning wheel — or a foot in the path when something drives sideways — **<span style="color:#b91c1c;">can bruise or crush</span>** badly enough to need attention. Assume the robot **<span style="color:#b91c1c;">can hurt you</span>**, not just the hardware.
5. **Turn power on only when everyone knows it's coming.** When you do reach Hub Setup, one person should **call it out** (this doc uses a **<span style="color:#0369a1;">battery captain</span>**) so nobody's fingers are in the drivetrain and no one is surprised by motion or noise.
6. **If something seems wrong, stop first.** <span style="color:#b45309;">Burning smell, smoke, crackling, or very hot connectors</span> → **<span style="color:#b91c1c;">main switch OFF</span>**, **<span style="color:#b91c1c;">unplug the battery</span>**, and get a mentor before trying again.

---

## 1. The Six Things on Every FTC Robot

Every FTC robot, no matter how fancy, is built out of the same six kinds of parts.

<svg viewBox="0 0 720 360" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="The six major components of an FTC robot, arranged around a central robot icon">
  <style>
    .box { fill: #ffffff; stroke: #2c3e50; stroke-width: 2; }
    .label { font-family: -apple-system, system-ui, sans-serif; font-size: 14px; font-weight: 600; fill: #2c3e50; }
    .sub { font-family: -apple-system, system-ui, sans-serif; font-size: 11px; fill: #555; }
    .center { fill: #fef3c7; stroke: #b45309; stroke-width: 2; }
    .conn { stroke: #94a3b8; stroke-width: 1.5; stroke-dasharray: 4 3; fill: none; }
  </style>
  <rect class="center" x="290" y="150" width="140" height="60" rx="8"/>
  <text class="label" x="360" y="175" text-anchor="middle">FTC Robot</text>
  <text class="sub" x="360" y="195" text-anchor="middle">(the whole thing)</text>
  <rect class="box" x="20"  y="20"  width="180" height="60" rx="6"/>
  <text class="label" x="110" y="42" text-anchor="middle">1. Drive Base</text>
  <text class="sub" x="110" y="62" text-anchor="middle">Frame, motors, wheels</text>
  <rect class="box" x="520" y="20"  width="180" height="60" rx="6"/>
  <text class="label" x="610" y="42" text-anchor="middle">2. Control Hub</text>
  <text class="sub" x="610" y="62" text-anchor="middle">The robot's brain</text>
  <rect class="box" x="20"  y="150" width="180" height="60" rx="6"/>
  <text class="label" x="110" y="172" text-anchor="middle">3. Battery</text>
  <text class="sub" x="110" y="192" text-anchor="middle">12V SLA, the energy</text>
  <rect class="box" x="520" y="150" width="180" height="60" rx="6"/>
  <text class="label" x="610" y="172" text-anchor="middle">4. Driver Hub</text>
  <text class="sub" x="610" y="192" text-anchor="middle">Controller + screen</text>
  <rect class="box" x="20"  y="280" width="180" height="60" rx="6"/>
  <text class="label" x="110" y="302" text-anchor="middle">5. Mechanism</text>
  <text class="sub" x="110" y="322" text-anchor="middle">Arm, lift, claw, etc.</text>
  <rect class="box" x="520" y="280" width="180" height="60" rx="6"/>
  <text class="label" x="610" y="302" text-anchor="middle">6. Sensors</text>
  <text class="sub" x="610" y="322" text-anchor="middle">IMU, distance, color…</text>
  <path class="conn" d="M200 50  Q 245 100 290 165"/>
  <path class="conn" d="M520 50  Q 475 100 430 165"/>
  <path class="conn" d="M200 180 L 290 180"/>
  <path class="conn" d="M520 180 L 430 180"/>
  <path class="conn" d="M200 310 Q 245 260 290 200"/>
  <path class="conn" d="M520 310 Q 475 260 430 200"/>
</svg>

Find each one on your robot:

- **Drive Base** — the rectangular frame with four wheels. Yours is the goBILDA Strafer with 104mm GripForce mecanums.
- **Control Hub** — the black box with all the wires plugged into it. There's a tiny Android computer inside, and it has WiFi.
- **Battery** — the heavy black brick. **12 volts.** It's the only part of the robot that actually *stores* energy. Everything else just moves it around.
- **Driver Hub** — the gray slab with a screen and a couple of gamepad ports. You'll use it to talk to the robot.
- **Mechanism** — whatever does the game-specific job, like an arm or a lift. **Your robot doesn't have one yet** — today the drive base *is* the whole robot.
- **Sensors** — the Control Hub already has an **IMU** (inertial measurement unit) inside, which can tell you which way the robot is facing. You'll meet other sensors later.

**Try this:** Walk around the robot and point at each of the six things. If you can't find one, look at the photos linked below — then come back and find it on the real robot.

### Reference photos

- Strafer Chassis (full build, photos & video): [goBILDA — Strafer Chassis Kit (104mm GripForce)](https://www.gobilda.com/strafer-chassis-kit-104mm-gripforce-mecanum-wheels/)
- Control Hub (photos, port labels): [REV — Control Hub](https://www.revrobotics.com/rev-31-1595/)
- Driver Hub: [REV — Driver Hub](https://www.revrobotics.com/rev-31-1596/)
- 12V slim battery: [REV — 12V Slim Battery](https://www.revrobotics.com/rev-31-1302/)

---

## 2. The Strafer Drive Base

Mecanum wheels are what make this drive base special. With four normal wheels, a robot can drive forward and backward and turn — but it can't slide sideways. Mecanum wheels let it.

### What's actually in the chassis

- **4 × goBILDA 5203 series 312 RPM Yellow Jacket motors** — one per wheel. Each one has an encoder built in, which counts how far the wheel has spun. *(Older Strafer kits — V2 through V4 — used the 5202 series motor instead. Either way you'll pick `goBILDA 5203 series` in the configuration step later.)*
- **4 × 104 mm goBILDA GripForce mecanum wheels** — two "left" wheels and two "right" wheels. They are *not* identical — the rollers around the rim tilt opposite ways.
- An **aluminum frame** built from goBILDA's U-channel system.
- **Miter gears + hub-shafts on ball bearings** — the motors live *parallel* to the wheels and turn the wheels through a 1:1 90° gearset. That's why the chassis is so compact.

If you have a spare mecanum wheel nearby, **spin one of the rollers with a finger**. Notice that the roller spins really easily — that's the trick. Each roller can roll freely along its own axis, while the whole wheel can also spin like a normal wheel.

### Why mecanums can strafe

A normal wheel can only push the robot in the direction it's pointing. A mecanum wheel still rolls forward like a normal wheel, **but** because each roller is angled, the wheel also tries to push the robot diagonally. By spinning the four wheels in different directions, the diagonal forces add up to whichever direction you want.

The four wheels are mounted in an **"X" pattern** when you look down on the robot from above. The rollers on the front-left and back-right wheels tilt one way, and the rollers on the front-right and back-left tilt the other way.

<svg viewBox="0 0 600 420" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="Top-down view of the Strafer drive base showing the X-pattern roller orientation of mecanum wheels">
  <style>
    .frame { fill: #f5f5f5; stroke: #333; stroke-width: 2; }
    .wheel { fill: #1e293b; }
    .roll  { stroke: #f59e0b; stroke-width: 3; stroke-linecap: round; }
    .lbl { font: 600 13px -apple-system, system-ui, sans-serif; fill: #111; }
    .ttl { font: 700 16px -apple-system, system-ui, sans-serif; fill: #111; }
    .ax  { stroke: #2563eb; stroke-width: 2; fill: none; marker-end: url(#arr); }
    .axlbl { font: 700 12px -apple-system, system-ui, sans-serif; fill: #2563eb; }
  </style>
  <defs>
    <marker id="arr" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="6" markerHeight="6" orient="auto">
      <path d="M0,0 L10,5 L0,10 z" fill="#2563eb"/>
    </marker>
  </defs>
  <text class="ttl" x="300" y="28" text-anchor="middle">Strafer drive base — top-down view</text>
  <rect class="frame" x="160" y="80" width="280" height="280" rx="6"/>
  <text class="lbl" x="300" y="225" text-anchor="middle">FRONT ↑</text>
  <rect class="wheel" x="100" y="90"  width="60" height="80" rx="4"/>
  <line class="roll" x1="106" y1="100" x2="154" y2="160"/>
  <line class="roll" x1="106" y1="125" x2="154" y2="165" opacity="0.55"/>
  <line class="roll" x1="106" y1="140" x2="140" y2="165" opacity="0.35"/>
  <text class="lbl" x="65"  y="135" text-anchor="middle">FL</text>
  <rect class="wheel" x="440" y="90"  width="60" height="80" rx="4"/>
  <line class="roll" x1="494" y1="100" x2="446" y2="160"/>
  <line class="roll" x1="494" y1="125" x2="446" y2="165" opacity="0.55"/>
  <line class="roll" x1="494" y1="140" x2="460" y2="165" opacity="0.35"/>
  <text class="lbl" x="535" y="135" text-anchor="middle">FR</text>
  <rect class="wheel" x="100" y="270" width="60" height="80" rx="4"/>
  <line class="roll" x1="106" y1="340" x2="154" y2="280"/>
  <line class="roll" x1="106" y1="315" x2="154" y2="275" opacity="0.55"/>
  <line class="roll" x1="106" y1="300" x2="140" y2="275" opacity="0.35"/>
  <text class="lbl" x="65"  y="315" text-anchor="middle">BL</text>
  <rect class="wheel" x="440" y="270" width="60" height="80" rx="4"/>
  <line class="roll" x1="494" y1="340" x2="446" y2="280"/>
  <line class="roll" x1="494" y1="315" x2="446" y2="275" opacity="0.55"/>
  <line class="roll" x1="494" y1="300" x2="460" y2="275" opacity="0.35"/>
  <text class="lbl" x="535" y="315" text-anchor="middle">BR</text>
  <line class="ax" x1="300" y1="395" x2="300" y2="370"/>
  <text class="axlbl" x="312" y="392">forward</text>
</svg>

The orange lines show the angle of the rollers on each wheel. Notice that the rollers form a giant **X** when you look at the whole robot.

**Check this on your robot.** Look down at all four wheels. Do the rollers form an X? If they don't, two wheels are on the wrong corners — the robot will not strafe correctly. (It's a common mistake: it's easy to swap a left wheel and a right wheel without noticing.)

### What happens when wheels spin different directions

| All four wheels spin… | Robot moves… |
|---|---|
| forward (top of wheel goes away from you) | **forward** |
| backward | **backward** |
| FL & BR forward, FR & BL backward | **strafe right** |
| FL & BR backward, FR & BL forward | **strafe left** |
| Right side forward, left side backward | **rotate right (CW)** |
| Right side backward, left side forward | **rotate left (CCW)** |

**Try this:** Read each row out loud, then **push the robot by hand** to test it. (Battery still disconnected!) Pretend each wheel is spinning the way the row says, and predict which way the robot would go. Then try the next row.

---

## 3. Wiring Verification

Before anyone touches the power switch, you and a partner are going to verify the wiring end-to-end. The rule for this whole section: **the battery stays disconnected.** The XT30 connector stays unmated. No exceptions.

### The Control Hub port map

<svg viewBox="0 0 740 318" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="Diagram of REV Control Hub port locations: motors 0-3, servos 0-5, USB, sensor ports, and XT30 power" style="display:block;margin-top:0.5rem;max-width:100%;height:auto">
  <style>
    .hub { fill: #111; stroke: #444; stroke-width: 2; }
    .port { fill: #1f2937; stroke: #6b7280; stroke-width: 1; }
    .pwr { fill: #b91c1c; stroke: #7f1d1d; stroke-width: 1; }
    .usb { fill: #1d4ed8; stroke: #1e3a8a; stroke-width: 1; }
    .lbl { font: 600 11px -apple-system, system-ui, sans-serif; fill: #fff; }
    .lbl-out { font: 600 12px -apple-system, system-ui, sans-serif; fill: #111; }
    .ttl { font: 700 16px -apple-system, system-ui, sans-serif; fill: #111; }
    .group { font: 700 12px -apple-system, system-ui, sans-serif; fill: #444; }
  </style>
  <text class="ttl" x="370" y="24" text-anchor="middle">REV Control Hub — simplified port map</text>
  <rect class="hub" x="60" y="60" width="620" height="200" rx="10"/>
  <text class="group" x="80" y="85">MOTORS (0 – 3)</text>
  <g>
    <rect class="port" x="80"  y="95" width="50" height="32" rx="3"/><text class="lbl" x="105" y="116" text-anchor="middle">M0</text>
    <rect class="port" x="138" y="95" width="50" height="32" rx="3"/><text class="lbl" x="163" y="116" text-anchor="middle">M1</text>
    <rect class="port" x="196" y="95" width="50" height="32" rx="3"/><text class="lbl" x="221" y="116" text-anchor="middle">M2</text>
    <rect class="port" x="254" y="95" width="50" height="32" rx="3"/><text class="lbl" x="279" y="116" text-anchor="middle">M3</text>
  </g>
  <text class="group" x="330" y="85">SERVOS (0 – 5)</text>
  <g>
    <rect class="port" x="330" y="95" width="38" height="32" rx="3"/><text class="lbl" x="349" y="116" text-anchor="middle">S0</text>
    <rect class="port" x="374" y="95" width="38" height="32" rx="3"/><text class="lbl" x="393" y="116" text-anchor="middle">S1</text>
    <rect class="port" x="418" y="95" width="38" height="32" rx="3"/><text class="lbl" x="437" y="116" text-anchor="middle">S2</text>
    <rect class="port" x="462" y="95" width="38" height="32" rx="3"/><text class="lbl" x="481" y="116" text-anchor="middle">S3</text>
    <rect class="port" x="506" y="95" width="38" height="32" rx="3"/><text class="lbl" x="525" y="116" text-anchor="middle">S4</text>
    <rect class="port" x="550" y="95" width="38" height="32" rx="3"/><text class="lbl" x="569" y="116" text-anchor="middle">S5</text>
  </g>
  <rect class="usb" x="610" y="95" width="58" height="32" rx="3"/><text class="lbl" x="639" y="116" text-anchor="middle">USB</text>
  <text class="group" x="80" y="165">SENSORS  (I²C · digital · analog)</text>
  <g>
    <rect class="port" x="80"  y="175" width="44" height="30" rx="3"/><text class="lbl" x="102" y="195" text-anchor="middle">I²C 0</text>
    <rect class="port" x="130" y="175" width="44" height="30" rx="3"/><text class="lbl" x="152" y="195" text-anchor="middle">I²C 1</text>
    <rect class="port" x="180" y="175" width="44" height="30" rx="3"/><text class="lbl" x="202" y="195" text-anchor="middle">I²C 2</text>
    <rect class="port" x="230" y="175" width="44" height="30" rx="3"/><text class="lbl" x="252" y="195" text-anchor="middle">I²C 3</text>
    <rect class="port" x="80"  y="212" width="50" height="30" rx="3"/><text class="lbl" x="105" y="232" text-anchor="middle">D 0/1</text>
    <rect class="port" x="135" y="212" width="50" height="30" rx="3"/><text class="lbl" x="160" y="232" text-anchor="middle">D 2/3</text>
    <rect class="port" x="190" y="212" width="50" height="30" rx="3"/><text class="lbl" x="215" y="232" text-anchor="middle">D 4/5</text>
    <rect class="port" x="245" y="212" width="50" height="30" rx="3"/><text class="lbl" x="270" y="232" text-anchor="middle">D 6/7</text>
    <rect class="port" x="300" y="212" width="50" height="30" rx="3"/><text class="lbl" x="325" y="232" text-anchor="middle">A 0/1</text>
    <rect class="port" x="355" y="212" width="50" height="30" rx="3"/><text class="lbl" x="380" y="232" text-anchor="middle">A 2/3</text>
  </g>
  <text class="group" x="450" y="165">POWER (XT30)</text>
  <rect class="pwr" x="450" y="175" width="100" height="32" rx="3"/><text class="lbl" x="500" y="196" text-anchor="middle">XT30 IN ⚡</text>
  <rect class="pwr" x="558" y="175" width="100" height="32" rx="3"/><text class="lbl" x="608" y="196" text-anchor="middle">XT30 OUT</text>
  <text class="lbl-out" x="80" y="291">D = digital (4 ports, 8 channels) · A = analog (2 ports, 4 channels) · ⚠ male XT30 = battery in</text>
</svg>

The actual Control Hub may have its ports arranged a little differently than the diagram above — what matters is the **labels** on the connectors. Find each labeled port on the real hub.

### How the motors are plugged in

For your Strafer drive base with one Control Hub, each motor goes to a specific port:

| Port | What plugs in       |
|------|---------------------|
| M0   | **Front-Left** motor  |
| M1   | **Front-Right** motor |
| M2   | **Back-Left** motor   |
| M3   | **Back-Right** motor  |
| XT30 IN | The battery (red wire to red, black to black) |

### The Wiring Checklist

Find a partner. One of you reads, the other points at the wire and **traces it with a finger from one end to the other**. Then swap. Don't skip the tracing — that's the part that catches mistakes.

- [ ] Battery is **disconnected** (XT30 not mated). Confirm by sight, not by feel.
- [ ] Main power switch is in the **OFF** position.
- [ ] Battery cable runs from the battery → main switch → Control Hub **XT30 IN** (the *male* connector on the hub).
- [ ] All four motor cables are seated firmly in M0–M3. Give each a gentle tug — no wiggle.
- [ ] Each motor cable goes to the *correct* wheel per the table above. **Trace the wire.**
- [ ] Each motor's encoder cable (the smaller 4-pin one) is plugged in next to the matching motor port.
- [ ] No bare wire is visible at any connector.
- [ ] No wire is pinched between two pieces of metal.
- [ ] No wire is in the path of a wheel or a roller.
- [ ] Driver Hub is charged (or plugged into its USB-C charger).

**One thing to be paranoid about:** *reverse polarity on the battery.* If you plug the red wire into the black side and the black into the red, you can damage the Control Hub. The XT30 is shaped to make this hard, but not impossible. **Always confirm red-to-red, black-to-black before mating the connector.**

---

## 4. Hub Setup

Now you can power on. Pick someone in your group to be the **battery captain** — they're in charge of saying "battery!" before anyone reaches for the switch.

### Step 1: Power on, carefully

1. Battery captain announces: "battery on in 3, 2, 1."
2. Mate the XT30 connector (red to red).
3. Flip the main switch to **ON**.
4. Watch the Control Hub LED. It will blink for about 30 seconds while the Android computer boots, then settle into a steady pattern.

### Step 2: Pair the Driver Hub

The Control Hub creates its own WiFi network. Your job is to connect the Driver Hub to it.

1. Power on the Driver Hub.
2. Tap the **3-dot menu** → **Settings** → **Pair with Robot Controller**.
3. Tap **WiFi Settings**.
4. Find the network whose name starts with `FIRST-` or `FTC-` followed by your hub's nickname.
5. The default password is `password` (your team will change this in a later lesson).
6. Tap **Connect**.
7. Back out to the main screen. You should see a green "Connected" bar with **ping time** and **battery voltage**.

**Battery voltage should read between 12.0 V and 13.0 V on a fresh charge.** If it reads below 11.0 V, stop and swap to a freshly-charged battery — a tired battery causes weird, hard-to-debug behavior later.

### Step 3: Build the robot configuration

The configuration is a list that says, *"this name in code is the same as that physical port on the hub."* The Control Hub doesn't figure this out by itself — you have to tell it.

1. On the Driver Hub: **3-dot menu** → **Configure Robot**.
2. Tap **New** to create a configuration. Name it `StraferBase`.
3. Tap the entry for the Control Hub.
4. Tap **Motors**. You'll see ports 0–3.
5. For each port, set the **type** to `goBILDA 5203 series` and the **name** to:
   - Port 0 → `frontLeft`
   - Port 1 → `frontRight`
   - Port 2 → `backLeft`
   - Port 3 → `backRight`
6. **Done** → **Done** → **Save**.

The configuration is now active. The names `frontLeft`, `frontRight`, `backLeft`, `backRight` are how you'll refer to the motors in your program.

**Naming matters.** The names in the configuration must *exactly* match the names in the code, including capitalization. `frontLeft` is **not** the same as `FrontLeft` or `front_left`. Computers are very picky about this.

---

## 5. Your First Program (Blocks)

You're going to write your program in **OnBot Java's Blocks editor**, which runs in a web browser. The Driver Hub serves the editor over WiFi.

### Open the Blocks editor

1. On your laptop or tablet, connect to the Control Hub's WiFi network — the same one the Driver Hub is on.
2. In a browser, go to `http://192.168.43.1:8080`.
3. Tap **Blocks**.
4. Tap **Create New OpMode**.
5. Name it `StraferTeleop`. Choose template **BasicOpMode_Linear**.

### Warm-up: drive forward for 2 seconds

Before you build anything fancy, write a tiny program just to confirm everything is working. The goal is simple: when you press start, all four wheels spin forward for 2 seconds, and then the robot stops.

The Blocks structure looks like this:

<svg viewBox="0 0 820 568" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="Mock-up of the Blocks program in the FTC OnBot Java Blocks editor: when the OpMode runs, set telemetry status to ready, update telemetry, wait for start, set each of the four mecanum motors to power 0.5, sleep for 2000 milliseconds, then set all four motors back to power 0">
<style>
  .ttl  { font: 700 16px -apple-system, system-ui, sans-serif; fill: #111; }
  .blk-evt { fill: #F4B622; stroke: #B0860F; stroke-width: 1.5; }
  .blk-tel { fill: #5BA55B; stroke: #3F7B3F; stroke-width: 1.5; }
  .blk-mot { fill: #BD42BD; stroke: #8B2F8B; stroke-width: 1.5; }
  .lbl-w { font: 600 14px -apple-system, system-ui, sans-serif; fill: #ffffff; }
  .num-w { font: 600 12px Menlo, Consolas, monospace; fill: #ffffff; }
  .pill-bg { fill: rgba(0,0,0,0.22); stroke: rgba(0,0,0,0.35); stroke-width: 1; }
  .annot { font: italic 12px -apple-system, system-ui, sans-serif; fill: #666; }
  .group-bracket { fill: none; stroke: #aaa; stroke-width: 1; stroke-dasharray: 3 3; }
  .group-lbl { font: italic 12px -apple-system, system-ui, sans-serif; fill: #666; }
</style>
<text class="ttl" x="410.0" y="24" text-anchor="middle">Blocks: drive forward for 2 seconds, then stop</text>
<path class="blk-evt" d="M 90,54 Q 90,44 100,44 H 680 Q 690,44 690,54 V 86 H 138 l -4,5 l -8,0 l -4,-5 H 90 Z"/>
<text class="lbl-w" x="108" y="70">when run OpMode</text>
<path class="blk-tel" d="M 90,86 H 122 l 4,5 l 8,0 l 4,-5 H 690 V 122 H 138 l -4,5 l -8,0 l -4,-5 H 90 Z"/>
<text class="lbl-w" x="108" y="108">telemetry · addData</text>
<rect class="pill-bg" x="296" y="94" width="78" height="20" rx="10"/><text class="num-w" x="335.0" y="108" text-anchor="middle">&quot;status&quot;</text>
<rect class="pill-bg" x="396" y="94" width="76" height="20" rx="10"/><text class="num-w" x="434.0" y="108" text-anchor="middle">&quot;ready&quot;</text>
<path class="blk-tel" d="M 90,122 H 122 l 4,5 l 8,0 l 4,-5 H 690 V 158 H 138 l -4,5 l -8,0 l -4,-5 H 90 Z"/>
<text class="lbl-w" x="108" y="144">telemetry · update</text>
<path class="blk-evt" d="M 90,158 H 122 l 4,5 l 8,0 l 4,-5 H 690 V 194 H 138 l -4,5 l -8,0 l -4,-5 H 90 Z"/>
<text class="lbl-w" x="108" y="180">waitForStart</text>
<path class="blk-mot" d="M 90,194 H 122 l 4,5 l 8,0 l 4,-5 H 690 V 230 H 138 l -4,5 l -8,0 l -4,-5 H 90 Z"/>
<text class="lbl-w" x="108" y="216">set frontLeft · power to</text>
<rect class="pill-bg" x="310" y="202" width="50" height="20" rx="10"/><text class="num-w" x="335.0" y="216" text-anchor="middle">0.5</text>
<path class="blk-mot" d="M 90,230 H 122 l 4,5 l 8,0 l 4,-5 H 690 V 266 H 138 l -4,5 l -8,0 l -4,-5 H 90 Z"/>
<text class="lbl-w" x="108" y="252">set frontRight · power to</text>
<rect class="pill-bg" x="310" y="238" width="50" height="20" rx="10"/><text class="num-w" x="335.0" y="252" text-anchor="middle">0.5</text>
<path class="blk-mot" d="M 90,266 H 122 l 4,5 l 8,0 l 4,-5 H 690 V 302 H 138 l -4,5 l -8,0 l -4,-5 H 90 Z"/>
<text class="lbl-w" x="108" y="288">set backLeft · power to</text>
<rect class="pill-bg" x="310" y="274" width="50" height="20" rx="10"/><text class="num-w" x="335.0" y="288" text-anchor="middle">0.5</text>
<path class="blk-mot" d="M 90,302 H 122 l 4,5 l 8,0 l 4,-5 H 690 V 338 H 138 l -4,5 l -8,0 l -4,-5 H 90 Z"/>
<text class="lbl-w" x="108" y="324">set backRight · power to</text>
<rect class="pill-bg" x="310" y="310" width="50" height="20" rx="10"/><text class="num-w" x="335.0" y="324" text-anchor="middle">0.5</text>
<path class="blk-evt" d="M 90,338 H 122 l 4,5 l 8,0 l 4,-5 H 690 V 374 H 138 l -4,5 l -8,0 l -4,-5 H 90 Z"/>
<text class="lbl-w" x="108" y="360">sleep ·</text>
<rect class="pill-bg" x="160" y="346" width="60" height="20" rx="10"/><text class="num-w" x="190.0" y="360" text-anchor="middle">2000</text>
<text class="lbl-w" x="232" y="360">milliseconds</text>
<path class="blk-mot" d="M 90,374 H 122 l 4,5 l 8,0 l 4,-5 H 690 V 410 H 138 l -4,5 l -8,0 l -4,-5 H 90 Z"/>
<text class="lbl-w" x="108" y="396">set frontLeft · power to</text>
<rect class="pill-bg" x="310" y="382" width="38" height="20" rx="10"/><text class="num-w" x="329.0" y="396" text-anchor="middle">0</text>
<path class="blk-mot" d="M 90,410 H 122 l 4,5 l 8,0 l 4,-5 H 690 V 446 H 138 l -4,5 l -8,0 l -4,-5 H 90 Z"/>
<text class="lbl-w" x="108" y="432">set frontRight · power to</text>
<rect class="pill-bg" x="310" y="418" width="38" height="20" rx="10"/><text class="num-w" x="329.0" y="432" text-anchor="middle">0</text>
<path class="blk-mot" d="M 90,446 H 122 l 4,5 l 8,0 l 4,-5 H 690 V 482 H 138 l -4,5 l -8,0 l -4,-5 H 90 Z"/>
<text class="lbl-w" x="108" y="468">set backLeft · power to</text>
<rect class="pill-bg" x="310" y="454" width="38" height="20" rx="10"/><text class="num-w" x="329.0" y="468" text-anchor="middle">0</text>
<path class="blk-mot" d="M 90,482 H 122 l 4,5 l 8,0 l 4,-5 H 690 V 518 H 90 Z"/>
<text class="lbl-w" x="108" y="504">set backRight · power to</text>
<rect class="pill-bg" x="310" y="490" width="38" height="20" rx="10"/><text class="num-w" x="329.0" y="504" text-anchor="middle">0</text>
<path class="group-bracket" d="M 700,194 V 338 M 700,194 H 706 M 700,338 H 706"/>
<text class="group-lbl" x="712" y="270.0">drive forward</text>
<path class="group-bracket" d="M 700,374 V 518 M 700,374 H 706 M 700,518 H 706"/>
<text class="group-lbl" x="712" y="450.0">all stop</text>
<text class="annot" x="410.0" y="556" text-anchor="middle">Drag each block from the menu on the left and stack them in this order. Save the OpMode and run.</text>
</svg>

**Build it block-by-block.** Drag each block from the menu on the left into the editor, in the order shown. Save your OpMode.

**Run it for the first time with the robot up on a stand**, so the wheels spin freely. (Books or a small box work fine.) All four wheels should spin in the same direction. If one or more wheels spin the wrong way, that's expected — fix it in the next step.

### Reverse the wheels that spin the wrong way

The right-side motors are mirrored on the chassis, so they actually need to be told to run "backwards" to make all four wheels push the robot forward. You don't fix this by rewiring — you fix it in code, with the **set direction** block:

- `set frontRight · direction to REVERSE`
- `set backRight · direction to REVERSE`

Add those blocks near the top of your program, before the powers are set. Re-run. Now all four wheels should drive the robot forward when you put it on the ground.

### Add the gamepad — full mecanum teleop

Now make the robot listen to the joystick. The classic mecanum mixing formula is:

```
y  = -gamepad1.left_stick_y      // forward / back  (we negate because the stick reads -1 when pushed up)
x  =  gamepad1.left_stick_x      // strafe left / right
rx =  gamepad1.right_stick_x     // rotate

frontLeft  = y + x + rx
frontRight = y - x - rx
backLeft   = y - x + rx
backRight  = y + x - rx
```

In Blocks, you'll wrap the program in a `while opModeIsActive()` loop containing:

1. Read the three gamepad values into variables `y`, `x`, and `rx`.
2. Compute each motor's power using the four formulas above.
3. Set each motor's power.
4. Update telemetry so the Driver Hub shows `y`, `x`, and `rx` as numbers — that way you can see what the joysticks are reporting.

**Why these formulas work.** Each motor "votes" on what to do. *Forward* power says "all four spin forward." *Strafe-right* power says "FL and BR forward, FR and BL backward." *Rotate-right* power says "left side forward, right side backward." When you add the votes together, every motor does exactly the right thing for any combination of stick inputs.

### Test plan

Try these in order. **For the first three, keep the robot up on a stand** so the wheels spin freely.

1. Push the left stick **forward** → all four wheels spin forward.
2. Push the left stick **right** → wheels make the strafe-right pattern (FL and BR forward, FR and BL backward).
3. Push the right stick **right** → robot tries to rotate clockwise (left side forward, right side backward).
4. *Now* put the robot on the ground. Drive a square: forward, strafe right, backward, strafe left, back to start.
5. Try rotating in place with the right stick.

---

## 6. Peek at the OnBot Java

In the Blocks editor, click **"Show Java"** in the top-right. You'll see the same program written out in Java text. Don't panic — it's the same blocks, just in a different shape.

```java
public class StraferTeleop extends LinearOpMode {

    @Override
    public void runOpMode() {
        DcMotor frontLeft  = hardwareMap.dcMotor.get("frontLeft");
        DcMotor frontRight = hardwareMap.dcMotor.get("frontRight");
        DcMotor backLeft   = hardwareMap.dcMotor.get("backLeft");
        DcMotor backRight  = hardwareMap.dcMotor.get("backRight");

        frontRight.setDirection(DcMotor.Direction.REVERSE);
        backRight.setDirection(DcMotor.Direction.REVERSE);

        telemetry.addData("status", "ready");
        telemetry.update();
        waitForStart();

        while (opModeIsActive()) {
            double y  = -gamepad1.left_stick_y;
            double x  =  gamepad1.left_stick_x;
            double rx =  gamepad1.right_stick_x;

            frontLeft.setPower(y + x + rx);
            frontRight.setPower(y - x - rx);
            backLeft.setPower(y - x + rx);
            backRight.setPower(y + x - rx);

            telemetry.addData("y", y);
            telemetry.addData("x", x);
            telemetry.addData("rx", rx);
            telemetry.update();
        }
    }
}
```

**Try this:** Match each block from the diagram in section 5 to a line of Java. You'll see that "set frontLeft power" is the same as `frontLeft.setPower(...)`. Same idea, different keyboard.

---

## 7. Powering Down

When you're done, shut things off in this order:

1. Stop the OpMode in the Driver Hub.
2. Flip the main switch to **OFF**.
3. The battery captain disconnects the XT30 connector.
4. Put the battery back on the charger.

---

## If something weird happens

**A wheel spins backward.** It's almost always a missing or extra `setDirection(REVERSE)`. Check that the *right-side* motors are reversed.

**The robot strafes backward when you push the stick right.** Two motor cables are probably swapped between the left and right sides. Power off and re-trace the wires.

**The robot rotates instead of going forward.** One side's motors are reversed (or both right-side motors aren't reversed). Power off and re-check the `setDirection` calls.

**The Driver Hub can't find the Control Hub WiFi.** Power-cycle the Control Hub. Wait the full 30 seconds for Android to boot before looking again.

**The battery voltage is below 11 V.** Swap to a fresh battery. Tired batteries cause every kind of weirdness — motors stuttering, configurations losing changes, the Driver Hub disconnecting. **When something is mysterious, suspect the battery first.**

---

## Want to learn more?

The official references behind these notes:

- [goBILDA — Strafer Chassis Kit (104mm GripForce)](https://www.gobilda.com/strafer-chassis-kit-104mm-gripforce-mecanum-wheels/) — product page, photos, specs.
- [goBILDA — 104mm Strafer Assembly Instructions (PDF)](https://www.gobilda.com/content/user_manuals/3209-0001-0007_assembly-instructions.min.pdf) — full mechanical build steps and parts list.
- [REV — Control Hub User's Manual (PDF)](https://revrobotics.ca/content/docs/REV-31-1595-UM.pdf) — official port descriptions and power notes.
- [FTC Docs — Control Hub Ports](https://ftc-docs.firstinspires.org/en/latest/control_hard_compon/rc_components/hub/ports/ch-ports.html) — authoritative port reference.
- [FTC Docs — Configuring Your Android Devices](https://ftc-docs.firstinspires.org/programming_resources/shared/configuring_android/Configuring-Your-Android-Devices.html) — full pairing walkthrough.
- [FIRST — Robot Wiring Guide (PDF)](https://www.firstinspires.org/sites/default/files/uploads/resource_library/ftc/robot-wiring-guide.pdf) — wiring best practices.
- [Game Manual 0 — Wiring](https://gm0.org/en/latest/docs/power-and-electronics/wiring.html) — community wiring reference.
- [Game Manual 0 — Mecanum TeleOp](https://gm0.org/en/latest/docs/software/tutorials/mecanum-drive.html) — mecanum mixing formula and code samples.
