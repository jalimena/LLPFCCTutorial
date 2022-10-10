# LLPFCCTutorial
Tutorial for getting started studying LLPs at the FCC

## What this tutorial will teach you

## What this tutorial will NOT cover
- How to generate samples and run pythia8 and Delphes. If you want to do that, you can look [here](https://github.com/jalimena/FCCeePhysicsPerformance/tree/master/case-studies/BSM/LLP/DisplacedHNL/HNL_sample_creation)


## Step 0. Setup

1. ssh to lxplus

2. Do 
  ```
  bash
  git clone https://github.com/HEP-FCC/FCCAnalyses
  cd FCCAnalyses/
  ```
  and then follow the **Getting started** directions explained in the README. In these **Getting started** directions, you will run a `cmake .. -DCMAKE_INSTALL_PREFIX=../install` command to compile the code: if it doesn't get to 100%, your code is not compiled.
  
3. I encourage you to read the rest of the FCCAnalyses README file as well: it explains how the FCCAnalyses software works in general. We will apply this software to the specific case of long-lived Heavy Neutral Leptons (HNLs).
  
4. Then do `cd examples/FCCee/bsm/LLPs/DisplacedHNL/` to change to the long-lived HNL directory, where you will run the rest of this tutorial.

## Step 1. Run `analysis_stage1.py`: Create interesting variables

This step will run over previously created EDM4hep samples and create root files with TTrees of interesting variables (gen and reco) for the long-lived HNL analysis.

To run this step, do:
```
fccanalysis run analysis_stage1.py
```

The output of this step (trees) will live in the `output_stage1/` directory.

## Step 2. Run `analysis_final.py`: Apply selections

This second step will run over your "stage 1" output and apply selections (or cut sets) to the variables of interest. For each selection, a root file will be created containing histograms. We have set up 5 cut sets for the long-lived HNL analysis:

 - **selNone** applies no selection at all
 - **sel2RecoEle** requires exactly two reconstructed electrons
 - **sel2RecoEle_vetoes** requires exactly two reconstructed electrons and vetoes events with any reconstructed jets, muons, or photons
 - **sel2RecoEle_vetoes_MissingEnergyGt10** requires exactly two reconstructed electrons; vetoes events with any reconstructed jets, muons, or photons; and requires the total missing energy to be > 10 GeV
 - **sel2RecoEle_vetoes_MissingEnergyGt10_absD0Gt0p5** requires exactly two reconstructed electrons; vetoes events with any reconstructed jets, muons, or photons; requires the total missing energy to be > 10 GeV; and requires that both reconstructed electrons have |d0|> 0.5 mm

To run this step, do:
```
fccanalysis final analysis_final.py
```

The output of this step (histograms) will live in the `output_finalSel/` directory.


## Step 3. Run `analysis_plots.py`: Make plots

This final step makes jpg and/or pdf plots of the histograms you made in Step 2.

To run this step, do:
```
fccanalysis plots analysis_plots.py
```

The output of this step (plots) will live in the `plots/` directory. Each subdirectory corresponds to the different selections you applied, and each plot will be saved (by default) as a pdf with both a linear and a log y-axis. You can use the `evince` command to look at a pdf on lxplus.
