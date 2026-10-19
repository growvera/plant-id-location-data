> [!WARNING]
> **🌐 THIS REPOSITORY IS PUBLIC.** Anyone on the internet can see everything in it.
> It exists to publish the open-data-derived files below, which GBIF's citation (DOI)
> points to. **Do not add code, credentials, customer data, or anything internal.**
> Only add new versions of the files in `data/`.

# Plant ID location data (Growvera / Dryline)

Species range maps and elevation bands for western North American shrubs and
trees, used by the offline plant-identification feature in Growvera's Dryline
mobile app. The app multiplies its photo-based match scores by how well each
plant fits the place (and, when known, the elevation) where the photo was
taken, so plants that don't grow there are ranked lower.

Built entirely from open data — see [Sources and licences](#sources-and-licences).

## Files

Each release lives in its own dated folder; older folders are kept unchanged so
citations stay accurate.

| File | What it is |
|---|---|
| `data/2026-10-19/plant-id.ranges.json` | The range grid and elevation bands, exactly as bundled in the app. |
| `data/2026-10-19/gbif-source-datasets.csv` | Every GBIF source dataset used and how many of its records went in (`datasetKey,count`). |

### `plant-id.ranges.json`

- `grid`: a regular latitude/longitude grid — origin `lat0`/`lon0` (south-west
  corner), cell size `step` degrees (0.25°, about 25 km), `rows` × `cols` cells,
  numbered row-major from the south-west.
- `ranges`: scientific name → run-length-encoded list of in-range cells,
  `[start, length, start, length, …]`. 112 of the app's 114 classes (84 species,
  3 genera, 30 subspecies) have one.
- `elevation`: scientific name → `[low, high]` metres above sea level — the
  2nd–98th percentile of recorded elevations plus a 150 m margin (102 classes).
- `source`, `attribution`, `citation`, `geomodelVersion`, `gbifSnapshot`:
  provenance.

## How it was made

- **Species ranges:** iNaturalist's open range maps (Geomodel 2.34),
  rasterised onto the grid; a cell counts as in range if the range touches it.
  Species classes include their subspecies' ranges, and genus classes the union
  of every species in the genus (including renamed genera, e.g. *Prosopis* →
  *Neltuma*).
- **Subspecies ranges** (iNaturalist publishes none): cells containing at least
  one GBIF occurrence of the subspecies, grown by one cell; subspecies with at
  least 30 records.
- **Elevation bands:** recorded elevations of GBIF occurrences, classes with at
  least 40 values.
- **GBIF records used:** GBIF monthly snapshot of 1 October 2026, filtered to
  kingdom Plantae; licence **CC0 or CC BY 4.0 only** (CC BY-NC excluded);
  occurrence status present; not cultivated (`establishmentMeans` not
  `MANAGED`); coordinates within 24–60° N, 95–140° W with uncertainty ≤ 5 km;
  records with GBIF coordinate issue flags removed. 374,934 records from 290
  datasets.

## Sources and licences

This data is published under **[CC BY 4.0](LICENSE)**. If you use it, credit
Growvera and the sources below.

- **Range maps:** iNaturalist Open Range Map Dataset (Geomodel), iNaturalist
  (inaturalist.org), CC BY 4.0.
- **Occurrence data:** GBIF.org (19 October 2026) Filtered export of GBIF
  occurrence data https://doi.org/10.15468/dd.867xze — the registered derived
  dataset for exactly the records used (from the 1 October 2026 snapshot,
  https://doi.org/10.15468/dl.tup2g6). Only records licensed CC0 or CC BY 4.0
  by their publishers; the individual source datasets are listed in
  `gbif-source-datasets.csv`.

The `citation` field inside `data/2026-10-19/plant-id.ranges.json` predates the
derived-dataset registration and names the snapshot DOI; the data itself is
identical. Cite the derived dataset above.

Provided as is, without warranty: range maps and occurrence records are models
and observations, not ground truth.

## Contact

Growvera — https://growvera.com/
