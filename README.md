<h1 align="center">Extract Source Time Series</h1>

<p align="center">
  Extracts mean source time courses for selected labels of a FreeSurfer atlas from an MEG/EEG source estimate with MNE-Python, and outputs a timeseries table, surface GIfTI time courses and a report.
</p>

<p align="center">
  <a href="https://github.com/brainlife/abcd-spec"><img src="https://img.shields.io/badge/ABCD_Spec-v1.1-green.svg" alt="ABCD Spec v1.1"></a>
  <a href="https://doi.org/10.25663/brainlife.app.932"><img src="https://img.shields.io/badge/Brainlife-brainlife.app.932-blue.svg" alt="Run on brainlife.io"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-see_LICENSE-lightgrey.svg" alt="License"></a>
</p>

## At a glance

<!-- brainlife-app:begin (generated from the App manifest; edit the manifest, not this block) -->

> **Extracts mean source time courses for selected labels of a FreeSurfer atlas from an MEG/EEG source estimate with MNE-Python, and outputs a timeseries table, surface GIfTI time courses and a report.**

Reads the left and right hemisphere STC files, loads the requested atlas labels with mne.read_labels_from_annot, averages the source amplitude within each label at every time point, paints the label time courses back onto the surface vertices, and renders plots into an HTML report. Use it to get ROI time series from source-localised MEG/EEG. Needs the subject's FreeSurfer directory with the atlas annotation and a FreeSurfer license.

