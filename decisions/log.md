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
