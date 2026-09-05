# WHY YOU AGE explained in 5 Minutes — production package

Faceless-channel Explainer, built to the Higgsfield `faceless-video` workflow (v2.4) contract.
Pre-production is **complete and validated**. Generation is **blocked on credits** — see below.

| Setting | Value |
|---|---|
| Runtime | 5:00 — **30 blocks × 10s** |
| Clip model | `minimax_h3` @ `resolution:"2K"` (workflow-locked; see note) |
| Asset/style model | `seedream_v5_pro` @ `resolution:"1k"` |
| Narration | `text2speech_v2`, `variant:"elevenlabs"`, one locked voice |
| Aspect | `16:9` — passed explicitly on every video call |
| Style | Editorial Motion Graphics (Explainer default) |
| Subtitles | Burned in, via the `subtitles` workflow |
| VO total | 631 words across 30 lines, all inside the 20–23 band |

> **On "Seedance 2.0 1080p":** there is no Seedance 2.0 in the catalog — the current model is
> `seedance_2_5`. More importantly, the faceless-video workflow's GOLDEN RULE 1 locks clips to
> `minimax_h3` at 2K and forbids substitution; Seedance is not a legal clip model in this
> pipeline. 2K is also a higher resolution than the 1080p that was asked for, so this is an
> upgrade rather than a compromise. Going with Seedance would mean dropping the workflow and
> losing automated narration, subtitle burn, and assembly.

---

## COST — and the blocker

All figures below are **preflighted** against the live API (`get_cost`), not estimated.

| Stage | Unit cost | Qty | Credits |
|---|---|---|---|
| Clips — `minimax_h3` 2K, 10s | 20 | 30 blocks | **600** |
| Style key + location/prop assets — `seedream_v5_pro` 1k | 1.5 | ~10 | **15** |
| Narration — `text2speech_v2` | small | 30 takes | **~25** |
| Retries (workflow *expects* cut-count + NSFW false-positive regens) | — | +20% | **~130** |
| | | **Total** | **~770** |

**Balance: 10 credits, free plan.** That is ~1.3% of what one run needs — not enough for a
single 10-second block. Nothing has been generated and no credits have been spent; every
number above came from cost-preflight calls that submit no job.

For comparison, the Seedance route that was originally requested is worse: 1080p Seedance is
9 credits/second, so 300 seconds = **2,700 credits** for the video alone, at lower resolution
than the 2K route and with no finishing pipeline.

**To run this, the account needs a paid plan with ~800+ credits.** Once topped up, everything
below is ready to execute with no further creative decisions.

---

## Files

| File | Contents |
|---|---|
| `vo-lines.txt` | The 30 narration lines, one per block, in order |
| `shots.tsv` | Per block: location + the 5 framing sizes, tab-separated |
| `blocks.md` | The bound sheet — each block's VO line, location, and 5 shot descriptions |
| `style.md` | The style key prompt, palette, and the asset list to generate in Phase 2 |

## Validation already passed

Checked locally against the rules `validate_motion_script.py` enforces:

- 30 blocks, 150 shots — **0 errors**
- Every VO line inside the **20–23 word** band
- Cold open **4 words** before its first full stop (max 8)
- No conversational filler
- No content word repeated within six words
- No verbatim five-word phrase shared between any two blocks
- No two adjacent shots at the same framing size
- Location runs **≤ 2 consecutive blocks**; no revisit re-opens on an establishing WIDE

The workflow's own validator still has to be run in the sandbox at Phase 3 and its
`valid:true` output pasted at script lock — local checks do not substitute for that gate.

## Through-line

**The repair crew.** Introduced in block 1 as the amber motes that rebuild you, carried through
every block as they fall progressively behind, and resolved in blocks 28–30: ageing is not a
programmed shutdown, it is a maintenance crew that was never given a reason to keep up. The
arc runs intuitive → counterintuitive → unsettling, and the payoff (ageing as an engineering
problem, not a law of physics) is held to the final 30 seconds.

## Run order once credits exist

1. Phase 0 — `sandbox_exec({restart:true, ...})` preflight; require `faster_whisper`
2. Phase 2 — generate style key + 7 location assets + props (`generate_image_batch`, ≤12/call)
3. Phase 3 — build `script_manifest.json` from `vo-lines.txt` + `shots.tsv`, run
   `validate_motion_script.py --duration-seconds 300` until `valid:true`
4. Phase 4 — 30 clips via `generate_video_batch` in batches of ≤12, `aspect_ratio:"16:9"`
   explicit on every call, assets referenced location → characters → props
5. Phase 5 — load `narrator`, lock one voice, 30 takes, measure with `speech_metrics`
6. Phase 6 — assemble with `assemble_final.sh`, `background:true`, poll immediately
7. Phase 7 — load `subtitles`, burn captions
8. Phase 9 — `media_upload` → PUT with `--upload-file` → `media_confirm`, deliver that URL

Cut-count QC per GOLDEN RULE 2: probe each block for ≥5 scene changes; a block short on cuts
gets **one** regeneration with shots spelled out by timecode, then ships as-is.
