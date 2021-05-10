# Retraining Instructions

1) Create a working directory.
2) Copy the input data files from ./input_data to the working directory.
3) Create and activate the anconda environment:

```
conda create --name retraining --file requirements.txt
conda activate retraining
```

4) Download the data for the tokenizer:

```
python -m nltk.downloader punkt
```

5) Run the retraining script:

```
python -m BmCS.retrain --workdir /path/to/workdir
```

6) When the script has finished, copy the following files from the working directory to the BmCS/models folder in the biomedical-citation-selector repository:
      - journal_ids.txt
      - word_indices.txt

7) Note that the retrained models have been saved to the follwoing folders in the working directory:
      - ./cnn_model/checkpoints/best_model.hdf5 (new CNN model)
      - ./voting_model/voting_model.joblib (new ensemble model)

8) Note that optimized decision threshold values have been saved in the combined_optimum_thresholds.txt file in the working directory. The first line of this file contains the high recall threshold for detecting articles that are in-scope for MEDLINE, and the second line contains the high precision threshold for detecting articles that are in-scope for MEDLINE and do not need to be manually reviewed.
     
9) In BmCS/thresholds.py in the biomedical-citation-selector repository, update COMBINED_THRESH with the high recall threshold saved in the first line of the combined_optimum_thresholds.txt file, and also update PRECISION_THRESH with the high precision theshold saved in the second line of the combined_optimum_thresholds.txt file. After these changes the system is now configured to run with the new retrained models.

10) Use the BmCS package --test option to confirm that the BmCS system is performing as expected:

```
BmCS /path/to/workdir/cnn_model/checkpoints/best_model.hdf5 /path/to/workdir/voting_model/voting_model.joblib --test --tolerance=0.04
```
