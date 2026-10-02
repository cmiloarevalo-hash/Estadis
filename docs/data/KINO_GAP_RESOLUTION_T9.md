# Issue #24 — T9 gap/conflict resolution

## Resolution attempt

A final authorized-source pass was performed for the recent PROVISIONAL draws 3280–3286.

No complete official 14-number result was discovered through the public/indexed Lotería surfaces available without automation.

One additional independent secondary article was found for draw 3280:

- Epicentro Chile, 2026-09-16;
- draw 3280;
- numbers: 1,2,5,6,9,10,11,13,14,18,20,22,24,25;
- matches ResultadosKinoChile exactly;
- estimated total pool CLP 6.7B and Kino main estimate CLP 860M.

This changes draw 3280 from one numeric secondary group to two independent secondary groups, but **does not** satisfy VALIDATED because official complete numeric evidence is still absent.

## Provenance correction

T6 processed rows for the eight Sorteos en Vivo indexed observations accidentally omitted their root-level `source_id`. T9 corrects this only in processed v2:

`kino.official.direct.sorteos_en_vivo`

Raw evidence remains unchanged.

## Status after T9

- VALIDATED: 0
- PROVISIONAL: 7
- CONFLICTED: 0
- INCOMPLETE: 10
- REJECTED: 0

No material conflict was discovered or silently resolved.

## Gap groups

- G1 ACQUISITION_GAP: Official dynamic result interfaces do not expose complete numeric historical payload through permitted static/manual path; further exhaustive acquisition would require prohibited automation/endpoint work.
- G2 VALIDATION_GAP: Complete secondary numeric evidence exists but no acquired official source contains the full 14-number result.
- G3 SOURCE_GAP: Observed official records lack one or more core fields; eight video pages lack date and numbers, two institutional records lack numbers.
- G4 REGIME_GAP: Detailed pre-2893 product rule regime was not established for these old observations.
- G5 UNKNOWN_GAP: Historical continuity was not exhaustively enumerated; absence from v1 cannot be interpreted as a missing lottery draw.

## Saturation

```text
ACQUISITION SATURATION: REACHED
```

Reason: further exhaustive official historical extraction would require browser/request automation or scraper-like development explicitly forbidden by Master #15. The available official institutional and independent secondary discovery routes have been reasonably exercised for this v1.

Partial but auditable coverage is retained rather than weakening VALIDATED criteria.
