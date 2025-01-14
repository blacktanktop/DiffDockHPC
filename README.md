# DiffDockHPC
DiffDockHPC is a fork of [DiffDock](https://github.com/gcorso/DiffDock), which adds support to run DiffDock on HPC systems using Slurm and Singularity.  
For more details about DiffDock itself, we refer to the [DiffDock Github](https://github.com/gcorso/DiffDock) and the [Paper on arXiv](https://arxiv.org/abs/2210.01776)

### Requirements:
* Singularity 
* Slurm

### Installation instructions:
1. Clone the repository  
    ```
    git clone https://github.com/Jnelen/DiffDockHPC
    ```
2. Download the singularity image (~7.6 GB). It contains all the necessary packages and dependencies to run DiffDock correctly
   ```
   cd DiffDockHPC
   ```  
   ```
   wget --no-check-certificate -r "https://drive.google.com/u/1/uc?export=download&confirm=TLUF&id=1pcLD6gjwvJhpG8vEZ_RGpILvDOdOnSl3" -O DiffDockHPC.sif
   ```  
   
3. run a test example to generate the necessary cache look-up tables for SO(2) and SO(3) distributions. (This only needs to happen once and should only take about 5-10 minutes)  
   ```
   python inferenceVS.py -p data/1a0q/1a0q_protein_processed.pdb -l data/1a0q/ -out TEST -j 1
   ```  
   Or if you have access to a GPU, you can also add the -gpu tag like this:  
   ```
   python inferenceVS.py -p data/1a0q/1a0q_protein_processed.pdb -l data/1a0q/ -out TEST -j 1 -gpu
   ```  

### Options

The main file to use is `inferenceVS.py`. It has the following options/flags:  

- `-p`, `-r`, `--protein_path`: 
  Path to the protein/receptor `.pdb` file.

- `-l`, `--ligand`: 
  The path to the directory of (separate) `mol2`/`sdf` ligand files.

- `-o`, `--out`, `--out_dir`: 
  Directory where the output structures will be saved to.

- `-j`, `--jobs`: 
  Number of jobs to use.

- `-qu`, `--queue`: 
  On which node to launch the slurm jobs. The default value is the default queue for the user. Might need to be specified if there is no default queue configured.

- `-m`, `--mem`: 
  How much memory to use for each job. The default value is `8GB`.

- `-gpu`, `--gpu`: 
  Use GPU resources. This will accelerate docking calculations if a compatible GPU is available.

- `-c`, `--cores`: 
  How many cores to use for each job. The default value is `1` when used with the GPU option enabled, otherwise it defaults to `4` cores.

- `-n`, `--num_outputs`: 
  How many structures to output per compound. The default value is `1`.

- `--remove_hs`: 
  Remove the hydrogens in the final output structures.

- `--keep_local_structures`: 
  Keeps the local structure when specifying an input with 3D coordinates instead of generating them with RDKit.

- `--keep_cache`: 
  Keep the Cache directories after finishing the calculations. This can save time when rerunning calculations with the same input files. (Not recommended)

- `-h`, `--help`: 
  Show the help message and exit.

## License
MIT

