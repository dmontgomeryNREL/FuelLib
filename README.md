# PyFLARE
The Python Fuel Library for Advanced Research on Evaporation (PyFLARE) utilizes the tables and functions of the Group Contribution Method (GCM) as proposed by [Constantinou and Gani (1994)](https://doi.org/10.1002/aic.690401011) and [Constantinou, Gani and O'Connel (1995)](https://doi.org/10.1016/0378-3812(94)02593-P), with additional physical properties discussed in [Govindaraju & Ihme (2016)](https://doi.org/10.1016/j.ijheatmasstransfer.2016.06.079).  The code is based on Pavan B. Govindaraju's [Matlab implementation](https://github.com/gpavanb-old/GroupContribution) of the GCM, and has been expanded to include additional thermodynamic properties and mixture properties.  The fuel library contains gas chromatography (GC x GC) data for a variety of fuels ranging from simple single component fuels to complex jet fuels.  The GC x GC data for POSF jet fuels comes from [Edwards (2020)](https://apps.dtic.mil/sti/pdfs/AD1093317.pdf).  

## Python Environment
The following conda environment is required to run this code:
~~~
conda create --name pyflare-env ipython matplotlib pandas scipy openpyxl
~~~

## Running the code
This repository includes an example `ex-mixtureProperties.py` that calculates a given mixture's density, viscosity and vapor pressure from GC x GC data of the mixture.  The results are plotted against data from NIST and [Edwards (2020)](https://apps.dtic.mil/sti/pdfs/AD1093317.pdf).

# Contributing
New contributions are always welcome.  If you have an idea for a new feature follow these steps:
1. Fork the main repository
2. Create a `newFeature` branch that contains your changes
3. Update the sphinx documentation in `newFeature`
4. Open a Pull Request (PR) from `newFeature` on your fork to branch `main` GCM-Python repository.

## Sphinx Documentation
This repository uses [Sphinx](https://www.sphinx-doc.org/en/master/usage/quickstart.html) to generate documentation.  This requires the following Conda environment:
~~~
conda create --name sphinx-env
conda activate sphinx-env
conda install python
conda install anaconda::sphinx
conda install conda-forge::sphinx_rtd_theme 
~~~

To view the documentation locally, build the html using the following: 
~~~
cd PyFLARE/docs/sphinx
sphinx-build -M html . _build/
~~~
You should now be able to view the html by opening `PyFLARE/docs/sphinx/_build/html/index.html` in a web browser. 
