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

This step will run over previously created EDM4hep samples

## Step 2. Run `analysis_final.py`: Apply selections

## Step 3. Run `analysis_plots.py`: Make plots
