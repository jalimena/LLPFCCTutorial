# LLP FCC Tutorial
Tutorial for getting started studying long-lived particles (LLPs) at the FCC-ee

## Who this tutorial is for

This tutorial is for anyone interested in working with the FCC software. It assumes some basic knowledge of ROOT, C++, linux, and (very basic knowledge of) git. It should be appropriate for beginner Masters and PhD students.

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
  git clone git@github.com:FCC-LLP/FCCAnalyses.git
  cd FCCAnalyses/
  ```
Notice that I am having you clone the FCC-LLP version of the FCCAnalyses repository, since it contains a few new developments for this specific tutorial. However, it should be consistent with the main FCCAnalyses repository at https://github.com/HEP-FCC/FCCAnalyses

Then follow the **Getting started** directions explained in the README. In these **Getting started** directions, you will run a `cmake .. -DCMAKE_INSTALL_PREFIX=../install` command to compile the code: if it doesn't get to 100%, your code is not compiled.
  
*Note that every time you start a new session, you will need to source the setup file. The rest of the setup steps do not need to be repeated every time, unless you need to recompile.
  
3. I encourage you to read the rest of the FCCAnalyses README file as well: it explains how the FCCAnalyses software works in general. We will apply this software to the specific case of long-lived Heavy Neutral Leptons (HNLs).
  
4. Then do `cd examples/FCCee/bsm/LLPs/DisplacedHNL/` to change to the long-lived HNL directory, where you will run the rest of this tutorial.

## Step 1. Run `analysis_stage1.py`: Create interesting variables

This step will run over previously created EDM4hep samples and create root files with TTrees of interesting variables (gen and reco) for the long-lived HNL analysis.

The code for this step is in `FCCAnalyses/examples/FCCee/bsm/LLPs/DisplacedHNL/analysis_stage1.py`. Open this file in github or in the terminal with your favorite editor and take a look. Compare what you find there with the general description in the readme file (https://github.com/HEP-FCC/FCCAnalyses). Notice that lines 1-53 contain configuration parameters that you should consider changing for your situation.

### Step 1 a (Run `analysis_stage1.py` over centrally-produced backgrounds):
**Change the parameters in lines 1-53 of `analysis_stage1.py` such that you run a test job, where you run over a small fraction of the centrally-produced `p8_ee_Zee_ecm91` sample that uses the `spring2021` tag and the `IDEA` detector, and make the output go to a new local directory called `output_stage1/`. Notice which parameters in lines 1-53 need to be uncommented or commented out. Run this test locally, i.e. not in batch.**

Notice that the rest of the `analysis_stage1.py` file computes the variables of interest. You can take a look here and see what is going on.

**To run this step, do:**
```
fccanalysis run analysis_stage1.py
```



<details>
  <summary>Click here to see what the log output of this step might look like:</summary>
  

  ```
  $ fccanalysis run analysis_stage1.py 
===============args bin  Namespace(command='run', pathToAnalysisScript='analysis_stage1.py', files_list=[], output='output.root', nevents=-1, test=False, bench=False, ncpus=None, preprocess=False, validate=False, rerunfailed=False, jobdir='output.root', eloglevel='kUnset', batch=False)
!Warning in <TInterpreter::ReadRootmapFile>: class  edm4hep::ObjectID found in libedm4hepDict.so  is already in libedm4drDict.so 
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
 
</details>
  
  
The root file output of this step (trees) will live in the `output_stage1/` directory. Open your background output with root and check that you get a tree with filled branches.
  
<details>
  <summary>Click here to see what the root file output of this step might look like:</summary>
  
<img width="1043" alt="output_stage1_Zee_TBrowser" src="https://user-images.githubusercontent.com/5177191/194885131-87c059ad-5a64-44b7-be84-a6c44c7da8f9.png">
  
</details>

  
Note that if you wanted to run over all of the background samples used in the long-lived HNL analysis, you would probably need to run in batch and put the output in eos or a similar space.  
  
  
### Step 1 b (Run `analysis_stage1.py` over privately-produced signals):

