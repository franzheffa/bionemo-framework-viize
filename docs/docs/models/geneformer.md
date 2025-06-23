# Geneformer

## Model Overview

### Description:

Geneformer generates a dense representation of a scRNA cell by learning co-expression patterns within single cells. Geneformer is a tabular count model trained on scRNA from the Chan Zuckerberg CELLxGENE census. Geneformer computes a complete embedding for each cell over the top 1024 expressed genes. The embeddings are used as features for a variety of predictive tasks. This model is ready for both commercial and academic use.

### References:

- Geneformer, reference foundation model for single-cell RNA: [Transfer learning enables predictions in network biology | Nature](https://www.nature.com/articles/s41586-023-06139-9)
- scGPT, alternative foundation model for single-cell RNA: [scGPT: toward building a foundation model for single-cell multi-omics using generative AI | Nature Methods](https://www.nature.com/articles/s41592-024-02201-0)
- scBERT, alternative foundation model for single-cell RNA: [scBERT as a large-scale pretrained deep language model for cell type annotation of single-cell RNA-seq data | Nature Machine Intelligence](https://www.nature.com/articles/s42256-022-00534-z)
- scFoundation, alternative foundation model for single-cell RNA: [Large Scale Foundation Model on Single-cell Transcriptomics | bioRxiv](https://www.biorxiv.org/content/10.1101/2023.05.29.542705v4)
  CELLxGENE census, public repository for scRNA experiments: [CZ CELLxGENE Discover - Cellular Visualization Tool (cziscience.com)](https://cellxgene.cziscience.com/)

### Model Architecture:

**Architecture Type:** [Bidirectional Encoder Representations from Transformers (BERT)](https://arxiv.org/abs/1810.04805) <br>
**Network Architecture:** [Geneformer](https://rdcu.be/ddrx0) <br>

### Input:

**Input Type(s):** Number (Row represents cell, containing gene names and single cell expression counts) <br>
**Input Format(s):** Array [AnnData](https://anndata.readthedocs.io/en/latest/)<br>
**Input Parameters:** 1D <br>

### Output:

**Output Type(s):** Vector (Dense Embedding Predictions)embeddings. <br>
**Output Format:** NumPy <br>
**Output Parameters:** 1D <br>
**Other Properties Related to Output:** Numeric floating point vector (fp16, bf16, or fp32); geneformer-10M-240530 outputs 256 dimensional embeddings; geneformer-106M-240530 outputs 768 dimensional embeddings <br>

### Software Integration:

**Runtime Engine(s):**

- BioNeMo, NeMo 1.2 <br>

**Supported Hardware Microarchitecture Compatibility:** <br>

- Ampere <br>
- Hopper <br>
- Volta <br>

**[Preferred/Supported] Operating System(s):** <br>

- Linux <br>

### Model Versions:

- **geneformer-10M-240530**
  - 10.3M parameter Geneformer variant
  - 25429 ensemble ID based gene tokens
  - 256 hidden dimensions with 4 heads, 6 layers and a 512 dimensional FFN
  - ReLU activation
  - 1e-12 EPS layernorm
  - bf16 mixed precision training with 32 bit residual connections
  - 2% hidden dropout, 10% attention dropout
- **geneformer-106M-240530**
  - 106M parameter Geneformer variant
  - 25429 ensemble ID based gene tokens
  - 768 hidden dimensions with 12 heads, 12 layers and a 3072 dimensional FFN
  - ReLU activation
  - 1e-12 EPS layernorm
  - bf16 mixed precision training with 32 bit residual connections
  - 2% hidden dropout, 10% attention dropout

## Training & Evaluation:

### Training Dataset:

Single cell expression counts from CELLxGENE Census used for the direct download of data matching similar criteria to those described in the Geneformer publication. Limiting cell data to organism="Homo sapiens", with a non "na" suspension_type, is_primary_data=True, and disease="normal" to limit to non-diseased tissues that are also the primary data source per cell to make sure that cells are only included once in the download. We tracked metadata including "assay", "sex", "development_stage", "tissue_general", "dataset_id" and "self_reported_ethnicity". The metadata "assay", "tissue_general", and "dataset_id" were used to construct dataset splits into train, validation, and test sets.

The training set represented 99% of the downloaded cells. We partitioned the data by dataset_id into a train set (99%) and a hold-out set (1%), to make sure that the hold-out datasets were independently collected single cell experiments, which helps evaluate generalizability to new future datasets.

In this training split, we made sure that all "assay" and "tissue_general" labels were present in the training set so that our model would have maximal visibility into different tissues and assay biases.

The 1% hold-out evaluation set was split further into a validation and test set. This final split was mostly done randomly by cell; however, we set aside a full dataset into the test split so that we could evaluate performance after training on a completely unseen dataset, including when monitoring the validation loss during training.

**Link:** Datasets downloaded from [CZ CELLxGENE Discover - Cellular Visualization Tool (cziscience.com)](https://cellxgene.cziscience.com/) <br>
**Data Collection Method by dataset**
- [Human] <br>

**Labeling Method by dataset**
- Hybrid: Automated, Human <br>

**Properties (Quantity, Dataset Descriptions, Sensor(s)):**
23.64 million non-diseased and human-derived single cells were chosen from the CZI CELLxGENE census, which is characterized as follows: <br>

- **Assay Bias:**
  - The vast majority of the dataset is one of the 10x genomics assays. Approximately 20M of the 26M cells are genomic assays, 4M are sci-RNA-seq, while remaining assays (microwell-seq, drop-seq, bd rhapsody, smart-seq, seq-well, and MARS-seq) represent small fractions of the full datasets.
- **Sex:**
  - 12.5M are male-derived cells; 10M are female derived cells. The remaining cells are not annotated.
- **Self-Reported Ethnicity:**
  - Approximately 12M cells are not annotated; 9M are annotated as "European." .5M are annotated as "Han Chinese." followed by "African American".
- **Age Bias:**
  - The dataset is heavily biased toward donors less than one year. The next highest group would be the segment that includes ages 21-30.
- **Tissue Type Bias:**
  - 9M cells are "brain" derived. 4M are blood derived, followed by "lung", "breast", "heart" and "eye" at approximately 1M cells each.

Dataset was derived from a limited number of public sources where methods and protocols may not represent sufficiently diverse sources to capture the full scope of gene expression.

## Evaluation Dataset:

Adamson et al 2016 PERTURB-seq dataset, accessed by Harvard dataverse.
**Link:** [adamson.zip - Harvard Dataverse](https://dataverse.harvard.edu/file.xhtml?fileId=6154417) <br>
**Data Collection Method by dataset**
- Human <br>

**Labeling Method by dataset**
- Automated - Molecular Barcoding <br>

**Properties (Quantity, Dataset Descriptions, Sensor(s)):** There are ~20k single cells, half of which represent unperturbed control samples, and the other half which contain an additional datatable containing the CRISPR knock-out targets for each cell.

**Link:** [CZ CELLxGENE Discover - Cellular Visualization Tool (cziscience.com)](https://cellxgene.cziscience.com/) <br>
**Data Collection Method by dataset**
- Human <br>

**Labeling Method by dataset**
- Hybrid: Automated, Human <br>

**Properties (Quantity, Dataset Descriptions, Sensor(s)):**
- 240,000 single cells were chosen from the CZI CELLxGENE census such that they did not share a `dataset_id` with any cell in the training data described previously.

### Inference:

**Engine:** BioNeMo, NeMo <br>
**Test Hardware:** <br>

- Ampere <br>
- Hopper <br>
- Volta <br>

\*Additional description content may be included here

### Ethical Considerations:

NVIDIA believes Trustworthy AI is a shared responsibility and we have established policies and practices to enable development for a wide array of AI applications. When downloaded or used in accordance with our terms of service, developers should work with their internal team to ensure this model meets requirements for the relevant industry and use case and addresses unforeseen product misuse. For more detailed information on ethical considerations for this model, please see our documentation on Explainability, Bias, Safety & Security, and Privacy. Please report security vulnerabilities or NVIDIA AI Concerns [here](https://www.nvidia.com/en-us/support/submit-security-vulnerability/).

## Training Diagnostics

### geneformer-10M
<!-- WandB Logs: https://wandb.ai/clara-discovery/Geneformer-pretraining-jsjconfigs/runs/i8LWOctg?nw=nwuserjomitchell -->
Training was performed on 8 servers with 8 A100 GPUs each for a total of 81485 steps using the CELLxGENE split with a per-GPU micro batch size 32 and global batch size of 2048. Training took a total of 4 days, 8 hours of wallclock time. As can be seen in the following images, training and validation curves both decreased fairly smoothly throughout the course of training.

![Training Loss Over Time for Geneformer 10M Model](../assets/images/geneformer/geneformer_10m_training_loss.png)
![Validation Loss Over Time for Geneformer 10M Model](../assets/images/geneformer/geneformer_10m_val_loss.png)



### geneformer-106M
<!-- WandB Logs https://wandb.ai/clara-discovery/geneformer-pretraining-106m-16node-spike -->
This checkpoint was trained for approximately 35,650 steps using the CELLxGENE split. Training was performed on 16 servers with 8 A100 GPUs each for a total of 35,650 steps using the CELLxGENE split with a per-GPU micro batch size 16 and global batch size of 2,048. Training took a total of 8 hours of wallclock time. As can be seen in the following image, training and validation curves both decreased fairly smoothly throughout the course of training.

![Training Loss Over Time for Geneformer 106M Model](../assets/images/geneformer/Geneformer_steven_106m_train.png)
![Validation Loss Over Time for Geneformer 106M Model](../assets/images/geneformer/Geneformer_steven_106m_val.png)

## Benchmarking

### Accuracy Benchmarks

#### Masked Language Model (MLM) Loss

The following describes the BERT MLM token loss. Like in the original BERT paper, and the Geneformer paper, 15% of all tokens are included in the loss. Of the included tokens, 80% are `"[MASK]"` token, 2% are a random gene token, and 18% are the correct output token. Note that this was an unintentional deviation from the original publication, but so far it seems to be working well. In the future, we will test the intended 80%/10%/10% mixture proposed in the paper. The token loss in the following table is the mean cross entropy loss of the 15% of tokens included in the loss mask averaged across cells. As a baseline, Geneformer was downloaded from [the ctheodoris/Geneformer page on HuggingFace on 2024/11/04](https://huggingface.co/ctheodoris/Geneformer) and applied to the same masking/unmasking problem on this dataset, but with model-specific cell representations due to the updated tokenizer and medians dictionary used to train, and the update from training with 2048 tokens to 4096 tokens per cell. The held-out `test` dataset from our training splits described previously was used, and it should be noted that some of these cells may have been involved in training the baseline Geneformer.

| Model Description      | Token Loss (lower is better) |
| ---------------------- | ---------------------------- |
| Baseline Geneformer    | 3.206*                      |
| geneformer-10M-240530  | 3.18                        |
| geneformer-106M-240530 | 2.89                        |

!!! bug "Baseline Geneformer was recently updated on HuggingFace making loss comparisons challenging."

    [Geneformer](https://huggingface.co/ctheodoris/Geneformer) was recently updated on HuggingFace to a new version.
    In a future release we will make checkpoint conversion scripts available so that the public model can be ran
    directly. Some key differences follow:

     * Trained on a much larger 95M cell dataset. Our current checkpoints were trained with 23M cells.
     * The new 12 layer baseline Geneformer variant sits between our 10M and 106M parameter models in parameter count with
      approximately 38M parameters.
     * The model is trained with a 4096 context rather than a 2048 context. When forcing the model to make predictions
      with a 2048 context, the MLM loss drops to *2.76*, which is probably unfair because this may be "out of domain" for
      training. It is really hard to compare these loss numbers directly is the only take-home here.
     * The model was trained on a set of 20,275 genes, rather than the older set of 25,426 genes. This would also be
      expected to give a boost in loss since there are fewer tokens to choose from.

#### Downstream Task Accuracy

Here we benchmark four models, with two baselines. These models are tasked with cell type classification, using the Crohn's disease small intestine dataset from
Elmentaite et al. (2020), Developmental Cell. This dataset contains approximately 22,500 single cells from both healthy children aged 4-13 and children with Crohn's disease. This dataset contains 31 unique cell types which we assume to be annotated accurately. This dataset was held out of our pre-training dataset as all diseased samples were removed.

- Baseline 1) scRNA workflow: this model uses PCA with 10 components and random forest on normalized and log transformed expression counts to produce a result.
- Baseline 2) geneformer-qa, a model trained for approximately 100 steps with approximately random weights. We expect this model to perform no differently than working on counts directly.
- geneformer-10M-240530 and geneformer-106M-240530 as described above.

For more details see the example notebook titled Geneformer-celltype-classification-example.ipynb


![F1-Score Comparison Across Model Variants on Cell Type Classification Task](../assets/images/geneformer/f1-score-models-04-18-2025.png)
![Average Accuracy Comparison Across Model Variants on Cell Type Classification Task](../assets/images/geneformer/accuracy-models-04-18-2025.png)

### Performance Benchmarks

The 106M parameter variant of Geneformer achieves over 50 TFLOPS per GPU during training. This is consistent whether trained with 1 or 8 A100s.

![GPU Performance (TFLOPS) Comparison Between Geneformer Model Variants on A100 GPUs](../assets/images/geneformer/model_tflops_per_gpu_chart_geneformer.png)

Performance will increase if the `num_dataset_workers` and the `micro_batch_size` are set appropriately. For the above metrics, we set `num_dataset_workers=8`. For the 10m model, set `micro_batch_size=120` and for the 106m model set the `micro_batch_size=16`. This will enable you to achieve similar performance results.
