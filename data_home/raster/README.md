## Sample data

Epoch `20231214` contains all scans from `sb44` and `sb45`.

Epoch `20231215` contains all scans from `sb44` and `sb45`, with the addition of `sb46`.

The idea is to have enough sample data to simulate the situation where new data has been added, but I want the scripts to re-use any artifacts or labeled data from previous runs. In particular, hand-categorized data in `03_VISUAL_HAND_LABELED` should preserve any work I did in previous runs.

To set this up, I had already categorized (in my actual data_home outside of this repo's sample data) the pages corresponding to the 

```python
PREV_EPOCH = "20231214"
CURR_EPOCH = "20231215"

PREV_EPOCH_RASTER = singleline_data_home(default='../data_home') / f"raster/epoch-{PREV_EPOCH}"
CURR_EPOCH_RASTER = singleline_data_home(default='../data_home') / f"raster/epoch-{CURR_EPOCH}"

PREV_01_FLAT = PREV_EPOCH_RASTER / "01_FLAT"
PREV_03_HAND = PREV_EPOCH_RASTER / "03_VISUAL_HAND_LABELED"

prev_files = get_image_files(PREV_01_FLAT)

prev_basenames = [os.path.basename(f) for f in prev_files]
print(prev_basenames[:5])

EXTERNAL_EPOCH = singleline_data_home(default='/Users/al/Dropbox/2-Areas/2-Sketchbooks/singleline_data') / f"raster/epoch-{PREV_EPOCH}"
EXTERNAL_03_HAND = EXTERNAL_EPOCH / '03_VISUAL_HAND_LABELED'
external_hand_labeled = get_image_files(EXTERNAL_03_HAND)

import shutil

for f in external_hand_labeled:
    f_basename = os.path.basename(f)
    f_dirname = os.path.dirname(f)
    f_dirname_basename = os.path.basename(f_dirname)

    if f_basename in prev_basenames:
        new_dir = os.path.join(PREV_03_HAND, f_dirname_basename)
        if not os.path.isdir(new_dir):
            os.makedirs(new_dir)
        shutil.copy(f, os.path.join(new_dir, f_basename))
```