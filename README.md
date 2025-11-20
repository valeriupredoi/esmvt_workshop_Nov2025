## Next Generation ESMValTool

### Mission Requirements

#### Overview

![legacy](https://raw.githubusercontent.com/valeriupredoi/esmvt_workshop_Nov2025/main/images/legacy.jpg)

ESMValTool ``<2.0`` was good, but we had to accomodate a lot more functionality via a modular approach, and support for new interfaces and data, with solid testing, portability, and deployment.

![superbug](https://raw.githubusercontent.com/valeriupredoi/esmvt_workshop_Nov2025/main/images/superbug.png)

ESMValTool ``>=2.0, <3.0`` is a fantastic upgrade - virtually a brand new tool: modular (ESMValCore), benefiting from modern software approach,
extensive documented functionality, support for multiple interfaces, data sources, and with a large user base.

![f35](https://raw.githubusercontent.com/valeriupredoi/esmvt_workshop_Nov2025/main/images/f35.jpg)

ESMValTool ``>=3.0`` is what we are aiming for, and we are already in a transitional period towards it, even if not reflected in the version change perse.

#### Requirements

Externally, we are in a transition period that is dominated by a number of technical requirements:

- new model and OBS data **infrastructure**:
  - ESGF2 via [intake-esgf](https://github.com/esgf2-us/intake-esgf) - we have [support for it](https://github.com/ESMValGroup/ESMValCore/pull/2765) thanks to Bouwe Andela for a brilliant implementation (and yours truly, V Predoi for revieweing and stresstesting the wits out of it); there are, however, a number of functionalities still needed:
    - support for STAC nodes (e.g. CEDA)
    - support for an integrated ``File``-object load, without physical downloads
    - support for other catalogs via different ``intake``-typed libraries
  - new file formats are now starting to be supported:
    - [Zarr support](https://github.com/ESMValGroup/ESMValCore/pull/2785) is currently enabled, as an option in ``esmvalcore.preprocessor._io``, but we need to:
      - expose Zarr loading and usage via front-end interface via ``Recipe``
      - keep working with the [ncdata](https://github.com/pp-mo/ncdata) folks (Patrick Peglar et al) to solve any data issues
      - integrate Zarr support through catalogs
      - have an efficient Zarr integration (so far only basic testing has been done)
  - CMIP7 and new OBS datasets (see below for AI stuff)
  - (going further) new storage infrastructure: "Ask not what you can do for the storage, ask what the storage can do for you!" (JFK, 1961)
    - as much as possible minimize the download sizes by performing [in-storage processing with PyActiveStorage](https://github.com/NCAS-CMS/PyActiveStorage):
      - PyActiveStorage is fully functional and is already deployed on CEDA-JASMIN, to be deployed on various other HPCs part of EU projects
      - it's optimized for full-parallel HDF5 file access via a brand new, pure-Python, **thread-safe** HDF5 reader called [Pyfive](https://github.com/NCAS-CMS/pyfive)
      - full Dask low-level integration, so "bolt-on" factor is pretty high

