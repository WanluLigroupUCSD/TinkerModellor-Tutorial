Command Line Version Overview
====================

Basic Usage
-----------

1. Introduction
---------------

TinkerModellor is a powerful command-line tool designed for handling various tasks in molecular simulations. Whether you need to convert file formats, merge systems, calculate RMSD, or perform more complex electric field calculations, TinkerModellor is equipped to meet your needs. In this section, we will introduce how to run basic TinkerModellor commands and lay the foundation for subsequent operations.

2. Basic Command Structure
--------------------------

The basic command structure for using TinkerModellor is as follows:

.. code-block:: bash

   tkm [sub-command] [options]

**[sub-command]**: The sub-command specifies the specific type of operation. Examples include:

- `transform`: Convert file formats.
- `merge`: Merge two molecular systems.
- `delete`: Remove specific atoms from the system.
- `rmsd`: Calculate the root-mean-square deviation of a trajectory file.

**[options]**: The options provide the necessary parameters for the sub-command, such as file paths and format selections. Here are some common options explained:

- `--top`: Specifies the input topology file path. Supported formats are Amber (.prmtop/.top), CHARMM (.psf), and GROMACS (.top).
- `--crd`: Specifies the input coordinate file path. Supported formats are Amber (.inpcrd/.crd), CHARMM (.crd), and GROMACS (.gro).
- `--xyz`: Specifies the output Tinker format file path.
- `--clean`: Removes temporary files (optional) created during GROMACS format conversion. Default: False.
- `--format`: Specifies the input file format (e.g., amber, gmx, charmm). The default is GROMACS.
- `--ff`: Selects the force field type (optional). Default: 1 (1: AMOEBABIO18, 2: AMOEBABIO09, 3: AMOEBAPRO13).

3. Running a Basic Command
--------------------------

Here is an example of how to perform a simple file conversion using TinkerModellor:

.. code-block:: bash

   tkm transform --top my_topology.prmtop --crd my_coordinates.inpcrd --xyz my_output.xyz --clean --ff AMOEBABIO18

**Command Explanation:**

- `transform` is the sub-command, indicating file format conversion.
- `--top my_topology.prmtop`: Specifies the input Amber topology file path.
- `--crd my_coordinates.inpcrd`: Specifies the input Amber coordinate file path.
- `--xyz my_output.xyz`: Specifies the output Tinker format file path.
- `--clean`: Removes temporary files after the conversion is complete.
- `--ff AMOEBABIO18`: Uses the AMOEBABIO18 force field for the conversion.

After running this command, you will obtain a `.xyz` file containing the converted molecular structure data, and the temporary files will be deleted.

4. Viewing Help Information
---------------------------

TinkerModellor includes detailed built-in help documentation, which you can access at any time via the command line:

- **To view all sub-commands:**

  .. code-block:: bash

     tkm --help

  This command will list all available sub-commands along with a brief description of each.

- **To view help for a specific sub-command:**

  .. code-block:: bash

     tkm [sub-command] --help

  For example, to view detailed usage information for the `merge` sub-command, you can run:

  .. code-block:: bash

     tkm merge --help

  This will display all available options and usage instructions for the `merge` sub-command.

5. Common Issues and Tips
-------------------------

While using TinkerModellor, you may encounter some common issues. Here are a few examples and their solutions:

