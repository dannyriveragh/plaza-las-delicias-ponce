# Plaza Las Delicias — 60-second Blender cinematic

Build Plaza Las Delicias in Ponce, Puerto Rico, in Blender and render a 60-second cinematic on a sunny day. The stars are the **Parque de Bombas** (with its restored 1920s fire truck visible inside) and the **Catedral de Nuestra Señora de Guadalupe**, followed by the plaza's fountains and monuments. Compose the music in code: an original danza inspired by Juan Morel Campos.

Keep this file open as your reference. Re-read it whenever you resume after a break or a context compaction. It is the hub: the subject details are in `brief/` (see **The brief, by subject** below).

---

## Ground rules

- **Work headless.** Use `/Applications/Blender.app/Contents/MacOS/Blender` from the CLI. No MCP. Report the Blender version at the start.
- **Use Cycles only, never EEVEE.** That covers stills, previews, benchmarks, drafts and finals. Use the Metal GPU device locally (see **Rendering on Vast.ai** for NVIDIA).
- **Stop at every checkpoint** and wait for my approval before continuing.
- **Use real-world meters.** Save the scene as `ponce.blend`.
- **Long-running commands** (renders, benchmarks, bakes) run in the background. Monitor them by reading their logs, so a tool timeout never kills them.
- **ffmpeg:** use `/opt/homebrew/opt/ffmpeg-full/bin/ffmpeg` for everything. The plain `ffmpeg` on this Mac lacks libass and freetype. This is ffmpeg 9: use `-fps_mode`, not the removed `-vsync`.
- **Python:** use a virtual environment in `.venv/` for any packages, not the system Python.
- **Git:** this folder is already a git repo. Commit at every checkpoint with a clear message so I can roll back. `.gitignore` should include `/frames/` (with the leading slash, so `refs/video/frames/` stays tracked), `*.mp4`, `*.wav`, `.venv/`, `__pycache__/`, and `*.blend1`.
- **Paths:** keep every path relative to this folder.

---

## References — the source of truth

I've hand-curated everything in `refs/`. Some photo folders are sparse on purpose; I deleted unusable images. `refs/SOURCES.txt` lists where each file came from. The references are for modeling only: never show them in the film.

**Layout references (highest priority):**
- `refs/video/frames/`: frames I picked from a drone video of the plaza. Use these first for layout, proportions and the look of each building from above.
- `refs/photos/01-aerial/`: Google Earth 3D screenshots. Use them for layout and proportions only; the Google 3D meshes are crude up close, so not for surface detail. Where a filename gives a view direction or measurements, trust it.

**Detail references:**
- `refs/photos/<subject>/`: photos by subject: plaza overview, Parque de Bombas, cathedral, Fuente de los Leones, north and south squares, the Morel Campos statue, other monuments, surroundings, Indian laurel trees, and the fire truck and museum hall (`13-parque-de-bombas-truck-and-interior`).
- `refs/nrhp/`: the NPS National Register file for Parque de Bombas, including its original description and photos.

**Music references:**
- `refs/music/`: three public-domain piano danzas by Juan Morel Campos (IMSLP scans): *La Olimpia, Op. 166*, *La Incógnita*, and *Una caña al aire*.

**Working with them:**
- **Before you start Checkpoint 1, ask me:** "Do you want to use only the reference pictures provided here, or do you wish to visit Alamy.com or other websites to find more reference pictures and place them in the correct folder?" If I add pictures, wait until I say I'm done, then add a line for each new file to `refs/SOURCES.txt` (its source, if I give one). Don't download from stock-photo sites yourself.
- To save tokens, first build low-res contact sheets of each folder with ffmpeg's `tile` filter. Then open only the images you need at full size, when you model that element. Don't re-open images you've already studied.
- **Where the references disagree with the brief, the references win.** If two references disagree with each other, tell me.

---

## The brief, by subject

Read `brief/01-layout.md` before Checkpoint 1. Read each other file when you start work on its subject, and re-read it before the checkpoint that shows that subject.

| File | Subject | Needed for |
|---|---|---|
| `brief/01-layout.md` | Squares, streets, orientations, surrounding buildings | Checkpoint 1 onward |
| `brief/02-parque-de-bombas.md` | The firehouse, outside and the museum hall inside | Checkpoints 1–2 |
| `brief/03-fire-truck.md` | The 1920s Ford Model A fire truck "Engine Co. No. 2" | Checkpoint 2 |
| `brief/04-cathedral.md` | Catedral de Nuestra Señora de Guadalupe | Checkpoints 1–2 |
| `brief/05-fuente-de-los-leones.md` | The lions fountain and its water | Checkpoint 2 |
| `brief/06-monuments.md` | Statues and fountains in both squares | Checkpoint 2 |
| `brief/07-grounds.md` | Walkways, benches, lampposts, trees, flower beds, pigeons | Checkpoint 2 |
| `brief/08-camera-and-titles.md` | The six shots and the titles | Checkpoint 3 |
| `brief/09-music.md` | The original danza | Checkpoint 3 |

