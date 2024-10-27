Python API
===========================

1. Transform Module
-------------------

Overview
~~~~~~~~

The `transform` function in the `TinkerModellor` class allows you to convert a molecular system described in Gromacs file formats (`.gro`, `.top`) into a Tinker system. Optionally, you can save the transformed system to a `.xyz` file.

Function Definition
~~~~~~~~~~~~~~~~~~~

.. code-block:: python

    def transform(self, gmx_gro: str, gmx_top: str, tinker_xyz: str = None, forcefield: int = 1) -> TinkerSystem:
        gmx_gro = os.path.abspath(gmx_gro)
        gmx_top = os.path.abspath(gmx_top)
        if tinker_xyz is not None:
            tinker_xyz = os.path.abspath(tinker_xyz)
        gmx = GMXSystem()
        gmx.read_gmx_file(gmx_gro, gmx_top)
        transformer = Transformer(forcefield=forcefield)
        tk = transformer(gmx)
        if tinker_xyz is not None:
            tk.write(tinker_xyz)
        return tk

Parameters
~~~~~~~~~~

- **gmx_gro (str)**: Path to the Gromacs `.gro` file containing the system’s coordinates.
- **gmx_top (str)**: Path to the Gromacs `.top` file that defines the topology.
- **tinker_xyz (str, optional)**: Path to save the resulting Tinker `.xyz` file. If not provided, the system is not saved.
- **forcefield (int, optional)**: Specifies the force field for the Tinker system. Defaults to `1`.
  - `1`: AMOEBABIO18
  - `2`: AMOEBABIO09
  - `3`: AMOEBAPRO13

Workflow
~~~~~~~~

- **Path Handling**: The function first converts the provided paths to absolute paths for consistency.
- **Read Gromacs Files**: The Gromacs system is read using `GMXSystem()`, which processes the `.gro` and `.top` files.
- **Transform to Tinker**: The `Transformer` class is used to convert the Gromacs system into a Tinker system according to the specified force field.
- **Optional Saving**: If the `tinker_xyz` path is provided, the transformed system is saved as a `.xyz` file.
- **Return Value**: The function returns a `TinkerSystem` object, which can be used for further processing within the TinkerModellor framework.

Usage Example
~~~~~~~~~~~~~

.. code-block:: python

    from tinkermodellor import TinkerModellor

    # Initialize the TinkerModellor
    tkm = TinkerModellor()

    # Transform Gromacs system to Tinker system
    tinker_system = tkm.transform(
        gmx_gro='/path/to/your/gromacs.gro',
        gmx_top='/path/to/your/gromacs.top',
        tinker_xyz='/path/to/save/tinker_system.xyz',  # Optional
        forcefield=1  # Use AMOEBABIO18
    )

**Explanation:**

- **`gmx_gro`**: The file path for your Gromacs `.gro` coordinate file.
- **`gmx_top`**: The file path for your Gromacs `.top` topology file.
- **`tinker_xyz`**: (Optional) The file path where the transformed Tinker `.xyz` file will be saved.
- **`forcefield`**: Specifies the force field to use during transformation. In this example, `1` corresponds to `AMOEBABIO18`.

If you do not wish to save the transformed system to a file, you can omit the `tinker_xyz` argument. The function will then return the `TinkerSystem` object for further use in your Python code. The selected force field (`AMOEBABIO18`) is applied during the transformation.

2. Merge Module
---------------

Overview
~~~~~~~~

The `merge` function in the `TinkerModellor` class allows you to combine two separate Tinker systems into a single unified system. This is particularly useful when you have different molecular systems or components that you want to simulate together. The function also supports merging force field files associated with the Tinker systems.

Function Definition
~~~~~~~~~~~~~~~~~~~

.. code-block:: python

    def merge(self, tk1: str, tk2: str, tinker_xyz: str = None, ff1: str = None, ff2: str = None, ffout: str = None) -> TinkerSystem:
        tk1 = os.path.abspath(tk1)
        tk2 = os.path.abspath(tk2)
        if tinker_xyz is not None:
            tinker_xyz = os.path.abspath(tinker_xyz)
        if ff1 is not None:
            ff1 = os.path.abspath(ff1)
        if ff2 is not None:
            ff2 = os.path.abspath(ff2)
        if ffout is not None:
            ffout = os.path.abspath(ffout)
        
        tks1 = TinkerSystem()
        tks1.read_from_tinker(tk1)
        tks2 = TinkerSystem()
        tks2.read_from_tinker(tk2)

        merge = MergeTinkerSystem()
        tks_merged = merge(tks1, tks2, ff1, ff2, ffout)

        if tinker_xyz is not None:
            tks_merged.write(tinker_xyz)

        return tks_merged

Parameters
~~~~~~~~~~

- **tk1 (str)**: Path to the first Tinker system’s `.xyz` file.
- **tk2 (str)**: Path to the second Tinker system’s `.xyz` file.
- **tinker_xyz (str, optional)**: Path to save the merged Tinker system in a `.xyz` file. If not provided, the merged system is not saved.
- **ff1 (str, optional)**: Path to the first force field file associated with `tk1`. Optional but required if `ff2` is provided.
- **ff2 (str, optional)**: Path to the second force field file associated with `tk2`. Required if `ff1` is provided.
- **ffout (str, optional)**: Path to save the output merged force field file. If not provided, the merged force field is not saved.

Workflow
~~~~~~~~

- **Path Handling**: The function begins by converting all provided file paths (`tk1`, `tk2`, `tinker_xyz`, `ff1`, `ff2`, `ffout`) to absolute paths for consistency.
- **Read Tinker Systems**: The function reads the two Tinker systems from the specified `.xyz` files using the `TinkerSystem` class.
- **Merging**: The `MergeTinkerSystem` class is used to merge the two systems. If force field files are provided, they are also merged.
- **Optional Saving**: The merged system can be saved to a `.xyz` file if the `tinker_xyz` argument is provided. Similarly, the merged force field can be saved to the specified `ffout` path.
- **Return Value**: The function returns the merged `TinkerSystem` object for further manipulation or analysis.

Usage Example
~~~~~~~~~~~~~

