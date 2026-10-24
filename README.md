# Free Claude Skills for creators

Two free Claude skills that write your content in your own voice.

| Skill | You give it | You get back | Download |
| --- | --- | --- | --- |
| **Reel Script Writer** | A topic, plus a base hook if you have one | 3 hook options and one timed script: voice-over, on-screen text, timings | [reel-script-writer.zip](downloads/reel-script-writer.zip) |
| **Carousel Writer** | A topic | 3 slide-1 hooks, one 6–8 slide carousel (headline, body, visual note per slide) and a caption with hashtags | [carousel-writer.zip](downloads/carousel-writer.zip) |

Both work for Instagram Reels, YouTube Shorts, TikTok and LinkedIn carousels, in any niche.

## How it learns your voice

The first time you use either skill, it asks for 2–3 of your old scripts or captions and a few quick questions. From those it makes a short **voice card** (`my-voice.md`).

Save that file. Next time, attach it to the chat or add it to a Claude Project, and the skill skips the questions. One voice card works in both skills.

## Install

### Claude app (web, desktop or mobile), including the free plan

1. Download the ZIP for the skill you want from the table above. Don't unzip it.
2. In Claude, go to **Settings → Capabilities** and turn on **Code execution and file creation** (called **Create and edit files** in newer versions). Skills need it.
3. Go to **Customize → Skills**, click **+**, then **Create skill → Upload a skill**, and pick the ZIP.
4. Make sure the skill is toggled on in your skills list.
5. Start a new chat and ask, for example:
   - "Write a Reel script about how I meal-prep for the week"
   - "Write a carousel on 5 mistakes new bakers make"

### Claude Code

Copy the skill folder (not the ZIP) into `~/.claude/skills/`:

```bash
git clone https://github.com/Suharshit/Free-Claude-skills.git
cp -r Free-Claude-skills/reel-script-writer Free-Claude-skills/carousel-writer ~/.claude/skills/
```

## Things they will and won't do

- **Your numbers only.** They never invent views, results, times or earnings. Anything they can't know is left as a `[bracket]` for you to fill, and listed at the end.
- **No research.** They write from what you give them. Prices, dates and stats they're unsure of become brackets, not guesses.
- **No hype.** No "go viral" or income promises, no filler like "game-changer".
- **Original.** They don't copy other creators. If you share someone else's post as a style, they borrow the structure only.
- **One CTA.** If you give a comment keyword, it goes at the end in CAPITALS.

## What's in this repo

```
reel-script-writer/     skill source (SKILL.md + references)
carousel-writer/        skill source (SKILL.md + references)
downloads/              the same skills as ready-to-upload ZIPs
```

## License

Free to use for your own content, personal or commercial. Please don't resell these skills or repackage them as your own.

Made by Suharshit Singh.
