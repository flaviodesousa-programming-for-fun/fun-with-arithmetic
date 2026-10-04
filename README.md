# Fun with Arithmetic, the 1980s Way

An implementation of Stack Overflow's **Fun with Arithmetic** challenge in **TRS-80 Model III ROM BASIC**.

The arithmetic looked more like a chore than a challenge, so I added an unnecessary constraint: use the technology I worked with as a teenager in my first job. This repository records the resulting experiment, including the awkward bits and wrong jumps.

Read the story: **[Fun with Arithmetic, the 1980s Way](https://web.archive.org/web/20261004/https://flaviodesousa.com/fun-with-arithmetic-the-1980s-way/)**.

## The challenge

Given a list of positive integers, calculate:

1. **Mean**, rounded to the nearest whole integer, with halfway values rounded upward.
2. **Median**, using the rounded average of the two middle values when the list has an even number of elements.
3. **Mode**, with a special rule: if several values share the highest frequency, return their rounded average. If no values repeat, the challenge permits zero.
4. **Odd digit count**, the total occurrences of `1`, `3`, `5`, `7`, and `9` across all input values.
5. **Even digit count**, the total occurrences of `0`, `2`, `4`, `6`, and `8`.

Concatenate those five results in that order to form the final number. The digit counts concern individual decimal digits, not the parity of each input value.

See the [challenge and my contribution](https://web.archive.org/web/20261004/https://stackoverflow.com/beta/challenges/80003558/80007730) for the full specification.

## Run it in your browser

The program and its input data are contained in [`SOCHAL23.BAS`](https://web.archive.org/web/20261004/https://github.com/flaviodesousa-programming-for-fun/fun-with-arithmetic/blob/main/SOCHAL23.BAS). No separate input file is needed.

1. Open `SOCHAL23.BAS` on GitHub and use **Copy raw file**, or open **Raw** and copy the complete listing. Include all the numbered `DATA` lines.
2. Open the [TRS-80 Model III emulator](https://web.archive.org/web/20261004/https://trs80emu.netlify.app/).
3. Focus the emulator's keyboard input. Press **Enter** at the `Cass?` and `Memory Size?` prompts to reach `READY`.
4. If another BASIC program is already loaded, enter `NEW` and press **Enter** to clear it.
5. Open **MACHINE → Paste BASIC from clipboard**. Allow clipboard access if your browser requests it. Wait for the simulated typing to finish before entering another command.
6. Enter `RUN` and press **Enter**.

The emulator also offers **Turbo** to accelerate execution.

### Expected output

```text
FINAL NUMBER = 3251179966045634169
```

BASIC returns to `READY` afterward. The value above is the result reported in my challenge contribution and shown in the successful-run screenshot.

If you see a syntax error or an unexpected result, first check that the complete listing was entered and that no lines from a previous program remain. This listing targets TRS-80 BASIC; another BASIC dialect may behave differently.

## How it works

| Lines | Purpose |
| --- | --- |
| `10–15` | Set integer defaults and allocate the input array. |
| `20–50` | Read the data, sort it, calculate the results, and print the output. |
| `100–140` | Read `DATA` values until an error ends the reading loop. |
| `200–299` | Sort the array using Shell sort. |
| `300` | Calculate the median from the sorted array. |
| `310–400` | Accumulate the sum, track repetitions for the mode, and count decimal digits. |
| `410–460` | Assemble the mean, median, mode, odd digit count, and even digit count. |
| `999` | Convert each result to text and append it to the output string. |
| `1000` onward | Supply the challenge's input through `DATA` statements. |

Sorting places the middle values where the median calculation can find them and groups equal values for repetition counting. Digit extraction consumes each array entry after its original value has been used for the other calculations.

## BASIC peculiarities

A few conveniences had to be rediscovered—or worked around:

- **No `MOD` operator.** Decimal digit extraction uses division and subtraction, with an integer temporary for the quotient.
- **Integer defaults.** `DEFINTA-Z` makes ordinary numeric variables integers. The sum uses an explicit double-precision variable, `S#`.
- **Count versus index.** After reading, `M` is the last occupied array index; the number of values is `M+1`.
- **End-of-data handling.** An early version used a `-1` sentinel. The listing instead uses `ON ERROR GOTO` and `RESUME`. It does not distinguish exhausted data from other errors in that reading routine.
- **Numbered destinations.** A wrong jump target is still a wrong jump target. The last correction came with the explanation: “Wrong GOTO, duh.”
- **Output as text.** `STR$` and `MID$` assemble the final number as a string, removing the leading space from each positive component's representation.

This is an experiment with a specific dataset and an old language, rather than a general-purpose statistics library. Changes to the input require attention to numeric limits, array capacity, rounding behavior, and edge cases.

## Follow the experiment

The [commit history](https://web.archive.org/web/20261004/https://github.com/flaviodesousa-programming-for-fun/fun-with-arithmetic/commits/main/) records the progression from loading and sorting the data to calculating the statistics, counting digits, cleaning up, and correcting the jumps.

For the personal journey behind the code, see the [blog article](https://web.archive.org/web/20261004/https://flaviodesousa.com/fun-with-arithmetic-the-1980s-way/).

## License

This repository uses **CC0 1.0 Universal**. See [`LICENSE`](https://web.archive.org/web/20261004/https://github.com/flaviodesousa-programming-for-fun/fun-with-arithmetic/blob/main/LICENSE).