.. code-block:: python

    from tinkermodellor import TinkerModellor

    # Initialize the TinkerModellor
    tkm = TinkerModellor()

    # Merge two Tinker systems with optional force field files
    merged_system = tkm.merge(
        tk1='/path/to/first_system.xyz',
        tk2='/path/to/second_system.xyz',
        tinker_xyz='/path/to/save/merged_system.xyz',  # Optional
        ff1='/path/to/first_forcefield.prm',           # Optional
        ff2='/path/to/second_forcefield.prm',          # Optional
        ffout='/path/to/save/merged_forcefield.prm'    # Optional
    )

**Explanation:**

- **`tk1`**: The file path for your first Tinker `.xyz` file.
- **`tk2`**: The file path for your second Tinker `.xyz` file.
- **`tinker_xyz`**: (Optional) The file path where the merged Tinker `.xyz` file will be saved.
- **`ff1`**: (Optional) The file path for the first force field file.
- **`ff2`**: (Optional) The file path for the second force field file.
- **`ffout`**: (Optional) The file path where the merged force field file will be saved.

If you wish to merge the force fields, you must provide all three (`ff1`, `ff2`, and `ffout`). Otherwise, the systems will be merged without modifying the force fields.

3. Delete Module
----------------

Overview
~~~~~~~~

The `delete` function in the `TinkerModellor` class allows you to remove specified atoms from a Tinker system. This is useful when you need to modify a molecular system by eliminating unwanted atoms or residues before performing further simulations or analyses.

Function Definition
~~~~~~~~~~~~~~~~~~~

.. code-block:: python

    def delete(self, tk: str, index: Union[int, List[int], List[str], str], tinker_xyz: str = None) -> TinkerSystem:
        tk = os.path.abspath(tk)
        tinker_xyz = os.path.abspath(tinker_xyz)
        dele = DeleteTinkerSystem()
        tks = TinkerSystem()

        tks.read_from_tinker(tk)
        tks_deleted = dele(tks, index)
        if tinker_xyz is not None:
            tks_deleted.write(tinker_xyz)
        return tks_deleted

Parameters
~~~~~~~~~~

- **tk (str)**: Path to the Tinker system's `.xyz` file from which atoms will be deleted.
- **index (Union[int, List[int], List[str], str])**: Specifies the index or indices of the atoms to be deleted. It can be:
  - A single integer (e.g., `5`).
  - A list of integers (e.g., `[2, 8, 15]`).
  - A string representing a range or combination of indices (e.g., `"1-3, 7, 9-10"`).
- **tinker_xyz (str, optional)**: Path to save the resulting Tinker `.xyz` file after the deletion operation. If not provided, the modified system will not be saved to a file.

Workflow
~~~~~~~~

- **Path Handling**: The function first converts the provided paths to absolute paths for consistency.
- **Read Tinker System**: The Tinker system is read using the `TinkerSystem` class, which processes the `.xyz` file specified by `tk`.
- **Delete Atoms**: The `DeleteTinkerSystem` class is used to remove the specified atoms from the system. The indices of atoms to delete are passed as the `index` argument.
- **Optional Saving**: If the `tinker_xyz` path is provided, the modified system is saved as a new `.xyz` file.
- **Return Value**: The function returns the modified `TinkerSystem` object after deletion, which can be used for further processing within the TinkerModellor framework.

Usage Example
~~~~~~~~~~~~~

.. code-block:: python

    from tinkermodellor import TinkerModellor

    # Initialize the TinkerModellor
    tkm = TinkerModellor()

    # Delete specific atoms from a Tinker system
    deleted_system = tkm.delete(
        tk='/path/to/your/system.xyz',
        index=[2, 5, 8],  # Specify the atoms to delete
        tinker_xyz='/path/to/save/modified_system.xyz'  # Optional: Save the output system
    )

**Explanation:**

- **`tk`**: The file path for your Tinker `.xyz` file.
- **`index`**: A list specifying the indices of atoms to delete. In this example, atoms with indices `2`, `5`, and `8` will be removed.
- **`tinker_xyz`**: (Optional) The file path where the modified Tinker `.xyz` file will be saved. If omitted, the changes will only be reflected in the returned `TinkerSystem` object and not saved to a file.

4. Replace Module
-----------------

Overview
~~~~~~~~

The `replace` function in the `TinkerModellor` class is designed to merge two Tinker systems while eliminating coincident water and ion molecules in the first system (`tk1`). This function is particularly useful when you want to update or replace parts of a molecular system with a new one, while ensuring no duplication of certain molecules. The function also supports merging associated force field files if necessary.

Function Definition
~~~~~~~~~~~~~~~~~~~

.. code-block:: python

    def replace(self, tk1: str, tk2: str, tinker_xyz: str = None, ff1: str = None, ff2: str = None, ffout: str = None) -> TinkerSystem:
        tk1 = os.path.abspath(tk1)
        tk2 = os.path.abspath(tk2)
        if tinker_xyz is not None:
            tinker_xyz = os.path.abspath(tinker_xyz)
        if ff1 is not None:
            ff1 = os.path.abspath(ff1)
        if ff2 is not None:
            ff2 = os.path.abspath(ff2)
        if ffout is not None:
            ffout = os.path.abspath(ffout)
        
        tks1 = TinkerSystem()
        tks1.read_from_tinker(tk1)
        tks2 = TinkerSystem()
        tks2.read_from_tinker(tk2)
        replace = ReplaceTinkerSystem()
        tks_replaced = replace(tks1, tks2, ff1, ff2, ffout)
        if tinker_xyz is not None:
            tks_replaced.write(tinker_xyz)
        return tks_replaced

Parameters
~~~~~~~~~~

- **tk1 (str)**: Path to the first Tinker system's `.xyz` file.
- **tk2 (str)**: Path to the second Tinker system's `.xyz` file.
- **tinker_xyz (str, optional)**: Path to save the resulting merged Tinker system in a `.xyz` file. If not provided, the merged system is not saved.
- **ff1 (str, optional)**: Path to the first force field file associated with `tk1`. This is optional, but if provided, `ff2` must also be specified.
- **ff2 (str, optional)**: Path to the second force field file associated with `tk2`. Required if `ff1` is provided.
- **ffout (str, optional)**: Path to save the output merged force field file. If not provided, the merged force field is not saved.

Workflow
~~~~~~~~

