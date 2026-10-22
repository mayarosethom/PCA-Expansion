# PCA Expansion

Single-cell analysis of Visium HD spatial data (2 µm bins) from three samples, MIA-1, MIA-2, and MIA-3. Bins are grouped into cells using StarDist and bin2cell, the cells are refined with a PCA-based reassignment step, and the three samples are then integrated with bbknn.

## Pipeline order

1. **PCA Expansion Competitive Reassignment MIA-1.ipynb** (run once per sample)
   - Loads the Visium HD data with bin2cell and destripes it
   - Runs StarDist on the H&E image to find nuclei
   - Expands each nucleus up to 4 µm using PCA similarity. If more than one nucleus claims a bin, the bin goes to the nucleus whose PCA centroid is closest
   - Makes a gene expression image, runs StarDist again (fluorescence model), and adds the extra nuclei to the labels (`labels_joint`)
   - Saves a per-sample `.h5ad` file

2. **NEWEST Integration on PCA Expansion (MIA-1, MIA-2, MIA-3).ipynb**
   - Converts bins to cells with `bin_to_cell`
   - Filters, normalizes, and log transforms each sample
   - Combines MIA-1, MIA-2, and MIA-3 and corrects batch effects with bbknn
   - Runs UMAP and Leiden clustering at several resolutions and finds marker genes for each
   - Saves the integrated object as an `.h5ad` file

## Requirements

Python with stardist, bin2cell, scanpy, squidpy, anndata, bbknn, numpy, pandas, scikit-learn, and matplotlib.

## Notes

- Data files (`.h5ad`, `.npz`, images) are not included because of their size.
- File paths in the notebooks are hardcoded to my local machine and need to be changed to run them elsewhere.
- The same PCA expansion notebook was run for each sample, with only the file paths changed.
