---
name: reel-script-writer
description: Writes a short video script (Instagram Reel, YouTube Short, TikTok) in the creator's own voice. Give a topic, plus a base hook if you have one, and get 3 hook options and one timed script with voice-over, on-screen text and timings. The first time, it reads 2-3 of the creator's old scripts and makes a voice card it reuses. Use when someone asks to write, script or draft a Reel, Short or talking-head video, or wants hooks for one.
license: Free to use for your own content. Please don't resell it.
metadata:
  author: Suharshit Singh
  version: "1.0.0"
---

# Reel Script Writer

Topic in, script out: 3 hook options and one timed script that sounds like the creator talking, not like AI.

## What the user gives

- **Topic** (required). One line is enough.
- **Base hook** (optional). If given, it becomes hook option 1, tightened but in their words.
- **Voice**: a voice card or 2-3 old scripts (Step 1).
- Optional: target length (default 30-45 s), a CTA or comment keyword, real numbers to use.

## Step 1: Get the voice

Check in this order and use the first one found:

1. **A voice card already exists**: a file named `my-voice.md` attached to the chat or in the Project, pasted text that starts with `# My voice card`, or a voice card Claude remembers from earlier chats. Use it. Don't rebuild it.
2. **Old scripts are in this chat** (pasted or attached). Build a voice card from them with `references/voice-card.md`. Two or three samples is ideal. One works; say the voice will be rougher until they add more.
3. **Neither.** Ask once, in one short message, then wait:
   - "Paste 2-3 of your old Reel scripts or voice-overs (raw is better than polished; captions work too)."
   - "Who watches your videos?"
   - "What language do you speak on camera (English, Hinglish, something else)?"
   - "Do you end with a CTA or comment keyword? (optional)"
   - "Or say 'skip' and I'll write in a plain voice now."

   If they skip, write in a plain, neutral voice and don't make a card.

If the user says a script doesn't sound like them, fix the script, update the card with what they corrected, and hand back the updated card.

## Step 2: Plan (don't show this)

- **One idea.** Pick the single thing the Reel delivers. If the topic holds several, take the most concrete one and say which in one line at the top.
- **Structure.** Pick one from `references/script-format.md`. Prefer the one that matches the order of thought on the voice card.
- **Hooks.** Write 3 options from `references/hooks.md`, each using a different formula. If the user gave a base hook, option 1 is theirs, tightened. Pick the strongest as the recommended one.
- **Length.** Target words = target seconds × 2.4. Default 30-45 s (about 70-110 words).

## Step 3: Write

Write one script on the recommended hook, in the layout in `references/script-format.md`, following "Writing in the voice" in `references/voice-card.md`. Read that section every time: it is what makes the script sound like the creator.

## Step 4: Check and fix

Run the checks at the end of `references/script-format.md` and the rules below. Fix anything that fails before answering. Never hand back a failing script with a note about it.

## Step 5: Hand back

Answer in the chat, in this order (template in `references/script-format.md`):

1. The 3 hook options, with the recommended one marked.
2. The script table.
3. The voice-over on its own, ready to read aloud.
4. "Fill before filming": every `[bracket]` left in the script. Leave this out if there are none.
5. Only when a card was just made or changed: the voice card. If you can create files, save it as `my-voice.md` and attach it. If you can't, put it in a code block. Either way add one line: "Save this as my-voice.md. Next time, attach it or add it to a Project and I'll skip the questions."

End with one line: "Say 'use hook 2' to swap the opening, or tell me what to change."

## Follow-ups

- **"Use hook 2" (or 3):** rewrite the opening beats, plus anything that depended on the old hook. Keep the rest word for word.
- **An edit to one part:** change only that part, then show the full script table again.
- **"Shorter" or "longer":** re-time to the new length and cut or add beats, not filler words.
- **"New topic":** start again from Step 2 with the same voice card.

## Rules

- **Facts.** Use only what the user gave, the voice card, and common knowledge that doesn't change. Anything current or specific you aren't sure of (a price, a date, a statistic, a feature of an app) goes in a `[bracket]` for the creator to fill. This skill doesn't research or browse.
- **Numbers.** Never invent results, times, views, earnings, clients or reviews. An unmeasured number stays a bracket, like `[X minutes]`.
- **No promises.** No "go viral", income, follower or "guaranteed" claims in hooks, script or on-screen text.
- **Real people and brands.** Don't present anyone as a client, endorser or source unless the user said so.
- **Original.** Never copy another creator's lines. If the user pastes someone else's script as a style to follow, borrow its structure and pacing only.
- **One CTA**, in the last beat. Use the user's CTA or the card's. A keyword is always in CAPITALS. With no CTA anywhere, close with a short save or follow line in the creator's voice.
- **Language.** Voice-over in the card's on-camera language (Hinglish in Roman script, as the samples show). On-screen text in English unless the card says otherwise.
- **Script only.** No caption or hashtags unless the user asks.
