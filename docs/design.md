# Design notes

The controller uses a three-state FSM to select the traffic light phase and the active countdown branch. This document covers the state table and logic equations.

## State and output table

`s0` and `s1` are the current-state bits, in that order. `S0` and `S1` denote the next-state bits. `C1` and `C2` are active-high countdown-completion indications.

| Current state | `C1 C2=00` | `01` | `10` | `11` | `en1` | `en2` | Green | Yellow | Red |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| A (`00`) | A (`00`) | A (`00`) | B (`01`) | B (`01`) | 1 | 0 | 1 | 0 | 0 |
| B (`01`) | B (`01`) | C (`10`) | B (`01`) | C (`10`) | 0 | 1 | 0 | 1 | 0 |
| C (`10`) | C (`10`) | C (`10`) | A (`00`) | A (`00`) | 1 | 0 | 0 | 0 | 1 |

![State assignment table extracted from slide 22](../assets/state-assignment-table.png)

State `11` is unused and is treated as a don't-care in the Karnaugh maps. The state table does not define recovery from this encoding.

## Karnaugh simplification

Using `~` for NOT, `&` for AND and `|` for OR, the next-state equations shown in slide 23 are:

```text
S0 = (~C1 & s0) | (C2 & s1)
S1 = (~s0 & ~s1 & C1) | (~C2 & s1)
```

The enable and light-output equations are:

```text
en1    = ~s1
en2    =  s1
Green  = ~s0 & ~s1
Yellow = ~s0 &  s1
Red    =  s0 & ~s1
```

These equations reproduce the table for the three legal state encodings.

| Next `s0` | Next `s1` |
| --- | --- |
| ![Karnaugh map for next s0, slide 23](../assets/kmap-next-s0.png) | ![Karnaugh map for next s1, slide 23](../assets/kmap-next-s1.png) |

| `en1` | `en2` |
| --- | --- |
| ![Karnaugh map for en1, slide 23](../assets/kmap-enable1.png) | ![Karnaugh map for en2, slide 24](../assets/kmap-enable2.png) |

![Original light-output equations, slide 24](../assets/output-equations.png)

## Countdown and display path

The circuit uses three 74192 BCD counters: two digits for the green/red branch and one for yellow. The display path uses 74LS247 decoders and seven-segment displays. The FSM uses 4013 flip-flops and logic gates.

`C1` signals completion of the green/red countdown, and `C2` signals completion of the yellow countdown. For a two-digit countdown, detecting `00` requires checking both BCD digits. The four-bit zero-detection expression on slide 21 describes a single digit.

## Project figures

The figures below come from the group presentation. The [slide PDF](technical-slides.pdf) contains original pages 5 and 21–25; the page numbers here refer to that presentation.

| Figure | Original page |
| --- | --- |
| Block diagram | 5 |
| FSM diagram | 21 |
| State table | 22 |
| Next-state and enable Karnaugh maps | 23–24 |
| Light-output equations | 24 |
| Proteus circuit | 25 |
