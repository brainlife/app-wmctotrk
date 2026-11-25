<h1 align="center">Convert wmc to trk</h1>

<p align="center">
  A single TrackVis .trk tractogram from a white matter classification and its .tck tractogram, with each streamline's tract stored as <code>bundle_code</code>.
</p>

<p align="center">
  <a href="https://github.com/brainlife/abcd-spec"><img src="https://img.shields.io/badge/ABCD_Spec-v1.1-green.svg" alt="ABCD Spec v1.1"></a>
  <a href="https://doi.org/10.25663/brainlife.app.129"><img src="https://img.shields.io/badge/Brainlife-brainlife.app.129-blue.svg" alt="Run on brainlife.io"></a>
  <a href="https://github.com/brainlife/app-wmctotrk/blob/master/LICENSE"><img src="https://img.shields.io/badge/License-MIT-lightgrey.svg" alt="MIT license"></a>
</p>

> **This branch, `1.1`, is the deprecated brainlife.io App Convert wmc to trk** ([doi:10.25663/brainlife.app.129](https://doi.org/10.25663/brainlife.app.129)). It is kept so earlier results stay reproducible. Use **Convert wmc to trk** ([doi:10.25663/brainlife.app.231](https://doi.org/10.25663/brainlife.app.231)) instead. The other Apps in this repository are listed under **Versions on brainlife.io** below.

## At a glance

<!-- brainlife-app:begin (generated from the App manifest; edit the manifest, not this block) -->

> **Convert a white matter classification (wmc) structure to a tractogram as a single trk file**

| | |
|---|---|
| **Inputs** | `neuro/wmc-deprecated` · `neuro/anat/t1w` |
| **Outputs** | `neuro/track/trk` |
| **Container** | `brainlife/dipy:0.14.1` |
| **Resources** | 1 CPUs |
| **Run it** | [doi.org/10.25663/brainlife.app.129](https://doi.org/10.25663/brainlife.app.129) |

#### Inputs

| Input | Datatype | Required | Description |
|---|---|---|---|
| `wmc` | `neuro/wmc-deprecated` | yes |  |
| `t1` | `neuro/anat/t1w` | yes |  |

#### Outputs

| Output | Datatype | Description |
|---|---|---|
| `trk` | `neuro/track/trk` |  |

<!-- brainlife-app:end -->

## Versions on brainlife.io

Each version is a separate App on brainlife.io, built from its own branch of this repository.

| Version | Container | Branch | Run it | Status |
|---|---|---|---|---|
| Convert wmc to trk | `brainlife/dipy:1.1.1` | [`1.2`](https://github.com/brainlife/app-wmctotrk/tree/1.2) | [doi:10.25663/brainlife.app.231](https://doi.org/10.25663/brainlife.app.231) | current |
| **Convert wmc to trk** (this branch) | `brainlife/dipy:0.14.1` | [`1.1`](https://github.com/brainlife/app-wmctotrk/tree/1.1) | [doi:10.25663/brainlife.app.129](https://doi.org/10.25663/brainlife.app.129) | deprecated |
| Convert wmc to multiple trk | `brainlife/dipy:0.14.1` | [`1.0`](https://github.com/brainlife/app-wmctotrk/tree/1.0) | [doi:10.25663/brainlife.app.127](https://doi.org/10.25663/brainlife.app.127) | deprecated |

The deprecated versions took the older `neuro/wmc-deprecated` datatype; version 1.0 wrote one .trk file per tract.

## Running the App

### On brainlife.io

Open the App at [doi.org/10.25663/brainlife.app.231](https://doi.org/10.25663/brainlife.app.231), choose a `neuro/wmc` white matter classification, the `neuro/track/tck` tractogram it was computed from and the subject's `neuro/anat/t1w` image, and press **Execute**. The result is archived to your project as a `neuro/track/trk` dataset tagged `wmc`, ready for tools that read TrackVis files.

### Locally

You need [Singularity/Apptainer](https://apptainer.org/). `main` runs `wmc_to_trk.py` (nibabel and scipy) in the `brainlife/dipy:1.1.1` container.

1. Clone this repository (the default branch matches version `1.2`).
2. Create `config.json` with the paths to your inputs (see `config.json.sample`):

   ```json
   {
       "wmc": "testdata/classification.mat",
       "tck": "testdata/track.tck",
       "t1": "testdata/t1.nii.gz"
   }
   ```

3. Run the App:

   ```bash
   ./main
   ```

### Sample data

Download sample datasets from brainlife.io with the [brainlife CLI](https://github.com/brainlife/cli):

```bash
npm install -g brainlife
bl login
mkdir input
bl dataset download <dataset-id>
```

## Outputs

```text
trk/
└── track.trk   all streamlines of the input .tck, with data_per_streamline['bundle_code']
```

`bundle_code` is the classification index of each streamline (0 = unclassified; otherwise the position of its tract in `classification.names`). The T1 affine is passed to nibabel as the tractogram's `affine_to_rasmm`.

## Troubleshooting

- **Wrong or shifted streamlines**: the classification index is read streamline by streamline, so the `wmc` must have been computed from the same `.tck` file, and the T1 must be the one the tractogram is aligned to.

## Citing

> [!TIP]
> If you use this App in your work, please cite the App, the software it builds on and brainlife.io (below). GitHub's **Cite this repository** button gives the App citation in APA and BibTeX.

- Convert wmc to trk. brainlife.io App. [doi:10.25663/brainlife.app.129](https://doi.org/10.25663/brainlife.app.129)
- Garyfallidis et al. Dipy, a library for the analysis of diffusion MRI data. *Frontiers in Neuroinformatics* 8 (2014). [doi:10.3389/fninf.2014.00008](https://doi.org/10.3389/fninf.2014.00008)
- Hayashi S, Caron BA, Heinsfeld AS, et al. brainlife.io: a decentralized and open-source cloud platform to support neuroscience research. *Nature Methods* 21:809–813 (2024). [doi:10.1038/s41592-024-02237-2](https://doi.org/10.1038/s41592-024-02237-2)
- Avesani P, McPherson B, Hayashi S, et al. The open diffusion data derivatives, brain data upcycling via integrated publishing of derivatives and reproducible open cloud services. *Scientific Data* 6:69 (2019). [doi:10.1038/s41597-019-0073-y](https://doi.org/10.1038/s41597-019-0073-y)

## Authors

- David Hunt
- Paolo Avesani

**Contributors:** Soichi Hayashi, Franco Pestilli, Lindsey Kitchell

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

<sub>brainlife.io is publicly funded, and acknowledging that support helps keep it running. This App was developed with support from Wellcome, the U.S. National Institutes of Health (including the BRAIN Initiative BRAIN CONNECTS program: The Axonal Projectome EXchange, U24NS140384, and the Center for Mesoscale Connectomics, UM1NS132207) and the U.S. National Science Foundation.</sub>

## License

[MIT](https://github.com/brainlife/app-wmctotrk/blob/master/LICENSE). DIPY and nibabel are distributed under their own licences.