- **Path Handling**: Convert the provided file paths into absolute paths to ensure they can be accessed correctly.
- **Read Tinker Systems**: Load the two Tinker systems `tk1` and `tk2`, each representing a molecular structure.
- **Replace and Merge Systems**: Utilize the `ReplaceTinkerSystem` class to merge `tk2` into `tk1`, removing overlapping water and ion molecules from `tk1`.
- **Save Merged System**: If the `tinker_xyz` path is provided, save the merged Tinker system.
- **Return Merged System**: Return the final merged Tinker system object for further operations.

Usage Example
~~~~~~~~~~~~~

.. code-block:: python

    from tinkermodellor import TinkerModellor

    # Initialize the TinkerModellor
    tkm = TinkerModellor()

    # Replace overlapping water/ion molecules in the first Tinker system with components from the second system
    replaced_system = tkm.replace(
        tk1='/path/to/first_system.xyz',
        tk2='/path/to/second_system.xyz',
        tinker_xyz='/path/to/save/replaced_system.xyz',  # Optional
        ff1='/path/to/first_forcefield.prm',             # Optional
        ff2='/path/to/second_forcefield.prm',            # Optional
        ffout='/path/to/save/replaced_forcefield.prm'    # Optional
    )

**Explanation:**

- **`tk1`**: The file path for your first Tinker `.xyz` file.
- **`tk2`**: The file path for your second Tinker `.xyz` file.
- **`tinker_xyz`**: (Optional) The file path where the merged Tinker `.xyz` file will be saved.
- **`ff1`**: (Optional) The file path for the first force field file.
- **`ff2`**: (Optional) The file path for the second force field file.
- **`ffout`**: (Optional) The file path where the merged force field file will be saved.

This function merges two Tinker systems located at `tk1` and `tk2`, replacing parts of `tk1` with `tk2` while removing coincident water and ion molecules. If force field files are provided (`ff1`, `ff2`, and `ffout`), they will be merged accordingly.

5. RMSD Module
--------------

Overview
~~~~~~~~

The `rmsd` function in the `TinkerModellor` class is used to calculate the Root Mean Square Deviation (RMSD) of atomic coordinates from a Tinker trajectory file relative to a reference structure. This is a common measure of the difference between atomic positions, useful in evaluating the similarity of molecular conformations over time or between different structures.

Function Definition
~~~~~~~~~~~~~~~~~~~

.. code-block:: python

    def rmsd(self, xyz: str, arc: str, ref: str = None, skip: int = None, ndx: List[int] = None,
             bfra: int = 0, efra: int = -1) -> List[float]:
        if isinstance(xyz, str):
            xyz = os.path.abspath(xyz)
        else:
            raise TypeError("The xyz file path must be a string.")
        
        if isinstance(arc, str):
            arc = os.path.abspath(arc)
        else:
            raise TypeError("The arc file path must be a string.")
        
        input = TKMTrajectory()
        input.read_from_tinker(xyz)
        input.read_from_traj(arc)
        traj = input.AtomCrds
        if skip is not None:
            print(f"Skipping every {skip} frames. Total frames: {len(traj)}.")
            traj = input.AtomCrds[::skip]
        if ref is not None:
            ref_struct = TKMTrajectory()
            ref_struct.read_from_tinker(ref)
            ref_traj = ref_struct.AtomCrds[0]
        else:
            ref_traj = traj[0]
        if ndx is not None:
            # Convert the index to 0-based
            # The index in the trajectory file is 1-based
            ndx = [elemnt - 1 for elemnt in ndx]
            traj = traj[:, ndx]
            ref_traj = ref_traj[ndx]
            ref_copy = np.copy(ref_traj)
            traj_copy = np.copy(traj[bfra:efra])
        else:
            ref_copy = np.copy(ref_traj)
            traj_copy = np.copy(traj[bfra:efra])
        
        print(traj_copy)
        print(ref_copy)
        output = ttk.rmsd(ref_copy, traj_copy)
        output = [round(float(i), 6) for i in output]
        return output

Parameters
~~~~~~~~~~

- **xyz (str)**: Path to the Tinker `.xyz` file containing the initial atomic coordinates.
- **arc (str)**: Path to the Tinker `.arc` trajectory file containing the time-series data of atomic coordinates.
- **ref (str, optional)**: Path to the reference `.xyz` file. If not provided, the first frame of the trajectory is used as the reference.
- **skip (int, optional)**: Specifies the number of frames to skip during RMSD calculation, useful for reducing computational load. Default is `None`.
- **ndx (List[int], optional)**: Specifies the indices of atoms to include in the RMSD calculation. If not provided, all atoms are used.
- **bfra (int, optional)**: The beginning frame to start RMSD calculations. Default is `0`.
- **efra (int, optional)**: The ending frame to stop RMSD calculations. Default is `-1` (the last frame).

Workflow
~~~~~~~~

- **Path Validation**: The function first checks if the paths provided for the `.xyz` and `.arc` files are strings and converts them into absolute paths.
- **Trajectory Reading**: The Tinker trajectory is read using the `TKMTrajectory` class, which loads the atomic coordinates for analysis.
- **Frame Selection**: If `skip` is specified, the trajectory data is subsampled by skipping the specified number of frames.
- **Reference Structure**: If a reference structure is provided, it is loaded; otherwise, the first frame of the trajectory is used as the reference.
- **Atom Indexing**: If specific atom indices are provided, the function adjusts the indices to be 0-based (as Tinker uses 1-based indexing) and extracts the corresponding atomic coordinates.
- **RMSD Calculation**: The RMSD between the reference structure and the selected frames is calculated using the `ttk.rmsd` function.
- **Output**: The calculated RMSD values are rounded and returned as a list.

Usage Example
~~~~~~~~~~~~~

.. code-block:: python

    from tinkermodellor import TinkerModellor

    # Initialize the TinkerModellor
    tkm = TinkerModellor()

    # Calculate RMSD for a Tinker trajectory
    rmsd_values = tkm.rmsd(
        xyz='/path/to/your/structure.xyz',
        arc='/path/to/your/trajectory.arc',
        ref='/path/to/your/reference.xyz',  # Optional
        skip=5,  # Optional, skip every 5 frames
        ndx=[1, 2, 3],  # Optional, calculate RMSD for atoms 1, 2, and 3 only
        bfra=0,  # Optional, starting frame
        efra=100  # Optional, ending frame
    )

**Explanation:**

