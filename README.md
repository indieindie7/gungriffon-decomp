# Gungriffon (Sega Saturn, Game Arts 1996) reverse-engineering notes and symbols

Hand-named function maps, review inputs and tools for the Saturn first-person mech shooter **Gungriffon**
(Europe, MK-81046 V1.00). Each stage is its own executable (disc files `1`..`10`), plus the title program
(GSTART) and the briefing program (BRIEF), all loaded at 0x0600E800. Round 1 named every function:

| Program | Functions | Still `provisional` |
|---|---|---|
| STAGE1 (disc file `1`) | 576 | 431 |
| STAGE2 | 533 | 440 |
| STAGE3 | 580 | 487 |
| STAGE4 | 597 | 503 |
| STAGE5 | 595 | 502 |
| STAGE6 | 588 | 495 |
| STAGE7 | 589 | 495 |
| STAGE8 | 557 | 467 |
| STAGE9 | 539 | 448 |
| STAGE10 | 562 | 468 |
| GSTART | 152 | 145 |
| BRIEF | 187 | 174 |
| **Total** | **6,055** | **5,055** |

0 placeholder (`FUN_`) names left.

Method:
1. Functions found by SH-2 prologue scan in each raw image (`ghidra/scripts/FindSh2FunctionsMem.java`).
2. STAGE1 named slice by slice from its decompile (prompt in `ghidra/naming/`).
3. The other programs share most code at shifted addresses: `tools/xstage.py` carries names on identical
   normalized bodies (~330 per stage); bodies at least 90% similar (difflib) take the STAGE1 name too
   (comment `[fuzzy r STAGE1 0x..]`); the remaining 260 body clusters (618 functions) were named once per cluster
   and spread to every copy (comment `[r2 rep ...]`).
4. Next: every provisional name re-read with its callers by a local model (prompt in `ghidra/review/`, verdicts
   applied with `tools/srprovapply.py`), STAGE1 first, then re-spread.

Non-commercial, done for study and preservation (early console mech FPS design reference).

**No game data is included**: no disc image, no executables, no decompiled C, no game text, no Ghidra database.
You need your own copy of the game to use any of this.

## What is here

| Path | Content |
|---|---|
| `ghidra/hand_<PROG>.txt` | names: `0x<addr> func <name> hand <comment>` (apply with ApplyNames.java from the Scorcher / AMOK repos, hand mode) |
| `ghidra/funcinfo_<PROG>.tsv` | per-function export: `F` address name source size, `C` callees (string rows removed) |
| `ghidra/naming/prompt_GG.txt` | the round-1 naming prompt |
| `ghidra/review/prompt_r1_gg.txt` | the provisional-review prompt |
| `ghidra/scripts/` | `FindSh2FunctionsMem.java`, `CreateAt.java`, `CsDumpAll.java` |
| `tools/` | `xstage.py` (cross-stage name carry), `ckprovctx.py` (review contexts), `srprovapply.py`, `pubfuncinfo.py` |

Setup: import each program as raw binary, `SuperH:BE:32:SH-2`, base 0x0600E800; run FindSh2FunctionsMem, then
ApplyNames with the matching `hand_<PROG>.txt`.

Related: [robotica-decomp](https://github.com/indieindie7/robotica-decomp), [deadoralive-decomp](https://github.com/indieindie7/deadoralive-decomp).

## Licence

Notes, names and tools: MIT (see `LICENSE`). Robotica / Daedalus belong to their rights holders; nothing from the game is distributed here.
