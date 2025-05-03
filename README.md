Hieu's notes:

ATAT: Automated Tissue Alignment and Traversal in Spatial Transcriptomics with Self-Supervised Learning

Why I care/want to learn about:
* implementation of triplet loss / alignment
* code that produces the (shortest) path -> very interesting
* practices to write good code in a ML research project (esp for bash jobs)

Other important remarks:
- Align tiles in H/E slides -> cells -> with spatial transcriptomics or gene expressions / locations/coordinates
	- morphological/shape/visual/location info from H/E slides combined with expression vectors and their coordinates on tissue
* Why this matters?
	* it’s hard to compare two tiles if everything inside (cells, visuals) is messy or in different places. 
	* doctors usually line tiles up by hand => very manual
	* Steven's model looks at the tissue H/E, figure out its shape/visual of the tiles, and trace a path through it
		* see how the gene expression changes along that trail, helping us understand diseases better without needing doctors to draw everything
- Why care about the (shortest) path on the H/E slide?
	- trace how cells are organized spatially i.e. how they connect or change over space. 
	- theory: how most "natural" route genes or tissue types might change
- How the path traverses, and why the weights and colors?
	- slide -> tiles, hex grid contains 7 tiles to form the hex (why?)
	- edge weights: smaller means more similar -> prioritized in traversing
	- within-color: similar-looking tiles
	- across-color: "how gene expression changes as we move through similar-looking regions of the tissue" -> must be learned (?) learned how? triple loss
		- start in one region, walk tile-by-tile based on small weights, and train/learn how gene levels vary along a walk 
		- visual change = gene expression change
- Training:
	- only learn the visual features using triplet loss (anchor tile then compared to similar tiles and dissimilar tiles -> learn the weights! rmb: no gene info here yet
	- Then: overlay gene expressions on these tiles, path is for analysis (no learning!)

Tissue vs gene:
- tissue (physical structure) is a group of cells that work together in the body like brain or colon  
	- H/E slides = images of tissue sections / biopsy
- gene (biological, chemical) is a segment/part of DNA that has instructions to make proteins
	- H/E slides can also be used to extract gene expression via RNA-seq
- a tile can contain many cells, each cell expresses thousands of gene expressionss 
- merge each data via same location -> allows cross-modal alignment/richer embeddings


---

Run scripts from the root directory of this repo.

Code for training the encoder model are contained in the `learn` subfolder with training scripts in `learn/scripts`.

Code for traversal and alignment are contained in the `traversal` subfolder with example scripts in `traversal/scripts`.

All code for generating the figures presented in the paper are contained within the `paper` subfolder. To examine the methods used to generate a figure, check out it's corresponding notebook.

Colon and stomach datasets are available upon reasonable request to the corresponding author.

Data should be organized as follows:
```
.
├── colon (organ)
│   ├── CD (disease or patient)
│   │   ├── A (slide)
│   │   │   ├── A.tif
│   │   │   └── outs (spaceranger output)
│   │   │       ├── filtered_feature_bc_matrix.h5
│   │   │       └── spatial
│   │   │           ├── scalefactors_json.json
│   │   │           ├── tissue_hires_image.png
│   │   │           └── tissue_positions_list.csv
│   │   └── B
│   │       ├── B.tif
│   │       └── outs
│   │           └── [...]
│   └── UC
│       └── [...]
└── dlpfc
    └── [...]
```

GitHub classifies this repository as 99% Jupyter Notebook because the paper figures are saved within each notebook, thus drastically inflating the file size of each notebook. The bulk of the methods are however written in plain python files.