---

## Quality — a high-end architectural visualization, not a blockout

### Geometry
- **Polygon budgets.** Hero assets get full geometric detail: Parque de Bombas, the fire truck, the cathedral's west facade and towers, and the Fuente de los Leones. Budget about 1–3 million triangles each. Surrounding buildings can be simpler (about 100–300k each) since they're backdrop.
- **Bevels.** Bevel every hard edge (Bevel modifier, 2–3 segments, a few cm wide) so edges catch the light. No razor-sharp box edges anywhere.
- **Smooth curves.** Curved forms must read smooth up close: at least 64 segments on cylinders and columns, at least 64×32 on the silver domes, plus subdivision where needed.
- **Real detail.** Model architectural detail as real geometry, not painted textures: cornices, moldings and trim swept from profile curves; recessed windows and doors with real depth; the fanlights and the pediment; the lampposts; the octagonal fountain wall with its coping.
- **Statues and animals.** Model the lions, statues and pigeons as real sculpted forms (Subdivision + sculpting, the Skin modifier with subdivision, or another approach that gets there) that read clearly as their subjects at the distances the cameras see them. No primitive stand-ins in final shots.
- **Instancing.** Use Geometry Nodes instancing for repeated elements: benches, lampposts, balusters, tree leaves, flowers and paving tiles.
- **Trees.** Real branching structure, dense instanced leaf cards (tens of thousands of leaves per tree), and a subtle wind sway driven by noise. Build them procedurally, not as sphere blobs.

### Materials
- PBR materials with roughness variation everywhere. Nothing perfectly uniform.
- **Weathering:** grime at wall bases, water staining under cornices and sills, slight color variation within painted surfaces, and wood grain visible on the firehouse siding. (The fire truck is the exception: see its brief.)
- **Surfaces:** the mosaic walkways need a real tile pattern with grout lines. Give the marble benches and fountain stone a subtle sheen. Wet stone around the fountain should be darker and glossier.
- **Domes:** they should look like brushed metal that catches the sun, not flat gray.

### Water
- Fountain jets as particle or mesh streams with splashes, plus a rippling basin surface (animated displacement).
- Bake every simulation and cache to disk inside the project folder.

### Lighting, camera, look
- **Sky:** Nishita physical sky (or a Poly Haven HDRI, downloaded via its public API) with the sun set for mid-morning in Ponce in summer. Clear blue sky with a few cumulus clouds, crisp shadows, sparkling water.
- **Color:** AgX view transform with a slight contrast boost. In the compositor: a gentle glare on highlights and a subtle vignette.
- **Lenses:** between 24 and 50 mm (avoid wide-angle distortion on the facades). Subtle depth of field on detail shots, and deliberate rack focus where the shot list asks for it. Motion blur on.
- **Camera moves:** smooth and eased (bezier ease in/out), with no jitter or sudden speed changes.

---

## Render settings (Cycles only)

| | Draft | Final |
|---|---|---|
| Resolution | 640×360 | **2560×1440** (scene base resolution) |
| Frames | every 3rd frame (480 frames, 8 fps), each held 3× in the video so it plays at 24 fps | all 1440 frames, 24 fps |
| Samples | 32, adaptive threshold 0.1 | up to 512, adaptive threshold 0.02 (tune from the benchmark) |
| Denoiser | OpenImageDenoise | OpenImageDenoise |
| Light bounces | max 4, filter glossy 1.0 | normal, filter glossy ~0.5 |
| Simplify | on (max subdivision 1) | off |
| Output | `frames/draft/####.png` | `frames/final/####.png` (8-bit PNG) |

- Persistent data on. Overwrite off and placeholders on, so every render is resumable and skips finished frames.
- Apply draft and final settings as command-line overrides (`--python-expr`, placed before `-a`), so `ponce.blend` is never edited just to switch quality. The same goes for the render device: Metal by default, with an override for OptiX or CUDA on NVIDIA.
- Wrap local renders in `caffeinate -i` so the Mac doesn't sleep.

---

## Rendering on Vast.ai (optional)

I have a Vast.ai account for renting NVIDIA GPUs (for example RTX 5090s) when a local render would take too long. Either the draft or the final may run there, but only once I've approved it.