- **`xyz`**: The file path for the initial Tinker structure.
- **`arc`**: The file path for the Tinker trajectory.
- **`ref`**: (Optional) A reference structure; if not provided, the first frame in the trajectory is used.
- **`skip`**: Specifies that every 5th frame should be used for the RMSD calculation.
- **`ndx`**: Only atoms with indices `1`, `2`, and `3` are considered in the RMSD calculation.
- **`bfra`** and **`efra`**: Define the range of frames to analyze.

This example demonstrates how to calculate RMSD for selected atoms over a specified range of frames, skipping unnecessary frames to optimize performance. The result is a list of RMSD values that can be used to assess the structural stability or evolution of the molecular system over time.

6. Distance Module
------------------

Overview
~~~~~~~~

The `distance` function in the `TinkerModellor` class calculates the distance between two specified atoms across all frames in a Tinker trajectory file. This is particularly useful for analyzing how the distance between two atoms (or groups of atoms) changes over time during a molecular dynamics simulation.

Function Definition
~~~~~~~~~~~~~~~~~~~

.. code-block:: python

    def distance(self, xyz: str, arc: str, skip: int = None, ndx: List[int] = None,
                 bfra: int = 0, efra: int = -1) -> Tuple[List[float], float]:
        if isinstance(xyz, str):
            xyz = os.path.abspath(xyz)
        else:
            raise TypeError("The xyz file path must be a string.")
        if isinstance(arc, str):
            arc = os.path.abspath(arc)
        else:
            raise TypeError("The arc file path must be a string.")
        input = TKMTrajectory()
        input.read_from_tinker(xyz)
        input.read_from_traj(arc)
        traj = input.AtomCrds
        if skip is not None:
            print(f"Skipping every {skip} frames. Total frames: {len(traj)}.")
            traj = input.AtomCrds[::skip]
        if len(ndx) != 2:
            raise ValueError("The index must contain two elements.")
        else:
            # Convert the index to 0-based
            # The index in the trajectory file is 1-based
            ndx = [elemnt - 1 for elemnt in ndx]
            atom1_traj = traj[:, ndx[0]]
            atom2_traj = traj[:, ndx[1]]
            atom1_copy = np.copy(atom1_traj[bfra:efra])
            atom2_copy = np.copy(atom2_traj[bfra:efra])
        output = ttk.distance(atom1_copy, atom2_copy)
        output = [round(float(i), 6) for i in output]
        avg_output = sum(output) / len(output)
        print(f"The average distance is {avg_output}.")
        return output, avg_output

Parameters
~~~~~~~~~~

- **xyz (str)**: Path to the Tinker `.xyz` file that contains the initial atomic coordinates.
- **arc (str)**: Path to the Tinker `.arc` trajectory file, which holds the time-series data of atomic coordinates.
- **skip (int, optional)**: Number of frames to skip during distance calculation. Useful for reducing computational load by analyzing fewer frames. Default is `None`, meaning no frames are skipped.
- **ndx (List[int])**: A list of two indices corresponding to the atoms whose distance is to be calculated. Both indices are required.
- **bfra (int, optional)**: The index of the beginning frame to consider in the distance calculation. Default is `0` (the first frame).
- **efra (int, optional)**: The index of the ending frame to consider in the distance calculation. Default is `-1` (the last frame).

Workflow
~~~~~~~~

- **Path Validation**: The function first ensures that the paths provided for the `.xyz` and `.arc` files are valid strings and converts them into absolute paths.
- **Trajectory Reading**: The Tinker trajectory is loaded using the `TKMTrajectory` class, which reads the atomic coordinates from the `.xyz` and `.arc` files.
- **Frame Selection**: If the `skip` parameter is provided, the trajectory data is subsampled by skipping the specified number of frames.
- **Atom Selection**: The function checks that exactly two atom indices are provided in `ndx`. These indices are converted to 0-based indexing and used to extract the trajectories of the two atoms.
- **Distance Calculation**: The distance between the two atoms is computed for each frame using the `ttk.distance` function.
- **Output**: The function returns a tuple containing:
  - A list of distances between the two atoms for each frame.
  - The average distance across the specified frames.

Usage Example
~~~~~~~~~~~~~

.. code-block:: python

    from tinkermodellor import TinkerModellor

    # Initialize the TinkerModellor
    tkm = TinkerModellor()

    # Calculate the distance between two atoms in a Tinker trajectory
    distances, avg_distance = tkm.distance(
        xyz='/path/to/your/structure.xyz',
        arc='/path/to/your/trajectory.arc',
        skip=5,  # Optional, skip every 5 frames
        ndx=[1, 2],  # Indices of the two atoms whose distance is calculated
        bfra=0,  # Optional, start from the first frame
        efra=100  # Optional, end at the 100th frame
    )

**Explanation:**

- **`xyz`**: The file path for the initial Tinker structure.
- **`arc`**: The file path for the Tinker trajectory.
- **`skip`**: Specifies that every 5th frame should be used for distance calculation.
- **`ndx`**: Specifies the two atoms (indices `1` and `2`) for which the distance will be calculated.
- **`bfra`** and **`efra`**: Define the range of frames to analyze, starting from the first frame and ending at the 100th frame.

This example demonstrates how to calculate the distance between two atoms over a specified range of frames, skipping unnecessary frames to optimize performance. The result includes both the list of calculated distances and their average, providing insights into the dynamic behavior of the molecular system.

7. Angle Module
---------------

Overview
~~~~~~~~

The `angle` function in the `TinkerModellor` class is used to calculate the angle formed by three specified atoms across all frames in a Tinker trajectory file. This analysis is essential for understanding the structural properties of a molecule as it evolves over time, particularly in tracking changes in bond angles during molecular dynamics simulations.

Function Definition
~~~~~~~~~~~~~~~~~~~

