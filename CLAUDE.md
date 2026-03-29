# seantallen.com

Personal website built with Hugo and the Blowfish theme.

## Building and Testing

Hugo is not installed locally. Use Docker:

```bash
# Dev server
docker run --rm -p 1313:1313 -v $(pwd):/src hugomods/hugo:exts server --bind 0.0.0.0

# One-shot build
docker run --rm -v $(pwd):/src hugomods/hugo:exts --gc --minify
```

Config changes require a server restart. Content changes live-reload.

## Page Bundles

All content uses page bundles (`content/posts/SLUG/index.md`). To add
new content:

```bash
mkdir -p content/posts/SLUG
```

Create `index.md` inside, and drop `feature.png` alongside it for the
hero/thumbnail image.

## Feature Images

Images are generated via ChatGPT (DALL-E) and added as `feature.png`
in each page bundle.

**Constants** (every image):
- Wide landscape (16:9 aspect ratio)
- Keep ALL details in the **center band** of the image height — leave
  the top 30% and bottom 10% as simple background. The hero crops the
  top and bottom (desktop shows only ~288px tall from a full-width image)
- No text in the image
- Bold, expressive, playful energy

**Style rotation** — vary the style across posts so they feel distinct
but cohesive. Pick a different style each time; don't repeat the
previous post's style.

| Style | Prompt fragment |
|-------|-----------------|
| Clean cartoon | whimsical cartoon style with bold outlines and warm colors |
| Street graffiti | cartoony graffiti style with bold spray-paint outlines, vivid colors, and dripping paint accents |
| Ren & Stimpy | exaggerated cartoon graffiti style — think Ren & Stimpy meets street art. Thick uneven spray-paint outlines, dripping ink, splashy neon colors, warped perspective like a Warner Brothers background |
| Retro comic book | retro comic book style with halftone dots, bold primary colors, thick ink outlines, and dramatic action poses |
| Watercolor cartoon | loose watercolor cartoon style with soft bleeding edges, bright washes of color, and playful pen-ink outlines over the paint |
| Pop art | bold pop art style with flat vibrant colors, thick black outlines, Ben-Day dots, and Lichtenstein-inspired dramatic framing |
| Dexter's Lab | angular geometric cartoon style inspired by Dexter's Laboratory — sharp shapes, exaggerated proportions, bold flat colors, and dramatic perspective |
| Ed Edd n Eddy | wobbly hand-drawn cartoon style inspired by Ed, Edd n Eddy — thick squiggly outlines that never sit still, crayon-like coloring, and manic energy |
| Cow and Chicken | grotesque rubbery cartoon style inspired by Cow and Chicken — thick outlines, absurdist proportions, bold colors, and chaotic slapstick energy |
| Roger Rabbit | classic golden-age cartoon style inspired by Who Framed Roger Rabbit — exaggerated squash and stretch, vibrant Technicolor palette, glossy ink outlines, and vaudeville showmanship |
| Wacky Races | Hanna-Barbera cartoon style inspired by Wacky Races — clean bold lines, flat colors, speed lines, and that 60s-70s adventure cartoon energy |
| Powerpuff Girls | thick outlines, candy colors, simple geometric shapes, and high-energy action inspired by Powerpuff Girls |
| Gorillaz | gritty urban cartoon style inspired by Jamie Hewlett's Gorillaz album art — heavy ink, street culture aesthetic, moody lighting, and cool detachment |
| Invader Zim | angular and dark but colorful cartoon style inspired by Invader Zim — sharp geometric shapes, moody dramatic lighting, slightly menacing whimsy, and neon accents against dark backgrounds |
| Archer | flat shading, clean vector lines, and mid-century modern palette inspired by Archer — deadpan cool, sharp angles, and cinematic framing |
| Jungle Book | classic Disney hand-painted animation style inspired by The Jungle Book — soft pencil lines, lush watercolor backgrounds, warm earthy tones, and expressive character animation |
| Chanoir street mural | bold Latin American street art style with thick black outlines, saturated candy colors, mischievous cartoon characters, layered overlapping figures, bubble lettering, and a packed wall-mural composition where every inch is filled |
| Peter Bagge's Hate | underground comics style inspired by Peter Bagge's Hate — exaggerated rubbery anatomy, bulging eyes, angular distorted perspectives, grungy ink lines, and sardonic slacker energy |

**Prompt template:**

> Create a wide landscape illustration (16:9 aspect ratio) in a
> [STYLE FROM TABLE]. Keep ALL details in the center band of the image
> height — leave the top 30% and bottom 10% as simple background.
> [SCENE DESCRIPTION]. No text in the image.

## Deployment

Netlify builds automatically on push. Deploy previews on PRs.
