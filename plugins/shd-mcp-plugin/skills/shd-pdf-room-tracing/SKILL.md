---
name: shd-pdf-room-tracing
description: Trace, review, and correct room polygons from architectural PDF plans in SHD PDF Scheme Editor, especially around wall thickness, door thresholds, columns, boxes, and convector niches.
metadata: modules=pdf-scheme-editor
---

# Trace rooms from architectural PDFs

Produce room polygons that follow the room-facing side of the drawn boundaries. Read the source PDF for geometry; existing room annotations are evidence to review, not geometry to copy unless the user explicitly asks for a copy.

## Work from the drawing

- Make one overview pass: render the full plan once, then call `shd_analyze_pdf_scheme_editor_vectors` once for the page to get style groups and paths. Build a checklist of turns, junctions, columns, boxes, convectors, and doors from that data. Inspect a high-resolution crop only for an item the vector data does not settle, with a budget of at most 4 crops per room unless a semantic check fails. Prefer PDF vector paths and dimensions; use the rendered page to identify which side of each line is room interior.
- Use explicit scale calibration from a printed dimension. Keep PDF page coordinates throughout tracing.
- Trace the actual outline, including concave, angled, stepped, and recessed parts. Do not simplify a contour into a rectangle when the drawing shows a recess, box, column, or niche.
- Follow the room-facing boundary of wall bands. A room may touch a wall boundary; it must not cover the wall band. Include the open floor area around a convector/radiator when the drawing shows a room-side niche, and follow the niche edges around the device. Do not infer that every heater creates a niche.
- Treat a door opening as a shared threshold between the two spaces it connects. At each interior doorway, use one exact threshold segment on both adjacent room polygons and check that both polygons contain the full segment. Keep the wall thickness on either side of the opening outside the rooms. On an exterior entrance, stop at the inside threshold face and keep the exterior landing outside the corridor; floor under an inward-opening door swing still belongs to the corridor.
- Before tracing, enumerate every convector/radiator visible on the source page from the vector analysis and the overview render, not from a crop per device. For each one, check the full device footprint and the entire vector-defined room-side window recess, including side and end strips around the device; a heater bounding box is not the niche boundary. Do not leave a blank gap under or behind the device. If room boundaries divide a long heater niche, assign each segment to the room it opens into and check the union for gaps.
- Trace a box, shaft, or column as an indentation in the adjacent room boundary. Do not fill its footprint or create a separate room for it. Verify a sample point inside the box is outside the room polygon and a point just beyond it is inside.
- At the entrance, verify points just inside the threshold are in the corridor and points on the exterior landing are outside all room polygons. Never let a room contour follow the door leaf or swing arc, and do not carve out interior floor merely because the door swing passes over it.
- Preserve openings between rooms as boundary thresholds. Do not use dimension lines, text leaders, furniture, devices, door arcs, or annotation overlays as room edges.

## Use the SHD tracing tools safely

- Use `shd_analyze_pdf_scheme_editor_vectors` to inspect vector styles and exact path coordinates. A style group is only a visual clue; confirm it against the rendered source before using it as a wall.
- Use `shd_trace_pdf_scheme_editor_room_candidates` to evaluate fresh room hints against the PDF wall vectors. Supply room seeds only in clear interior floor areas. Supply an apartment `allowed_polygon` when the outer boundary or entrance could be confused with room interior.
- If a boundary hint leaves the allowed apartment footprint, intersects itself, overlaps another room, or cannot be related to the drawing, correct it before saving. A geometrically valid polygon is not proof that it matches the PDF.
- Inspect a source overlay after tracing. Confirm every segment against its local crop, including wall-facing side, thresholds, recessed convectors, structural columns/boxes, and the entrance. Use room area labels as a consistency check, not as a substitute for the drawn outline.
- When useful, pass `room_inclusion_regions` for the full vector-defined room-side convector recess or other floor areas that must belong to a room, and `room_exclusion_regions` for the exterior landing, wall thickness, column, or box footprint. Take these regions from the analysis results of the same page and document revision, not from repeated renders. Pass `room_shared_boundaries` for each known interior doorway so the tool can catch a one-sided offset or gap. Treat failed semantic checks as a stop condition. Keep a short per-room boundary review and inspect the full apartment overlay plus close-ups before mutating the document.
- Reuse results. Analysis, room trace, and preview results are cached per document revision. Do not repeat `shd_analyze_pdf_scheme_editor_vectors` for a page you already analysed, and do not request a preview of an area you already checked with the same parameters.
- For a requested fresh trace, do not copy room polygons from another document. Remove only the specifically authorized room records, keep unrelated annotation types, then add the new room polygons with fresh IDs and optimistic revisions. Re-read and inspect the saved result.
