# Conformal Coating: Solderability and Type Comparison

*Source: rusEFI Discord, 2025-11-18 | Channel: 1411925674498719775*
*Contributors: @!! 🍄 Psychedelia 💀 !!, @kostasl*

## Summary

A brief discussion covered the practical difficulty of reworking PCBs after conformal coating has been applied. The key takeaway is that polyurethane-based conformal coatings are significantly more resistant to soldering heat than acrylic-based coatings, making desoldering and rework considerably more difficult after polyurethane is applied.

## Details

### Conformal Coating for ECU Protection

Conformal coating is applied to PCBs (including rusEFI/epicEFI boards) to protect against moisture, dust, and vibration. The choice of coating type has direct implications for future rework or repair.

### Coating Type Comparison

| Property | Acrylic | Polyurethane |
|---|---|---|
| Heat resistance | Lower | Higher |
| Solder resistance | Lower | Higher |
| Ease of rework/desoldering | Easier | Difficult |
| Protection level | Moderate | Better |

**Key quote from community member:**
> "Not really, polyurethane conformal is way more resistant to heat and solder than say something acrylic based"

### Practical Guidance

- If you anticipate needing to rework or repair a board in the future, prefer **acrylic-based** conformal coating — it can be dissolved with isopropyl alcohol or penetrated more easily with a soldering iron.
- **Polyurethane conformal coating** provides better environmental protection but makes desoldering components very difficult. A sharp tool or dedicated conformal coating remover solvent is typically needed before attempting rework.
- Apply conformal coating only after all assembly and testing is complete to avoid unnecessary rework complications.
