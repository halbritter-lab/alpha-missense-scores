# Alpha Missense Scores

This repository serves as a static file hosting API for precomputed JSON files containing AlphaMissense scores. The JSON files are generated from the [AlphaMissense_aa_substitutions.tsv.gz](https://storage.googleapis.com/dm_alphamissense/AlphaMissense_aa_substitutions.tsv.gz) dataset, provided by DeepMind, and hosted in the `scores/json` directory. These files are accessible via simple HTTP queries and are intended to be used when local precomputed files are not available.
Querying or loading the JSON files directly from this repository or locally can be more efficient than fetching the original TSV file and processing it.

## Repository Structure

The repository is organized as follows:

```plaintext
alpha-missense-scores/
│
└── scores/
    └── json/
        ├── P82251.AlphaMissense_aa_substitutions.json
        ├── Q07837.AlphaMissense_aa_substitutions.json
        ├── Q9Y5I7.AlphaMissense_aa_substitutions.json
        └── ...
```

- **`scores/json/`**: This directory contains JSON files, where each file represents the AlphaMissense scores for a specific UniProt ID.

## Data Source: AlphaMissense TSV

The original data is derived from the `AlphaMissense_aa_substitutions.tsv.gz` file, which has the following structure:

```tsv
uniprot_id      protein_variant am_pathogenicity        am_class
A0A024R1R8      M1A             0.4673                  ambiguous
A0A024R1R8      M1C             0.3828                  ambiguous
A0A024R1R8      M1D             0.8267                  pathogenic
...
```

- **`uniprot_id`**: The UniProt ID for the protein.
- **`protein_variant`**: The amino acid substitution (e.g., M1A means methionine at position 1 is substituted by alanine).
- **`am_pathogenicity`**: The AlphaMissense pathogenicity score for the variant (ranging between 0 and 1).
- **`am_class`**: The classification of the variant (e.g., `pathogenic`, `ambiguous`, `benign`).

## JSON File Structure

Each JSON file corresponds to a single UniProt ID and contains the following structure:

```json
{
    "uniprot_id": "P82251",
    "variants": {
        "M1A": {
            "am_pathogenicity": 0.4673,
            "am_class": "ambiguous"
        },
        "M1C": {
            "am_pathogenicity": 0.3828,
            "am_class": "ambiguous"
        },
        ...
    },
    "statistics": {
        "total_variants": 20,
        "am_class_counts": {
            "pathogenic": 5,
            "ambiguous": 10,
            "benign": 5
        },
        "am_pathogenicity_stats": {
            "average": 0.5124,
            "min": 0.1025,
            "max": 0.8267,
            "median": 0.4769,
            "quantile_25": 0.3828,
            "quantile_75": 0.5724
        },
        "protein_length": 235,
        "creation_date": "2024-08-21T12:34:56",
        "source_file": "AlphaMissense_aa_substitutions.tsv.gz"
    }
}
```

### Fields Explanation:

- **`uniprot_id`**: The UniProt ID of the protein.
- **`variants`**: A dictionary where each key is a `protein_variant` and the value is an object with `am_pathogenicity` and `am_class`.
- **`statistics`**: Additional statistics for the protein's variants:
  - **`total_variants`**: The total number of variants for the protein.
  - **`am_class_counts`**: A count of variants by `am_class`.
  - **`am_pathogenicity_stats`**: Descriptive statistics for `am_pathogenicity` (average, min, max, median, quantiles).
  - **`protein_length`**: The length of the protein, based on the position of the last variant.
  - **`creation_date`**: The date when the JSON file was created.
  - **`source_file`**: The original source file.

## Example Queries

You can retrieve any precomputed JSON file by querying the corresponding URL. For example:

```
https://raw.githubusercontent.com/halbritter-lab/alpha-missense-scores/main/scores/json/P82251.AlphaMissense_aa_substitutions.json
```

To fetch a specific file programmatically, you can use `curl` or any HTTP library. Example using `curl`:

```
curl -O https://raw.githubusercontent.com/halbritter-lab/alpha-missense-scores/main/scores/json/P82251.AlphaMissense_aa_substitutions.json
```

Or using Python:

```python
import requests

url = "https://raw.githubusercontent.com/halbritter-lab/alpha-missense-scores/main/scores/json/P82251.AlphaMissense_aa_substitutions.json"
response = requests.get(url)

if response.status_code == 200:
    data = response.json()
    print(data)
else:
    print(f"Error fetching data: {response.status_code}")
```

## License

The original data is licensed under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/). Please attribute the original authors when using this data.