.. code-block:: python

    def angle(self, xyz: str, arc: str, skip: int = None, ndx: List[int] = None,
              bfra: int = 0, efra: int = -1) -> Tuple[List[float], float]:
        if isinstance(xyz, str):
            xyz = os.path.abspath(xyz)
        else:
            raise TypeError("The xyz file path must be a string.")
        if isinstance(arc, str):
            arc = os.path.abspath(arc)
        else:
            raise TypeError("The arc file path must be a string.")
        input = TKMTrajectory()
        input.read_from_tinker(xyz)
        input.read_from_traj(arc)
        traj = input.AtomCrds
        if skip is not None:
            print(f"Skipping every {skip} frames. Total frames: {len(traj)}.")
            traj = input.AtomCrds[::skip]
        if len(ndx) != 3:
            raise ValueError("The index must contain three elements.")
        else:
            # Convert the index to 0-based
            # The index in the trajectory file is 1-based
            ndx = [elemnt - 1 for elemnt in ndx]
            atom1_traj = traj[:, ndx[0], :]
            atom2_traj = traj[:, ndx[1], :]
            atom3_traj = traj[:, ndx[2], :]
            atom1_copy = np.copy(atom1_traj[bfra:efra])
            atom2_copy = np.copy(atom2_traj[bfra:efra])
            atom3_copy = np.copy(atom3_traj[bfra:efra])
        output = ttk.angle(atom1_copy, atom2_copy, atom3_copy)
        output = [round(float(i), 6) for i in output]
        avg_output = sum(output) / len(output)
        print(f"The average angle is {avg_output}.")
        return output, avg_output

Parameters
~~~~~~~~~~

- **xyz (str)**: Path to the Tinker `.xyz` file that contains the initial atomic coordinates.
- **arc (str)**: Path to the Tinker `.arc` trajectory file, which holds the time-series data of atomic coordinates.
- **skip (int, optional)**: Number of frames to skip during angle calculation. Useful for reducing computational load by analyzing fewer frames. Default is `None`, meaning no frames are skipped.
- **ndx (List[int])**: A list of three indices corresponding to the atoms that form the angle. All three indices are required.
- **bfra (int, optional)**: The index of the beginning frame to consider in the angle calculation. Default is `0` (the first frame).
- **efra (int, optional)**: The index of the ending frame to consider in the angle calculation. Default is `-1` (the last frame).

Workflow
~~~~~~~~

- **Path Validation**: The function checks if the paths provided for the `.xyz` and `.arc` files are valid strings and converts them into absolute paths.
- **Trajectory Reading**: The Tinker trajectory is loaded using the `TKMTrajectory` class, which reads the atomic coordinates from the `.xyz` and `.arc` files.
- **Frame Selection**: If the `skip` parameter is provided, the trajectory data is subsampled by skipping the specified number of frames.
- **Atom Selection**: The function checks that exactly three atom indices are provided in `ndx`. These indices are converted to 0-based indexing and used to extract the trajectories of the three atoms.
- **Angle Calculation**: The angle formed by the three atoms is calculated for each frame using the `ttk.angle` function.
- **Output**: The function returns a tuple containing:
  - A list of angles (in degrees) formed by the three atoms for each frame.
  - The average angle across the specified frames.

Usage Example
~~~~~~~~~~~~~

.. code-block:: python

    from tinkermodellor import TinkerModellor

    # Initialize the TinkerModellor
    tkm = TinkerModellor()

    # Calculate the angle between three atoms in a Tinker trajectory
    angles, avg_angle = tkm.angle(
        xyz='/path/to/your/structure.xyz',
        arc='/path/to/your/trajectory.arc',
        skip=5,  # Optional, skip every 5 frames
        ndx=[1, 2, 3],  # Indices of the three atoms forming the angle
        bfra=0,  # Optional, start from the first frame
        efra=100  # Optional, end at the 100th frame
    )

**Explanation:**

- **`xyz`**: The file path for the initial Tinker structure.
- **`arc`**: The file path for the Tinker trajectory.
- **`skip`**: Specifies that every 5th frame should be used for angle calculation.
- **`ndx`**: Specifies the three atoms (indices `1`, `2`, and `3`) that form the angle.
- **`bfra`** and **`efra`**: Define the range of frames to analyze, starting from the first frame and ending at the 100th frame.

This example demonstrates how to calculate the angle between three atoms over a specified range of frames, skipping unnecessary frames to optimize performance. The result includes both the list of calculated angles and their average, providing insights into the dynamic behavior of the molecular system.

8. Connect Module
-----------------

Overview
~~~~~~~~

The `connect` function in the `TinkerModellor` class is designed to establish connections between specified atoms within a Tinker system. This can be particularly useful for modeling covalent bonds or other interactions that require explicit connection between atoms in a molecular simulation.

Function Definition
~~~~~~~~~~~~~~~~~~~

.. code-block:: python

    def connect(self, tk: str, index: Union[int, List[int], List[str], str], tinker_xyz: str = None) -> TinkerSystem:
        tk = os.path.abspath(tk)
        tinker_xyz = os.path.abspath(tinker_xyz)
        conn = ConnectTinkerSystem()
        tks = TinkerSystem()
        tks.read_from_tinker(tk)
        tks_connected = conn(tks, index)
        if tinker_xyz is not None:
            tks_connected.write(tinker_xyz)
        return tks_connected

Parameters
~~~~~~~~~~

- **tk (str)**: Path to the Tinker `.xyz` file that contains the atomic coordinates of the system.
- **index (Union[int, List[int], List[str], str])**: The index or indices of the atoms to be connected. This can be provided as:
  - A single integer (for connecting a pair of atoms).
  - A list of integers (for connecting multiple pairs of atoms).
  - A string or list of strings (if using atom names or labels).
- **tinker_xyz (str, optional)**: Path to save the modified Tinker system after the connections are made. If not provided, the connected system is not saved.

Workflow
~~~~~~~~

- **Path Validation**: The function checks and converts the provided path for the Tinker `.xyz` file into an absolute path for consistent file handling.
- **System Loading**: The Tinker system is loaded using the `TinkerSystem` class, which reads the atomic coordinates and other relevant data from the `.xyz` file.
- **Atom Connection**: The `ConnectTinkerSystem` class is invoked to establish connections between the specified atoms based on their indices or labels.
- **Optional Saving**: If the `tinker_xyz` path is provided, the function writes the modified Tinker system with the new connections to the specified file.
- **Return**: The function returns the `TinkerSystem` object with the updated atomic connections.

Usage Example
~~~~~~~~~~~~~

.. code-block:: python

    from tinkermodellor import TinkerModellor

    # Initialize the TinkerModellor
    tkm = TinkerModellor()

    # Connect atoms in a Tinker system
    connected_system = tkm.connect(
        tk='/path/to/your/structure.xyz',
        index=[1, 2, 3],  # Indices of the atoms to be connected
        tinker_xyz='/path/to/save/connected_system.xyz'  # Optional
    )

