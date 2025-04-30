# BioLogicGP package
This package is designed to find a logical relationship between biological data such as microRNAs. However, it can be used for other types of data that are classified as binary (patient/healthy, positive/negative, spam/non-spam, etc.).
## Installation
You can install the 'BioLogicGP' package using one of the following methods. Both methods require Python 3.6 or higher and 'pip' to be installed.

## Method 1: Install from Wheel File or source distribution (.whl or .tar.gz)
The wheel file is a pre-built binary distribution, which is typically faster to install.

    Install the Package: Run the following command in your terminal, replacing <path-to-whl> with the path to the .whl file:

    pip install <path-to-whl> 
    or 
    pip install <path-to-tar-gz>
    Example:
    pip install dist/biologicgp-1.0.0-py3-none-any.whl 
    or
    pip install dist/biologicgp-1.0.0.tar

Verify Installation: Test that the package is installed correctly:

    python -c "from BioLogicGP import BooleanFunction; print('Installation successful')"

## Method 2: Install from Source code
    built the distribution files: 
        open your terminal, navigate to the directory contaimimg your setup.py file, and RUN:
        python setup.py sdist bdist_wheel
    This command will generate to two distribution files in a dist directory:
        A source distribution(.tar.jz)
        A whell file(.whl)
    Install using pip install:
    Now you can use the method 1 to install your package using the generated distribution files for example:
        pip install <path-to-whl> 
        or 
        pip install <path-to-tar-gz>


## Quick Start
The primary function for finding a Boolean function that describes the logical relationship in your data.

Inputs:

    file_path (str): Path to the input data file (e.g., CSV with features and binary labels).
    priority_d (dict): Dictionary mapping feature names to integer priorities (higher = more important).
    number_f (int): Number of high-priority features to exclude from Boolean function derivation.
    algorithm (str, optional): Algorithm to use (default: "gp" for genetic programming).

Process:

    Excludes high-priority features based on number_f.
    Computes thresholds for continuous features using ROC analysis.
    Converts the dataset to binary format.
    Applies the selected algorithm (genetic programming or quine mccluskey) to derive a Boolean function.

Output:

    A Boolean function or model representing the logical relationship in the data.

Here’s an example of how to use the main function, bio_logic_gp, to find a logical relationship in a dataset:

    from BioLogicGP.BooleanFunction import bio_logic_gp
    # Load your dataset
    file_path = "path/to/your/data.csv"

    # Define feature priorities (higher integer = higher priority)
    priority_dict = {"feature1": 2, "feature2": 1, "feature3": 0}
    number_f = 1
    algorithm_name = 'gp'  # or you can choose QC:Quine McCluskey

    # Run the main function
    result = bio_logic_gp(
        file_path,
        priority_dict,
        number_f,       # Exclude 1 highest-priority feature from simplification
        algorithm_name  
    )
    print(result)  # Output: Boolean function


## How to use the different functions in the package:

### GP_f

    Directly applies the genetic programming algorithm to a binary dataset.

    Inputs:

        data (pandas.DataFrame): Binary dataset (all features must be 0s and 1s).
        pop_size (int, optional): Population size for genetic programming (default: 400).
        generations (int, optional): Number of generations (default: 80).
        cx_prob (float, optional): Crossover probability (default: 0.5).
        mut_prob (float, optional): Mutation probability (default: 0.2).

    Output:

        A Boolean function derived from the genetic programming algorithm.


### find_thresholds

Calculates ROC-based thresholds for converting continuous features to binary.

**Inputs**:
- `data` (pandas.DataFrame): Dataset with continuous features and binary labels.
- `target_name` (str): Name of the target column in the dataset.

**Output**:
- Dictionary mapping feature names to their corresponding thresholds.

**Example**:
```python
import pandas as pd
from BioLogicGP.Thresholds import find_thresholds

data = pd.read_csv("data.csv")
target_name = "target"
thresholds = find_thresholds(data, target_name)
print(thresholds)  # Output: {'feature1': 5.0, 'feature2': 10.0, ...}
### convert_dataset

    Converts a dataset’s features to binary based on provided thresholds.

    Inputs:

        x (pandas.DataFrame): Features in your dataset.
        y : Target column in the dataset
        thresholds (list): Thresholds for each feature.

    Output:

        Binary pandas DataFrame.
    Example:
        from BioLogicGP.Thresholds import convert_dataset
        import pandas as pd
        data = pd.read_csv("path/to/your/data.csv")
        x = data.drop(target_name, axis=1)
        y = df[target_name]
        binary_data = convert_dataset(data, y, thresholds)

# Use Cases

    Biological Research: Identify logical relationships between microRNAs or other biomarkers to distinguish between healthy and diseased states.
    General Binary Classification: Apply to any dataset with binary outcomes.
