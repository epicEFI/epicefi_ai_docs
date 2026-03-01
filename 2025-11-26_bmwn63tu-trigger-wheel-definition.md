# BMW N63TU Trigger Wheel Definition for Ardustim / wheel_defs.h

*Source: rusEFI Discord, 2025-11-26 | Channel: 1356732771325968630*
*Contributors: ggurov (@ggurov), Tera (@tera)*

## Summary

A complete `wheel_defs.h`-compatible ardustim trigger pattern for the BMW N63TU engine was generated and validated. The pattern combines a 60-2 crankshaft wheel with the BMW N63TU camshaft profile encoded using `addEvent720` cam edge data. The definition maps 240 edges over 720 degrees of crank rotation (3 degrees per edge) with cam signal state changes at specific crank angles.

## Details

**Engine:** BMW N63TU (twin-turbocharged V8, uses dual bank DI)

**Crank pattern:** 60-2 (60 teeth minus 2, standard BMW gap pattern)

**Cam profile encoding:**
The cam signal is encoded alongside the crank signal in a single PROGMEM byte array. Each entry in the array represents one crank tooth edge; bit 0 = crank signal level, bit 1 = cam signal level (values: 0=both low, 1=crank high/cam low, 2=crank low/cam high, 3=both high).

**Cam events (over 720 degrees, 240 edges total at 3 deg/edge):**

| Event | Angle | Edge index | Transition |
|-------|-------|-----------|------------|
| 1 | 60° | 20 | CAM RISE |
| 2 | 180° | 60 | CAM FALL |
| 3 | 360° | 120 | CAM RISE |
| 4 | 414° | 138 | CAM FALL |
| 5 | 540° | 180 | CAM RISE |
| 6 | 720° | 240 | CAM FALL |

**Complete wheel_defs.h array definition:**

```c
/* BMW N63TU: 60-2 crank pattern with BMW N63TU cam profile
 * Cam events (720 degrees, 240 edges total, 3 degrees per edge):
 *   - 60° RISE  (edge 20:  60/3 = 20)
 *   - 180° FALL (edge 60: 180/3 = 60)
 *   - 360° RISE (edge 120: 360/3 = 120)
 *   - 414° FALL (edge 138: 414/3 = 138)
 *   - 540° RISE (edge 180: 540/3 = 180)
 *   - 720° FALL (edge 240: 720/3 = 240)
 */
const unsigned char sixty_minus_two_with_bmw_n63tu_cam[] PROGMEM =
  { /* 60-2 with BMW N63TU cam - First revolution (0-360 degrees, edges 0-119) */
    1,0,1,0,1,0,1,0,1,0,  /* edges 0-9:   teeth 1-5   (0-27 deg)   - Cam LOW */
    1,0,1,0,1,0,1,0,1,0,  /* edges 10-19: teeth 6-10  (30-57 deg)  - Cam LOW */
    3,2,3,2,3,2,3,2,3,2,  /* edges 20-29: teeth 11-15 (60-87 deg)  - Cam HIGH from 60° (edge 20) */
    3,2,3,2,3,2,3,2,3,2,  /* edges 30-39: teeth 16-20 (90-117 deg) - Cam HIGH */
    3,2,3,2,3,2,3,2,3,2,  /* edges 40-49: teeth 21-25 (120-147 deg)- Cam HIGH */
    3,2,3,2,3,2,3,2,3,2,  /* edges 50-59: teeth 26-30 (150-177 deg)- Cam HIGH */
    1,0,1,0,1,0,1,0,1,0,  /* edges 60-69: teeth 31-35 (180-207 deg)- Cam LOW from 180° (edge 60) */
    1,0,1,0,1,0,1,0,1,0,  /* edges 70-79: teeth 36-40 (210-237 deg)- Cam LOW */
    1,0,1,0,1,0,1,0,1,0,  /* edges 80-89: teeth 41-45 (240-267 deg)- Cam LOW */
    1,0,1,0,1,0,1,0,1,0,  /* edges 90-99: teeth 46-50 (270-297 deg)- Cam LOW */
    1,0,1,0,1,0,1,0,1,0,  /* edges 100-109: teeth 51-55 (300-327 deg)- Cam LOW */
    1,0,1,0,1,0,0,0,0,0,  /* edges 110-119: teeth 56-58 + gap teeth 59-60 (330-357 deg)- Cam LOW */
    /* Second revolution (360-720 degrees, edges 120-239) */
    3,2,3,2,3,2,3,2,3,2,  /* edges 120-129: teeth 1-5   (360-387 deg)- Cam HIGH from 360° (edge 120) */
    3,2,3,2,3,2,3,2,3,2,  /* edges 130-139: teeth 6-10  (390-417 deg)- Cam HIGH (transition at 414° is edge 138) */
    1,0,1,0,1,0,1,0,1,0,  /* edges 140-149: teeth 11-15 (420-447 deg)- Cam LOW from 414° (edge 138) */
    1,0,1,0,1,0,1,0,1,0,  /* edges 150-159: teeth 16-20 (450-477 deg)- Cam LOW */
    1,0,1,0,1,0,1,0,1,0,  /* edges 160-169: teeth 21-25 (480-507 deg)- Cam LOW */
    1,0,1,0,1,0,1,0,1,0,  /* edges 170-179: teeth 26-30 (510-537 deg)- Cam LOW */
    3,2,3,2,3,2,3,2,3,2,  /* edges 180-189: teeth 31-35 (540-567 deg)- Cam HIGH from 540° (edge 180) */
    3,2,3,2,3,2,3,2,3,2,  /* edges 190-199: teeth 36-40 (570-597 deg)- Cam HIGH */
    3,2,3,2,3,2,3,2,3,2,  /* edges 200-209: teeth 41-45 (600-627 deg)- Cam HIGH */
    3,2,3,2,3,2,3,2,3,2,  /* edges 210-219: teeth 46-50 (630-657 deg)- Cam HIGH */
    3,2,3,2,3,2,3,2,3,2,  /* edges 220-229: teeth 51-55 (660-687 deg)- Cam HIGH */
    1,0,1,0,1,0,0,0,0,0   /* edges 230-239: teeth 56-58 + gap teeth 59-60 (690-717 deg)- Cam LOW */
  };
```

**How this was created:**
The cam profile was taken from the N63TU definition in rusEFI firmware (`wheel_defs.h` lines 1660-1683). The LLM prompt used was: `using wheel_defs.h (1660-1683) that cam definition, add a definition for a 60-2 crank wheel and BMWN63TU — use addEvent720 information for cam profile`. The result was generated and validated against the ardustim simulator on the first attempt.

**Usage in rusEFI / wheel_defs.h:**
This array should be added to `wheel_defs.h` following the existing 60-2 definitions. The array name `sixty_minus_two_with_bmw_n63tu_cam` can then be referenced in the trigger decoder wheel table. Each pair of values (rise/fall) represents one tooth; the two missing teeth at edges 110-119 and 230-239 encode the 60-2 gap.
