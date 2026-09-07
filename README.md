# label-timecourse

Brainlife.io app to extract label time courses from a MEG/EEG source estimate (STC).

Given a source estimate and a FreeSurfer parcellation atlas, this app extracts the mean source amplitude time course for each requested label and saves the result as a timeseries datatype and a surface GIFTI file.

## Inputs

| Input | Description |
|-------|-------------|
| `stc` | Source estimate directory (`*-lh.stc` + `*-rh.stc`) |
| `freesurfer` | FreeSurfer subject directory (optional — falls back to fsaverage) |

## Outputs

| Output | Description |
|--------|-------------|
| `out_dir/timeseries.tsv.gz` | Label time courses (rows = timepoints, columns = labels) |
| `out_dir/timeseries.json` | Metadata (sampling frequency, label names, atlas) |
| `out_surface/left.gii` | Surface GIFTI — left hemisphere (vertex × time) |
| `out_surface/right.gii` | Surface GIFTI — right hemisphere (vertex × time) |
| `out_figs/label_tc_lh.png` | Time course plot — left hemisphere |
| `out_figs/label_tc_rh.png` | Time course plot — right hemisphere |
| `out_figs/brain_labels_*.png` | Brain surface with selected labels highlighted |
| `out_report/report.html` | HTML report with all plots |

## Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| `atlas` | `aparc` | FreeSurfer parcellation atlas (e.g. `aparc`, `aparc.a2009s`) |
| `labels` | _(all)_ | Comma-separated label names to extract (e.g. `G_oc-temp_lat-fusifor`). Leave empty to extract all labels. |
| `hemi` | `both` | Hemisphere: `lh`, `rh`, or `both` |
| `subject` | _(auto)_ | FreeSurfer subject name (auto-detected from directory) |

## Container

```
docker://aunnikri642/app-freesurfer-mne-source-recon
```

8 GB RAM · 30 min wall time

## Authors

- obVdo
- Maximilien Chaumon (https://github.com/dnacombo)

## Citations

Hayashi, S., Caron, B.A., Heinsfeld, A.S. et al. brainlife.io: a decentralized and open-source cloud platform to support neuroscience research. Nat Methods 21, 809–813 (2024). https://doi.org/10.1038/s41592-024-02237-2

### MNE-Python

Gramfort A, Luessi M, Larson E, Engemann DA, Strohmeier D, Brodbeck C, Goj R, Jas M, Brooks T, Parkkonen L, and Hämäläinen MS. **MEG and EEG data analysis with MNE-Python**. Frontiers in Neuroscience, 7(267):1–13, 2013. https://doi.org/10.3389/fnins.2013.00267

## Funding Acknowledgement

brainlife.io is publicly funded. We kindly ask that you acknowledge the funding below in your code and publications.

[![NSF-BCS-1734853](https://img.shields.io/badge/NSF_BCS-1734853-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1734853)
[![NSF-BCS-1636893](https://img.shields.io/badge/NSF_BCS-1636893-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1636893)
[![NSF-ACI-1916518](https://img.shields.io/badge/NSF_ACI-1916518-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1916518)
[![NSF-IIS-1912270](https://img.shields.io/badge/NSF_IIS-1912270-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1912270)
[![NIH-NIBIB-R01EB030896](https://img.shields.io/badge/NIH_NIBIB-R01EB030896-green.svg)](https://grantome.com/grant/NIH/R01-EB030896-01)

## License

Copyright (c) 2026 brainlife.io

This project is licensed under the AGPL-3.0 License - see the LICENSE file for details.

