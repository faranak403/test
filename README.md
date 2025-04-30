# BioLogicGP package
This package is designed to find a logical relationship between biological data such as microRNAs. However, it can be used for other types of data that are classified as binary (patient/healthy, positive/negative, spam/non-spam, etc.).
## Installation
You can install the 'BioLogicGP' package using one of the following methods. Both methods require Python 3.6 or higher and 'pip' to be installed.

### Method 1: Install from Wheel File or source distribution (.whl or .tar.gz)
The wheel file is a pre-built binary distribution, which is typically faster to install.

**Install the Package**: 
Run the following command in your terminal, replacing <path-to-whl> with the path to the .whl file:

Replace `<path-to-file>` with the path to the `.whl` or `.tar.gz` file:
Example:
```bash
pip install dist/biologicgp-1.0.0-py3-none-any.whl 
```
or
```bash
pip install dist/biologicgp-1.0.0.tar
```
Verify Installation: 
Comfirm the package is installed::
```bash
python -c "from BioLogicGP import BooleanFunction; print('Installation successful')"
```
### Method 2: Install from Source code
Built the distribution files: 
1. open your terminal, navigate to the directory contaimimg your setup.py file. 
2. Run the following command to generate distribution files:
```bash
python setup.py sdist bdist_wheel

This creates two files in dist directory:
+ A source distribution(.tar.jz)
+ A whell file(.whl)
3. Install using pip with one of the generated files, as described in Methode 1:
bash
pip install <path-to-whl> 
or 
bash
pip install <path-to-tar-gz>
```

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

**Inputs**:

- `data (pandas.DataFrame)`: Binary dataset (all features must be 0s and 1s).
- `pop_size (int, optional)`: Population size for genetic programming (default: 400).
- `generations (int, optional)`: Number of generations (default: 80).
- `cx_prob (float, optional)`: Crossover probability (default: 0.5).
- `mut_prob (float, optional)`: Mutation probability (default: 0.2).

**Output**:

- A Boolean function derived from the genetic programming algorithm.

**Example**:
```python
import pandas as pd
from BioLogicGP.GP import GP_f

# Load binary dataset
data = pd.read_csv("data.csv")  # Ensure all features are binary
boolean_function = GP_f(data, pop_size=400, generations=80)
print(boolean_function)
```

### find_thresholds

Calculates ROC-based thresholds for converting continuous features to binary.
**Inputs**:
- `data` (pandas.DataFrame): Dataset with continuous features and binary labels.
- `target name`: this is the target column name in your dataset
**Output**:
- Dictionary of thresholds for each feature The key of which is the name of the features and the value is the corresponding threshold..

**Example**:
```python
from BioLogicGP.Thresholds import find_thresholds
import pandas as pd

data = pd.read_csv("path/to/your/data.csv")
target_name = "target name of your dataset" 
thresholds = find_thresholds(data, target_name)
print(thresholds)  # Output: {'feature1':5, 'feature1':10, ...}
```
### convert_dataset
Converts a dataset’s features to binary based on provided thresholds.

**Inputs**:
- `x` (pandas.DataFrame): Features in your dataset.
- `y` (pandas.Series): Binary target values
- `thresholds (dict)`: Thresholds for each feature.
**Output**:
- Binary pandas DataFrame.
**Example**:
```python
from BioLogicGP.Thresholds import convert_dataset
import pandas as pd
data = pd.read_csv("path/to/your/data.csv")
x = data.drop(target_name, axis=1)
y = df[target_name]
binary_data = convert_dataset(data, y, thresholds)
```

# Use Cases

Biological Research: Identify logical relationships between microRNAs or other biomarkers to distinguish between healthy and diseased states.
General Binary Classification: Apply to any dataset with binary outcomes.
