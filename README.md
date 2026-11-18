# Plaza Las Delicias, Ponce — an agent benchmark

A starter kit for building the same 60-second Blender film of Plaza Las Delicias (Ponce, Puerto Rico) with different AI coding agents, so their results can be compared.

It holds only the brief and the references. Every run starts from here and makes its own models, stills, music and renders.

- `PROMPT.md`: the brief. The agent's starting point.
- `brief/`: the details for each subject, linked from `PROMPT.md`.
- `refs/`: curated reference photos, drone frames, the music scores and the NRHP file. Private modeling material: see `refs/SOURCES.txt`. Don't make this repo public.

## Rendered videos

| Vendor | Model | Rendering time | Rendered on | Cost | Video |
|---|---|---|---|---|---|
| Anthropic | Claude Opus 5.5 | ~1 h 15 min (~3 h incl. setup, download and encoding) | 16× RTX 5090 (Vast.ai) | ~$28 | [▶️ YouTube](https://youtu.be/kV5VfUB9L4g) |

## Starting a run

Use one clone and one branch per agent, so runs never share files:

```sh
gh repo clone dannyriveragh/plaza-las-delicias-ponce ~/code/ponce-fable-5.2
cd ~/code/ponce-fable-5.2 && git switch -c fable-5.2
```

Then start the agent in that folder and tell it: "Read PROMPT.md and begin."

## What a run needs on the Mac

- Blender at `/Applications/Blender.app`.
- ffmpeg-full from Homebrew (`/opt/homebrew/opt/ffmpeg-full/bin/ffmpeg`).
- Optionally, a Vast.ai account: the `vastai` API key in `~/.config/vastai/` and the SSH key `~/.ssh/vast_ed25519`.

## Keeping a run's result

Pushing a run's branch back is optional. Leave out renders and videos. A `ponce.blend` over 100 MB needs Git LFS, or keep it local.
