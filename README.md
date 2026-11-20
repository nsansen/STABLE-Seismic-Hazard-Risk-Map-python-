# STABLE — seismic risk map of a building stock (Python)

Calculates the seismic risk map for the building stock of an area under a given seismic hazard,
using a simplified mechanical method. Risk is expressed as the probability that a given level of
damage will be exceeded. Four damage levels are considered — **P1, P2, P3, P4** — where the
severity increases with the index. Each probability takes a value between 0 (entirely unlikely)
and 1 (certainty).

## Method

The pipeline runs in the following steps, matching the section headings inside the notebook:

1. **Spatial join** of the building stock to the nearest seismic-hazard grid points
   (nearest-neighbour join via a k-d tree).
2. **Capacity curve per building** —
   - estimation of the eigenperiod; for masonry buildings (typology 1–7) this is approximated
     from the footprint area,
   - evaluation of the yielding acceleration `ay` for unreinforced-masonry buildings
     (this step covers masonry structures only).
3. **Seismic hazard** applied at each building's location.
4. **Seismic risk calculation** per building, giving the exceedance probability for P1–P4.
5. **Risk map plotting**.

## Input data

The notebooks expect the following in the working directory. No sample data is included in this
repository.

| Input | Description |
| --- | --- |
| `structural.shp` | Building-stock shapefile: structural, geometrical and (where available) post-earthquake damage attributes per building |
| `acceleration.shp` | Seismic-hazard shapefile: the grid of points carrying the acceleration spectra |
| `*.txt` | Acceleration spectra, read automatically from whitespace-separated text files via a glob pattern — one spectrum per hazard grid point |

Parameters expected per building include typology, construction period, use, post-earthquake
damage data where available, detection of different construction phases, total height, number of
storeys, perimeter, footprint area, average bearing-wall thickness for masonry structures, and a
plan view of at least the ground floor.

The seismic hazard data comprise acceleration spectra at a grid of points across the area of
interest, each derived from a seismic hazard analysis accounting for the seismic sources and local
soil effects (e.g. Tselentis et al., 2010).

## Notebooks

| Notebook | Contents |
| --- | --- |
| `FragMa_16 python geoPandas v5- automatic read txt.ipynb` | The full pipeline (122 code cells), including the optional "non-crucial" plotting and diagnostic sections |
| `FragMa_16 python geoPandas v5 - automatic read txt-mini.ipynb` | A reduced version of the same pipeline (40 code cells) with the non-crucial sections removed — faster to run end to end |

Both notebooks are committed with their stored outputs, so the risk maps can be inspected without
re-running them. The full notebook writes an intermediate shapefile (`delete_me1.shp`) to the
working directory; it is a temporary artifact and is git-ignored.

## Installation

```bash
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
```

The geospatial stack (GeoPandas, Shapely, Fiona, libpysal) is often easier to install via conda:

```bash
conda install -c conda-forge geopandas folium libpysal pyshp descartes
```

## Funding

Developed within **STABLE** (STructural stABiLity risk assEssment, 2018–2022), funded by the
European Union's Horizon 2020 research and innovation programme under the Marie Skłodowska-Curie
RISE action, Grant Agreement No. 823966.

Project record: https://cordis.europa.eu/project/id/823966

## License

Released under the MIT License — see [LICENSE](LICENSE).