**Now change the parameters in lines 1-53 of `analysis_stage1.py` again to run over the privately-produced HNL signal samples, and make the output go to your local `output_stage1/` directory. Notice which parameters in lines 1-53 need to be uncommented or commented out. Run this test locally, i.e. not in batch (it shouldn't take long, as these samples are much smaller than the backgrounds).**

**To run this step, do:**
```
fccanalysis run analysis_stage1.py
```

<details>
  <summary>Click here to see what the log output of this step might look like:</summary>
  

  ```
  $ fccanalysis run analysis_stage1.py 
----> Load cxx analyzers from libFCCAnalyses... 
--------------loading analysis file   /afs/cern.ch/work/j/jalimena/FCCeeLLP/FCCAnalyses/examples/FCCee/bsm/LLPs/DisplacedHNL/analysis_stage1.py
The variable <outputDirEos> is optional in your analysis.py file, return default empty string
----> Running process eenu_30GeV_1p41e-6Ve with fraction=1, output=eenu_30GeV_1p41e-6Ve, chunks=1
----> Running Locally
----> Create dataframe object from files: 
    /eos/experiment/fcc/ee/analyses/case-studies/bsm/LLPs/HNLs/HNL_eenu_MadgraphPythiaDelphes/eenu_30GeV_1p41e-6Ve/HNL_eenu_30GeV_1p41e-6Ve_Delphes3_5_1_pre01.root
----> nevents original=0  local=50000
The variable <geometryFile> is optional in your analysys.py file, return default value empty string
The variable <readoutName> is optional in your analysys.py file, return default value empty string
----> Init done, about to run 50000 events on 4 CPUs
==============================SUMMARY==============================
Elapsed time (H:M:S)     :   00:00:45
Events Processed/Second  :   1104
Total Events Processed   :   50000
Reduction factor local   :   1.0
===================================================================
 
 
----> Running process eenu_50GeV_1p41e-6Ve with fraction=1, output=eenu_50GeV_1p41e-6Ve, chunks=1
----> Running Locally
----> Create dataframe object from files: 
    /eos/experiment/fcc/ee/analyses/case-studies/bsm/LLPs/HNLs/HNL_eenu_MadgraphPythiaDelphes/eenu_50GeV_1p41e-6Ve/HNL_eenu_50GeV_1p41e-6Ve_Delphes3_5_1_pre01.root
----> nevents original=0  local=50000
The variable <geometryFile> is optional in your analysys.py file, return default value empty string
The variable <readoutName> is optional in your analysys.py file, return default value empty string
----> Init done, about to run 50000 events on 4 CPUs
==============================SUMMARY==============================
Elapsed time (H:M:S)     :   00:00:21
Events Processed/Second  :   2369
Total Events Processed   :   50000
Reduction factor local   :   1.0
===================================================================
 
 
----> Running process eenu_70GeV_1p41e-6Ve with fraction=1, output=eenu_70GeV_1p41e-6Ve, chunks=1
----> Running Locally
----> Create dataframe object from files: 
    /eos/experiment/fcc/ee/analyses/case-studies/bsm/LLPs/HNLs/HNL_eenu_MadgraphPythiaDelphes/eenu_70GeV_1p41e-6Ve/HNL_eenu_70GeV_1p41e-6Ve_Delphes3_5_1_pre01.root
----> nevents original=0  local=50000
The variable <geometryFile> is optional in your analysys.py file, return default value empty string
The variable <readoutName> is optional in your analysys.py file, return default value empty string
----> Init done, about to run 50000 events on 4 CPUs
==============================SUMMARY==============================
Elapsed time (H:M:S)     :   00:00:21
Events Processed/Second  :   2326
Total Events Processed   :   50000
Reduction factor local   :   1.0
===================================================================
 
 
----> Running process eenu_90GeV_1p41e-6Ve with fraction=1, output=eenu_90GeV_1p41e-6Ve, chunks=1
----> Running Locally
----> Create dataframe object from files: 
    /eos/experiment/fcc/ee/analyses/case-studies/bsm/LLPs/HNLs/HNL_eenu_MadgraphPythiaDelphes/eenu_90GeV_1p41e-6Ve/HNL_eenu_90GeV_1p41e-6Ve_Delphes3_5_1_pre01.root
----> nevents original=0  local=50000
The variable <geometryFile> is optional in your analysys.py file, return default value empty string
The variable <readoutName> is optional in your analysys.py file, return default value empty string
----> Init done, about to run 50000 events on 4 CPUs
==============================SUMMARY==============================
Elapsed time (H:M:S)     :   00:00:22
Events Processed/Second  :   2262
Total Events Processed   :   50000
Reduction factor local   :   1.0
===================================================================

  ```
</details>
  
The root file output of this step (trees) will again live in the output_stage1/ directory. Open your signal output with root and check that you get a tree with filled branches.

  
## Step 2. Run `analysis_final.py`: Apply selections

This second step will run over the "stage 1" output (either yours or ones we have done previously) and apply selections (or cut sets) to the variables of interest. For each selection, a root file will be created containing histograms. We have set up 5 cut sets for the long-lived HNL analysis:

 - **selNone** applies no selection at all
 - **sel2RecoEle** requires exactly two reconstructed electrons
 - **sel2RecoEle_vetoes** requires exactly two reconstructed electrons and vetoes events with any reconstructed jets, muons, or photons
 - **sel2RecoEle_vetoes_MissingEnergyGt10** requires exactly two reconstructed electrons; vetoes events with any reconstructed jets, muons, or photons; and requires the total missing energy to be > 10 GeV
 - **sel2RecoEle_vetoes_MissingEnergyGt10_absD0Gt0p5** requires exactly two reconstructed electrons; vetoes events with any reconstructed jets, muons, or photons; requires the total missing energy to be > 10 GeV; and requires that both reconstructed electrons have |d0|> 0.5 mm

The code for this step is in `FCCAnalyses/examples/FCCee/bsm/LLPs/DisplacedHNL/analysis_final.py`. Open this file in github or in the terminal with your favorite editor and take a look. Notice the parameters that can be set, including the **procesList** and **cutList**. Notice the format for how you define a histogram.
  
**You can run over the small Zee sample you made in Step 1, or you can run over larger samples that were previously produced. To run over the larger background samples that are in `/eos/experiment/fcc/ee/analyses/case-studies/bsm/LLPs/HNLs/output_stage1/`, I suggest you do them one at a time (comment out the others in `processList` in `analysis_final.py`). The Zee and Ztautau samples will take about 10 min each, but the other backgrounds are quite big and perhaps take too much time to run locally. The stage 1 signal HNL samples in eos should be pretty quick to run locally.**
  
**To run this step, do:**
```
fccanalysis final analysis_final.py
```

<details>
  <summary>Click here to see what the log output of this step might look like:</summary>
  

  ```
  $ fccanalysis final analysis_final.py
===============args bin  Namespace(command='final', pathToAnalysisScript='analysis_final.py', eloglevel='kUnset')
Warning in <TInterpreter::ReadRootmapFile>: class  edm4hep::ObjectID found in libedm4hepDict.so  is already in libedm4drDict.so 
----> Load cxx analyzers from libFCCAnalyses... 
args in mains code============================== Namespace(command='final', pathToAnalysisScript='analysis_final.py', eloglevel='kUnset')
--------------loading analysis file   /afs/cern.ch/work/j/jalimena/FCCeeLLP_test/FCCAnalyses/examples/FCCee/bsm/LLPs/DisplacedHNL/analysis_final.py
----> file  /eos/experiment/fcc/ee/analyses/case-studies/bsm/LLPs/HNLs/output_stage1/p8_ee_Zee_ecm91.root   does not exist. Try if it is a directory as it was processed with batch
----> open directory  /eos/experiment/fcc/ee/analyses/case-studies/bsm/LLPs/HNLs/output_stage1/p8_ee_Zee_ecm91.root
  ---->  /eos/experiment/fcc/ee/analyses/case-studies/bsm/LLPs/HNLs/output_stage1/p8_ee_Zee_ecm91/chunk0.root
  ---->  /eos/experiment/fcc/ee/analyses/case-studies/bsm/LLPs/HNLs/output_stage1/p8_ee_Zee_ecm91/chunk1.root
  ---->  /eos/experiment/fcc/ee/analyses/case-studies/bsm/LLPs/HNLs/output_stage1/p8_ee_Zee_ecm91/chunk10.root
  ---->  /eos/experiment/fcc/ee/analyses/case-studies/bsm/LLPs/HNLs/output_stage1/p8_ee_Zee_ecm91/chunk11.root
  ---->  /eos/experiment/fcc/ee/analyses/case-studies/bsm/LLPs/HNLs/output_stage1/p8_ee_Zee_ecm91/chunk12.root
  ---->  /eos/experiment/fcc/ee/analyses/case-studies/bsm/LLPs/HNLs/output_stage1/p8_ee_Zee_ecm91/chunk13.root
  ---->  /eos/experiment/fcc/ee/analyses/case-studies/bsm/LLPs/HNLs/output_stage1/p8_ee_Zee_ecm91/chunk14.root
  ---->  /eos/experiment/fcc/ee/analyses/case-studies/bsm/LLPs/HNLs/output_stage1/p8_ee_Zee_ecm91/chunk15.root
  ---->  /eos/experiment/fcc/ee/analyses/case-studies/bsm/LLPs/HNLs/output_stage1/p8_ee_Zee_ecm91/chunk16.root
  ---->  /eos/experiment/fcc/ee/analyses/case-studies/bsm/LLPs/HNLs/output_stage1/p8_ee_Zee_ecm91/chunk17.root
  ---->  /eos/experiment/fcc/ee/analyses/case-studies/bsm/LLPs/HNLs/output_stage1/p8_ee_Zee_ecm91/chunk18.root
  ---->  /eos/experiment/fcc/ee/analyses/case-studies/bsm/LLPs/HNLs/output_stage1/p8_ee_Zee_ecm91/chunk19.root
  ---->  /eos/experiment/fcc/ee/analyses/case-studies/bsm/LLPs/HNLs/output_stage1/p8_ee_Zee_ecm91/chunk2.root
  ...
processed events  {'p8_ee_Zee_ecm91': 10000000}
events in ttree   {'p8_ee_Zee_ecm91': 10000000}

---->  Running over process :  p8_ee_Zee_ecm91
The variable <defineList> is optional in your analysis_final.py file, return empty dictonary
----> Defining snapshots and histograms
----> Evaluating...
----> Done
----> Cutflow
       All events                                                      : 10000000
       After selection selNone                                         : 10000000
       After selection sel2RecoEle                                     : 7961732
       After selection sel2RecoEle_vetoes                              : 6994080
       After selection sel2RecoEle_vetoes_MissingEnergyGt10            : 32253
       After selection sel2RecoEle_vetoes_MissingEnergyGt10_absD0Gt0p5 : 0
----> Saving outputs
==============================SUMMARY==============================
Elapsed time (H:M:S)     :   00:08:47
Events Processed/Second  :   18945
Total Events Processed   :   10000000
===================================================================
  ```
</details>
  

The root file output of this step (histograms) will live in the `output_finalSel/` directory. Open your output with root and check that you get histograms.

<details>
  <summary>Click here to see what the root file output of this step might look like:</summary>
  
  <img width="1410" alt="output_final_Zee_sel2RecoEle_vetoes_MissingEnergyGt10_histo" src="https://user-images.githubusercontent.com/5177191/195290459-a48f92d3-5068-4904-8c97-7f91f33aff91.png">

</details>
  
  

## Step 3. Run `analysis_plots.py`: Make plots

This final step makes jpg and/or pdf plots of the histograms you made in Step 2.

**To run this step, do:**
```
fccanalysis plots analysis_plots.py
```

The output of this step (plots) will live in the `plots/` directory. Each subdirectory corresponds to the different selections you applied, and each plot will be saved (by default) as a pdf with both a linear and a log y-axis. You can use the `evince` command to look at a pdf on lxplus.

## Questions for going deeper
  
1. Why does this analysis apply the selections that it does?
2. How do the distributions of variables change as different selections are applied? Why?
3. If you were to add additional variables for study in the long-lived HNL analysis, which would you try and how would you add them?
4. How do you find what variables are available in FCCAnalyses::MCParticle? And in ReconstructedParticle?
5. How do you access a variable pertaining to a specific gen electron in the event?
6. How do you normalize the plots to 1? How do normalize the plots to the expected number of events at a given integrated luminosity? Where are the cross sections defined in this framework?
  
## Additional resources

- LLPs at the FCC-ee (Snowmass white paper): https://arxiv.org/abs/2203.05502
- Juliette's presentation at the ECFA meeting at DESY (Oct 2022): https://indico.desy.de/event/33640/timetable/?view=standard#97-long-lived-particles-at-the
- FCC code tutorial: https://hep-fcc.github.io/fcc-tutorials/software-basics/README.html
- FCC tutorial at CERN (October 2022): https://indico.cern.ch/event/1182767/timetable/?view=standard
- FCC-ee spring2021 samples: https://fcc-physics-events.web.cern.ch/fcc-physics-events/FCCee/spring2021/Delphesevents_IDEA.php

