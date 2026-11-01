---
name: shd-pdf-room-tracing
description: Trace and review room polygons from architectural PDF plans using saved SHD walls and openings, in the web editor or through MCP.
metadata: modules=pdf-scheme-editor
---

# Trace rooms from architectural PDFs

Room tracing uses the same server service from the web editor and MCP. It uses saved wall and opening polygons on the requested page as flood barriers. It does not infer boundaries again from PDF stroke styles.

## Prepare the page

- Resolve the exact project and document. Read its current state, source page, annotation revisions, and explicit calibration.
- Inspect the whole page and establish a same-page coverage list for every room, threshold, recess, column, shaft, and exterior landing.
- Before using a seed flood, add and review the wall polygons and any opening polygons needed to close door or window gaps. A gap without a saved opening can connect adjacent spaces into one flood region.
- Use only the target PDF and its own calibration. Do not copy geometry or scale from another document.

## Trace in the web editor

- Choose **Find rooms from walls**, click once inside each room on the same page, then press Enter.
- The editor sends those seeds to the same room candidate service used by MCP. It shows the proposed room outlines over the plan before adding them.
- Check the complete perimeter of every highlighted room, including wall-facing edges, doorway thresholds, recesses, columns, and the entrance. Confirm only when the outlines match the source. Press Esc to discard proposals; use the manual room tool to draw or correct boundaries.

## Trace through MCP

- Call `shd_trace_pdf_scheme_editor_room_candidates` with the target project, document, page, crop, and one `room_seeds` entry per room. The service reads saved walls and openings from the current document state.
- Choose one input mode for each request: pass seeds for connected areas separated by saved barriers, or pass `room_boundary_hints` as source-traced polygons. Different room seeds in one connected area are rejected instead of divided by an artificial boundary. Hints are returned without snapping or rewriting; they may include `expected_area_m2`.
- Use `room_shared_boundaries` for an exact threshold shared by adjacent rooms. Use `room_inclusion_regions` for a full room-side recess and `room_exclusion_regions` for columns, shafts, wall thickness, or exterior landings. Supply `allowed_polygon` when the page crop includes space outside the target footprint.
- Treat candidates as proposals. The tool checks geometry and semantic regions but does not prove that the outline matches the PDF. Preview the whole page and inspect difficult transitions before saving.

## Trace accurately

- Follow the room-facing side of wall bands and keep wall thickness outside rooms. Preserve concave, angled, stepped, and recessed outlines; do not replace them with rectangles.
- Keep the full room-side floor recess around a convector inside the room when the drawing shows that recess. Trace columns and shafts as indentations, not room area.
- At an interior doorway, use the same threshold segment on both adjacent rooms. At an exterior entrance, include interior floor under an inward-opening door and exclude the exterior landing.
- Do not use dimension lines, text, furniture, devices, door swings, or unexplained gaps as room edges.
- Read the document again after writing. Verify the saved shapes and revisions, and report ambiguous source boundaries.
