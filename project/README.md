# Project Description 

In this project, I take a closer look at the Tonkin Gulf located between the Eastern coast of Vietnam and Hainan island to study processes in an enclosed area of ocean. I investigate the following question:

*What role do winds and precipitation play in an enclosed area where oceanic waves aren't as prevalant?*

The objectives of this experiment are to explore and quantify how winds and rain play a role in shaping the temperatures, salinity, and other variables within the Tonkin Gulf. 
My model will focus on the region of the Tonkin Gulf, at 18.5°–22°N, 105.5°–110°E and will be ran for a year. To show the role that wind and rain play in the ocean, two models will be run. One model will contain winds and rain while the other doesn't. The model will be ran starting from January 1st, 2004. I think that the model without any wind or rain will result in higher temperatures, salinity, and less water movement in the area due to lower zonal and meridional velocities,

To initialize the model, I will utilize data from the ECCO Version 5 in January of 2004, which will also be used to to construct the boundary and external forcing conditions. To visualize the results, two time series of each model will be created to allow for analysis through time. Additionally, several frames of graphs will be made into a movie, thus allowing for a visual comparison as to how rain and wind contribute to oceanic conditions within an enclosed region of the ocean.

To recreate this experiment, follow the steps below:

# Step 1 : Generating the Input Files for our Model

In order to conduct this experiment, follow the notebooks in this order:

1. Model Grid
2. Bathymetry
3. Setting Up Initial Conditions
4. External Forcing Conditions
5. Creating the Boundaries

Files generated from steps 1-5 should be put into the `input` directory for model initialization.

# Step 2: Moving files onto a computing cluster for running

Once the files have been generated and put into the correct folder, we can start building the model to run the experiment on the computing cluster. Begin by cloning a copy of [MITgcm](https://github.com/MITgcm/MITgcm) into your scratch directory and make a folder for the configuration, .e.g.
```
mkdir MITgcm/configurations/tgulf
```

Then, use the `scp` command to send the `code`, `input`, and `namelist` directories to your configuration directory. 

# Step 3: Compiling the model
Once all of the files are on the computing cluster, the model can be compiled. Make a `build` directory in the configuration directory and run the following lines:
```
../../../tools/genmake2 -of ../../../tools/build_options/darwin_amd64_gfortran -mods ../code -mpi
make depend
make
```

# Step 4.1: Run the model with wind and rain
After the compilation is complete, run the model with the wind and rain. Make and move to the run directory, link everything from `input` and `code`.
```
mkdir run
cd run
ln -s ../namelist/* .
ln -s ../input/* .
ln -s ../build/mitgcmuv .
mkdir diags
mkdir diags/[one folder for each diag in data.diagnostics]
```
The job script should use 16 processors, as that is what is defined within the `SIZE.h` file, although that can be configured to suit the needs or constraints of your computing cluster.

Note: Before running this model, I chose to make a separate configuration for my no wind and rain model `tgulf_zero`. However, a much easier way would have been to create another directory named `run_zero` within our pre-existing `tgulf` configuration instead of copying the whole model.

# Step 4.2: Run the model without wind and rain
Next, run the model without wind to complete the experiment. You can do this by either copying the entire model to a different configuration in your `configurations` folder, or you can create a directory `run_zero` in your `tgulf` directory. If the former, then follow steps 2 through 4.1, and if the latter, link everything from `input` and `code` to a directory called `run_zero`. Then, for either method, edit the `data.exf` file to point to the modified wind and rain files (see the Creating the External Forcing Conditions.ipynb notebook for details). Then, submit the job script again to rerun the model.

# Step 5: Analyzing the Results
There are two notebooks provided for analysis:
1. Generating Visual Graphs

   This notebook is provided to have a look into how the two models ran in comparison to one another, side by side. It also generates the visualizations provided in the `plots/movies/Side-by-Side Comparison Movies` directory.
   
2. Analyzing the Differences Between Models
   
   This notebook is meant to answer the science question posed above by creating graphs of the differences between the two models and the variables we chose to output in `data.diagnostics`. It also generates the visualizations provided in the `plots/movies/Difference Movies` directory.

# Step 6: Concluding Statements
There is an additional notebook which contains some reflections on the experiment, as well as acknowledging and analyzing possible shortcomings and reasons as to why the model didn't pan out the way I thought it would. This can be found in `notebooks/Concluding Statements.ipynb`.