- **File Path Errors**: Ensure that the file paths you provide are correct and that the files exist. If the path is incorrect, you may see an error message like "File not found."
- **Parameter Type Errors**: If a parameter type does not match the expected format (e.g., providing a string instead of an integer), TinkerModellor will throw an error. In such cases, check your input parameters and refer to the help information to ensure correct usage.
- **Option Conflicts**: Some sub-commands do not allow specific option combinations to be used simultaneously (e.g., the `ef` sub-command's `--point` and `--ndx` options cannot be used together). If a command fails, TinkerModellor will indicate the conflicting options.

6. Transition to Detailed Modules
---------------------------------

In addition to command-line operations, TinkerModellor can also be used as a Python package for direct invocation within your code. If you wish to learn how to utilize TinkerModellor's functionalities within a script, please refer to the “Package Interface” section in the following parts.

Now that you understand how to run basic TinkerModellor commands, we will explore each sub-command's specific functionalities and options in detail, helping you fully harness the power of TinkerModellor.

Command Line Version Detailed Functionalities
===========================

1. Transform Module
-------------------

1.1 Overview
~~~~~~~~
The `Transform` module in TinkerModellor is designed to convert molecular simulation files from widely-used formats such as Amber, CHARMM, and GROMACS into Tinker-compatible `.xyz` files. This module is particularly useful for users who need to integrate their molecular data into the Tinker simulation environment, allowing for seamless transitions between different molecular dynamics software.

1.2 Command Syntax
~~~~~~~~
To use the Transform module, the command structure is as follows:

.. code-block:: bash

   tkm transform --top <topology_file> --crd <coordinate_file> --xyz <output_file> [options]

**Options:**

- `--top`: Specifies the path to the input topology file. Supported formats include Amber (`.prmtop`/`.top`), CHARMM (`.psf`), and GROMACS (`.top`).
- `--crd`: Specifies the path to the input coordinate file. Supported formats include Amber (`.inpcrd`/`.crd`), CHARMM (`.crd`), and GROMACS (`.gro`).
- `--xyz`: Specifies the path to the output Tinker `.xyz` file. If not provided, the output will be saved with a default name based on the input files.
- `--clean`: (Optional) Removes temporary files created during GROMACS format conversion. Use `--clean` to activate this option. The default is `False`, meaning that temporary files will be retained unless this option is specified.
- `--format`: (Optional) Specifies the input file format. Available options are `amber`, `gmx`, and `charmm`. The default is `GROMACS`.
- `--ff`: (Optional) Selects the force field type. Available options are:
  1. AMOEBABIO18 (default)
  2. AMOEBABIO09
  3. AMOEBAPRO13 (protein only)
  4. AMOEBANUC17 (nuclear acid only)

1.3 Example Usage
~~~~~~~~
Below is a practical example of how to use the `Transform` module to convert an Amber topology and coordinate file into a Tinker `.xyz` file:

.. code-block:: bash

   tkm transform --top my_topology.prmtop --crd my_coordinates.inpcrd --xyz my_output.xyz --clean --format amber --ff 1

**Explanation:**

- `--top my_topology.prmtop`: Specifies the path to the Amber topology file.
- `--crd my_coordinates.inpcrd`: Specifies the path to the Amber coordinate file.
- `--xyz my_output.xyz`: Specifies the output file name in Tinker `.xyz` format.
- `--clean`: Removes temporary files generated during the conversion process.
- `--format amber`: Indicates that the input files are in Amber format.
- `--ff 1`: Uses the AMOEBABIO18 force field for the conversion.

After running this command, the tool will generate a `.xyz` file that contains the converted molecular structure data in Tinker format. Additionally, any temporary files created during the conversion process will be automatically deleted due to the `--clean` option.

1.4 Examples
~~~~~~~~
#Example 1: Converting Amber Files

**Location:** `example/transform/amber_format/`

In this example, we will convert Amber topology and coordinate files (`.prmtop` and `.inpcrd`) into a Tinker-compatible `.xyz` file.

**Files Involved:**

- `solvate.prmtop`: Amber topology file.
- `solvate.inpcrd`: Amber coordinate file.
- `tkm.xyz`: Output Tinker `.xyz` file.

**Command:**

.. code-block:: bash

   tkm transform --top solvate.prmtop --crd solvate.inpcrd --xyz tkm.xyz --format amber

After executing this command, you should find the `tkm.xyz` file in the directory. This file contains the converted molecular structure data in Tinker format.

#Example 2: Converting GROMACS Files

**Location:** `example/transform/gmx_format/ex1/`

In this example, we will convert GROMACS topology and coordinate files (`.top` and `.gro`) into a Tinker-compatible `.xyz` file.

**Files Involved:**

- `gromacs.top`: GROMACS topology file.
- `gromacs.gro`: GROMACS coordinate file.
- `tinker.xyz`: Output Tinker `.xyz` file.

**Command:**

.. code-block:: bash

   tkm transform --top gromacs.top --crd gromacs.gro --xyz tinker.xyz --format gmx

After running this command, you should see the `tinker.xyz` file in the directory, containing the converted molecular structure data in Tinker format.

2. Merge Module
-------------------

2.1 Overview
~~~~~~~~
The `Merge` module in TinkerModellor is designed to combine two molecular systems into a single Tinker-compatible `.xyz` file. This module is particularly useful when you need to merge separate molecular simulations or when combining different segments of a molecular structure. The module also supports merging the force fields of the two systems, ensuring that the resulting system is fully compatible with the Tinker simulation environment.

2.2 Command Syntax
~~~~~~~~
To use the `Merge` module, the command structure is as follows:

.. code-block:: bash

   tkm merge --tk1 <first_txyz_file> --tk2 <second_txyz_file> --xyz <output_file> [options]

**Options:**

- `--tk1`: Specifies the path to the first input TXYZ file.
- `--tk2`: Specifies the path to the second input TXYZ file.
- `--xyz`: Specifies the path to the output TXYZ file that will contain the merged system.
- `--ff1`: (Optional) Specifies the path to the force field file of the first system.
- `--ff2`: (Optional) Specifies the path to the force field file of the second system.
- `--ffout`: (Optional) Specifies the path to the force field file for the output system. If either `ff1` or `ff2` is provided, then all three (`ff1`, `ff2`, and `ffout`) must be specified.

2.3 Example Usage
~~~~~~~~
Below is a practical example of how to use the Merge module to combine two Tinker `.xyz` files into a single system:

**Explanation:**

- `--tk1 system1.xyz`: Specifies the path to the first Tinker `.xyz` file.
- `--tk2 system2.xyz`: Specifies the path to the second Tinker `.xyz` file.
- `--xyz merged_system.xyz`: Specifies the path for the output `.xyz` file that will contain the merged system.
- `--ff1 forcefield1.prm`: Specifies the path to the force field file for the first system.
- `--ff2 forcefield2.prm`: Specifies the path to the force field file for the second system.
- `--ffout merged_forcefield.prm`: Specifies the path for the merged force field file for the output system.

After running this command, the tool will generate a new `.xyz` file that contains the combined molecular structure data from both input systems. If force field files were provided, they would also be merged into a single force field file.

2.4 Example: Merging Molecular Systems
~~~~~~~~
In this section, we will walk you through a practical example of using the `Merge` module to combine two molecular systems. This example is located in the `example/merge/ex1/` directory and demonstrates how to merge two Tinker `.xyz` files with and without merging their respective force fields.

**Files Involved:**

- `amoebabio18.prm`: A force field file used for the first system (protein).
- `ligand.prm`: A force field file used for the second system (ligand).
- `ligand.xyz`: The `.xyz` file representing the molecular structure of the ligand.
- `protein.xyz`: The `.xyz` file representing the molecular structure of the protein.
- `merged_ff.prm`: The resulting merged force field file.
- `merged_with_ff.xyz`: The resulting merged `.xyz` file when both force fields are combined.
- `merged_without_ff.xyz`: The resulting merged `.xyz` file when force fields are not combined.

**Steps to Merge the Systems**

**Merging Without Force Fields**

To merge the two systems without combining their force fields, you would use the following command:

.. code-block:: bash

   tkm merge --tk1 protein.xyz --tk2 ligand.xyz --xyz merged_without_ff.xyz

After running this command, the `merged_without_ff.xyz` file is generated. This file contains the combined molecular structure of the protein and ligand without any changes to their respective force fields.

**Merging With Force Fields**

To merge the two systems along with their force fields, you would use the following command:

.. code-block:: bash

   tkm merge --tk1 protein.xyz --tk2 ligand.xyz --xyz merged_with_ff.xyz --ff1 amoebabio18.prm --ff2 ligand.prm --ffout merged_ff.prm

After running this command, the tool generates two files:

- `merged_with_ff.xyz`: Contains the combined molecular structure of the protein and ligand with their force fields merged.
- `merged_ff.prm`: Contains the combined force fields from both the protein and ligand.

3. Delete Module
-------------------

3.1 Overview
~~~~~~~~
The `Delete` module in TinkerModellor is designed to remove specific atoms from a molecular system represented in a Tinker-compatible `.xyz` file. This module is particularly useful when you need to refine or modify a molecular structure by selectively removing atoms, such as deleting solvent molecules or unwanted residues from your system.

3.2 Command Syntax
~~~~~~~~
To use the Delete module, the command structure is as follows:

.. code-block:: bash

   tkm delete --tk <input_txyz_file> --ndx <atom_indices> --xyz <output_file>

**Options:**

- `--tk`: Specifies the path to the input TXYZ file from which atoms will be deleted.
- `--xyz`: Specifies the path to the output TXYZ file that will contain the modified system after the deletion.
- `--ndx`: Specifies the indices of the atoms to be deleted. This can be:
  - A single integer (e.g., `10`).
  - A comma-separated list of integers (e.g., `1,2,3`).
  - A range of integers (e.g., `1-10`).
  - A combination of the above (e.g., `1,2,3,5-10`).

3.3 Example Usage
~~~~~~~~
Below is a practical example of how to use the Delete module to remove specific atoms from a Tinker `.xyz` file:

.. code-block:: bash

   tkm delete --tk input_file.xyz --ndx 5,10-15 --xyz output_file.xyz

**Explanation:**

- `--tk input_file.xyz`: Specifies the path to the input `.xyz` file.
- `--ndx 5,10-15`: Specifies that atoms with indices 5 and those in the range 10-15 should be deleted.
- `--xyz output_file.xyz`: Specifies the path for the output `.xyz` file that will contain the molecular system after the specified atoms have been removed.

After running this command, the tool will generate a new `.xyz` file that contains the modified molecular structure data, excluding the specified atoms.

3.4 Example: Deleting Atoms from a Molecular System
~~~~~~~~
In this section, we'll walk you through a practical example of using the Delete module to remove specific atoms from a molecular system. This example is located in the `example/delete/ex1/` directory and demonstrates how to delete atoms from a Tinker `.xyz` file.

**Files Involved:**

- `protein.xyz`: This is the input file representing the molecular structure of a protein. It contains the atom coordinates and connectivity information in Tinker `.xyz` format.
- `deleted.xyz`: This is the output file generated after specific atoms have been removed from the `protein.xyz` file.

**Command:**

.. code-block:: bash

   tkm delete --tk protein.xyz --ndx 1,2,3,5-10 --xyz deleted.xyz

**Explanation:**

- `--tk protein.xyz`: Specifies the path to the input `.xyz` file.
- `--ndx 1,2,3,5-10`: Specifies that atoms with indices 1, 2, 3, and 5 through 10 should be deleted.
- `--xyz deleted.xyz`: Specifies the path for the output `.xyz` file that will contain the molecular system after the specified atoms have been removed.

After running this command, the `deleted.xyz` file will be generated, containing the molecular structure of the protein with the specified atoms removed.

4. Replace Module
-------------------

4.1 Overview
~~~~~~~~
The `Replace` module in TinkerModellor is designed to merge two molecular systems into a single Tinker-compatible `.xyz` file, with the unique functionality of removing coincident water and ion molecules in the first system (`tk1`). This module is particularly useful when you need to integrate two molecular systems but want to avoid overlapping solvent or ion molecules, ensuring a clean and accurate final structure.

4.2 Command Syntax
~~~~~~~~
To use the Replace module, the command structure is as follows:

.. code-block:: bash

   tkm replace --tk1 <first_txyz_file> --tk2 <second_txyz_file> --xyz <output_file> [options]

**Options:**

- `--tk1`: Specifies the path to the first input TXYZ file. This is the primary molecular system.
- `--tk2`: Specifies the path to the second input TXYZ file. This is the secondary molecular system.
- `--xyz`: Specifies the path to the output TXYZ file that will contain the merged system with coincident water and ion molecules removed from the first system.
- `--ff1`: (Optional) Specifies the path to the force field file of the first system.
- `--ff2`: (Optional) Specifies the path to the force field file of the second system.
- `--ffout`: (Optional) Specifies the path to the force field file for the output system. If either `--ff1` or `--ff2` is provided, then all three (`--ff1`, `--ff2`, and `--ffout`) must be specified.

4.3 Example Usage
~~~~~~~~
Below is a practical example of how to use the Replace module to merge two Tinker `.xyz` files while removing coincident water and ion molecules from the first system:

.. code-block:: bash

   tkm replace --tk1 system1.xyz --tk2 system2.xyz --xyz output_system.xyz --ff1 forcefield1.prm --ff2 forcefield2.prm --ffout output_forcefield.prm

**Explanation:**

- `--tk1 system1.xyz`: Specifies the path to the first Tinker `.xyz` file (e.g., the protein system).
- `--tk2 system2.xyz`: Specifies the path to the second Tinker `.xyz` file (e.g., a ligand or another molecular system).
- `--xyz output_system.xyz`: Specifies the path for the output `.xyz` file that will contain the merged system.
- `--ff1 forcefield1.prm`: Specifies the path to the force field file for the first system.
- `--ff2 forcefield2.prm`: Specifies the path to the force field file for the second system.
- `--ffout output_forcefield.prm`: Specifies the path for the merged force field file for the output system.

After running this command, the tool will generate a new `.xyz` file (`output_system.xyz`) that contains the merged molecular structure data from both input systems, with any overlapping water and ion molecules from the first system removed. If force field files were provided, they would also be merged into a single force field file.

4.4 Example: Replacing Water and Ions in Molecular Systems
~~~~~~~~
In this example, located in the `example/replace`, we will demonstrate how to use the Replace module to merge two Tinker-compatible `.xyz` files while removing coincident water and ion molecules from the first system. The files in this example include both molecular structure files and force field files, allowing us to showcase the full capabilities of the Replace module.

**Files Involved:**

- `amoebabio18.prm`: A force field file used for the first system (protein).
- `ligand.prm`: A force field file used for the second system (ligand).
- `ligand.xyz`: The `.xyz` file representing the molecular structure of the ligand.
- `protein.xyz`: The `.xyz` file representing the molecular structure of the protein.
- `merged_ff.prm`: The resulting merged force field file.
- `replaced.xyz`: The resulting merged `.xyz` file when water and ion molecules are removed from the first system.
- `replaced_with_ff.prm`: The resulting merged force field file after combining the force fields of the two systems.
- `replaced_with_ff.xyz`: The resulting merged `.xyz` file when both the systems and their force fields are combined.

**Steps to Replace and Merge the Systems**

**Merging Without Force Fields:**

To merge the two systems without combining their force fields and removing overlapping water and ion molecules, use the following command:

.. code-block:: bash

   tkm replace --tk1 protein.xyz --tk2 ligand.xyz --xyz replaced.xyz

After running this command, the `replaced.xyz` file is generated. This file contains the combined molecular structure of the protein and ligand, with overlapping water and ion molecules from the first system removed.

**Merging With Force Fields:**

To merge the two systems and their force fields, while removing overlapping water and ion molecules, use the following command:

.. code-block:: bash

   tkm replace --tk1 protein.xyz --tk2 ligand.xyz --xyz replaced_with_ff.xyz --ff1 amoebabio18.prm --ff2 ligand.prm --ffout replaced_with_ff.prm

After running this command, the tool generates two files:

- `replaced_with_ff.xyz`: Contains the combined molecular structure of the protein and ligand, with overlapping water and ion molecules removed and force fields merged.
- `replaced_with_ff.prm`: Contains the merged force fields from both the protein and ligand.

5. RMSD Module
-------------------

5.1 Overview
~~~~~~~~
The RMSD (Root Mean Square Deviation) module in TinkerModellor is designed to calculate the RMSD between different frames in a Tinker trajectory file (`.arc`). This is particularly useful for analyzing the structural stability of molecular simulations, comparing different conformations of a molecule over time.

5.2 Command Syntax
~~~~~~~~
To use the RMSD module, the command structure is as follows:

.. code-block:: bash

   tkm rmsd --xyz <xyz_file> --traj <trajectory_file> [options]

**Options:**

- `--xyz`: Specifies the path to the TXYZ file (`.xyz`) used to identify the topology.
- `--traj`: Specifies the path to the Tinker trajectory file (`.arc`).
- `--ref`: (Optional) Specifies the path to the reference coordinates file (`.xyz`). If not provided, the first frame of the trajectory will be used as the reference.
- `--skip`: (Optional) Skips frames in the trajectory file, useful for reducing computation time.
- `--ndx`: (Optional) Specifies the index of atoms for RMSD calculation. This can be a single integer (e.g., `10`), a list separated by commas (e.g., `1,2,3`), a range (e.g., `1-10`), or a combination of these.
- `--bfra`: (Optional) Specifies the beginning frame for the RMSD calculation. Default is `0`.
- `--efra`: (Optional) Specifies the ending frame for the RMSD calculation. Default is `-1` (i.e., the last frame).
- `--out`: (Optional) Specifies the path to the output CSV file where the RMSD values will be saved. The default is `./TKM_rmsd.csv`.

5.3 Example Usage
~~~~~~~~
Here’s a practical example of how to use the RMSD module to calculate the RMSD for a Tinker trajectory:

.. code-block:: bash

   tkm rmsd --xyz my_structure.xyz --traj my_trajectory.arc --ref reference_structure.xyz --ndx 1,2,3,5-10 --skip 2 --bfra 10 --efra 50 --out output_rmsd.csv

**Explanation:**

- `--xyz my_structure.xyz`: Specifies the path to the `.xyz` file used for topology.
- `--traj my_trajectory.arc`: Specifies the path to the Tinker trajectory file.
- `--ref reference_structure.xyz`: Specifies the path to the reference coordinates file.
- `--ndx 1,2,3,5-10`: Specifies the indices of atoms for RMSD calculation.
- `--skip 2`: Skips every second frame in the trajectory to speed up the calculation.
- `--bfra 10`: Starts the RMSD calculation from the 10th frame.
- `--efra 50`: Ends the RMSD calculation at the 50th frame.
- `--out output_rmsd.csv`: Outputs the RMSD values to `output_rmsd.csv`.

After running this command, you should find the `output_rmsd.csv` file containing the calculated RMSD values for the specified frames and atoms.

5.4 Example: Calculating RMSD for a Tinker Trajectory
~~~~~~~~
In this section, located in the `example/rmsd`, we will demonstrate how to use the RMSD module to calculate the RMSD of a molecular trajectory using the provided example files.

**Files Involved:**

- `pr_coord.xyz`: The reference Tinker XYZ file that defines the molecular topology.
- `pr_coord.arc`: The Tinker trajectory file (`.arc`) containing the time-series atomic coordinates.
- `rmsd.csv`: The output CSV file that will store the calculated RMSD values.

**Command:**

.. code-block:: bash

   tkm rmsd --xyz pr_coord.xyz --traj pr_coord.arc --out rmsd.csv

After executing this command, the tool will calculate the RMSD values for each frame in the trajectory relative to the reference structure. The results are stored in `rmsd.csv`, which can be opened and analyzed further.

6. Distance Module
-------------------

6.1 Overview
~~~~~~~~
The `Distance` module in TinkerModellor is designed to calculate the atomic distance between two specified atoms across different frames in a Tinker trajectory file (`.arc`). This module is particularly useful for analyzing the distance changes between specific atoms over the course of a molecular dynamics simulation, allowing researchers to monitor specific interactions or conformational changes within the system.

6.2 Command Syntax
~~~~~~~~
To use the Distance module, the command structure is as follows:

.. code-block:: bash

   tkm distance --xyz <xyz_file> --traj <trajectory_file> [options]

**Options:**

- `--xyz`: Specifies the path to the TXYZ file (`.xyz`) used to identify the topology.
- `--traj`: Specifies the path to the Tinker trajectory file (`.arc`).
- `--ndx`: Specifies the indices of the two atoms for which the distance will be calculated. The input can be a single integer (e.g., `10`), a list separated by commas (e.g., `1,2`), or a range (e.g., `1-2`), but must ultimately represent exactly two atoms.
- `--skip`: (Optional) Skips frames in the trajectory file, useful for reducing computation time.
- `--bfra`: (Optional) Specifies the beginning frame for the distance calculation. Default is `0`.
- `--efra`: (Optional) Specifies the ending frame for the distance calculation. Default is `-1` (i.e., the last frame).
- `--out`: (Optional) Specifies the path to the output CSV file where the distance values will be saved. The default is `./TKM_distance.csv`.

6.3 Example Usage
~~~~~~~~
Here’s a practical example of how to use the Distance module to calculate the distance between two atoms for a Tinker trajectory:

.. code-block:: bash

   tkm distance --xyz my_structure.xyz --traj my_trajectory.arc --ndx 1,2 --skip 2 --bfra 10 --efra 50 --out output_distance.csv

**Explanation:**

- `--xyz my_structure.xyz`: Specifies the path to the `.xyz` file used for topology.
- `--traj my_trajectory.arc`: Specifies the path to the Tinker trajectory file.
- `--ndx 1,2`: Specifies the indices of the two atoms for which the distance will be calculated.
- `--skip 2`: Skips every second frame in the trajectory to speed up the calculation.
- `--bfra 10`: Starts the distance calculation from the 10th frame.
- `--efra 50`: Ends the distance calculation at the 50th frame.
- `--out output_distance.csv`: Outputs the distance values to `output_distance.csv`.

After running this command, you should find the `output_distance.csv` file containing the calculated distance values between the two specified atoms for each selected frame.

7. Angle Module
-------------------

7.1 Overview
~~~~~~~~
The `Angle` module in TinkerModellor is designed to calculate the atomic angles between three atoms across different frames in a Tinker trajectory file (`.arc`). This module is particularly useful for analyzing the angular relationships between atoms in molecular dynamics simulations, providing insight into the structural changes over time.

7.2 Command Syntax
~~~~~~~~
To use the Angle module, the command structure is as follows:

.. code-block:: bash

   tkm angle --xyz <xyz_file> --traj <trajectory_file> --ndx <atom_indices> [options]

**Options:**

- `--xyz`: Specifies the path to the TXYZ file (`.xyz`) used to identify the topology.
- `--traj`: Specifies the path to the Tinker trajectory file (`.arc`).
- `--ndx`: Specifies the indices of the three atoms whose angle is to be calculated. The indices can be provided as a single integer (e.g., `10`), a list separated by commas (e.g., `1,2,3`), a range (e.g., `1-3`), or a combination of these.
- `--skip`: (Optional) Skips frames in the trajectory file to reduce computation time.
- `--bfra`: (Optional) Specifies the beginning frame for the angle calculation. Default is `0`.
- `--efra`: (Optional) Specifies the ending frame for the angle calculation. Default is `-1` (i.e., the last frame).
- `--out`: (Optional) Specifies the path to the output CSV file where the calculated angles will be saved. The default is `./TKM_angle.csv`.

7.3 Example Usage
~~~~~~~~
Here’s a practical example of how to use the Angle module to calculate the atomic angles for a Tinker trajectory:

.. code-block:: bash

   tkm angle --xyz my_structure.xyz --traj my_trajectory.arc --ndx 1,2,3 --skip 2 --bfra 10 --efra 50 --out output_angle.csv

**Explanation:**

- `--xyz my_structure.xyz`: Specifies the path to the `.xyz` file used for topology.
- `--traj my_trajectory.arc`: Specifies the path to the Tinker trajectory file.
- `--ndx 1,2,3`: Specifies the indices of the three atoms for which the angle is to be calculated.
- `--skip 2`: Skips every second frame in the trajectory to speed up the calculation.
- `--bfra 10`: Starts the angle calculation from the 10th frame.
- `--efra 50`: Ends the angle calculation at the 50th frame.
- `--out output_angle.csv`: Outputs the calculated angles to `output_angle.csv`.

After running this command, the tool will calculate the angles between the three specified atoms across the selected frames in the trajectory. The results are stored in `output_angle.csv`, which can be opened and analyzed further.

8. Connect Module
-------------------

8.1 Overview
~~~~~~~~
The `Connect` module in TinkerModellor is designed to connect two specified atoms within a Tinker system, effectively creating a bond between them. This module is particularly useful when you need to manually adjust the connectivity of atoms in a molecular structure, such as when preparing a molecular system for further simulations or analysis.

8.2 Command Syntax
~~~~~~~~
To use the Connect module, the command structure is as follows:

.. code-block:: bash

   tkm connect --tk <input_txyz_file> --xyz <output_txyz_file> --ndx <atom_indices>

**Options:**

- `--tk`: Specifies the path to the input TXYZ file.
- `--xyz`: Specifies the path to the output TXYZ file where the updated structure will be saved.
- `--ndx`: Specifies the indices of the two atoms that should be connected. This should be a list of two integers separated by a comma (e.g., `1,2`).

8.3 Example Usage
~~~~~~~~
Here’s a practical example of how to use the Connect module to create a bond between two atoms in a Tinker system:

.. code-block:: bash

   tkm connect --tk my_structure.xyz --xyz connected_structure.xyz --ndx 5,10

**Explanation:**

- `--tk my_structure.xyz`: Specifies the path to the input `.xyz` file that contains the molecular structure.
- `--xyz connected_structure.xyz`: Specifies the path to the output `.xyz` file where the modified structure will be saved.
- `--ndx 5,10`: Specifies the indices of the two atoms (`5` and `10`) that should be connected.

After running this command, the tool will connect the specified atoms (in this case, atoms `5` and `10`) within the molecular structure. The updated structure will be saved in the `connected_structure.xyz` file.

9. tk2pdb Module
-------------------

9.1 Overview
~~~~~~~~
The `tk2pdb` module in TinkerModellor is designed to convert a Tinker XYZ file into a PDB file, which is widely used in molecular modeling and simulation. This conversion is particularly useful for users who need to export molecular structures from Tinker into a format that is compatible with other software tools.

9.2 Command Syntax
~~~~~~~~
To use the `tk2pdb` module, the command structure is as follows:

.. code-block:: bash

   tkm tk2pdb --tk <input_txyz_file> --pdb <output_pdb_file> [options]

**Options:**

- `--tk`: Specifies the path to the input TXYZ file.
- `--pdb`: Specifies the path to the output PDB file.
- `--depth`: (Optional) Specifies the depth of the search algorithm. The default is `10000`.
- `--style`: (Optional) Specifies the TXYZ file style. Use `1` if the file was generated by TinkerModellor, or `2` if generated by the Tinker `pdbxyz` module. The default is `1`.

9.3 Example Usage
~~~~~~~~
Here’s a practical example of how to use the `tk2pdb` module to convert a Tinker XYZ file into a PDB file:

.. code-block:: bash

   tkm tk2pdb --tk my_structure.xyz --pdb output_structure.pdb --depth 5000 --style 1

**Explanation:**

- `--tk my_structure.xyz`: Specifies the path to the input TXYZ file.
- `--pdb output_structure.pdb`: Specifies the path to the output PDB file.
- `--depth 5000`: Sets the depth of the search algorithm to `5000`.
- `--style 1`: Indicates that the TXYZ file was generated by TinkerModellor.

After running this command, the tool will generate a PDB file (`output_structure.pdb`) based on the structure defined in the input TXYZ file.

9.4 Example: Converting a Complex Tinker XYZ File to PDB Format
~~~~~~~~
In this section, located in the `example/tk2pdb/ex2`, we will demonstrate how to use the `tk2pdb` module to convert a Tinker XYZ file into a PDB file. This is particularly useful when you need to use the molecular structure data in software that requires PDB format, such as PyMOL or other molecular visualization tools.

**Files Involved:**

- `complex.xyz`: The input Tinker XYZ file that contains the molecular structure information.
- `tk2pdb.pdb`: The output PDB file that is generated from the Tinker XYZ file.
- `complex.pdb`: A reference PDB file for comparison, demonstrating the expected format and structure of the PDB output.

**Command:**

.. code-block:: bash

   tkm tk2pdb --tk complex.xyz --pdb tk2pdb.pdb

After running this command, the tool will generate a PDB file (`tk2pdb.pdb`) from the input XYZ file (`complex.xyz`). This PDB file can be used for further analysis or visualization in tools that require PDB format, such as PyMOL.

10. Electric Field Module
-------------------

10.1 Overview
~~~~~~~~
The electric field (`ef`) module in TinkerModellor is designed to calculate the electric field for a given Tinker XYZ file. This module provides flexibility by allowing users to calculate the electric field at a specific point, on a grid, or projected along a bond. The module is especially useful for analyzing electrostatic properties in molecular simulations.

10.2 Command Syntax
~~~~~~~~
To use the `ef` module, the command structure is as follows:

.. code-block:: bash

   tkm ef --type <job_type> --tk <input_txyz_file> [options]

**Options:**

- `--type`: Specifies the job type, which can be one of the following:
  - `point`: Calculate the electric field at a specific point.
  - `grid`: Calculate the electric field on a grid.
  - `bond`: Calculate the electric field projected along a bond.
- `--tk`: Specifies the path to the input Tinker XYZ file.
- `--chg`: (Optional) Specifies the charge method. Options are `eem`, `qeq`, or `qtpie`. Default is `eem`.
- `--point`: (Optional) Specifies the point at which to calculate the electric field. Required for `point` or `grid` job types. Format: `x,y,z`.
- `--ndx`: (Optional) Specifies the atom index which is the center of the grid. Required for `grid` job type.
- `--rad`: (Optional) Specifies the radius of the grid. Default is `5.0` Angstroms.
- `--den`: (Optional) Specifies the density of the grid. Default is `3`, which corresponds to `20` grid points per Angstrom.
- `--dx`: (Optional) Specifies the prefix for the Pymol output file when using the grid type. Default is `TKM`.
- `--bond`: (Optional) Specifies the atom indices that define the bond for the `bond` job type. Format: `atom1,atom2`.
- `--mask`: (Optional) Masks the electric field of the bond molecule. Default is `True`.

10.3 Example Usage
~~~~~~~~
Here’s a practical example of how to use the `ef` module to calculate the electric field at a specific point:

.. code-block:: bash

   tkm ef --type point --tk my_structure.xyz --chg eem --point 0.0,0.0,0.0

**Explanation:**

- `--type point`: Specifies that the job type is to calculate the electric field at a point.
- `--tk my_structure.xyz`: Specifies the path to the input Tinker XYZ file.
- `--chg eem`: Uses the `eem` charge method for the calculation.
- `--point 0.0,0.0,0.0`: Calculates the electric field at the origin (`0.0, 0.0, 0.0`).

**For a grid-based calculation:**

.. code-block:: bash

   tkm ef --type grid --tk my_structure.xyz --chg qeq --ndx 1 --rad 5.0 --den 3 --dx my_grid

**Explanation:**

- `--type grid`: Specifies that the job type is to calculate the electric field on a grid.
- `--tk my_structure.xyz`: Specifies the path to the input Tinker XYZ file.
- `--chg qeq`: Uses the `qeq` charge method for the calculation.
- `--ndx 1`: Specifies that the atom with index `1` is the center of the grid.
- `--rad 5.0`: Sets the grid radius to `5.0` Angstroms.
- `--den 3`: Sets the grid density to `20` points per Angstrom.
- `--dx my_grid`: Outputs the grid data with the prefix `my_grid`.

After running these commands, the tool will generate the electric field data based on the specified parameters.

10.4 Example: Calculating Electric Field on a Grid
~~~~~~~~
In this section, we will demonstrate how to use the `ef` module to calculate the electric field on a grid using the provided example files. The example is located in the directory `example/ef/grid/ex1/` and illustrates a practical application of grid-based electric field calculations.

**Files Involved:**

- `ligand.xyz`: The Tinker XYZ file that defines the molecular structure for which the electric field will be calculated.
- `ANTECHAMBER_AC.AC` & `ANTECHAMBER_AC.AC0`: Files describing atomic types and force field parameters, which are necessary inputs for the electric field calculation.
- `ATOMTYPE.INF`: A file that details the atom types in the molecular system, crucial for defining the force field parameters.
- `TKM_Ex.dx`, `TKM_Ey.dx`, `TKM_Ez.dx`: These files contain the electric field components along the x, y, and z directions, respectively, calculated on the grid.
- `TKM_Magnitude.dx`: This file contains the magnitude of the electric field on the grid, providing a scalar value for the field strength at each grid point.
- `openbabel.sdf`, `new.mol2`, `openbabel.mol2`: These are additional molecular structure files, used for conversion or intermediate steps in preparing the input XYZ file.

**Command:**

.. code-block:: bash

   tkm ef --type grid --tk ligand.xyz --chg eem --rad 5.0 --den 3 --dx TKM

After running this command, the tool will generate the following files:

- `TKM_Ex.dx`: Contains the x-component of the electric field.
- `TKM_Ey.dx`: Contains the y-component of the electric field.
- `TKM_Ez.dx`: Contains the z-component of the electric field.
- `TKM_Magnitude.dx`: Contains the magnitude of the electric field on the grid.

These files can be visualized in molecular visualization software like PyMOL or VMD, allowing you to analyze the distribution and magnitude of the electric field around the molecule.

11. eftraj Module
-------------------

11.1 Overview
~~~~~~~~
The `eftraj` module in TinkerModellor is designed to calculate the electric field along a molecular trajectory described by a Tinker ARC file. This module allows users to compute the electric field at a specific point, on a grid, or projected along a bond as the molecule evolves over time. It is particularly useful for analyzing dynamic electrostatic properties in molecular simulations.

11.2 Command Syntax
~~~~~~~~
To use the `eftraj` module, the command structure is as follows:

.. code-block:: bash

   tkm eftraj --type <job_type> --tk <input_txyz_file> --arc <input_arc_file> [options]

**Options:**

- `--type`: Specifies the job type, which can be one of the following:
  - `point`: Calculate the electric field at a specific point.
  - `grid`: Calculate the electric field on a grid (not implemented yet).
  - `bond`: Calculate the electric field projected along a bond.
- `--tk`: Specifies the path to the input Tinker XYZ file.
- `--arc`: Specifies the path to the input Tinker ARC trajectory file.
- `--chg`: (Optional) Specifies the charge method. Options are `eem`, `qeq`, or `qtpie`. Default is `eem`.
- `--point`: (Optional) Specifies the point at which to calculate the electric field. Required for point job type. Format: `x,y,z`.
- `--ndx`: (Optional) Specifies the atom index which is the center of the grid. Required for `grid` job type.
- `--rad`: (Optional) Specifies the radius of the grid. Default is `5.0` Angstroms.
- `--den`: (Optional) Specifies the density of the grid. Default is `3`, which corresponds to `20` grid points per Angstrom.
- `--out`: (Optional) Specifies the output file name for the results. Required for point and bond job types. Default is `TKM`.
- `--dx`: (Optional) Specifies the prefix for the Pymol output file when using the grid type. Default is `TKM`.
- `--bond`: (Optional) Specifies the atom indices that define the bond for the bond job type. Format: `atom1,atom2`.
- `--mask`: (Optional) Masks the electric field of the bond molecule. Default is `True`.
- `--otf`: (Optional) Indicates whether to compute charges on the fly. Default is `False`.

11.3 Example Usage
~~~~~~~~
Here’s a practical example of how to use the `eftraj` module to calculate the electric field at a specific point along a trajectory:

.. code-block:: bash

   tkm eftraj --type point --tk my_structure.xyz --arc my_trajectory.arc --chg eem --point 0.0,0.0,0.0 --out electric_field.csv

**Explanation:**

- `--type point`: Specifies that the job type is to calculate the electric field at a point.
- `--tk my_structure.xyz`: Specifies the path to the input Tinker XYZ file.
- `--arc my_trajectory.arc`: Specifies the path to the input Tinker ARC trajectory file.
- `--chg eem`: Uses the `eem` charge method for the calculation.
- `--point 0.0,0.0,0.0`: Calculates the electric field at the origin (`0.0, 0.0, 0.0`).
- `--out electric_field.csv`: Saves the results to `electric_field.csv`.

**For a bond-based calculation along a trajectory:**

.. code-block:: bash

   tkm eftraj --type bond --tk my_structure.xyz --arc my_trajectory.arc --chg qeq --bond 1,2 --out bond_field.csv --otf

**Explanation:**

- `--type bond`: Specifies that the job type is to calculate the electric field projected along a bond.
- `--tk my_structure.xyz`: Specifies the path to the input Tinker XYZ file.
- `--arc my_trajectory.arc`: Specifies the path to the input Tinker ARC trajectory file.
- `--chg qeq`: Uses the `qeq` charge method for the calculation.
- `--bond 1,2`: Specifies that the bond is defined by the atoms with indices `1` and `2`.
- `--out bond_field.csv`: Saves the results to `bond_field.csv`.
- `--otf`: Enables on-the-fly charge computation.

After running these commands, the tool will calculate the electric field based on the specified parameters and save the results in the designated output files.

10.4 Example: Calculating Electric Field on a Grid
~~~~~~~~
In this section, we will demonstrate how to use the `ef` module to calculate the electric field on a grid using the provided example files. The example is located in the directory `example/ef/grid/ex1/` and illustrates a practical application of grid-based electric field calculations.

**Files Involved:**

- `ligand.xyz`: The Tinker XYZ file that defines the molecular structure for which the electric field will be calculated.
- `ANTECHAMBER_AC.AC` & `ANTECHAMBER_AC.AC0`: Files describing atomic types and force field parameters, which are necessary inputs for the electric field calculation.
- `ATOMTYPE.INF`: A file that details the atom types in the molecular system, crucial for defining the force field parameters.
- `TKM_Ex.dx`, `TKM_Ey.dx`, `TKM_Ez.dx`: These files contain the electric field components along the x, y, and z directions, respectively, calculated on the grid.
- `TKM_Magnitude.dx`: This file contains the magnitude of the electric field on the grid, providing a scalar value for the field strength at each grid point.
- `openbabel.sdf`, `new.mol2`, `openbabel.mol2`: These are additional molecular structure files, used for conversion or intermediate steps in preparing the input XYZ file.

**Command:**

.. code-block:: bash

   tkm ef --type grid --tk ligand.xyz --chg eem --rad 5.0 --den 3 --dx TKM

After running this command, the tool will generate the following files:

- `TKM_Ex.dx`: Contains the x-component of the electric field.
- `TKM_Ey.dx`: Contains the y-component of the electric field.
- `TKM_Ez.dx`: Contains the z-component of the electric field.
- `TKM_Magnitude.dx`: Contains the magnitude of the electric field on the grid.

These files can be visualized in molecular visualization software like PyMOL or VMD, allowing you to analyze the distribution and magnitude of the electric field around the molecule.


