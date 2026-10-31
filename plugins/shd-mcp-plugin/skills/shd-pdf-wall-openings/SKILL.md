---
name: shd-pdf-wall-openings
description: Trace and review wall regions and window or door openings from architectural PDFs in SHD PDF Scheme Editor. Use for recognizing, adding, or correcting walls, columns that form wall regions, windows, hinged doors, and sliding doors; use shd-pdf-room-tracing for room polygons.
metadata: modules=pdf-scheme-editor
---

# Trace walls and openings from architectural PDFs

Create wall and opening polygons that match the source drawing and fit the existing room geometry. The tools provide vector candidates and geometric checks; they do not semantically recognize every wall, window, or door without visual review.

## Read and classify the source

- Resolve the exact project and document with `shd_list_pdf_scheme_editor_documents`, then call `shd_get_pdf_scheme_editor_document` with `include_state=true`. Keep its current `base_revision` and each target annotation's `objectRevision`. Read `shd://pdf-scheme-editor/geometry-contract` when field shapes or validation rules are unclear.
- Use the PDF source as geometry evidence, and inspect the full page before zooming into junctions, columns, window bays, door jambs, and wall returns.
- For vector PDFs, call `shd_analyze_pdf_scheme_editor_vectors` to inspect fills, strokes, paths, and text anchors. Preview likely fill styles with `shd_preview_pdf_scheme_editor_vector_style`. A style is only a clue: confirm its hatching, paired faces, geometry, and continuity on the rendered PDF.
- When a fill style is visually confirmed as a wall material, call `shd_trace_pdf_scheme_editor_wall_candidates` with only those style IDs. It can join subpaths that share complete vector edges, including concave contours. It does not decide which fills are walls; ambiguous or partial paths remain unmerged. Review every returned candidate against the PDF.
- For raster PDFs or missing vector paths, trace polygons from high-resolution source crops. Do not invent geometry from a gap, dimension, label, door arc, furniture, radiator, or overlay.
- Treat a `wall` as the filled wall-area polygon following its actual faces and thickness, not a centerline. Trace short wall pieces, returns, jambs, columns, and stepped or angled sections when they are visibly part of the wall construction. For a shaft or boxed feature, trace only its drawn solid wall bands; do not fill a visible hollow interior as wall.
- Treat a window or door as an `opening` polygon in the actual wall gap. Identify it from its frame, leaf, track, swing, or other source symbol. Do not bridge an opening with wall geometry, and do not create an opening from an unexplained gap alone.

## Keep geometry aligned

- Use page-local coordinates from the editor preview. Preserve each change in direction; do not simplify a niche, angled return, narrow pier, or irregular outline into a rectangle.
- Set calibration only from a known printed dimension or other explicit measured length.
- Walls may meet room boundaries, but must not overlap room interiors. Keep actual wall thickness outside rooms. Where the source shows adjoining wall and room edges, align them to touch and avoid coordinate gaps.
- Call `shd_preview_pdf_scheme_editor_wall_opening_candidates` before saving. It shows near-parallel wall-edge snaps within its calibrated tolerance, permits shared boundary contact, and reports positive-area overlap with rooms, openings that overlap wall interiors, or openings that fail to share a non-zero boundary segment with a wall. Snapping is only a small alignment correction: it does not fill missing wall sections and never moves openings.
- Correct or discard every candidate marked `saveable=false`. Do not change a candidate just to make the checker pass if the source drawing does not support that geometry. If the boundary remains ambiguous, leave it unresolved and report where.
- Do not use Build Walls as PDF recognition. It fills a geometric complement around room polygons and can miss real wall faces, columns, short pieces, and openings.

## Model windows and doors

- Save both windows and doors as `type=opening`, with `openingKind=window` or `openingKind=door`.
- For a hinged door, use `doorType=swing` or omit it for the default. If the PDF clearly shows the swing and hinge side, set `doorOrientation` to the matching supported value 0–3; otherwise omit it when adding, or set it to `null` when updating so the editor can choose the adjacent-room side. Never derive the opening direction from the door arc's position alone if the hinge is unclear.
- For a sliding door, set `doorType=sliding` in the opening write and `door_type=sliding` in the candidate preview. Sliding doors use the track and slide arrows, with no swing arc; omit `doorOrientation`.
- Trace the opening's actual extent from the frame/threshold or track shown in the PDF. Keep its polygon out of wall interiors while ensuring it meets the wall boundary as required by the preview. Inspect the rendered symbol and orientation after saving.

## Save and verify

- Read current document and annotation revisions before a write. Use the typed `shd_add_pdf_scheme_editor_wall` / `shd_add_pdf_scheme_editor_opening` and matching update tools. Keep page, IDs, `base_revision`, `base_object_revision` for updates, and a unique `idempotency_key` current.
- Preview proposed wall and opening polygons before writing. A successful preview is not a save and is not proof that the geometry matches the PDF.
- After writing, read the document again and verify revision, polygons, opening kind/type, orientation, and alignment against the source. Check close-ups and the whole page, including wall segments between windows and around room corners.
- Do not delete unrelated annotations. Report any unrecognized or ambiguous wall/opening regions instead of silently guessing.
