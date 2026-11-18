# Camera and titles

## Camera — 60 s, 6 shots at 24 fps (1440 frames)

Rough lengths are below. Adjust them so cuts land on the music's phrase boundaries. See **Lighting, camera, look** in `PROMPT.md` for lenses, depth of field, motion blur and easing.

1. **About 10 s:** aerial establishing shot descending over the plaza, with the cathedral's silver domes and the red-and-black firehouse in frame. Use the drone frames as a guide for a believable drone-style move.
2. **About 11 s:** a pan across Parque de Bombas. It opens on the building; as the camera moves past the open bay, the fire truck inside comes into sharp focus, then goes soft again as the shot returns to the building. "PONCE" on the hood should be readable at the focus moment.
3. **About 10 s:** push-in on the cathedral's west facade, tilting up to the silver domes. Pigeons perch on the pediment, and some take off during the shot.
4. **About 9 s:** low, close pass around the Fuente de los Leones: lions, water, octagonal wall. Partway through, the camera gives one lion a deliberate, held moment in sharp focus.
5. **About 12 s:** tracking shot along the walkways past the Morel Campos statue, the Polvorín obelisk, and the Muñoz Rivera statue.
6. **About 8 s:** closing crane shot rising from Parque de Bombas to reveal the whole plaza, with the end card. A flock of pigeons crosses the frame.

## Titles — elegant serif lower-thirds

- **Font:** Cormorant Garamond (Google Fonts, OFL). Download the .ttf files from the google/fonts GitHub repo into `fonts/`.
- **Text style:** ivory white (#F5F0E6) with a soft, subtle drop shadow (no boxes or banners). Main line in Cormorant Garamond Medium; optional second line in small caps with wide letter spacing, separated by a thin ivory rule.
- **Sizes:** write the titles as an ASS subtitle file (`titles.ass`) with **PlayResX=2560, PlayResY=1440**. Main line about 86 px, second line about 38 px.
- **Placement:** lower-left third inside a 5% safe margin, never covering the subject. Move a title to the right if the subject is on the left.
- **Motion:** fade in over 0.8 s with a slight upward drift (`\fad` + `\move`), hold 3–4 s, fade out over 0.8 s. Only one title on screen at a time. Start each one about 1 s after its shot begins.
- **Titles by shot:**
  1. Aerial: "Plaza Las Delicias" / "PONCE, PUERTO RICO"
  2. Parque de Bombas: "Parque de Bombas"
  3. Cathedral: "Catedral de Nuestra Señora de Guadalupe"
  4. Lions fountain: "Fuente de los Leones"
  5. Tracking shot, each title appearing as its statue passes: "Juan Morel Campos", then "Monumento a los Bomberos del Polvorín", then "Luis Muñoz Rivera"
  6. Closing crane, a centered end card: "Plaza Las Delicias" / "PONCE, PUERTO RICO", and in the last 4 s a small centered credit: "Música inspirada en las danzas de Juan Morel Campos"
- **Burning them in:** burn titles in with ffmpeg-full's `subtitles` filter, with `fontsdir` pointing to `fonts/`. Before relying on it, confirm it's there: `/opt/homebrew/opt/ffmpeg-full/bin/ffmpeg -filters | grep subtitles`.
