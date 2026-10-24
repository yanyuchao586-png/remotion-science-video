---
name: remotion-science-video
description: Repeatable workflow for making vertical or landscape Douyin-style science/teaching explainer videos with Remotion: topic shortlist, beat-timed storyboard, music cutting, SFX and level balance, rendering, review, covers (3:4 and 4:3), posting copy and data review. Use when the user asks to make, plan, or fix a Remotion science or teaching video, storyboard, cover, beat-synced music, sound mix, or posting copy.
---

# Remotion science video workflow

> 中文版：[SKILL.zh-CN.md](SKILL.zh-CN.md)

Run the steps in order. Stop at every confirmation point and wait for the user. Do not skip ahead.

Paths in this document are relative to the project root (the folder that holds `package.json`). Replace `<project>` with the real folder when you use it.

## 0. Default spec (change only if the user asks)

- Landscape 1920×1080 at 60 fps is the default. Use 30 fps only if the user asks.
- No voiceover. Burn Chinese subtitles into the frame, and keep them above the bottom ~350 px, where the platform UI covers them.
- Aim for 45–75 seconds. If the content needs more, split it into a numbered series. Longer cuts (100–120 s) are acceptable when the opening is strong.
- Music: the track the user supplies. Ask for the file and keep a copy in `public/`. Record where it came from. If the user can't supply a track they own, generate a simple beat-based bed with a script (see `scripts/make_bgm.py` if it exists in the project).

## 1. Topic (wait for the user to choose)

- Propose 3–5 candidates. For each one give:
  - a one-line hook, shown as a large question on the first frame;
  - the "aha" mechanism a viewer can retell afterwards;
  - one everyday example;
  - why it will hold attention.
- Avoid topics that are already saturated, and topics that are just a list of facts with no chain of reasoning.
- Test: after watching, can a viewer explain the mechanism to a friend in one sentence?
- The user will pick one and ask for a storyboard with specs.

## 2. Storyboard (wait for the user to approve)

Write `分镜.md` (or `storyboard.md`) in a new folder for the video. It must contain:

1. **Music analysis:** BPM, bar count, sections (e.g. the drop, the pause before the answer, the low-pass section), and the second and bar where each section ends.
2. **Shot table:** time in seconds plus bar.beat (e.g. `V06.1`), visual, camera motion, on-screen text and subtitles, sound effects, and the beat each cut lands on.
3. **Opening 0–2 s design:** the first frame already shows the image and the question. The viewer guesses an answer (usually wrong), then the video reverses it.
4. **Sources** for the science or case studies.
5. **2–3 still sample frames** so the user can judge the look before building.

Write every time as bar.beat. Picture, subtitles and sound all read from one shared beats data file, so a timing change only means editing the bar number.

When the user says "start", build. When they say "change", revise the storyboard and wait again.

## 3. Music sync

- Analyse the track (librosa in a local `.venv`, or any equivalent). Get BPM and downbeats, then line each cut and key motion up with a beat.
- If the user wants a different length or pacing, rebuild the track from bar ranges (cut and join) rather than stretching it, so the pitch stays the same. A music script can do this from a list of bar ranges.
- Put the drop where the answer is revealed or the main character appears. Use the sections around it for pauses, one element per beat.
- If the pacing feels too fast, hold reading shots for two bars and give them an action every two beats. Hold transition shots for one extra bar.

## 4. Sound design and mix (the points users push back on most)

- **Style:** soft, clean, tonal sounds in the Monument Valley / information-visualization spirit: marimba, bell, glass, soft whoosh, low pulse.
- **Avoid:** realistic squeals, horns, harsh repeated stingers. For braking or amplification, use a falling pitch that gets heavier each time instead.
- **Levels:** the user has said sound effects should not be too quiet and music should not be too loud. Starting point, verified on earlier projects:
  - music at about ×0.6 gain;
  - effects peaking around −5.5 to −7 dBFS, sitting roughly 5–9 dB under the music median;
  - final loudness around −17 dBFS RMS, with a soft limiter against clipping.
  - Measure per scene (`ffmpeg -af volumedetect` or numpy RMS) before handing over.
- **Layering:** place effects in the gaps or on beats, not on top of the main melody.
- **Ending:** fade the picture and the music together over the last 2–2.5 s. Never cut hard.
- **Intro:** a 1 s music-free lead-in works well for a question-appears-then-scene-loads opening.

## 5. Build and render

- Project layout to aim for:
  - `src/data/`: beats, timeline and shot JSON;
  - `src/*.tsx`: scenes;
  - `scripts/`: music, audio, stills, contact sheet;
  - `package.json` with scripts such as `music`, `audio`, `render` (`remotion render Main out/video.mp4`) and `typecheck`.
- If you clone a project to make a new one, copy `node_modules` with a copy-on-write clone (`cp -Rc` on macOS) and symlink the Python venv instead of reinstalling.
- Render with `npm run render`. Rendering may need to run outside a sandbox.
- After rendering, check resolution, frame rate, duration, that an audio track exists, per-scene levels, and a contact sheet of frames. If you can't listen to the audio, tell the user the mix was checked by measurement only and that they should listen.

## 6. User review

- Send the rendered video to the user.
- Take notes one by one. Common feedback: balance between sound and music, pacing, the ending, subtitle position, unclear shots.
- Make covers only after the user says the video is good.

## 7. Covers (3:4 and 4:3)

- 3:4 = 1080×1440, the main feed. 4:3 = 1440×1080, for collections and profiles.
- Use a large question as the hook, the same one as the first frame. Highlight the key word in an accent colour. Check legibility at thumbnail size.
- You can take the first frame from the video or design a static image. View the result yourself before handing it over.

## 8. Posting copy (`发布文案.md` or `posting.md`)

- 3–5 titles, each with a reason. Recommend one. Put searchable words in the title (e.g. the topic's everyday name plus "为什么"), not only the project name.
- A 2–4 sentence description that ends with a comment prompt.
- Hashtags: 1 main, 2–3 audience, 1–2 content. Five to eight in total.
- Topical angles: list them as ideas only. Say you did not check the live hot list, and ask the user to confirm on the platform.
- A pinned comment with a poll or question to start discussion.
- Posting tips: the first two seconds are the question, and landscape video looks small in feeds, so use the 3:4 cover first.

## 9. Publish and review

- The user publishes the video. Do not publish on their behalf.
- After one to two days, ask for the numbers: 2-second drop-off, 5-second completion, average watch time, completion rate, like rate, and the feed vs search split. Small samples are only directional, so don't invent figures.
- Turn the findings into concrete rules for the next video, and store them in the project notes.

## Common pitfalls

- Music louder than effects: lower the music first, then raise effects to the target level.
- Hard cut at the end: add a fade.
- Harsh realistic effects: swap for tonal sounds.
- Reading shots too fast to read: lengthen them.
- An element covered by another (e.g. a stamp over a chart): check the contact sheet.
- Shell quoting problems with non-ASCII filenames: use absolute paths and quote them.
- Logins: never sign in to any website or platform for the user.
