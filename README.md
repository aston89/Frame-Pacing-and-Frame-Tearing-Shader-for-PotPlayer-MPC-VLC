#  Frame Pacing and Tearing Test shader
A lightweight pixel shader for media players that displays a moving, high-contrast vertical bar to help visually inspect frame pacing irregularities and possible screen tearing during video playback.

## Features
- **Frame pacing inspection:** Watch the bar's movement for uneven jumps or irregular motion.
- **Possible tearing detection:** Look for horizontal discontinuities where the bar appears at different horizontal positions above and below a scanline.
- **High visibility:** A bright magenta core with a black outline remains visible against most video content.
- **Adjustable speed and size:** Change the shader's parameters to suit your display and preferences.
- **No external dependencies:** Uses legacy HLSL syntax compatible with PotPlayer's pixel shader system.

## Instructions
1. Use the shader `FramePacingTearingTest.txt` if you use PotPlayer.
2. Use the shader `FramePacingTearingTest.hlsl` if you use MPC, VLC or any other media player who need an hlsl file instead of a txt.

Menu names and shader installation paths may vary between PotPlayer versions.

## Configuration
Edit the following constants near the top of the shader source:

```hlsl
#define CLOCK_SPEED 0.50
#define OUTER_HALF  0.010
#define INNER_HALF  0.003
```

| Parameter | Description |
|---|---|
| `CLOCK_SPEED` | Horizontal sweep speed, measured in screen widths per second. |
| `OUTER_HALF` | Half-width of the black outline, as a fraction of the screen width. |
| `INNER_HALF` | Half-width of the magenta bar, as a fraction of the screen width. |

At the default speed of `0.50`, the bar completes one horizontal sweep every two seconds.

## How to interpret the result
- **Smooth, consistent movement:** No obvious visual pacing irregularity is apparent in this test.
- **Uneven jumps or inconsistent motion:** May indicate stutter or irregular frame presentation.
- **Horizontal discontinuities:** May indicate screen tearing.

The bar's position is driven by PotPlayer's shader clock, not by the video's presentation timestamps.

## Limitations
This is a visual diagnostic, not a frame-time measurement tool. Results can be influenced by the renderer, vertical synchronization, display refresh rate, and the playback pipeline.
A smooth result does not prove that every frame is presented correctly and an apparent discontinuity alone does not establish the precise cause of tearing.
