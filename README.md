# Hotel Management System SRS

This package contains the final SRS artifacts for the Hotel Management System MVP+Staff v1.2.

## Files

| Path | Purpose |
|---|---|
| `hotel_management_system_srs_v1_2_staff_screen_mockup_ready.md` | Final Markdown SRS source. |
| `srs-final-mvp-semantic-repair.docx` | Final DOCX export for review/submission. |
| `hotel_management_system_srs_v1_2_assets/` | Mobile Flutter mock-up images referenced by the Markdown. |
| `diagrams/drawio/` | Draw.io source files for SRS diagrams. |
| `diagrams/png/` | PNG exports for SRS diagrams. |
| `diagrams/notes/` | Diagram prompt/notes Markdown files. |
| `scripts/export-md-to-docx.py` | Local export helper used to generate DOCX from Markdown. |

## Final Validation Snapshot

- 41 mobile mock-up images and 28 SRS diagrams included in the DOCX.
- Mock-up image width in DOCX: 2.65 inches.
- Diagram image width in DOCX: 6.2 inches.
- Integrated diagrams cover context, use case, activity, screen flow, logical ERD, and state machine views.
- Logical ERD integrated as 1 canonical ERD plus 6 readable module views.
- Alternative flows validated: 101 unique IDs, no duplicate IDs, branch/sub-step numbering coherent.
- Actor and message trace validated against detailed use cases.
- Finance/status fixes included: `BR-FIN-007`, `BR-PAY-007`, `Reconciliation Status`, `Commission Status`, and `Payment Collection Status`.

## Regenerate DOCX

The reference template is not bundled in this GitHub-ready folder. To regenerate the DOCX locally, run the export script with your local template path:

```powershell
python .\scripts\export-md-to-docx.py `
  --input .\hotel_management_system_srs_v1_2_staff_screen_mockup_ready.md `
  --template "C:\path\to\reference-template.docx" `
  --output .\srs-final-mvp-semantic-repair.docx `
  --engine python-docx `
  --mockup-width-inches 2.65 `
  --image-width-inches 6.2 `
  --overwrite
```
