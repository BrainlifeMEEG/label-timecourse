# Extract Source Time Series

[![Run on Brainlife.io](https://img.shields.io/badge/Brainlife-bl.app.932-blue.svg)](https://doi.org/10.25663/brainlife.app.932)

## Description

Extracts label (ROI) time courses from a MEG/EEG source estimate (STC), using
[`mne.read_source_estimate`](https://mne.tools/stable/generated/mne.read_source_estimate.html),
[`mne.read_labels_from_annot`](https://mne.tools/stable/generated/mne.read_labels_from_annot.html)
and `SourceEstimate.in_label`. For each requested label of a FreeSurfer cortical parcellation
atlas, the mean source amplitude across the label's vertices is computed at every time point.
The same per-label time course is also painted back onto every vertex of the label, producing a
vertex-wise surface time series. Brain-surface renders of the selected labels are generated via
`mne.viz.get_brain_class()` (PyVista `Brain`), using an offscreen 3D backend.

The app generates:
- a timeseries table of label time courses (`timeseries.tsv.gz` + `timeseries.json`)
- vertex-wise surface time courses as GIFTI files (`left.gii`, `right.gii`)
- time course plots per hemisphere and brain surface renders of the selected labels
- an HTML report with all plots, including the full atlas annotation

## Inputs

- **`stc`** (`neuro/meeg/stc`): source estimate directory containing `*-lh.stc` and `*-rh.stc` (required). On brainlife.io the two files are passed via the `stc-lh`/`stc-rh` config keys; for local testing, `stc` can instead point at a directory holding both files, or `stc-lh` can point directly at the `*-lh.stc` file (its `*-rh.stc` sibling is found alongside it).
- **`freesurfer`** (`neuro/freesurfer`): FreeSurfer subject directory (output of `recon-all`), used to read the parcellation atlas and inflated surface (optional). On brainlife.io this is passed via the `output` config key; for local testing, `freesurfer` can be used instead. If omitted, the app falls back to the built-in `fsaverage` template.

## Outputs

- **`out_dir/timeseries.tsv.gz`**, **`out_dir/timeseries.json`** (`generic/timeseries`): label time courses (rows = timepoints, columns = labels) and their metadata (sampling frequency, label names, atlas).
- **`out_surface/left.gii`**, **`out_surface/right.gii`** (`neuro/surface/data`): per-vertex time courses for the selected labels, one hemisphere per file.
- **`out_figs/*.png`**: time course plots per hemisphere (`label_tc_lh.png`, `label_tc_rh.png`) and brain surface renders of the selected labels (`brain_labels_*.png`); embedded in `product.json` and in the report.
- **`out_report/report.html`** (`report/html`): HTML report with the time course plots, the selected-label renders, and the full atlas annotation per hemisphere.

## Configuration Parameters

| key | type | default | description |
|---|---|---|---|
| `atlas` | string | `aparc` | FreeSurfer parcellation atlas to read labels from via `mne.read_labels_from_annot` (e.g. `aparc`, `aparc.a2009s`). |
| `labels` | string | `""` (all) | Comma-separated label names to extract (e.g. `G_oc-temp_lat-fusifor`). Empty extracts every label in the atlas. |
| `hemi` | string | `both` | Hemisphere to process: `lh`, `rh`, or `both`. |
| `subject` | string | `null` (auto) | FreeSurfer subject name. Auto-detected from the `freesurfer`/`output` input directory when omitted, or set to `fsaverage` when no FreeSurfer input is given. |

## Usage

### Running on Brainlife.io

1. Select a source estimate (STC) as the `stc` input.
2. Optionally select a FreeSurfer subject directory (output of recon-all) as the `freesurfer` input; omit it to use the built-in `fsaverage` atlas.
3. Set `atlas`, `labels` and `hemi` as needed — the defaults extract every `aparc` label on both hemispheres.
4. Submit the process.
5. Review the time course plots, the brain surface renders and the HTML report in the output viewer.

### Local Testing

```bash
# Edit config.json to point "stc" and "freesurfer" at real files/directories, then:
python3 main.py
```

## Authors

- obVdo (https://github.com/obVdo)
- Maximilien Chaumon (https://github.com/dnacombo)

## Citations

- Hayashi, S., Caron, B.A., Heinsfeld, A.S. et al. brainlife.io: a decentralized and open-source cloud platform to support neuroscience research. Nat Methods 21, 809–813 (2024). https://doi.org/10.1038/s41592-024-02237-2
- Gramfort, A. et al. MEG and EEG data analysis with MNE-Python. Front. Neurosci. 7, 267 (2013). https://doi.org/10.3389/fnins.2013.00267

## Funding Acknowledgement

brainlife.io is publicly funded. We kindly ask that you acknowledge the funding below in your code and publications.

[![NSF-BCS-1734853](https://img.shields.io/badge/NSF_BCS-1734853-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1734853)
[![NSF-BCS-1636893](https://img.shields.io/badge/NSF_BCS-1636893-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1636893)
[![NSF-ACI-1916518](https://img.shields.io/badge/NSF_ACI-1916518-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1916518)
[![NSF-IIS-1912270](https://img.shields.io/badge/NSF_IIS-1912270-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1912270)
[![NIH-NIBIB-R01EB029272](https://img.shields.io/badge/NIH_NIBIB-R01EB029272-green.svg)](https://grantome.com/grant/NIH/R01-EB029272-01)
[![NIH-NIBIB-R01EB030896](https://img.shields.io/badge/NIH_NIBIB-R01EB030896-green.svg)](https://grantome.com/grant/NIH/R01-EB030896-01)

## License

Copyright (c) 2026 MEEG Brainlife team. Licensed under AGPL-3.0, see [license.txt](license.txt).
