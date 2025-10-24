# Sora 2: Prompting Guide

## Crafting a Successful Video Prompt

### Before You Prompt

Think of prompting like briefing a cinematographer who has never seen your storyboard. If you leave out details, they’ll improvise – and you may not get what you envisioned. By being specific about what the “shot” should achieve, you give the model more control and consistency to work with.

But leaving some details open can be just as powerful. Giving the model more creative freedom can lead to surprising variations and unexpected, beautiful interpretations. **Detailed prompts give you control and consistency, while lighter prompts open space for creative outcomes.** The right balance depends on your goals and the result you’re aiming for. Treat your prompt as a creative wish list, not a contract. Like with ChatGPT, using **the same prompt multiple times will lead to different results** – this is a feature, not a bug.

Most importantly, be prepared to iterate. Small changes to camera, lighting, or action can shift the outcome dramatically. Collaborate with the model: you provide direction, and the model delivers creative variations.

This isn’t an exact science—think of the guidance below as helpful suggestions we’ve learned from working with the model.

---

## API Parameters

The prompt controls the content of the video, but certain attributes are governed only by API parameters. You cannot request them in prose, they must be set explicitly in your API call:

- **model**: `sora-2` or `sora-2-pro`.
- **size**: a string in the form {width}x{height}. Supported resolutions:
    - sora-2: 1280x720, 720x1280
    - sora-2-pro: 1280x720, 720x1280, 1024x1792, 1792x1024
- **seconds**: the clip length; supported values: “4”, “8”, “12” (default is “4”).

These parameters are the video’s container – resolution, duration, and quality will not change based on prose like “make it longer.” Set them explicitly in the API call; your prompt controls everything else (subject, motion, lighting, style).

---

### Video Resolution

Video resolution directly influences visual fidelity and motion consistency in Sora. Higher resolutions generate detail, texture, and lighting transitions more accurately, while lower resolutions compress visual information, often introducing softness or artifacts.

### Video Length

The model generally follows instructions more reliably in shorter clips. For best results, aim for concise shots.

---

## Prompt Anatomy That Works

A clear prompt describes a shot as if you were sketching it onto a storyboard. State the camera framing, note depth of field, describe the action in beats, and set the lighting and palette. Anchoring your subject with a few distinctive details keeps it recognizable, while a single, plausible action makes the shot easier to follow.

Describing multiple shots in one prompt is valid if you need a sequence. Keep each shot distinct: one camera setup, one subject action, one lighting recipe at a time. Treat each shot as a unit.

- Shorter prompts = more creative freedom; expect surprises.
- Longer, detailed prompts = more control, but may not always be followed reliably.

**Short Prompt Example:**

> In a 90s documentary-style interview, an old Swedish man sits in a study and says, "I still remember when I was young."

- `90s documentary` sets the style.
- `an old Swedish man sits in a study` describes subject/setting.
- `and says, ...` describes dialogue.

Details left open will be filled by the model (e.g., time of day, outfit, tone, etc.), so unless you specify, Sora will improvise.

---

### Going Ultra-Detailed

For complex, cinematic shots, specify look, camera setup, grading, soundscape, and rationale—much like a director briefs a crew. Use cues for lensing, filtration, lighting, grading, and motion.

**Ultra-Detailed Example:**

    Format & Look Duration 4s;
    180° shutter; digital capture emulating 65mm photochemical contrast; fine grain; subtle halation on speculars...
    Lenses: 32mm / 50mm spherical, Black Pro-Mist
    Grade: clean morning sunlight, amber lift; neutrals; teal shadows; soft blacks...
    Lighting: natural sunlight camera left; ultrabounce silver; sodium platform lights...
    Atmosphere: gentle mist; train exhaust...
    Location: urban commuter platform, dawn...
    Wardrobe: navy coat traveler, extras muted tones...
    Sound: rail screech, train brakes hiss, distant announcement, footsteps...

Each shot then gets block-by-block camera notes and actions.

---

## Visual Cues Steer the Look

**Style** is a powerful lever for guiding the model (e.g., “1970s film,” IMAX scene, 16mm B&W), framing all choices. Details will read differently depending on style chosen.

**Clarity Wins:** Instead of “a beautiful street,” use “wet asphalt, zebra crosswalk, neon sign reflection.”

