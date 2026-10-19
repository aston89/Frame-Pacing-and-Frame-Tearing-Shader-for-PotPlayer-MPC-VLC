/*
    Frame Pacing / Tearing Test (Moving Vertical Bar)
    For PotPlayer pixel shaders (legacy HLSL / PS 3.0 style)

    Purpose:
      A high-contrast vertical bar sweeps horizontally across the video.
      Watch for uneven jumps (stutter / irregular frame pacing) and for
      horizontal discontinuities where the bar appears at different X
      positions above and below a scanline (possible tearing).

    Notes:
      - Motion uses the shader clock in seconds, not the video timestamp.
      - CLOCK_SPEED is the fraction of the screen crossed per second.
        0.50 = one full sweep in 2 seconds; 0.25 = 4 seconds.
      - This is a visual diagnostic, not a frame-time meter.
*/

sampler s0 : register(s0);
float4 p0 : register(c0);
float4 p1 : register(c1);

#define Clock       (p0[3])

// --- User controls ------------------------------------------------------
#define CLOCK_SPEED 0.50   // screen widths per second; 0.50 = 2 s per sweep
#define OUTER_HALF  0.010  // black outline half-width, fraction of screen width
#define INNER_HALF  0.003  // bright bar half-width, fraction of screen width

float4 main(float2 tex : TEXCOORD0) : COLOR
{
    float4 src = tex2D(s0, tex);

    // Bar position follows elapsed shader time and wraps at the right edge.
    float barX = frac(Clock * CLOCK_SPEED);
    float dx = abs(tex.x - barX);

    // Black surround makes the coloured bar visible on bright and dark footage.
    if (dx < OUTER_HALF)
        src.rgb = float3(0.0, 0.0, 0.0);

    // Bright magenta core: easy to track, including against most scenes.
    if (dx < INNER_HALF)
        src.rgb = float3(1.0, 0.0, 1.0);

    return src;
}
