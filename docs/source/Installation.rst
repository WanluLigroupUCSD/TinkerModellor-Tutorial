Installing TinkerModeller
=========================


Installation for User
----------
Firstly, you can download it through git or zip file:

.. code-block:: bash

   git clone git@github.com:WanluLigroupUCSD/TinkerModellor.git

Then, we recommand to use conda to configure the environment:

.. code-block:: bash

   cd TinkerModeller
   conda env create -n tkm -f env.yml
   conda activate tkm
   
A simple way to build TinkerModeller is to use the shell script install.sh
 
 .. code-block:: bash
   ./install.sh
 
A successful installation will look like the following:

.. code-block:: bash

   # Installing collected packages: TinkerModeller
   # Successfully installed TinkerModeller-1.1

Installation for Developer
----------

After configuring the conda environment

.. code-block:: bash

   cd TinkerModeller
   export TKMROOT=$(pwd)
   
.. note::

   Notably, the step "export TKMROOT=$(pwd)" is necessary because the subpackage ''TinkerModeller.tkmtoolkit'' is written in C++. This command is required for compiling the C++ code.

To build TinkerModeller, execute the following code in the terminal:

.. code-block:: bash

   python -m build > build.log

This step will generate a folder called ''dist'', which contains .whl file for environment setup:

.. code-block:: bash

   pip dist/your_whl_name.whl

To make it straight, you could also use the shell script after getting into ''TinkerModeller'' directory:

.. code-block:: bash

   ./reinstall


Testing After Installation
----------
We always recommend that users run a test after every installation to ensure there are no bugs in the installation.

To automatically run the TinkerModeller tests, execute the following code in the terminal:

.. code-block:: sh

   bash test.sh
   pytest -n auto -s

