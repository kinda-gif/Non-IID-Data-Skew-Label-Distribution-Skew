# LabelSkewSplitter Library
LabelSkewSplitter is a Python library that partitions datasets by sorting samples according to their labels and dividing them into sequential segments. Each segment is saved as a client-specific subset, resulting in distinct label distributions. This method is particularly useful for simulating non-IID data conditions in **Federated Learning** and **Distributed Machine Learning** environments.

## Installation

To install the `labelskewsplitter` library, you can use pip:

```bash
pip install labelskewsplitter
```

Note: This library requires `pandas` to be installed.

## Usage

The labelskewsplitter library offers a straightforward function to divide a Pandas DataFrame into multiple sub-datasets according to **Label Distribution Skew**.

### Example

```python
import pandas as pd
from labelskewsplitter.splitter import skewed_split

# Create a sample DataFrame
data = {
    'feature1': [10, 20, 30, 40, 50, 60, 70, 80, 90, 100],
    'feature2': [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
    'label': [0, 1, 0, 1, 0, 1, 0, 1, 0, 1]
}
df = pd.DataFrame(data)

# Save the DataFrame to a CSV file (required by skewed_split for now)
df.to_csv('sample_data.csv', index=False)

# Split the dataset based on the 'label' column into 2 client files
skewed_split(
    input_csv='sample_data.csv',
    label_column='label',
    num_clients=2,
    name_prefix='Client'
)

# This will generate Client_Client1_LDS.csv and Client_Client2_LDS.csv
# in your current directory.
```

## Function Reference

### `skewed_split(input_csv, label_column, num_clients, name_prefix="Client")`

Splits the input dataset by label skew into multiple client files. The function sorts the input DataFrame by the specified `label_column` in ascending order and then divides the sorted data into `num_clients` approximately equal parts. This method ensures that each client dataset receives a portion of the data that maintains a certain level of label skew, as the data is not randomly shuffled before splitting.

**Parameters:**

- `input_csv` (str): Path to the CSV file containing the dataset.
- `label_column` (str): Name of the column in the CSV file that contains the labels to be used for skew-based splitting.
- `num_clients` (int): The number of client datasets to create.
- `name_prefix` (str, optional): Prefix for the output CSV file names. Defaults to `Client`.

**Returns:**

- None. The function saves the split datasets as CSV files in the current working directory.

## Mathematical Description of Label Distribution Skew

Label distribution skew in non-IID (Independent and Identically Distributed) data refers to the scenario where the distribution of labels varies across different subsets of a dataset. In the context of federated learning, this means that the local datasets held by different clients do not have the same proportion of classes as the global dataset or other client datasets.

Mathematically, let $Y$ be the label variable. For a dataset split into $K$ clients, the label distribution for client $k$ is $P_k(Y)$. Label distribution skew implies that for at least some clients $i$ and $j$ ($i \neq j$), $P_i(Y) \neq P_j(Y)$. This is in contrast to IID settings where $P_i(Y) = P_j(Y)$ for all $i, j$.

In the `labelskewsplitter` approach, by sorting the data based on the label column and then splitting it, we intentionally create subsets where certain labels are more prevalent at different parts of the sorted data, thus inducing a form of label distribution skew. This simulates real-world scenarios where data collected by different devices or entities naturally exhibits varying label proportions.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
