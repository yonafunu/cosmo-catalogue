# COSMO star catalogue

The local star catalogue used by COSMO, an astrophotography app: every Gaia DR3 star down to magnitude G 15,
36.9 million stars in 2578 sky zones (349 MB). COSMO downloads only the zones it needs for a target, about 1 MB for
a typical field, and keeps them on the user's disk. It uses them to place mosaic panels on the sky, for star colours
and colour calibration, and for field recognition — without querying the Gaia servers.

Served at <https://yonafunu.github.io/cosmo-catalogue/gaia-dr3-g15/>.

## Format

- `gaia-dr3-g15/manifest.json`: version, epoch, faintest magnitude, and for each zone its star count, size in bytes
  and SHA-256.
- `gaia-dr3-g15/zNN_MMM.npz`: one file per zone. The sky is cut into 4° declination bands, each cut into right
  ascension segments about 4° wide on the sky. Stars are sorted brightest first.
- Columns, at epoch 2026.0 (Gaia proper motions applied from 2016.0):
  - `ra`, `dec`: unsigned 32-bit integers, RA = ra / 2³² × 360° and DEC = dec / 2³² × 180° − 90°;
  - `g`: G magnitude in millimagnitudes (unsigned 16-bit);
  - `bp_rp`: BP − RP colour in millimagnitudes (signed 16-bit), -32768 when Gaia has no colour.

## Star details (`gaia-dr3-g15-details/`)

What else Gaia knows about the stars of the catalogue, for COSMO's Lab: parallax (the distance), proper motion,
radial velocity, RUWE, and whether the star is variable or not single. About 320 MB for the whole sky; COSMO
downloads the zones of a field when the Lab needs them.

- `gaia-dr3-g15-details/manifest.json`: version, and for each zone its star count, size, SHA-256 and `base`, the
  SHA-256 of the catalogue zone it is aligned with.
- `gaia-dr3-g15-details/zNN_MMM.npz`: one file per zone, the same stars in the same order as the catalogue zone
  (row i is star i of the zone). Columns, as integers:
  - `parallax`, `parallax_error`: hundredths of a milliarcsecond (signed 32-bit);
  - `pmra` (with cos δ), `pmdec`: hundredths of a milliarcsecond per year (signed 32-bit);
  - `ruwe`: hundredths (signed 16-bit);
  - `radial_velocity`: tenths of a km/s (signed 16-bit);
  - missing values: −2³¹ (32-bit) or −32768 (16-bit);
  - `flags`: bit 0 set when Gaia flags the star as variable; bits 1 and above hold Gaia's `non_single_star` code.

## Sky index (`blind-g15/`)

An index of star quads built from the catalogue, used by COSMO to recognise a field anywhere in the sky from its
stars alone, with no coordinates (the principle of astrometry.net). 7.5 million quads in six size bands, from
0.2°–0.4° to 6.4°–12.8°, 55 MB.

- `blind-g15/manifest.json`: version, and for each band its quad size, star and quad counts, size in bytes and
  SHA-256.
- `blind-g15/band_*.npz`: in each band, the 3 brightest stars of every sky cell of the band's size (`ra`, `dec` as
  above, stored cell by cell); the quads as `a` (the first star, as the difference from the previous quad's) and
  `bcd` (the other three stars, as offsets from `a`). The quad codes (where C and D fall in the frame where A is at
  (0, 0) and B at (1, 1)) are computed when the index is loaded.

## Source and licence

This catalogue is derived from Gaia DR3 data.

> This work has made use of data from the European Space Agency (ESA) mission Gaia
> (https://www.cosmos.esa.int/gaia), processed by the Gaia Data Processing and Analysis Consortium (DPAC,
> https://www.cosmos.esa.int/web/gaia/dpac/consortium). Funding for the DPAC has been provided by national
> institutions, in particular the institutions participating in the Gaia Multilateral Agreement.

Gaia data © ESA/Gaia/DPAC, distributed under the Creative Commons Attribution-ShareAlike 3.0 IGO licence
(CC BY-SA 3.0 IGO). This derived catalogue is shared under the same licence; see `LICENSE`.

Changes from the source: stars down to G 15 only; positions propagated to epoch 2026.0; only position, G,
BP − RP, parallax, proper motion, radial velocity, RUWE, variability and non-single-star flags kept, rounded as
described above.

## History

- 2026-11-23: 143 dense zones of the Milky Way rebuilt. The Gaia archive had cut some long answers short without
  an error, and these zones had lost 2.27 million stars, by whole patches of sky. The sky index was rebuilt
  with them. Zones whose stars did not change keep their file and SHA-256, so COSMO downloads only the rebuilt
  ones.
