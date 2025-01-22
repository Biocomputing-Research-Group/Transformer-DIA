
# Transformer_DIA
## Transformer-based de novo peptide sequencing for data-independent acquisition mass spectrometry.
 is a deep learning model designed for de novo peptide sequencing from Data-Independent Acquisition (DIA) mass spectrometry data. By leveraging the transformer architecture. This guide will help you get started with installation, dataset preparation, and running key functionalities like model training, evaluation, and prediction. Follow the instructions below to utilize Transformer-DIA effectively for your peptide sequencing tasks.

## Paper

For more details about the model and its implementation, refer to our paper:
[Transformer-DIA: Advanced De Novo Peptide Sequencing](https://pmc.ncbi.nlm.nih.gov/articles/PMC11044815/)

---

## Installation

To manage dependencies efficiently, we recommend using [conda](https://docs.conda.io/en/latest/). Start by creating a dedicated conda environment:

```sh
conda create --name transdia_env python=3.10
```

Activate the environment:

```sh
conda activate transdia_env
```

Install Transformer_DIA and its dependencies via pip:

```sh
pip install transdia
```

To verify a successful installation, check the command-line interface:

```sh
transdia --help
```

---

## Dataset 


## Data Preprocessing  

To use Transformer-DIA, you need to preprocess the data by generating a feature file. This feature file serves as an essential input for the model.
We provide a script to streamline this process. The script processes spectrum and feature files to create the required feature file in pickle format. The generated features include: 

- **Keys**: Peptide sequences 
- **Values**: List containing the following attributes: 
  - `precursor_mz` 
  - `precursor_charge` 
  - `scan_list_middle` 
  - `ms1` 
  - `mz_list` 
  - `int_list` 
  - `neighbor_right_count` 
  - `neighbor_size_half` 

You can run the script by providing the paths to your spectrum and feature files as input. The script validates the inputs to ensure compatibility. Follow the instructions in the script prompts for seamless data preprocessing. 

We used the feature and spectrum files released by the **DeepNovo-DIA model**, which are available here: [MassIVE MSV000082368](ftp://massive.ucsd.edu/v01/MSV000082368/).
 
You can run the script by providing the paths to your spectrum and feature files as input. The script validates the inputs to ensure compatibility. Follow the instructions in the script prompts for seamless data preprocessing.
### Download DIA Datasets

Annotated DIA datasets can be downloaded from the [datasets page]().

---

### Download Pretrained Model Weights

Transformer_DIA requires pretrained model weights for predictions in `denovo` or `eval` modes. Compatible weights (in `.ckpt` format) can be found on the [pretrained models page]().

Specify the model file during execution using the `--model` parameter.


---

## Usage

### Predict Peptide Sequences

Transformer_DIA predicts peptide sequences from MS/MS spectra stored in MGF files. Predictions are saved as a CSV file:

```sh
transdia --mode=denovo --model=pretrained_checkpoint.ckpt --peak_path=path/to/spectra.mgf --peak_feature=path/to/precursor_feature.pkl
```

---

### Evaluate *de novo* Sequencing Performance

To assess the performance of *de novo* sequencing against known annotations:

```sh
transdia --mode=eval --model=pretrained_checkpoint.ckpt --peak_path=path/to/spectra.mgf --peak_feature=path/to/precursor_feature.pkl
```

Annotations in the MGF file must include peptide sequences in the `SEQ` field.

---

### Train a New Model

To train a new Transformer model from scratch, provide labeled training and validation datasets in MGF format:

```sh
transdia --mode=train --peak_path=path/to/train/annotated_spectra.mgf \ 
--peak_feature=path/to/train/precursor_feature.pkl \
--peak_path_val=path/to/validation/annotated_spectra.mgf \
--peak_feature_val==path/to/validation/precursor_feature.pkl
```

MGF files must include peptide sequences in the `SEQ` field.

---

### Fine-Tune an Existing Model

To fine-tune a pretrained Transformer-DIA model, set the `--train_from_scratch` parameter to `false`:

```sh
transdia --mode=train --model=pretrained_checkpoint.ckpt \
--peak_feature=path/to/train/precursor_feature.pkl \
--peak_path_val=path/to/validation/annotated_spectra.mgf \
--peak_feature_val==path/to/validation/precursor_feature.pkl
```

---