**Explanation:**

- **`tk`**: The file path for your Tinker `.xyz` file.
- **`index`**: A list specifying the indices of atoms to connect. In this example, atoms with indices `1`, `2`, and `3` will be connected.
- **`tinker_xyz`**: (Optional) The file path where the modified Tinker `.xyz` file will be saved. If omitted, the connected system will only be reflected in the returned `TinkerSystem` object and not saved to a file.

This function establishes connections between specified atoms in a Tinker system and optionally saves the connected system to a file. The `connect` function is a crucial tool for defining molecular structures where explicit connections between atoms are necessary.

9. tk2pdb Module
----------------

Overview
~~~~~~~~

The `tk2pdb` function in the `TinkerModellor` class allows you to convert a molecular system described in a Tinker `.xyz` file into a PDB (Protein Data Bank) format. This is particularly useful for users who need to visualize their molecular systems in software that supports the PDB format or for further analysis using tools that require PDB files.

Function Definition
~~~~~~~~~~~~~~~~~~~

.. code-block:: python

    def tk2pdb(self, tk: str, pdb: str, depth: int = 10000, style: int = 1) -> str:
        tk = os.path.abspath(tk)
        pdb = os.path.abspath(pdb)
        tkpdb = Tinker2PDB(depth)
        tkpdb(tk, pdb, style)
        return tkpdb

Parameters
~~~~~~~~~~

- **tk (str)**: Path to the Tinker `.xyz` file containing the molecular system.
- **pdb (str)**: Path where the output PDB file will be saved.
- **depth (int, optional)**: The depth of the searching algorithm used in the conversion process. This parameter can be adjusted for more complex structures, but the default is `10000`.
- **style (int, optional)**: The style of the XYZ file. This parameter defines how the conversion interprets the Tinker file format. The default value is `1`.

Workflow
~~~~~~~~

- **Path Validation**: The function first converts the paths for the Tinker `.xyz` file and the output PDB file into absolute paths for consistency.
- **Conversion Initialization**: The `Tinker2PDB` class is initialized with the specified `depth` parameter, preparing the conversion process.
- **Conversion Execution**: The conversion from the Tinker format to PDB format is executed by passing the Tinker `.xyz` file, output PDB file path, and `style` parameter to the `Tinker2PDB` instance.
- **Return**: The function returns the path to the generated PDB file.

Usage Example
~~~~~~~~~~~~~

.. code-block:: python

    from tinkermodellor import TinkerModellor

    # Initialize the TinkerModellor
    tkm = TinkerModellor()

    # Convert Tinker system to PDB
    pdb_path = tkm.tk2pdb(
        tk='/path/to/your/tinker.xyz',
        pdb='/path/to/your/output.pdb',
        depth=10000,  # Optional, depth of the searching algorithm
        style=1  # Optional, style of the XYZ file
    )

**Explanation:**

- **`tk`**: The file path to your Tinker `.xyz` file that you want to convert.
- **`pdb`**: The file path where the PDB file will be saved after conversion.
- **`depth`**: Specifies the depth of the searching algorithm. The default is usually sufficient, but it can be adjusted for more complex structures.
- **`style`**: Indicates the style of the XYZ file to guide the conversion. The default is `1`, which works for most standard Tinker files.

This example demonstrates how to convert a Tinker molecular system into a PDB format file, which can be used for further analysis, visualization, or sharing with other researchers using tools that support the PDB format.

10. Electric Field Module
-------------------------

Overview
~~~~~~~~

The electric field (`ef`) module in `TinkerModellor` is designed to calculate the electric field for a given Tinker XYZ file. This module provides flexibility by allowing users to calculate the electric field at a specific point, on a grid, or projected along a bond. The module is especially useful for analyzing electrostatic properties in molecular simulations.

Electric Field Calculation at a Point
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The `electric_field_point` function calculates the electric field at a specified point within the molecular system, provided as an XYZ coordinate. It uses a charge method (`eem`, `qeq`, or `qtpie`) to compute the electric field.

Function Definition
~~~~~~~~~~~~~~~~~~~

.. code-block:: python

    def electric_field_point(self, point: Union[List, np.ndarray], charge_method: str = 'eem', 
                             tinker_xyz: str = None) -> List[float]:
        if tinker_xyz is None:
            raise ValueError("The Tinker system file must be provided.")
        else:
            tinker_xyz = os.path.abspath(tinker_xyz)
        # Convert list to np.ndarray if necessary
        if isinstance(point, list):
            point = np.array(point)
        # Check if point is a numpy array with the correct shape
        if not isinstance(point, np.ndarray):
            raise TypeError("Point must be a numpy array or a list.")
        
        if point.ndim != 1 or point.shape[0] != 3:
            raise ValueError("Each point must be a one-dimension 3-element array.")
        
        # Create the ElectricFieldCompute instance and compute
        compute = ElectricFieldCompute(charge_method=charge_method, tinker_xyz=tinker_xyz)
        electric_field = compute.compute_point_ef(point)
        print(f"The electric field at the each components is (MV/cm): ")
        print(f"x: {electric_field[0]}, y: {electric_field[1]}, z: {electric_field[2]}, |E|: {electric_field[3]}")
        return electric_field

Usage Example
~~~~~~~~~~~~~

.. code-block:: python

    from tinkermodellor import TinkerModellor

    # Initialize the TinkerModellor
    tkm = TinkerModellor()

    # Calculate the electric field at a specific point
    electric_field = tkm.electric_field_point(
        point=[0.0, 0.0, 0.0],
        charge_method='eem',
        tinker_xyz='/path/to/your/system.xyz'
    )

**Explanation:**

- **`point`**: The XYZ coordinates where the electric field is to be calculated.
- **`charge_method`**: Specifies the charge method to use (`eem`, `qeq`, or `qtpie`).
- **`tinker_xyz`**: The file path to your Tinker `.xyz` file containing the molecular system.

Electric Field Calculation Along a Bond
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The `electric_field_bond` method projects the electric field along a bond defined by two atoms in the molecular system. It computes the electric field in the direction from the first atom to the second.

Function Definition
~~~~~~~~~~~~~~~~~~~

