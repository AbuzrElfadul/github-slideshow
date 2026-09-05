# WHY YOU AGE explained in 2 Minutes — production package

Short cut of the same Explainer, built to the same `faceless-video` v2.4 contract.
Pre-production **complete and validated**. Generation still **blocked on credits**.

| Setting | Value |
|---|---|
| Runtime | 2:00 — **12 blocks × 10s** |
| Clip model | `minimax_h3` @ `resolution:"2K"` |
| Aspect | `16:9`, passed explicitly on every video call |
| Style / assets | shared with the 5:00 cut — see `../style.md` |
| VO total | 256 words across 12 lines, all inside the 20–23 band |
| Speech load | ~107s of narration inside 120s of video |

## Cost

Clip unit cost is preflighted (`minimax_h3` 2K, 10s = 20 credits); the rest carries over
from the 5:00 package.

| Stage | Unit | Qty | Credits |
|---|---|---|---|
| Clips | 20 | 12 blocks | **240** |
| Style key + 7 locations + props | 1.5 | ~10 | **15** |
| Narration — `text2speech_v2` | small | 12 takes | **~10** |
| Retries (+20%) | — | — | **~50** |
| | | **Total** | **~315** |

**Balance: 10 credits.** Still short by ~305. This cut is **~60% cheaper** than the 5:00
version (~315 vs ~770) and is the cheaper way to prove the channel's look and voice before
committing to full-length episodes.

## Not a truncation

Rungs that could not survive compression were dropped whole rather than trimmed, and the
surviving twelve were rewritten so each still creates the need for the next. What was cut:
the tissue-renewal rates, the Hayflick number itself, the corrupted-manual beat, the
senescent-signal refusal, the explicit energy feedback loop, the "died in the wild" and
"gene at seventy pays no penalty" pair, and the bowhead whale. What survived is the spine:
crew → copying → telomeres → damage → senescence → inflammation → evolution's indifference
→ animals that opt out → payoff.

## Validation passed

Same checks as the 5:00 cut, **0 errors**: 12 blocks / 60 shots; every line inside the
20–23 word band; cold open 6 words before its first full stop (max 8); no filler; no content
word repeated within six; no shared five-word phrase between blocks; no adjacent shots at the
same framing size; location runs ≤2 consecutive; no revisit re-opening on an establishing
WIDE. `blocks.md` cross-checks clean against `shots.tsv` and `vo-lines.txt`.

The workflow's own `validate_motion_script.py` still has to run in the sandbox at Phase 3
with `--duration-seconds 120`, and its `valid:true` output pasted at script lock.

## Run order

Identical to `../README.md`, with 12 blocks instead of 30 — a single `generate_video_batch`
call covers the whole cut, since 12 is exactly the per-call submission cap.
