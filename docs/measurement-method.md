# Measurement method — V1 draft

This is a bench characterization workflow, not a validated analytical method.

## Setup

- Use the same electrode revision and sample chamber for a measurement series.
- Record ambient conditions and sample temperature.
- Use a stable 3.3 V supply and short I2C/sensor connections.
- Keep cables, switching converters, and the ESP32 antenna away from the sensing area where practical.
- Use a documented cleaning procedure compatible with the materials.

## Per-sample record

Record sample_id, oil type/batch if known, electrode_id and geometry, firmware commit, FDC1004 configuration and CAPDAC, sample and ambient temperatures, timestamp, raw readings, summary statistic, dispersion, and handling notes.

## Repeatability

Collect repeated readings without changing the setup, then repeat after removing and replacing the sample. Keep within-sample technical repeats separate from independent sample batches.

## Interpretation

Do not infer degradation or TPC from capacitance alone. Temperature, moisture, composition, electrode geometry, air gaps, residues, and sample handling can affect the signal.

## Future reference comparison

A TPC correlation study requires a competent laboratory reference method, documented sample preparation, and independent sample batches. Food-derived lipid and fryer oil are distinct sample matrices.
