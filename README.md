# How Does a Hypercar Get Pushed Toward the Road? — Koenigsegg Jesko Attack downforce explainer
9:16, 76 s, 10 scenes. Three.js 0.180.0 (loaded from cdn.jsdelivr.net — the only online dependency), no build step.

## Run
    cd <this folder> && python3 -m http.server 8000      # then open http://localhost:8000
HTTP is required: browsers block local-file loading of the FBX/textures. Phone: same Wi-Fi, open `http://<computer-LAN-IP>:8000`.

## Controls
Play/Pause, Restart, scrubber, scene buttons 1–10, preview speed, volume/Sound, toggles (Airflow, Pressure, Vectors, Labels, Cutaway = force on, Simple, Inspect/orbit, Voice), Reset cam, Quality (Low/Med/High), Record.
Every frame is a pure function of the master time, so scrubbing/jumping/seeking back rebuilds car, camera, airflow, captions, vectors and audio parameters.
Inspect: orbit/pan the car, shows bounding box, axes and diagnostics; "Reset cam" returns to the scripted camera.

## Export (no lag) — "⬇ Export MP4"
Renders every frame offline at its exact timestamp (30 fps, 76.00 s) with WebCodecs H.264 and mixes the audio offline (engine recording + wind/tyre + hits) into AAC, then downloads a real .mp4. Speed depends only on your GPU, not on playback, so nothing can lag or drop. Needs desktop/Android Chrome or Edge and internet (loads mp4-muxer 5.1.3 from jsDelivr at click time). Choose "Export 1080×1920" or "540×960" first; keep the tab visible; the button turns into Cancel while running. If the browser lacks an H.264/AAC encoder it says so (video-only MP4 if only AAC is missing).
Fallback: "● Record (live)" captures real-time playback with MediaRecorder (WebM/MP4 depending on browser) and can stutter on slow devices.

## Assets (all in this folder)
- FINAL_MODEL_AT.fbx (16 MB) + 30 T_*.png / common_carbon05_*.png — hero Jesko Attack and its textures (names must stay unchanged)
- engine.mp3 (47 s) — your supplied Jesko engine recording, used as the engine bed (looped 1.5–45.5 s, playback rate follows the illustrative speed). It is a YouTube screen capture, so confirm you have the rights before publishing.
- bloem_hill_01_2k.hdr — lighting, reflections, sky
- asphalt_diff.jpg (2k), asphalt_normal.jpg & asphalt_rough.jpg (1k) — converted from the supplied asphalt_pit_lane_2k maps
- index.html — the whole application. Total ≈ 27 MB.

## What is real model vs explanatory
Real (verified in the FBX): 133 separate meshes, y-up, +Z forward, metres (≈4.63 × 2.2 × 1.22 m), wheels as independent meshes (spun from travelled distance; fronts steer slightly in the corner), spoiler meshes (SK_Spoiler_*) at the rear, front/rear bumper meshes with splitter and rear underside, floor ≈10–16 cm above the road, full interior/engine. Roof, width and floor envelopes used for streamlines were measured from this geometry.
Explanatory (procedural): streamlines/particles, pressure blobs, force and tire-load arrows, cutaway (a clipping plane through the car's centreline), the "active front flaps" schematic panel, bar/graph panels, all captions and labels. The wing is a fixed mesh — no articulation is animated. The model has no identifiable moving front flaps and no verified production-accurate floor; the flap panel is labelled schematic. Underbody/diffuser streamlines follow the measured floor height approximately, not a flow solution. Nothing is CFD.
Speeds on screen are illustrative; vehicle motion is time-scaled so wheels do not strobe. The 100→200 km/h panel is the simplified v² relationship, not Jesko data. Figures used exactly as your script states: 700 kg @ 250 km/h, 1,000 kg @ 290 km/h (kg-equivalent force, labelled manufacturer-published; I could not re-open koenigsegg.com offline). The earlier 1,400 kg maximum scene was removed because the new script does not mention it.
Audio: engine = the supplied recording; wind, tyre noise and scene-change hits are procedural. The narration is NOT in the audio file — it appears as timed captions (also in the recorded video). "Voice" reads the captions with the browser's speech synthesis only for preview; it is not captured by Record. Record your own voiceover from the script below.

## Narration / scenes (11 scenes, each cut lands on a sentence start; cue times assume ~2.55 words/s + 0.2 s between sentences)
00:00 S1 Hook — Why doesn't the Koenigsegg Jesko Attack just fly off the road at insane speeds?
00:06 S2 Rear wing — Look at that massive rear wing. It might look like it's only there to make the car look aggressive, but it's actually doing something much more important.
00:17 S3 Pushed down — It's using the air to push the car down toward the road. Here's how it works.
00:24 S4 Airflow — As the car speeds up, air flows around the body and over the rear wing.
00:30 S5 Wing redirects — The wing redirects that air, creating a downward force.
00:33 S6 Splitter + floor (cutaway) — Meanwhile, the front splitter and the shaped floor underneath help manage the airflow and keep the car balanced.
00:41 S7 Diffuser — The diffuser helps control how air leaves from underneath.
00:44 S8 Faster = stronger (100→200 km/h, v² panel) — And here's the crazy part. The faster the Jesko goes, the stronger that force becomes.
00:51 S9 Numbers — Koenigsegg says it produces 700 kilograms of downforce at 250 kilometers per hour, and 1,000 kilograms at 290.
00:58 S10 Tonne — That's the equivalent of pressing the car down with a force equal to roughly a tonne. It's not extra weight—it's the air helping push the tires into the road.
01:10 S11 Corner — And that's how the Jesko can attack corners at ridiculous speeds.
If your voiceover runs longer/shorter, adjust the SN array (script) and the scene a/b times at the top of index.html.

## Verification report (honest)
Verified here: FBX parsed offline (133 meshes, hierarchy, bounds, wheel/wing/floor geometry, materials/textures list); all referenced textures present; EXR→JPG conversion; engine.mp3 decoded offline (47.06 s, continuous tonal engine sound, no speech); index.html passes a Node syntax check; timeline logic unit-tested in Node (scene contiguity, scene cuts aligned to sentence starts, seek determinism, exact 4× v² ratio).
NOT verified: the app (including the MP4 exporter and the live recorder) has not been run in a browser (no network in my environment, so Three.js could not be loaded) — rendering, material look/exposure, camera framing, FBX load in Three.js, label placement, how the engine loop sounds (seam, level), performance, and the recording/export workflow are untested. Expect to tune camera positions/exposure after a first look; if something fails to load, the in-page error and the console name the file.
