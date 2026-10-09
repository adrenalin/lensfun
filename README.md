# Lens data for Kuvake, from Lensfun

This is a changed copy of the lens database of [Lensfun](https://github.com/lensfun/lensfun),
made for the photo editor [Kuvake](https://kuvake.app/). It holds the same measurements in
another shape: one small JSON file per lens, and an index to find a lens by, so that a
program can fetch the one lens of the photo at hand.

**All credit for the data belongs to the Lensfun project and the many people who measured
their lenses and sent in the results.** Lensfun was begun by Andrew Zabolotny and is
maintained by its contributors; see its
[credits](https://lensfun.github.io/manual/latest/credits.html). If your lens is missing or
wrong, the place to mend it is Lensfun itself, [by calibrating it](https://lensfun.github.io/calibration/):
this copy follows theirs.

## Licence

The data here is under the
[Creative Commons Attribution-ShareAlike 3.0 Unported](https://creativecommons.org/licenses/by-sa/3.0/)
licence, the same as Lensfun's database. The full text is in [LICENSE](LICENSE). You may
copy and change it, also for money, if you credit Lensfun and share what you change under
the same licence.

Nothing of Lensfun's library or programs is here, only data made from its database.

## Where it comes from

`index.json` names the commit of Lensfun the data was made from, under `source`.

## What was changed

The numbers are theirs, untouched. The shape is ours:

- **XML became JSON**, a file per lens in `lenses/`, named by the lens's maker and model.
  Where two entries would have the same name (one lens measured on two sensors), the later
  has `-2` added.
- **Names in other languages were left out**: only the default `maker` and `model` are kept.
- **Defaults are written out**: `aspect` is 1.5 and `type` is `rectilinear` where Lensfun's
  entry leaves them unsaid.
- **`focal` and `aperture` in the index** are the lens's range of focal lengths and its
  widest aperture: as the entry declares them, else as its name says them ("18-55mm
  f/3.5-5.6"), else, for the focal lengths, as its calibration covers. `aperture` is `null`
  where none of these tells.
- **`has` in the index** lists which kinds of calibration the lens has.

## The files

### `index.json`

- `source`: where the data is from, and its licence.
- `mounts`: for each camera mount, the other mounts whose lenses fit it.
- `cameras`: each with `maker`, `model`, `mount`, `crop` (the crop factor), and sometimes
  `variant`.
- `lenses`: each with `id` (its file is `lenses/<id>.json`), `maker`, `model`, `mounts`,
  `crop` (the crop factor of the camera it was measured on), `focal` as `[shortest, longest]`
  in millimetres, `aperture`, and `has`.

### `lenses/<id>.json`

What the index says of the lens, and:

- `aspect`: the longer side over the shorter of the camera it was measured on.
- `type`: the lens's projection, `rectilinear` unless it is a fisheye or the like.
- `distortion`: a list, one entry per focal length measured: `focal`, `model` (`ptlens`,
  `poly3` or `poly5`) and that model's numbers (`a`, `b`, `c`, or `k1`, `k2`), with
  `realFocal` where Lensfun gives it.
- `tca` (colour fringes): one entry per focal length: `focal`, `model` (`poly3` or
  `linear`) and its numbers (`vr`, `vb`, `br`, `bb`, `cr`, `cb`, or `kr`, `kb`).
- `vignetting` (darkening towards the corners): a list of rows, each
  `[focal, aperture, distance, k1, k2, k3]`, all of Lensfun's model `pa`.

What the models and their numbers mean is described in
[Lensfun's manual](https://lensfun.github.io/manual/latest/).
