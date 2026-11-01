---
name: shd-pdf-wall-openings
description: Trace and review wall regions and window or door openings from architectural PDFs in SHD PDF Scheme Editor.
metadata: modules=pdf-scheme-editor
---

# Trace walls and openings from architectural PDFs

The PDF and its rendered page provide the geometry evidence. Vector tools extract candidate contours and check geometry; they do not semantically classify arbitrary lines, fills, windows, or doors.

## Trace wall regions

- Resolve the exact document and read its current state and calibration. Inspect the full page before zooming into wall junctions, columns, window bays, jambs, and returns.
- For vector PDFs, call `shd_analyze_pdf_scheme_editor_vectors`, then preview likely fill styles with `shd_preview_pdf_scheme_editor_vector_style`. A style is a clue only. Confirm its contours and continuity against the rendered source.
- Call `shd_trace_pdf_scheme_editor_wall_candidates` only with fill styles visually confirmed as wall areas. It extracts and joins vector contours; it does not decide whether a style is a wall. Review every returned polygon against the PDF before saving.
- For raster PDFs or missing vector paths, trace the actual wall faces manually from the source page. Keep wall thickness and returns in the polygon; do not infer walls from room complements.
- A `wall` is a filled wall-area polygon, not a centerline. Trace only solid wall bands around shafts and boxes; leave visible hollow areas empty.

## Trace windows and doors

- Prepare each window or door as an `opening` polygon that matches the actual frame, threshold, track, or leaf shown on the PDF. Do not classify unexplained gaps as openings.
- Save windows with `openingKind=window`, doors with `openingKind=door`. Use `doorType=sliding` only when the drawing shows a sliding system; omit `doorOrientation` unless the hinge and swing side are clear.
- The opening candidate preview validates geometry against existing walls, rooms, and openings. It does not recognize or generate openings. Inspect proposals against the source before writing.

## Align and save

- Use page-local PDF Scheme Editor coordinates and explicit calibration from a printed dimension or another measured length.
- Keep wall polygons outside room interiors; shared boundary contact is allowed. Preview wall/opening candidates with `shd_preview_pdf_scheme_editor_wall_opening_candidates` and correct or discard each candidate marked `saveable=false`.
- Do not use the editor's **Build Walls** complement operation as PDF recognition. Manual wall and opening drawing tools remain available for source-traced geometry.
- Read current `base_revision` and target `objectRevision` before writes. Use typed add/update tools or one revisioned operations batch with a unique `idempotency_key`. Re-read state after writing, verify geometry against the PDF, and report ambiguous areas rather than guessing.
