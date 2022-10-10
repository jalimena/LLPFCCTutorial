# LLP FCC Tutorial
Tutorial for getting started studying long-lived particles (LLPs) at the FCC-ee

## What this tutorial will teach you

- How to run the [FCCAnalyses software](https://github.com/HEP-FCC/FCCAnalyses), using long-lived Heavy Neutral Leptons (HNLs) that decay to electrons and neutrinos as an example
- In particular, this tutorial is set up to walk you through the major analysis steps, running local jobs on lxplus.

## What this tutorial will NOT cover
- How to generate samples and run pythia8 and Delphes. If you want to do that, you can look [here](https://github.com/jalimena/FCCeePhysicsPerformance/tree/master/case-studies/BSM/LLP/DisplacedHNL/HNL_sample_creation)
- How to run batch jobs and put the output on eos on lxplus. (However, the code to do this is available in the tutorial files. It might require some additional permissions, though.)
- How to run the code on the Fermilab LPC (because the input samples are not there. In principle you can run at the LPC, but this has not been tested.)


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

The code for this step is in `FCCAnalyses/examples/FCCee/bsm/LLPs/DisplacedHNL/analysis_stage1.py`. Open this file in github or in the terminal with your favorite editor and take a look. Compare what you find there with the general description in the readme file (https://github.com/HEP-FCC/FCCAnalyses). Notice that lines 1-37 contain configuration parameters that you should consider changing for your situation.

**Change the parameters in lines 1-37 of `analysis_stage1.py` such that you run a test job, where you run over a small fraction of the centrally-produced `p8_ee_Zee_ecm91` sample that uses the `spring2021` tag and the `IDEA` detector, and make the output go to a new directory called `output_stage1/`. Run this test locally, i.e. not in batch.**

Notice that the rest of the `analysis_stage1.py` file computes the variables of interest. You can take a look here and see what is going on.

To run this step, do:
```
fccanalysis run analysis_stage1.py
```

The output of this step (trees) will live in the `output_stage1/` directory. Open your output with root and check that you get a tree with filled branches.

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

The log output of this step might look something like this:
```
$ fccanalysis run analysis_stage1.py 
===============args bin  Namespace(command='run', pathToAnalysisScript='analysis_stage1.py', files_list=[], output='output.root', nevents=-1, test=False, bench=False, ncpus=None, preprocess=False, validate=False, rerunfailed=False, jobdir='output.root', eloglevel='kUnset', batch=False)
Warning in <TInterpreter::ReadRootmapFile>: class  edm4hep::ObjectID found in libedm4hepDict.so  is already in libedm4drDict.so 
----> Load cxx analyzers from libFCCAnalyses... 
args in mains code============================== Namespace(command='run', pathToAnalysisScript='analysis_stage1.py', files_list=[], output='output.root', nevents=-1, test=False, bench=False, ncpus=None, preprocess=False, validate=False, rerunfailed=False, jobdir='output.root', eloglevel='kUnset', batch=False)
--------------loading analysis file   /afs/cern.ch/work/j/jalimena/FCCeeLLP_test/FCCAnalyses/examples/FCCee/bsm/LLPs/DisplacedHNL/analysis_stage1.py
The variable <outputDirEos> is optional in your analysis.py file, return default empty string
----> yaml file /afs/cern.ch/work/f/fccsw/public/FCCDicts/yaml/FCCee/spring2021/IDEA/p8_ee_Zee_ecm91/merge.yaml succesfully opened
----> Running process p8_ee_Zee_ecm91 with fraction=1e-06, output=p8_ee_Zee_ecm91, chunks=1
----> Running Locally
----> Create dataframe object from files: 
    /eos/experiment/fcc/ee/generation/DelphesEvents/spring2021/IDEA/p8_ee_Zee_ecm91/events_000216632.root
----> nevents original=0  local=100000
----> Init done, about to run 100000 events on 4 CPUs
==============================SUMMARY==============================
Elapsed time (H:M:S)     :   00:00:29
Events Processed/Second  :   3370
Total Events Processed   :   100000
Reduction factor local   :   1.0
===================================================================
```

The output of this step (histograms) will live in the `output_finalSel/` directory.


## Step 3. Run `analysis_plots.py`: Make plots

This final step makes jpg and/or pdf plots of the histograms you made in Step 2.

To run this step, do:
```
fccanalysis plots analysis_plots.py
```

The output of this step (plots) will live in the `plots/` directory. Each subdirectory corresponds to the different selections you applied, and each plot will be saved (by default) as a pdf with both a linear and a log y-axis. You can use the `evince` command to look at a pdf on lxplus.

## Additional resources

- LLPs at the FCC-ee (Snowmass white paper): https://arxiv.org/abs/2203.05502
- Juliette's presentation at the ECFA meeting at DESY (Oct 2022): https://indico.desy.de/event/33640/timetable/?view=standard#97-long-lived-particles-at-the
- FCC code tutorial: https://hep-fcc.github.io/fcc-tutorials/software-basics/README.html
- FCC-ee spring2021 samples: https://fcc-physics-events.web.cern.ch/fcc-physics-events/FCCee/spring2021/Delphesevents_IDEA.php

