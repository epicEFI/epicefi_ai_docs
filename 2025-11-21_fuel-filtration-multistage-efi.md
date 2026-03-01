# Multi-Stage Fuel Filtration for High-Performance EFI Systems

*Source: rusEFI Discord, 2025-11-21 | Channel: hardware-build (1411925674498719775)*

*Contributors: fast335xi, Joesphan*

## Summary

A community member described a three-stage fuel filtration system used with a Radium FCST (Fuel Cell Surge Tank) setup, covering filtration from the fuel cell through to the fuel rail. The stages use progressively finer filtration, with the final stage being a 10-micron magnetic filter. PTFE AN hose with ferrule fittings was recommended over rubber hose for ease of assembly and longevity.

## Details

### Three-Stage Filtration System

| Stage | Location | Filter Type | Micron Rating |
|-------|----------|-------------|---------------|
| 1 | Fuel cell → lift pump inlet | Holley Hydramat | ~75 µm (pre-filter equivalent) |
| 2 | Surge tank internal | Mesh screen (aligns pump inlets) | Coarse |
| 3 | Surge tank outlet → fuel rail | 10-micron magnetic filter | 10 µm |

- fast335xi: *"I have like a 3 stage filter system. Lift pump uses a Holley hydramat. In the surge tank has a mesh filter. And have that big 10 micron magnetic filter before the fuel rail"*
- Radium FCST (Fuel Cell Surge Tank) with this setup: 20-gallon fuel cell, Radium FCST housing, large cross Hydramat, 2× Walbro 535 + 2× Walbro 525 pumps.

### Stage 1: Holley Hydramat

- Functions as the lift pump pre-filter / pickup.
- Draws fuel from the cell and keeps the surge tank bucket filled.
- Return line feeds back into the surge tank bucket.

### Stage 2: Surge Tank Mesh Screen

- Internal to the surge tank; screens and aligns all pump inlets.
- Provides coarse protection for the main high-pressure pumps.

### Stage 3: 10-Micron Magnetic Filter (Before Fuel Rail)

- Racetronix filter housing and element recommended:
  - Housing: [https://www.racetronix.biz/p/fuel-filter-housing-50mm-10orb-black/filter-1010b](https://www.racetronix.biz/p/fuel-filter-housing-50mm-10orb-black/filter-1010b)
  - Element (10 µm, magnetic, E85-compatible): [https://www.racetronix.biz/p/filter-element-10-10-e85-magnetic/fem-1010](https://www.racetronix.biz/p/filter-element-10-10-e85-magnetic/fem-1010)
- Magnetic element captures ferrous particles that pass through the upstream stages.
- Recommended maintenance: keep a spare clean element on hand; swap elements, then clean and store the removed one — faster than draining and cleaning in place.

### Filter Micron Reference

- Walbro fabric pre-filters (pump sock): ~75 µm
- 30 µm: good intermediate filtration after 75 µm sock
- 10 µm: final stage before injectors (removes fine particles that could damage injector tips)

### AN Hose and Fitting Notes

- **PTFE (Teflon) AN hose** is recommended over rubber braid:
  - Easier to assemble (less force required than rubber)
  - Longer service life
  - Compatible with E85
- **Ferrules are single-use** — always have a spare pack available when assembling PTFE AN fittings (Joesphan: *"get a pack of spare ferrules as those are one time use"*).
- Racetronix conductive PTFE hose with stainless/black sheath: [https://www.racetronix.biz/p/-8-hose-teflon-conductive-ss-black/tft1170-08b](https://www.racetronix.biz/p/-8-hose-teflon-conductive-ss-black/tft1170-08b)
- Evil Energy AN kits are noted as cost-effective; buying 4-packs is more economical than individual fittings.
