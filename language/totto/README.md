# ToTTo Supplementary Repository

This repository serves as a supplementary for the main repository found [here](https://github.com/google-research-datasets/ToTTo/).

## ToTTo Dataset

ToTTo is a dataset for the controlled generation of descriptions of tabular data comprising over 100,000 examples. Each example is a aligned pair of a highlighted table and the description of the highlighted content.

During the dataset creation process, tables from English Wikipedia are matched with (noisy) descriptions. Each table cell mentioned in the description is highlighted and the descriptions are iteratively cleaned and corrected to faithfully reflect the content of the highlighted cells.

By providing multiple different descriptions from the same table, this dataset can be utilized as a testbed for the controllable generation of table description.

You can find more details, analyses, and baseline results in [our paper](#). You can cite it as follows:

```
@inproceedings{parikh2020totto,
  title =     {ToTTo: A Controlled Table-To-Text Generation Dataset},
  author =    {XXX},
  booktitle = {XXX},
  year =      {2020},
}
```

## Visualizing sample data

To help understand the dataset, you can find a sample of the train and dev sets in the `sample/` folder. We additionaly provide the `create_table_to_text_html.py` script that visualizes an example, the output of which you can also find in the `sample/` folder.


## Running the evaluation scripts locally

To encourage comparability of results between different systems, we encourage researchers to evaluate their systems using the scripts provided in this repository. For an all-in-one solution, you can call `totto_eval.sh` with the following arguments:

- `--prediction_path`: Path to your model's predictions, one prediction text per line. [Required]
- `--example_path`: Path to the `public_dev_data.jsonl` you want to evaluate. [Required]
- `--output_dir`: where to save the downloaded scripts and formatted outputs. [Default: `./temp/`]

`totto_eval.sh` requires python 3 and a few libraries. Please make sure that you have all the necessary libraries installed. You can use your favorite python environment manager (e.g., virtualenv or conda) to install the requirements listed in `eval_requirements.txt`.

You can test whether you are getting the correct outputs by running it on our provided development samples in the `sample/` folder, which also contains associated `sample_outputs.txt`. To do so, please run the following command:

```
totto_eval.sh --prediction_path sample/sample_outputs.txt --example_path sample/dev_sample.jsonl
```

You should see the following output:

```
======== EVALUATE OVERALL ========
Computing BLEU (overall)
BLEU+case.mixed+numrefs.3+smooth.exp+tok.13a+version.1.4.7 = 45.5 86.0/63.5/44.7/31.0 (BP = 0.869 ratio = 0.877 hyp_len = 57 ref_len = 65)
Computing PARENT (overall)
Evaluated 5 examples.
Precision = 0.7611 Recall = 0.4383 F-score = 0.5334
======== EVALUATE OVERLAP SUBSET ========
Computing BLEU (overlap subset)
BLEU+case.mixed+numrefs.3+smooth.exp+tok.13a+version.1.4.7 = 37.2 84.8/56.7/37.0/25.0 (BP = 0.809 ratio = 0.825 hyp_len = 33 ref_len = 40)
Computing PARENT (overlap subset)
Evaluated 3 examples.
Precision = 0.7140 Recall = 0.3135 F-score = 0.4134
======== EVALUATE NON-OVERLAP SUBSET ========
Computing BLEU (non-overlap subset)
BLEU+case.mixed+numrefs.3+smooth.exp+tok.13a+version.1.4.7 = 58.3 87.5/72.7/55.0/38.9 (BP = 0.959 ratio = 0.960 hyp_len = 24 ref_len = 25)
Computing PARENT (non-overlap subset)
Evaluated 2 examples.
Precision = 0.8317 Recall = 0.6256 F-score = 0.7135
```

## Testing the evaluation result

If you want to ensure that the results from totto_eval.sh are as expected, please run `pytest` inside of this folder. This will run the tests provided in the `test_eval_pipeline.py` file.
