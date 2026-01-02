# KUKA_KRL_Advanced_Palletizer_PRO_Mode

KUKA KRL palletizing program (PRO mode) for educational and practical use.  
The project demonstrates a structured palletizing workflow with safety input handling, modular subroutines (pick / check / calculate / place), and grid-based pallet positioning.

## 🔧 Features

- Modular palletizing structure:
  - `PICK_PART()`
  - `CHECK_PART()`
  - `CALC_PALLET_POS()`
  - `PLACE_PART()`
  - `SAFE_STOP()`
- Grid calculation using `stepX` and `stepY`
- Safety monitoring using interrupt:
  - `$IN[2]` as `SAFETY_OK`
- Ready for PLC integration (inputs/outputs mapped via `SIGNAL`)
- Uses standard KUKA motion:
  - `PTP` (fast positioning)
  - `LIN` (linear moves for approach/pick/place)

## 🦾 Requirements

- Robot: KUKA (KRC2 / KRC4 compatible)
- Language: KRL (KUKA Robot Language)
- Correct tool/base setup in `.dat`
- Optional: PLC or simulated inputs for `PART_OK` and `SAFETY_OK`

## 📂 Project Structure

Advanced_Palletizer_PRO_Mode.src -> main program + subroutines
Advanced_Palletizer_PRO_Mode.dat -> tool/base + pick/place points + steps

## ⚙️ I/O Mapping

### Outputs
- `$OUT[1]` -> `GRIP_CLOSE`
- `$OUT[2]` -> `GRIP_OPEN`

### Inputs
- `$IN[1]` -> `PART_OK` (part detected / available)
- `$IN[2]` -> `SAFETY_OK` (safety chain OK / cell safe)

> If you do not have PLC connected, these inputs must be provided by simulation/IO forcing, otherwise the program will wait forever.

## 🧠 Pallet Grid Logic

The placement position is calculated from a start point (`P_PLACE_START`) and two step values (`stepX`, `stepY`):

- Column increases `X`
- Row increases `Y`

Formula inside `CALC_PALLET_POS()`:

P_CALC = P_PLACE_START
P_CALC.X = P_PLACE_START.X + (c - 1) * stepX
P_CALC.Y = P_PLACE_START.Y + (r - 1) * stepY

## 🧪 Testing Without PLC (Simulation)

If you test offline and do not have a real PLC:
- Force `$IN[2]` (SAFETY_OK) = TRUE
- Force `$IN[1]` (PART_OK) = TRUE

⚠️ Do this only in simulation/training. Never bypass safety in real production.

## ⚠️ Safety

This program includes an interrupt:
- If `SAFETY_OK` becomes FALSE, the robot executes `SAFE_STOP()`.

Always confirm:
- correct safety wiring
- correct tool/base
- correct approach heights (`+150mm`) for safe motion

## 👤 Author

Created by: **Lozan Ivanov**
