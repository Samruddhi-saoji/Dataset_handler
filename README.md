# Dataset_handler
My personal library for to handle image datasets for classification tasks

It supports:
- Loading datasets into arrays (`X`, `Y`) for use with ML models.  
- Automatic category encoding for classification tasks
- Converting datasets into CSV format.  
- Compressing images to a smaller resolution.  
- Creating a compressed version of the dataset while preserving its original structure.  

---

## Dataset Organization

The dataset should be structured as follows:


- 'Dataset' directory containing one subfolder per class
- Subfolder name will be treated as the class name.   

---

## Class Attributes

### `Dataset`
- **dataset_path**: Path to the dataset folder.  
- **category_names**: List of all category (subfolder) names.  
- **encoding**: Dictionary of `{category: int}` mappings (category → encoded integer).  
- **num_categories**: Number of categories in the dataset.  
- **shape**: Expected shape of images (only images matching this shape are included).  
- **file_names**: List of image file names (only those matching the expected shape).  
- **X**: Array of all images (3D array).  
- **Y**: Array of labels (1D array, integer encoded).  
- **num_images**: Total number of images in the dataset.  

---

## Class Methods

### `load_data()`
- Loads images from the dataset into arrays.  
- Returns:  
  - **X**: Array of images.  
  - **Y**: Array of labels (integer encoded).  
- Filters out images that don’t match the expected shape.  

---

### `convert_to_csv(dest_path)`
- Converts the dataset into a CSV file.  
- Each row = one image.  
- Columns:  
  - First *n* columns = pixel values.  
  - Last column = integer label.  
- Saves the CSV file at `dest_path`.  

---

### `compress_data(r_new, c_new)`
- Compresses all images in the dataset from `(rows, cols)` → `(r_new, c_new)`.  
- Returns a list of compressed images (2D arrays).  

---

### `create_compressed_dataset(r_new, c_new, dest_path)`
- Creates a **new dataset folder** with compressed images.  
- Preserves the original dataset structure:  
  - Main folder → subfolders for each category.  
  - Image file names remain the same.  
- Saves compressed images into the new dataset at `dest_path`.  

---
## Example Usage

```python
dataset_path = "path to dataset"
image_shape = (0, 0)  # expected shape

# Load dataset
ds = Dataset(dataset_path, expected_shape=image_shape)
ds.load_data()
X, Y = ds.X, ds.Y 

# Shuffle the dataset
from sklearn.utils import shuffle
X, Y = shuffle(X, Y, random_state=42)  

# Split into training and test sets
from sklearn.model_selection import train_test_split
X_train, X_test, Y_train, Y_test = train_test_split(
    X, Y, test_size=0.3, random_state=42
)
```
