# COSMO star catalogue

The local star catalogue used by COSMO, an astrophotography app: every Gaia DR3 star down to magnitude G 15,
34.6 million stars in 2578 sky zones (329 MB). COSMO downloads only the zones it needs for a target, about 1 MB for
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

## Source and licence

This catalogue is derived from Gaia DR3 data.

> This work has made use of data from the European Space Agency (ESA) mission Gaia
> (https://www.cosmos.esa.int/gaia), processed by the Gaia Data Processing and Analysis Consortium (DPAC,
> https://www.cosmos.esa.int/web/gaia/dpac/consortium). Funding for the DPAC has been provided by national
> institutions, in particular the institutions participating in the Gaia Multilateral Agreement.

Gaia data © ESA/Gaia/DPAC, distributed under the Creative Commons Attribution-ShareAlike 3.0 IGO licence
(CC BY-SA 3.0 IGO). This derived catalogue is shared under the same licence; see `LICENSE`.

Changes from the source: stars down to G 15 only; positions propagated to epoch 2026.0; only position, G and
BP − RP kept, rounded as described above.
