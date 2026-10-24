---
name: carousel-writer
description: Writes the copy for an Instagram or LinkedIn carousel in the creator's own voice. Give a topic and get 3 slide-1 hook options, one 6-8 slide carousel (headline, body and a visual note for every slide) and a ready-to-post caption with hashtags. The first time, it reads 2-3 of the creator's old posts or captions and makes a voice card it reuses. Use when someone asks to write, plan or draft a carousel, slide post or swipe post, or wants slide-1 hooks for one.
license: Free to use for your own content. Please don't resell it.
metadata:
  author: Suharshit Singh
  version: "1.0.0"
---

# Carousel Writer

Topic in, carousel out: 3 slide-1 hooks, one carousel people want to save, and a caption, all in the creator's voice.

## What the user gives

- **Topic** (required). One line is enough.
- **Slide-1 idea** (optional). If given, it becomes hook option 1, tightened but in their words.
- **Voice**: a voice card or 2-3 old posts (Step 1).
- Optional: slide count (default 6-8), a CTA or comment keyword, the points they want covered, real numbers to use.

## Step 1: Get the voice

Check in this order and use the first one found:

1. **A voice card already exists**: a file named `my-voice.md` attached to the chat or in the Project, pasted text that starts with `# My voice card`, or a voice card Claude remembers from earlier chats. Use it. Don't rebuild it. (A card made by the Reel Script Writer skill works here too.)
2. **Old writing is in this chat** (pasted or attached): captions, carousel text, posts, Reel scripts. Build a voice card from it with `references/voice-card.md`. Two or three samples is ideal. One works; say the voice will be rougher until they add more.
3. **Neither.** Ask once, in one short message, then wait:
   - "Paste 2-3 of your old captions or carousel texts (raw is better than polished)."
   - "Who is this for?"
   - "Do you end with a CTA or comment keyword? (optional)"
   - "Or say 'skip' and I'll write in a plain voice now."

   If they skip, write in a plain, neutral voice and don't make a card.

If the user says the copy doesn't sound like them, fix it, update the card with what they corrected, and hand back the updated card.

## Step 2: Plan (don't show this)

- **One promise.** Decide the single thing the carousel gives the reader (a checklist, steps, a set of tips, a clear explanation). If the topic holds several, take the most useful one and say which in one line at the top.
- **Points.** List the points from what the user gave and from knowledge that doesn't go out of date. One point per content slide.
- **Structure.** Pick one from `references/carousel-format.md`.
- **Hooks.** Write 3 slide-1 options from `references/slide-hooks.md`, each from a different formula. If the user gave a slide-1 idea, option 1 is theirs, tightened. Pick the strongest as the recommended one.

## Step 3: Write

Write one carousel on the recommended hook, in the layout in `references/carousel-format.md`, following "Writing in the voice" in `references/voice-card.md`. Then the caption, per the caption rules in the same format file.

## Step 4: Check and fix

Run the checks at the end of `references/carousel-format.md` and the rules below. Fix anything that fails before answering. Never hand back failing copy with a note about it.

## Step 5: Hand back

Answer in the chat, in this order (template in `references/carousel-format.md`):

1. The 3 slide-1 hook options, with the recommended one marked.
2. The slides.
3. The caption with hashtags.
4. "Fill before posting": every `[bracket]` left in the slides or caption. Leave this out if there are none.
5. Only when a card was just made or changed: the voice card. If you can create files, save it as `my-voice.md` and attach it. If you can't, put it in a code block. Either way add one line: "Save this as my-voice.md. Next time, attach it or add it to a Project and I'll skip the questions."

End with one line: "Say 'use hook 2' to swap slide 1, or tell me which slide to change."

## Follow-ups

- **"Use hook 2" (or 3):** rewrite slide 1, plus slide 2 if it depended on the old hook. Keep the rest word for word.
- **A change to one slide:** change only that slide, then show all the slides again.
- **"More slides" or "fewer slides":** add or merge points; don't pad a slide or squeeze two points onto one.
- **"New topic":** start again from Step 2 with the same voice card.

## Rules

- **Facts.** Use only what the user gave, the voice card, and common knowledge that doesn't change. Anything current or specific you aren't sure of (a price, a date, a statistic, a feature of an app) goes in a `[bracket]` for the creator to fill. This skill doesn't research or browse.
- **Numbers.** Never invent results, times, earnings, clients or reviews. An unmeasured number stays a bracket, like `[X hours]`.
- **No promises.** No "go viral", income, follower or "guaranteed" claims on slides or in the caption.
- **Real people and brands.** Don't present anyone as a client, endorser or source unless the user said so.
- **Original.** Never copy another creator's slides. If the user shares someone else's carousel as a style to follow, borrow its structure only.
- **One CTA**, on the last slide and repeated once in the caption. Use the user's CTA or the card's. A keyword is always in CAPITALS. With no CTA anywhere, use a save or share line in the creator's voice.
- **Language.** Slides and caption in English unless the voice card says otherwise.
- **Copy only.** Give a short visual note per slide, but don't design or render slides unless the user asks.
