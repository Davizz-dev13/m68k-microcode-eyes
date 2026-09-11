# m68k-microcode-eyes

Custom Motorola 68000 microcode for the WepSIM educational processor, plus an
assembly program that draws animated eyes on a 24x8 LED matrix.

Academic project for Estructura de Computadores (Computer Architecture), UC3M.

## What it is

WepSIM lets you define your own instruction set by writing the microcode that
implements each instruction on its elementary processor. This project designs a
small, specialized 68000-style instruction set, hand-optimized for clock cycles,
and then uses it to run an animation (eyes that blink and look left and right)
on the simulator's LED matrix device.

The point: because the instructions do exactly what this application needs and
nothing more, the whole program runs in about **31% fewer clock cycles** than
the same program on the RISC-V reference firmware that ships with WepSIM.

## Files

- `microcode.txt` - the custom instruction set (exercise 1): `nop`, `stop`,
  `add`, `addi`, `lea`, `move`, `tst`, `jump`, `beq`, `bsr`, `rts`, `lw`/`move.w`
- `firmware.txt` - the full firmware (exercise 2): the instruction set above
  plus the `begin`/`fetch` microprogram
- `eyes.s` - the assembly program: 24x8 frames stored in `.data`, an output
  loop over the LED matrix ports, and the animation sequence
  (blink, look right, look left, blink)

## Cycle counts

Measured in WepSIM against the default RISC-V firmware running the same
program:

| Program     | RISC-V firmware | Custom firmware | Cycles saved |
|-------------|-----------------|-----------------|--------------|
| `show_eyes` | 757             | 497             | 34.4%        |
| `main`      | 7134            | 4942            | 30.7%        |

The RISC-V instructions are generic, so each one simulates in 3 or 4
micro-steps what the specialized instructions here do in 1. Fewer, denser
instructions win for a fixed embedded-style task like driving an LED matrix.

## Run it yourself

1. Open [WepSIM](https://wepsim.github.io/wepsim/) (processor: EP).
2. In **MicroCode**, paste `firmware.txt` and hit **ucompile**.
3. In **Assembly**, paste `eyes.s` and hit **Compile**.
4. In **Simulator**, open the right-panel dropdown and choose
   **Dev Led-Matrix**, then press **Run**.

The matrix shows the animation and the program finishes after about 4,942
clock cycles.
