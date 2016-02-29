**What it does?**

Adds a photographic view transform for Blender. It is the ACES Shaper LMT that grabs ten stops below middle grey
and six and a half stops over middle grey. Middle grey is pegged at 0.18, which maps to approximately 0.62
display referred. Simply by using it your can elevate your work by an order of a magnitude. To quote Alex Fry:

> "With modern physically-based renderers, realism suffers if things aren't set up in a way that mirrors the real > world. Therefore, no matter how physically accurate your renderer is, if you build a universe where the sun 
> outside, and a character inside both have to live in the narrow band between 0->1, nothing will respond
> realistically.”

**How do I use it?**

Git clone this repository, backup your */blender/bin/[VERSION]/datafiles/colormanagement* directory, and replace or
link the repository to its place. The view is currently located in the colour management panel of the Scene panel
in Properties.

**What are the transforms?**

**Views**
 * *-10-+6.5* This is the primary view mapping middle grey to 0.18 which ends up approximately 0.62 scene linear.
 the basic view covers sixteen and a half stops of latitude, with ten stops down and six and a half stops above
 middle grey.
 * *Raw* This is an untransformed dump of the scene referred values to the display. Effectively meaningless.
 * *sRGB* This is the sRGB specification transfer curve. Called *Default* in Blender's default configuration,
 renamed here for clarity. It covers approximately eight and a bit stops of latitude, mapping middle grey 0.18 to
 approximately 0.467 scene referred. It covers six and a bit stops down to approximately two and a bit stops over
 middle grey. It is unsuitable for a rendering view.
 
**Looks**
 * *None* This removes any additional transforms on the view.
 * *Team Argentina -10-+6.5 Desaturation Basic* This view adds a convergence point towards display referred white.
  The curve towards white begins at scene referred value 3.0 and converges all primaries towards white as their
  intensity approaches scene referred 16.291. This emulates a basic filmic / DSLR "blow out" and helps shape
  all colours to desaturate correctly, as opposed to without any desaturation / convergence / unity point.
 * *Team Argentina -10-+6.5 Desaturation Sharp 1.6-2.4* This is a range of looks that, in addition to the basic
  desaturation described above, adds on an additional power curve to add contrast. This is designed to give
  imagers a basic idea as to what a grade might look like when modified atop of the basic view with desaturation.
 * *Greyscale on Desaturation* This is a greyscale preview of the desaturated look helpful for evaluating
  contrast and other details in some contexts.
 * *Greyscale* This is a greyscale preview without the desaturation look applied. As above.
 * *False Colour Basic* This is a false colour "heat map" of exposure. The colour scheme is as follows:
   * Low Clipping = Black        = 1.762728758E-4 SL 1.081976960E-5 N
   * Nine Stops Down = Purple    = 3.513840218E-4 SL 2.156823131E-5 N
   * Eight Stops Down = Blue     = 7.024112686E-4 SL 4.311456347E-5 N
   * Six Stops Down = Cyan       = 2.814643098E-3 SL 1.727650366E-4 N
   * Two Stops Down = Green      = 4.456791864E-2 SL 2.735614366E-3 N
   * Middle Grey = Green         = 1.800914272E-1 SL 1.105415533E-2 N
   * Two Stops Over = Green      = 7.196344767E-1 SL 4.417173771E-2 N
   * Four Stops Over = Yellow    = 2.883658483E+0 SL 1.770012559E-1 N
   * Six Stops Over = Red        = 1.139491214E+1 SL 6.994287888E-1 N
   * High Clipping = White       = 1.629174024E+1 SL 1.000000000E+0 N

**Help! Grading is tricky!**
The following is a brief list of cautionary bits for imagers while working on scene referred data:
 * Blender desperately needs independent colour transform controls on every UI element. Using this set of LUTs should make it abundently clear.
 * For the past decade or so, Blender's basic mixing node has been broken with regards to certain AdobePDF blend modes, as they are strictly display referred. Screen, Burn, Dodge, Overlay, and a few others are strictly display referred blend modes, and will not work on scene referred data.
 * Lift, Gamma, and Gain is a strictly display referred tool. It will not work with scene referred data.
 * Curves. While curves work fine, without the specific colour transformation selection, it is very tricky. Middle
grey lives at 0.18, which gives you approximately 18% of the UI's interface region to adjust a curve that will
result in tweaking ten stops of your most critical latitude.
**I get it, Blender's broken in fundamental ways. Solutions?**
The ASC CDL transform was designed to operate on both display referred and scene linear imagery. It is found in
the Colour Correction node where *Lift, Gamma, Gain* is listed. Change it to the ASC CDL, adjust Power up to
compensate for the log viewing in the basic view version, and possibly tweak Slope down. Offset will literally
offset values up or down, which is likely sub-optimal in CGI. Adjust your lighting if you can.