| Weak prompt                        | Strong prompt                                    |
|------------------------------------|--------------------------------------------------|
| A beautiful street at night        | Wet asphalt, zebra crosswalk, neon signs in puddles|
| Person moves quickly               | Cyclist pedals three times, brakes, and stops    |
| Cinematic look                     | Anamorphic lens, shallow DOF, volumetric light   |

**Camera Examples:**
- wide establishing shot, eye level
- aerial wide shot, slight downward angle
- medium close-up shot, slight angle from behind

**Camera Motion Examples:**
- slowly tilting camera
- handheld ENG camera

---

## Control Motion and Timing

Movement is hardest, so keep it simple—one camera move and one subject action. Actions work best described in beats/counts so they feel grounded in time.

| Weak                        | Strong                                                 |
|-----------------------------|--------------------------------------------------------|
| Actor walks across the room | Actor takes four steps, pauses, and pulls curtain      |

---

## Lighting and Color Consistency

Light = mood. Describe quality and color anchors. Instead of “brightly lit room,” specify: “soft window light with a warm lamp fill, cool rim from hallway.” Naming three to five colors helps.

| Weak                                                | Strong                                                     |
|-----------------------------------------------------|------------------------------------------------------------|
| Lighting + palette: brightly lit room               | Soft window light, warm lamp, cool rim; amber, cream, walnut|

---

## Use Image Input For More Control

Use the `input_reference` parameter in POST /videos with a reference image (JPEG, PNG, WEBP, matching target resolution).

Example input/output pairs show style transfer and direct shot control.

Tip: Use [OpenAI’s image generation](https://platform.openai.com/docs/guides/image-generation) to produce reference images for Sora.

---

## Dialogue and Audio

Dialogue must be described directly, below the visual description. Keep lines concise and natural. For multi-character scenes, label speakers and alternate turns.

Sound tips: Suggest rhythm/timing, e.g., "distant traffic hiss," even in silent shots.

**Dialogue Example:**
> Detective: "You’re lying. I can hear it in your silence."
> Suspect: "Or maybe I’m just tired of talking."
> Detective: "Either way, you’ll talk before the night’s over."

---

## Iterate With the Remix Functionality

Remix makes controlled changes. Pin a result and only describe the tweak (“same shot, switch to 85mm”). If a shot fails, simplify, then layer on detail step by step.

| Original Video | Remix Generated Video |
|----------------|----------------------|
| Monster (purple)| Monster (orange)     |
| Monster (purple)| Second monster appears|

---

## Prompt Templates and Examples

### Prompt Structure

Use sections for each kind of info. Not every detail needs inclusion—leave things open for creativity. Highly descriptive prompts = controlled results; lighter prompts unlock freshness.

**Descriptive Prompt Template:**

> [Prose scene description. Detail characters, costumes, scenery, weather, etc. Be descriptive.]
> Cinematography: Camera shot: [framing/angle]
> Mood: [overall tone]
> Actions:
> - Action 1: specific gesture/beat
> - Action 2: another beat
> - Action 3: another action or dialogue
> Dialogue: [Add lines. Keep brief.]

---

### Prompt Examples

**Example 1**
Style: Hand-painted hybrid animation, soft textures, warm lighting, tactile stop-motion feel. Cozy, mechanical charm. Workshop with robot on bench, glowing eyes, nervous; rain patters, clock ticks.
Cinematography: medium close-up, slow push-in, gentle parallax; 35mm lens, shallow DOF.
Lighting: warm overhead, cool spill from window. Mood: gentle, whimsical, suspenseful.
Actions:
- Robot taps bulb, sparks.
- Flinches, eyes widen.
- Bulb tumbles, caught in time.
- Puff of steam; pride.
- Robot (quietly): "Almost lost it… but I got it!"
Background: rain, ticking, bulb sizzle.

**Example 2**
Style: 1970s romantic drama, shot on 35mm film. Soft focus, warm halation, gate weave, grain, vignette. Tenement rooftop, laundry sways, woman dances in flowing red dress, partner claps, city noise below.
Cinematography: medium-wide, slow dolly-in, eye level; 40mm spherical, shallow focus.
Lighting: golden hour, tungsten bounce, fairy bulbs. Mood: nostalgic, tender, cinematic.
Actions:
- Dress flares, sunlight.
- Woman: "See? Even the city dances with us tonight."
- Partner: catches hand, dips her.
- Man: "Only because you lead."
- Sheets veil and part skyline.
Background: wind, fabric, music, street noise.