.. code-block:: python

    def electric_field_bond(self, charge_method: str = 'eem', tinker_xyz: str = None, bond: List[int] = None, mask: bool = True) -> float:
        if tinker_xyz is None:
            raise ValueError("The Tinker system file must be provided.")
        else:
            tinker_xyz = os.path.abspath(tinker_xyz)
        if bond is None:
            raise ValueError("The bond must be provided.")
        # Check if bond length is 2
        if len(bond) != 2:
            raise ValueError("Bond must define exactly two atoms.")
        # Create the ElectricFieldCompute instance and compute
        compute = ElectricFieldCompute(charge_method=charge_method, tinker_xyz=tinker_xyz)
        bond_electric_field = compute.compute_bond_ef(bond, mask)

        print("The electric field projected along the bond is (MV/cm): ")
        print(bond_electric_field)
        return bond_electric_field

Usage Example
~~~~~~~~~~~~~

.. code-block:: python

    from tinkermodellor import TinkerModellor

    # Initialize the TinkerModellor
    tkm = TinkerModellor()

    # Calculate the electric field along a bond
    bond_electric_field = tkm.electric_field_bond(
        charge_method='eem',
        tinker_xyz='/path/to/your/system.xyz',
        bond=[1, 2],  # Indices of the two atoms defining the bond
        mask=True  # Optional, masks the field from bonded atoms
    )

**Explanation:**

- **`charge_method`**: Specifies the charge method to use (`eem`, `qeq`, or `qtpie`).
- **`tinker_xyz`**: The file path to your Tinker `.xyz` file containing the molecular system.
- **`bond`**: A list of two integers representing the indices of the atoms that define the bond.
- **`mask`**: (Optional) If `True`, masks the electric field generated by the bonded atoms themselves.

Electric Field Calculation on a Grid
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The `electric_field_grid` method calculates the electric field on a grid of points surrounding a molecule. The grid can be centered around a specified point or an atom in the molecular system.

Function Definition
~~~~~~~~~~~~~~~~~~~

.. code-block:: python

    def electric_field_grid(self, charge_method: str = 'eem', tinker_xyz: str = None, point: Union[np.ndarray, List[float]] = None,
                            center_atom: int = None, radius: float = 5.0, density_level: int = 3, if_output: bool = True, output_prefix: str = 'TKM') -> List[List[float]]:
        if tinker_xyz is None:
            raise ValueError("The Tinker system file must be provided.")
        else:
            tinker_xyz = os.path.abspath(tinker_xyz)
        try:
            radius = float(radius)
        except ValueError:
            raise ValueError("The radius must be a float.")
        if center_atom is None and point is None:
            raise ValueError("The center atom index or the center point coordinate must be provided.")
        if point is not None and center_atom is not None:
            raise ValueError("You can only provide either the center atom index or the center point coordinate, not both of them.")
        if point is not None:
            if isinstance(point, list):
                point = np.array(point)
            if point.ndim != 1 or point.shape[0] != 3:
                raise ValueError("The center point must be a 3-element array.")
        if center_atom is not None:
            if not isinstance(center_atom, int):
                try:
                    center_atom = int(center_atom)
                except:
                    raise TypeError("The center atom index must be an integer.")
        # Create the ElectricFieldCompute instance and compute
        compute = ElectricFieldCompute(charge_method=charge_method, tinker_xyz=tinker_xyz)
        # Get the center point
        if center_atom is not None:
            # The center atom index is 0-based
            point = compute.tinker_system_charged.AtomCrds[center_atom - 1]
        grid_electric_field = compute.compute_grid_ef(point=point, radius=radius, density_level=density_level, if_output=if_output, output_prefix=output_prefix)
        return grid_electric_field

Usage Example
~~~~~~~~~~~~~

.. code-block:: python

    from tinkermodellor import TinkerModellor

    # Initialize the TinkerModellor
    tkm = TinkerModellor()

    # Calculate the electric field on a grid
    grid_electric_field = tkm.electric_field_grid(
        charge_method='eem',
        tinker_xyz='/path/to/your/system.xyz',
        point=[0.0, 0.0, 0.0],  # Optional: Specify a point
        center_atom=1,            # Optional: Specify an atom index instead of a point
        radius=5.0,               # Radius in Angstroms
        density_level=3,          # Density level (points per Angstrom)
        if_output=True,           # Whether to output .dx files
        output_prefix='TKM'       # Prefix for output files
    )

**Explanation:**

- **`charge_method`**: Specifies the charge method to use (`eem`, `qeq`, or `qtpie`).
- **`tinker_xyz`**: The file path to your Tinker `.xyz` file containing the molecular system.
- **`point`**: (Optional) The XYZ coordinates where the grid will be centered.
- **`center_atom`**: (Optional) The index of the atom around which the grid will be centered. Cannot be used simultaneously with `point`.
- **`radius`**: The radius of the grid in Angstroms.
- **`density_level`**: Defines the number of grid points per Angstrom.
- **`if_output`**: If `True`, outputs `.dx` files for visualization.
- **`output_prefix`**: The prefix for the output `.dx` files.

This example demonstrates how to calculate the electric field on a grid centered around a specific point or atom, with the results saved as `.dx` files for visualization in molecular modeling tools like PyMOL.

11. eftraj Module
-----------------

Overview
~~~~~~~~

The `eftraj` module in the `TinkerModellor` class is designed to compute the electric field at specific points, along bonds, or across a grid over the course of a molecular dynamics trajectory described in Tinker `.xyz` and `.arc` files. This module is particularly useful for analyzing the time evolution of electrostatic properties in molecular simulations.

Electric Field Calculation at a Point Along a Trajectory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The `electric_field_point_traj` function calculates the electric field at a specified point along the trajectory. It computes how the electric field at the specified point changes as the molecular system evolves.

Function Definition
~~~~~~~~~~~~~~~~~~~

