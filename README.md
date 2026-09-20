# 135. Petal Post

Build a bouquet stem by stem, tap the vase to open every bud, then send the finished arrangement to someone who needs one.

**Live:** https://augustineiacopelli.github.io/appaday-135-petal-post/
**Portfolio:** https://augustineiacopelli.github.io/appaday/

## Where this one came from

I had no idea what to build today. I said so out loud, and my wife suggested a blooming flowers app. That was the whole spark. I took it a step further than a single blossom: you choose the flower, you choose the color, you add as many stems as you want, and the bouquet arranges itself in a vase as closed buds. Then you tap to open them one at a time. Since a bouquet is something you give rather than something you keep, the app finishes by handing the finished image off to your mail app.

## What it does

Pick from six flower types (rose, tulip, daisy, sunflower, peony, lily), pick a color from ten swatches or a custom color picker, and add up to twelve stems in any combination. The arrangement fans the stems out of the vase with taller stems toward the center, gives each one its own scale, lean, and leaf placement, and drops a bud on top of every stem.

Tapping the canvas opens the tallest closed bud. The sepals peel back, the petals unfold ring by ring from the center out, and the bud shell fades as the flower takes over. Tap again for the next one. Once everything is open, a tap near any bloom shakes a few petals loose and lets them drift down across the scene. There is a Bloom them all button for impatience and a Back to buds button to reset the arrangement without losing it.

Four vase finishes are included: glass, ceramic, clay, and cobalt. The glass vase is translucent, so the stems show through the water line.

You can add a short note that is rendered onto the image in italic serif, like a card tucked into the arrangement.

## Sending it

Everything runs in the browser, so there is no server sending mail on your behalf. Send bouquet renders the scene to a 1080 by 1350 PNG and then takes the best available path:

On mobile, where the Web Share API accepts files, the image goes straight into the native share sheet. Choosing Mail there attaches the PNG for you, which is the cleanest version of this.

On desktop, the PNG downloads and your mail client opens with the recipient, subject, and note already filled in. Attaching the downloaded file is the one manual step, since browsers do not allow a page to attach a file to an email for you.

Save image skips the mail step entirely and just gives you the PNG.

## Technical notes

Single `index.html`, no frameworks, no build step, no dependencies beyond Google Fonts. Everything is drawn on a 2D canvas.

Every flower is procedural. A flower type is a set of petal rings with a count, a scale, a petal width ratio, a cup factor, and optional center, seed pattern, ruffle, and recurve flags. Petals are two cubic beziers with a vertical gradient from a darkened base to a lightened tip. The bloom is driven by one value per stem between 0 and 1: ring open factors are staggered off that value, petal height and width grow with it, a horizontal squeeze relaxes as it rises, the sepals rotate outward, and the bud shell alpha falls to zero across the first third. The result is that the same drawing code renders a closed bud, every stage of opening, and a full bloom.

Layout is fully resolution independent. Stems store a normalized fan angle, a length factor, and a jitter seed rather than pixel coordinates, and every position is computed from the current canvas width and height. That is what lets the export render the identical scene at 1080 by 1350 on an offscreen canvas without a separate layout path.

Drifting petals and tap sparkles are stored in normalized coordinates for the same reason.

Flower choice, color, vase, note, email, and the full stem list persist to localStorage inside try/catch. Reloading brings your bouquet back as closed buds so you get to open it again.

Canvas rendering avoids `ctx.roundRect()` and `ctx.ellipse()` per project convention. Ovals are drawn with a scaled arc inside a save and restore.

## Built for AppADay

One complete, functional, mobile friendly, visually polished web app every day. See the whole archive at https://augustineiacopelli.github.io/appaday/
