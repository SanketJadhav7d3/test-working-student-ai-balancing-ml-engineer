# Decisions Log

Append-only history of balancing changes made through this workbench.

Append entries via `audit-change-lite` after every successful commit. Newest entries at the bottom.

---

## 2026-06-05 — Mira L20 attack bump to address mid-game weakness

**Commit:** `303d159`

**Change:**
- `Src_Hero_Data.txt` · `hero_id=1, level=20` · `attack`: 608 → 700
- `Src_Hero_Data.txt` · `hero_id=1, level=20` · `power`: 1725 → 1909 (recomputed)

**Reason:**
Producer request: Mira (#1) is feeling weak in the mid-game. Attack at L20 bumped from 608 to 700 (single cell). Power column recomputed using formula `round(hp/10 + attack*2 + defense*1.5)`.

**Validation:**
- `qa-check-lite`: PASS (no warnings)

**Caveats / follow-ups:**
- None

---

## 2026-06-05 — Tessa L10–L15 attack curve boost (+10% per level)

**Commit:** `be79d96`

**Change:**
- `Src_Hero_Data.txt` · `hero_id=11, levels=10–15` · `attack`: +10% per level (6 rows); power recomputed each row
  - L10: 375 → 413 (power 1062 → 1138)
  - L11: 398 → 438 (power 1127 → 1207)
  - L12: 422 → 464 (power 1196 → 1280)
  - L13: 444 → 488 (power 1260 → 1348)
  - L14: 468 → 515 (power 1328 → 1422)
  - L15: 490 → 539 (power 1391 → 1489)

**Reason:**
Producer request: Tessa (#11) needs a curve adjustment in the mid-game range. Attack at L10–L15 boosted by 10% at each level independently to steepen her power curve through that window.

**Validation:**
- `qa-check-lite`: PASS (no warnings)

**Caveats / follow-ups:**
- None — levels outside L10–L15 left unchanged per intent; no cascade on other heroes.

---

## 2026-06-05 — Thorne rebucketed to lower-tier unlock chain (unlock_require_id 5010 → 5005)

**Commit:** `0f20e30`

**Change:**
- `Src_Hero_Data.txt` · `hero_id=4, levels=1–30` · `unlock_require_id`: 5010 → 5005 (all 30 rows; power unaffected)

**Reason:**
Producer request: Thorne (#4) needs to move into the lower-tier unlock chain. `unlock_require_id` changed from 5010 to 5005 across all levels. 5005 is the same chain used by Aldric (#2), Doran (#10), and Roric (#16).

**Validation:**
- `qa-check-lite`: PASS (no warnings; 5010 was exclusive to Thorne so its removal creates no dangling references in the file)

**Caveats / follow-ups:**
- 5010 no longer appears anywhere in `Src_Hero_Data.txt` — if a require-table entry for 5010 exists server-side, it is now unreferenced and can be cleaned up separately.