.. code-block:: python

    def electric_field_point_traj(self, point: Union[List, np.ndarray], charge_method: str = 'eem', 
                                  tinker_xyz: str = None, tinker_arc: str = None, output: str = None, otf: bool = False) -> List[List[float]]:
        if tinker_xyz is None:
            raise ValueError("The Tinker system file must be provided.")
        else:
            tinker_xyz = os.path.abspath(tinker_xyz)
        if tinker_arc is None:
            raise ValueError("The Tinker trajectory file must be provided.")
        else:
            tinker_arc = os.path.abspath(tinker_arc)
        # Convert list to np.ndarray if necessary
        if isinstance(point, list):
            point = np.array(point)
        # Check if point is a numpy array with the correct shape
        if not isinstance(point, np.ndarray):
            raise TypeError("Point must be a numpy array or a list.")
        if point.ndim != 1 or point.shape[0] != 3:
            raise ValueError("Each point must be a one-dimension 3-element array.")
        
        # Create the ElectricFieldCompute instance and compute
        compute = ElectricFieldComputeTraj(charge_method=charge_method, tinker_xyz=tinker_xyz, tinker_arc=tinker_arc)
        electric_field = compute.compute_point_ef_traj(point=point, output=output)
        return electric_field

Usage Example
~~~~~~~~~~~~~

.. code-block:: python

    from tinkermodellor import TinkerModellor

    # Initialize the TinkerModellor
    tkm = TinkerModellor()

    # Calculate the electric field at a specific point along a trajectory
    electric_field = tkm.eftraj.electric_field_point_traj(
        point=[0.0, 0.0, 0.0],
        charge_method='eem',
        tinker_xyz='/path/to/your/system.xyz',
        tinker_arc='/path/to/your/trajectory.arc',
        output='electric_field.csv',
        otf=False
    )

**Explanation:**

- **`point`**: The XYZ coordinates where the electric field is to be calculated along the trajectory.
- **`charge_method`**: Specifies the charge method to use (`eem`, `qeq`, or `qtpie`).
- **`tinker_xyz`**: The file path to your Tinker `.xyz` file containing the molecular system.
- **`tinker_arc`**: The file path to your Tinker `.arc` trajectory file.
- **`output`**: (Optional) The file path where the electric field results will be saved.
- **`otf`**: (Optional) If `True`, enables on-the-fly charge computation.

Electric Field Calculation Along a Bond in a Trajectory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The `electric_field_bond_traj` method projects the electric field along a bond defined by two atoms as the molecule evolves over the trajectory. It computes the electric field direction from the first atom to the second in each frame of the trajectory.

Function Definition
~~~~~~~~~~~~~~~~~~~

.. code-block:: python

    def electric_field_bond_traj(self, bond: List[int] = None, charge_method: str = 'eem', 
                                 tinker_xyz: str = None, tinker_arc: str = None, mask: bool = True, 
                                 output: str = None, otf: bool = False) -> List[float]:
        if tinker_xyz is None:
            raise ValueError("The Tinker system file must be provided.")
        else:
            tinker_xyz = os.path.abspath(tinker_xyz)
        if tinker_arc is None:
            raise ValueError("The Tinker trajectory file must be provided.")
        else:
            tinker_arc = os.path.abspath(tinker_arc)
        # Create the ElectricFieldCompute instance and compute
        compute = ElectricFieldComputeTraj(charge_method=charge_method, tinker_xyz=tinker_xyz, tinker_arc=tinker_arc)
        electric_field = compute.compute_bond_ef_traj(bond=bond, mask=mask, output=output, on_the_fly=otf)
        return electric_field

Usage Example
~~~~~~~~~~~~~

.. code-block:: python

    from tinkermodellor import TinkerModellor

    # Initialize the TinkerModellor
    tkm = TinkerModellor()

    # Calculate the electric field along a bond over a trajectory
    bond_electric_field = tkm.eftraj.electric_field_bond_traj(
        bond=[1, 2],
        charge_method='qeq',
        tinker_xyz='/path/to/your/system.xyz',
        tinker_arc='/path/to/your/trajectory.arc',
        mask=True,
        output='bond_field.csv',
        otf=True
    )

**Explanation:**

- **`bond`**: A list of two integers representing the indices of the atoms that define the bond.
- **`charge_method`**: Specifies the charge method to use (`eem`, `qeq`, or `qtpie`).
- **`tinker_xyz`**: The file path to your Tinker `.xyz` file containing the molecular system.
- **`tinker_arc`**: The file path to your Tinker `.arc` trajectory file.
- **`mask`**: If `True`, masks the electric field generated by the bonded atoms themselves.
- **`output`**: (Optional) The file path where the electric field results will be saved.
- **`otf`**: (Optional) If `True`, enables on-the-fly charge computation.

Workflow
~~~~~~~~

- **Data Preparation**: Validate the input paths for the Tinker XYZ and ARC files, converting them into absolute paths. Convert the `point` argument to a NumPy array if necessary, ensuring it's correctly formatted.
- **Instance Creation**: Create an instance of the `ElectricFieldComputeTraj` class, initializing it with the specified charge method and paths to the Tinker system and trajectory files.
- **Performing Calculations**:
  - **Point Calculation**: Use `compute_point_ef_traj` to calculate the electric field at the specified point across the trajectory.
  - **Bond Calculation**: Calculate the electric field along the specified bond using `compute_bond_ef_traj`, with an option to mask the field from the bonded atoms.
- **Output and Results**: Print the calculated electric field values to the console or save them to an output file if specified. The function returns the electric field values as a list, which can be further analyzed within Python.

Usage Example
~~~~~~~~~~~~~

.. code-block:: python

    from tinkermodellor import TinkerModellor

    # Initialize the TinkerModellor
    tkm = TinkerModellor()

    # Analyze the electric field along a bond over a trajectory
    bond_field = tkm.eftraj.electric_field_bond_traj(
        bond=[18, 19],
        charge_method='eem',
        tinker_xyz='ke15_1.xyz',
        tinker_arc='ke15_1.arc',
        mask=True,
        output='18_to_19.csv'
    )

**Explanation:**

- **`bond`**: Specifies the bond defined by atoms with indices `18` and `19`.
- **`charge_method`**: Uses the `eem` charge method for the calculation.
- **`tinker_xyz`**: The Tinker `.xyz` file representing the initial molecular structure.
- **`tinker_arc`**: The Tinker `.arc` file containing the trajectory data.
- **`mask`**: Masks the electric field generated by the bonded atoms themselves.
- **`output`**: Saves the calculated electric field data to `18_to_19.csv`.

After running this command, the tool will calculate the electric field between atoms `18` and `19` for each frame in the trajectory and save the results in the `18_to_19.csv` file. The sign indicates the direction of the electric field, while the magnitude represents the strength.

FAQ
===

.. include:: faq.rst

Contact
=======

.. include:: contact.rst