**Make the project cloud-ready:**
- The render scripts accept a device override (Metal locally, OptiX on NVIDIA). Nothing else changes between local and cloud renders: same Blender version, same `ponce.blend`, same quality overrides.
- `ponce.blend` is self-contained: no absolute paths, and no caches, textures or assets outside the project folder.
- Frames can be split across many GPUs and machines.
- Before encoding, cloud frames are verified at home: every frame present, every file complete.

**Guardrails:**
- **Never rent a machine without my approval** of the plan, the machines and the expected cost.
- Use the `vastai` CLI installed in `.venv/`. My API key is already configured in `~/.config/vastai/`. Never print it, log it, or ask me to paste it.
- Connect only over SSH with `~/.ssh/vast_ed25519` (already registered with Vast.ai). Move files only over SSH/rsync; never open an unauthenticated server.
- Prefer hosts in North America.
- Keep the commands that ran in the repo (secrets excluded), so the render can be repeated.
- Destroy every instance as soon as its work is home and verified, and tell me what the run cost.

---

## Process — STOP at each checkpoint

### Checkpoint 1 — Layout and massing
- Build the ground plan, streets, both squares, and blocked-out versions of the cathedral, Parque de Bombas, fountains, monuments and surrounding buildings, at the real scale from the aerial references (`brief/01-layout.md`).
- Render 3 low-sample Cycles stills: an aerial, an east view of Parque de Bombas, and a west view of the cathedral. Show each side by side with the matching drone frame or Google Earth screenshot.
- Before showing me, confirm three things: the cathedral faces west, the firehouse is attached to its east end, and the lion fountain is in the south square.

### Checkpoint 2 — Detail and materials
- Build everything in `brief/02` through `brief/07`: the facades, domes, stripes, the museum hall and fire truck, fountains with water, statues, trees, flower beds, pigeons and paving.
- Show me 5–6 stills compared against the references, including at least one close-up of each hero asset (Parque de Bombas, the fire truck, the cathedral facade, the lions fountain), so I can judge bevels, curvature and materials.

### Checkpoint 3 — Music, cameras, benchmark
1. Do the music work in `brief/09-music.md` and give me `music-test.wav` (the first 20 s) plus `music.wav` (the full 60 s). Wait for my approval of the music.
2. Lay out the 6 camera shots in `brief/08-camera-and-titles.md`, timed to the music's phrases. Show me the first and last frame of each shot, plus a frame from the middle of each focus moment (shots 2 and 4).
3. Write and run `benchmark.sh` (in the background):
   - Render the middle frame of each shot with both the draft and final settings, timing each render.
   - Write `benchmark.md` with a per-shot table of seconds per frame at both quality levels, estimated total hours for a full draft and a full final on this Mac, and the draft and final sample stills side by side.
   - Add an estimate for rendering the draft and the final on rented NVIDIA GPUs (wall-clock time and cost for a sensible number of GPUs), and recommend local or cloud for each.
   - If the final estimate is long, suggest which settings to relax and what each would cost in quality.
4. Show me 3 stills with titles burned in (shot 1, the cathedral, and the end card) so I can check typography and placement.

### Checkpoint 4 — Draft video
- Write the scripts:
  - `render-draft.sh`: renders all draft frames to `frames/draft/`.
  - `render-final.sh`: renders at final settings to `frames/final/`. It accepts an optional shot number (e.g. `./render-final.sh 4`) to render just that shot, and otherwise renders whatever is missing.
  - `finish.sh draft|final`: combines that folder's frames with `music.wav` and burns in `titles.ass`.
    - `draft` produces `ponce-draft.mp4` (24 fps, scaled up to 2560×1440 so the titles render at full size).
    - `final` produces `ponce-final.mp4` (with titles) and `ponce-clean.mp4` (without titles).
    - Use H.264, CRF 16, preset slow, yuv420p, AAC 320 kbps, and `+faststart`.
  - All render scripts print a running percentage and ETA, log to `render.log`, and append every frame's render time to `timings.csv` so ETAs come from real numbers.
- Render the draft (locally in the background, or on Vast.ai if I approved that at Checkpoint 3) and monitor it, then run `finish.sh draft`. Tell me when `ponce-draft.mp4` is ready.

### Final — after I approve the draft
- I'll either run `render-final.sh` myself in a separate Terminal window, a shot or two per night, or ask you to run it on Vast.ai. Then `finish.sh final`.
- Before handing off, make sure:
  - `ponce.blend` is saved
  - all caches are baked
  - every script works from a fresh Terminal
  - `CREDITS.md` lists the font, every music sound source, and their licenses