| | |
|---|---|
| **Inputs** | `neuro/meeg/stc` · `neuro/freesurfer` (optional) |
| **Outputs** | `generic/timeseries` · `report/html` · `neuro/surface/data` |
| **Container** | `brainlifemeeg/mne-freesurfer:1.12.1-7.4.1` |
| **Resources** | 1 CPUs · 8 GB memory · 0.5 h walltime |
| **Requires** | `FREESURFER_LICENSE`, `FS_LICENSE` |
| **Run it** | [doi.org/10.25663/brainlife.app.932](https://doi.org/10.25663/brainlife.app.932) |

#### Inputs

| Input | Datatype | Required | Description |
|---|---|---|---|
| `stc` | `neuro/meeg/stc` | yes | Source estimate (left and right hemisphere .stc files). |
| `freesurfer` | `neuro/freesurfer` | no | FreeSurfer subject directory providing the atlas annotation labels. |

#### Outputs

| Output | Datatype | Description |
|---|---|---|
| `out_dir` | `generic/timeseries` | Label time courses (timeseries.tsv.gz with timeseries.json). |
| `out_report` | `report/html` | HTML report with time course plots and brain renders of the labels. |
| `out_surface` | `neuro/surface/data` | Vertex-wise surface time courses (left.gii, right.gii). |

<details>
<summary><b>Parameters</b> (4)</summary>

| Parameter | Type | Default | Description |
|---|---|---|---|
| `atlas` | string | `aparc` | aparc , aparc.a2009s etc |
| `labels` | string |  | comma separated label names to extract |
| `hemi` | string | `both` | lh, rh, both |
| `subject` | string | `output` | Freesurfer subject name |

</details>

<!-- brainlife-app:end -->

## Running the App

### On brainlife.io

Open the App at [doi.org/10.25663/brainlife.app.932](https://doi.org/10.25663/brainlife.app.932), choose `neuro/meeg/stc`, `neuro/freesurfer` from your project, set the parameters and press **Execute**. The results are saved to your project as `generic/timeseries`, `report/html`, `neuro/surface/data`.

### Locally

You need [Singularity/Apptainer](https://apptainer.org/) (the App runs in `brainlifemeeg/mne-freesurfer:1.12.1-7.4.1`), [`jq`](https://jqlang.github.io/jq/), `FREESURFER_LICENSE`, `FS_LICENSE`.

1. Clone this repository.
2. Create `config.json` with the paths to your inputs and the parameters:

   ```json
   {
       "stc-rh": "/path/to/stc/stc-rh",
       "stc-lh": "/path/to/stc/stc-lh",
       "output": "/path/to/freesurfer/output",
       "atlas": "aparc",
       "labels": "",
       "hemi": "both",
       "subject": "output"
   }
   ```

3. Run `./main`.

## Configuration Parameters

| key | type | default | description |
|---|---|---|---|
| `atlas` | string | `aparc` | FreeSurfer parcellation atlas to read labels from via `mne.read_labels_from_annot` (e.g. `aparc`, `aparc.a2009s`). |
| `labels` | string | `""` (all) | Comma-separated label names to extract (e.g. `G_oc-temp_lat-fusifor`). Empty extracts every label in the atlas. |
| `hemi` | string | `both` | Hemisphere to process: `lh`, `rh`, or `both`. |
| `subject` | string | `null` (auto) | FreeSurfer subject name. Auto-detected from the `freesurfer`/`output` input directory when omitted, or set to `fsaverage` when no FreeSurfer input is given. |

## Local Testing

```bash
# Edit config.json to point "stc" and "freesurfer" at real files/directories, then:
python3 main.py
```

## Citing

If you use this App, please cite it, the software it builds on and brainlife.io:

- Extract Source Time Series. brainlife.io App. [doi:10.25663/brainlife.app.932](https://doi.org/10.25663/brainlife.app.932)
- Gramfort A, et al. MEG and EEG data analysis with MNE-Python. Front Neurosci 7(267):1-13 (2013). [doi:10.3389/fnins.2013.00267](https://doi.org/10.3389/fnins.2013.00267)
- Fischl. FreeSurfer. *NeuroImage* 62:774-781 (2012). [doi:10.1016/j.neuroimage.2012.01.021](https://doi.org/10.1016/j.neuroimage.2012.01.021)
- Hayashi S, Caron BA, Heinsfeld AS, et al. brainlife.io: a decentralized and open-source cloud platform to support neuroscience research. *Nature Methods* 21:809–813 (2024). [doi:10.1038/s41592-024-02237-2](https://doi.org/10.1038/s41592-024-02237-2)
- Avesani P, McPherson B, Hayashi S, et al. The open diffusion data derivatives, brain data upcycling via integrated publishing of derivatives and reproducible open cloud services. *Scientific Data* 6:69 (2019). [doi:10.1038/s41597-019-0073-y](https://doi.org/10.1038/s41597-019-0073-y)

## Authors

- obVdo (https://github.com/obVdo)
- Maximilien Chaumon (https://github.com/dnacombo)

## Funding

[![Wellcome-226486/Z/22/Z](https://img.shields.io/badge/Wellcome-226486%2FZ%2F22%2FZ-purple.svg)](https://wellcome.org/)
[![NIH-NINDS-U24NS140384](https://img.shields.io/badge/NIH_NINDS-U24NS140384-green.svg)](https://reporter.nih.gov/project-details/11261037)
[![NIH-NINDS-UM1NS132207](https://img.shields.io/badge/NIH_NINDS-UM1NS132207-green.svg)](https://reporter.nih.gov/project-details/11411328)
[![NIH-NIBIB-R01EB029272](https://img.shields.io/badge/NIH_NIBIB-R01EB029272-green.svg)](https://reporter.nih.gov/project-details/10262925)
[![NIH-NIBIB-R01EB030896](https://img.shields.io/badge/NIH_NIBIB-R01EB030896-green.svg)](https://reporter.nih.gov/project-details/10428625)
[![NIH-NIMH-R01MH126699](https://img.shields.io/badge/NIH_NIMH-R01MH126699-green.svg)](https://reporter.nih.gov/project-details/10460628)
[![NIH-NIDCD-R01DC020088](https://img.shields.io/badge/NIH_NIDCD-R01DC020088-green.svg)](https://reporter.nih.gov/project-details/11140284)
[![NSF-BCS-1734853](https://img.shields.io/badge/NSF_BCS-1734853-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1734853)
[![NSF-BCS-2203524](https://img.shields.io/badge/NSF_BCS-2203524-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2203524)
[![NSF-IIS-1636893](https://img.shields.io/badge/NSF_IIS-1636893-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1636893)
[![NSF-OAC-2148729](https://img.shields.io/badge/NSF_OAC-2148729-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2148729)
[![NSF-IIS-1912270](https://img.shields.io/badge/NSF_IIS-1912270-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1912270)
[![NSF-IIS-2148700](https://img.shields.io/badge/NSF_IIS-2148700-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2148700)
[![NSF-OAC-1916518](https://img.shields.io/badge/NSF_OAC-1916518-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1916518)

<sub>brainlife.io, which runs this App, is publicly funded by Wellcome, the U.S. National Institutes of Health and the U.S. National Science Foundation; acknowledging that support helps keep it running.</sub>

## License

See [LICENSE](LICENSE).
