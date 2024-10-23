# Mass Spectrometry Data Processing and Prediction Pipeline

The Mass Spectrometry Data Processing and Prediction Pipeline is designed to process mass spectrometry data, perform predictions using a trained Convolutional Neural Network (CNN) model, and match the predictions against a reference database to identify potential candidate molecules.


## Features

- **Data Preprocessing:** Converts MSP files to structured DataFrames and preprocesses spectra data for model input.
- **Model Prediction:** Utilizes a pre-trained CNN model to predict molecular embeddings from spectra data.
- **Candidate Matching:** Matches predicted embeddings with a reference database to find top candidate molecules based on cosine similarity.
- **Support for Multiple Input Types:** Handles MSP files both **with** and **without** SMILES annotations, controlled via a configuration parameter.
- **Configurable Parameters:** Allows users to adjust parameters like intensity thresholds, resolution, and the number of top candidates via a YAML configuration file.
- **Modular Codebase:** Organized into separate modules for easy maintenance and scalability.

## Setup and Configuration

Before running the pipeline, ensure that all dependencies are installed and properly configured. The application is controlled through a `config.yaml` file, which specifies all input/output paths and parameters.

### Installation

**Clone this repository to your local machine**:

```bash
git clone https://github.com/yourusername/your_project.git
cd your_project
```

**Create a virtual environment (optional but recommended)**:

```bash
python -m venv venv
source venv/bin/activate  # On Windows use `venv\Scripts\activate`
```

**Install required dependencies**:

```bash
pip install -r requirements.txt
```

**Download data and model from this link**:

```bash
pip install gdown
gdown https://drive.google.com/uc?id=your_file_id
unzip data_and_model.zip
```

Note: If a `requirements.txt` file is not present, install the dependencies individually (see Dependencies).

### How to Run

**Execute the main script using the following command**:

```bash
python main.py --config config.yaml
```

Note: If you do not specify the `--config` argument, the script will default to using `config.yaml`.

### Configuration of `config.yaml` File

The application is configured through a `config.yaml` file, which contains several sections:

#### Input Files
- `msp_file`: Path to the input MSP file containing spectra data.
- `reference_database`: Path to the reference database pickle file.
- `model_path`: Path to the pre-trained CNN model file.

#### Output Files
- `preprocessed_data`: Path where the preprocessed data will be saved (pickle format).
- `prediction_results`: Path for the final prediction results CSV file. Supports variable substitution for `top_n_candidates`.

#### Parameters
- `tolerance`: Tolerance for grouping m/z values during preprocessing. (default: 0.01)
- `max_mz`: Maximum m/z value to consider when aligning spectra. (default: 700)
- `resolution`: m/z resolution for aligning spectra. (default: 0.01)
- `intensity_threshold`: Minimum intensity percentage to consider peaks. (default: 1)
- `top_n_candidates`: Number of top candidate molecules to retrieve from the reference database. (default: 5)
- `input_file_type`: Type of the input MSP file. Options are 'with_smiles' and 'without_smiles'. (default: 'with_smiles')

#### Example `config.yaml` File

```yaml
# config.yaml

# Input files
msp_file: 'input_spectra.msp'                    # Path to your MSP file
reference_database: 'sample_reference_database.pkl'  # Path to your reference database
model_path: 'model.mol2vec_ints_nl_ddb_0_01_up_wo_dup_bin'  # Path to your trained model

# Output files
preprocessed_data: 'preprocessed_data.pkl'       # Path to save preprocessed data
prediction_results: 'prediction_results.csv'     # Path to save prediction results

# Parameters
tolerance: 0.01
max_mz: 700
resolution: 0.01
intensity_threshold: 1        # Intensity threshold in percentage
top_n_candidates: 5           # Number of top candidates to retrieve

# Input type
input_file_type: 'with_smiles'   # Options: 'with_smiles', 'without_smiles'
```

### Using Different Input File Types

- `with_smiles`: Use this option if your MSP file includes SMILES strings for each spectrum. The pipeline will utilize SMILES information during data processing and candidate matching, including Tanimoto similarity calculations.

- `without_smiles`: Use this option if your MSP file does not include SMILES strings. The pipeline will process the data accordingly and perform candidate matching without relying on SMILES information.

### Dependencies

Ensure you have the following libraries installed:
- Python 3.6+
- pandas >= 1.0.0
- numpy >= 1.18.0
- rdkit >= 2020.09.1
- torch >= 1.5.0
- scipy >= 1.4.0
- pyyaml >= 5.3.0
- argparse (Built-in)

Note: `rdkit` is best installed via conda due to its dependencies.

### Custom Modules

Ensure the `cnn_train` module is present in your project and contains `up_cnn_model.py` and `spectra_inference_dataset_loader.py`.

### Project Structure

```
spectra2moleculeCombine/
├── data_processing.py                  # Functions related to data preprocessing
├── model_utils.py                      # Functions related to model loading and prediction
├── reference_utils.py                  # Functions related to reference database processing
├── main.py                             # Main script to run the pipeline
├── config.yaml                         # Configuration file
├── data_loaders/                       # Directory containing data loader modules
│   ├── __init__.py
│   ├── inference_dataset_loader.py          # For 'with_smiles' input type
│   └── spectra_inference_dataset_loader.py  # For 'without_smiles' input type
├── cnn_train/                          # Directory containing custom model modules
│   ├── __init__.py
│   └── up_cnn_model.py
├── input_spectra.msp                   # Input spectra file
├── sample_reference_database.pkl       # Reference database file
├── requirements.txt                    # List of required Python packages
└── README.md                           # Project documentation
```

### Additional Information

#### Adjusting the Python Path

If your project relies on locally stored libraries or specific directories that are not installed in standard Python paths, you may need to adjust the Python path. Here's how you can do it within your script:

```python
import sys

# Adjust the path to include the directory where your local libraries are stored
sys.path.append('path/to/your/library')
```

#### Variable Substitution in `config.yaml`

The `prediction_results` filename in `config.yaml` can include `${top_n_candidates}` which will be replaced with the actual number specified in `top_n_candidates`.

## Contributing

Contributions are welcome! Please open an issue or submit a pull request for any improvements or bug fixes.

## License

This project is licensed under the MIT License.

## Acknowledgements

- RDKit: Open-source cheminformatics software.
- PyTorch: Deep learning framework used for model implementation.

Enjoy using the Mass Spectrometry Data Processing and Prediction Pipeline!

If you have any questions or need assistance, feel free to reach out.
```
