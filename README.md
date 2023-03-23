# cobalt-perspective-2050 ![GitHub release (latest by date)](https://img.shields.io/github/v/release/premise-community-scenarios/cobalt-perspective-2050) [![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.6984292.svg)](https://doi.org/10.5281/zenodo.6984292)


Description
-----------

This is a repository containing a scenario that implements the projections of the 
van der Meide et al., 2022 study for:

* cobalt. 

It is meant to be used in `premise` in addition to a global IAM scenario, to provide 
refined projections for the future supply of cobalt on the global market.

This data package contains all the files necessary for `premise` to implement
this scenario and create market-specific composition for cobalt (including supply from recycling channels).

Sourced from publication
------------------------

van der Meide, M., Harpprecht, C., Northey, S., Yang, Y., & Steubing, B. (2022). Effects of the energy transition on environmental impacts of cobalt supply: A prospective life cycle assessment study on future supply of cobalt. Journal of Industrial Ecology, 1– 15. https://doi.org/10.1111/jiec.13258

Data validation 
---------------

[![goodtables.io](https://goodtables.io//badge/github/premise-community-scenarios/cobalt-perspective-2050.svg)](https://goodtables.io//github/premise-community-scenarios/cobalt-perspective-2050)

Test 
----

![example workflow](https://github.com/premise-community-scenarios/cobalt-perspective-2050/actions/workflows/main.yml/badge.svg?branch=main)

Ecoinvent database compatibility
--------------------------------

ecoinvent 3.8 cut-off

IAM scenario compatibility
---------------------------

The following coupling is done between IAM and the cobalt scenarios:

| IAM scenario           | EP2050+ scenario        |
|------------------------| ------------------------|
| IMAGE SSP2-Base        | Business As Usual       |
| IMAGE SSP2-RCP26       | Sustainable development |

What does this do?
------------------

This external scenario update the ecoinvent global market for cobalt, according
to the projections described in van der Meide et al., 2022.

Cobalt
******

The following market is introduced:

* `market for cobalt (CP2050)` (GLO)

It is supplied by six cobalt production pathways:
* cobalt from cobalt mining
* cobalt as a co-product from copper mining (ore extraction, processing and electrowinning)
* cobalt as a co-product from nickel mining
* cobalt as a co-product from zinc mining
* cobalt recovered from discarded batteries (hydro- and pyro-metallurgical processes)

This market is relinked to activities that consume metallic cobalt throughout the database, if any.


Flow diagram
------------

![diagram ammonia markets](assets/flow_diagram.png)

How to use it?
--------------

```python

    import brightway2 as bw
    from premise import NewDatabase
    from datapackage import Package
    
    
    fp = r"https://raw.githubusercontent.com/premise-community-scenarios/cobalt-perspective-2050/main/datapackage.json"
    cobalt2050 = Package(fp)
    
    bw.projects.set_current("your_bw_project")
    
    ndb = NewDatabase(
            scenarios = [
                {"model":"image", "pathway":"SSP2-Base", "year":2050,},
                {"model":"image", "pathway":"SSP2-RCP26", "year":2030,},
            ],        
            source_db="ecoinvent 3.8 cutoff",
            source_version="3.8",
            key='xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx',
            external_scenarios=[
                cobalt2050, # <-- list datapackages here
            ] 
        )
        
    ndb.update_external_scenario()
```

