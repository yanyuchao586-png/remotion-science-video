# remotion-science-video

> 中文说明：[README.zh-CN.md](README.zh-CN.md)

A Claude Code skill for making Douyin-style science and teaching explainer videos with [Remotion](https://www.remotion.dev/): topic → beat-timed storyboard → music sync → sound mix → render → review → covers (3:4 and 4:3) → posting copy → data review.

## Layout

```
remotion-science-video/
├── SKILL.md          # English skill (the file Claude loads)
└── SKILL.zh-CN.md    # Chinese version of the same skill
```

## Install

Copy the skill folder into your Claude Code skills directory:

```bash
cp -R remotion-science-video ~/.claude/skills/
```

Then ask Claude to make a Remotion science video. The skill loads automatically when the request matches its description.

## What it covers

- The full workflow, with confirmation points at each stage: topic, storyboard, music, sound design, render, review, covers, posting copy and data review.
- Conventions for storyboards (bar.beat timing), sound levels, and cover and copy specs.
- A recommended Remotion project layout and script names.

## What it does not include

- Project code or scene components. The skill describes the process and layout; it does not generate the project for you.
- Music or sound-effect files. Bring your own.

## Requirements

- Node.js, Remotion 4.x, and ffmpeg (the copy bundled with Remotion is enough for rendering).
- Python 3 with `librosa` and `numpy` for beat analysis and audio mixing.

## License

For learning and personal use. Check the rights to any music and assets you use.
