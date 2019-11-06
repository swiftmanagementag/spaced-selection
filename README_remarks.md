# Spaced-Selection 

## Requirements
pip3 install -r code/requirements.txt
pip3 install sklearn
change environment in code to python3

## dws
dws init --create-resources code,results   
dws add local-files data/*.csv
dws add local-files output 


## Swift data to HLR format
./code/swift_to_hlr.py data output/outputhlr201910.csv output/outputsim201910.csv

## HLR Parameter learning
```
./code/hlr_learning.py output/outputhlr201910.csv -o output/201910
```

## HLR model evaluation

```
./code/hlr_eval.py output/201910 output/eval201910.csv

## Simulation

```
➔ ./simulation.py --help
Usage: simulation.py [OPTIONS] DIFFICULTY_PARAMS USER_SESSIONS_CSV
                     SIM_RESULTS_CSV

  Run the simulation with the given output of training the memory model in
  the file DIFFICULTY_PARAMS weights file.

  It also reads the user session information from USER_SESSIONS_CSV to
  generate feasible teaching times.

  Finally, after running the simulations for 10-seeds, the results are saved
  in SIM_RESULTS_CSV.

Options:
  --seed INTEGER                  Random seed for the experiment.  [default: 42]
  --difficulty-kind [HLR|POWER]   Which memory model to assume for the
                                  difficulty_params.  [default: HLR]
  --student-kind [HLR|POWER|REPLAY]
                                  Which memory model to assume for the
                                  student.  [default: HLR]
  --teacher-kind [RANDOMIZED|SPACED_SELECTION|REPLAY_SELECTOR|ROUND_ROBIN|SPACED_SELECTION_DYN|SPACED_RANKING]
                                  Which teacher model to simulate.  
                                  [default: RANDOMIZED]
  --num-users INTEGER             How many users to run the experiments for.
                                  [default: 100]
  --user-id TEXT                  Which user to run the simulation for? [Runs
                                  for the user with maximum attempts
                                  otherwise.]
  --force / --no-force            Whether to overwrite output file.  
                                  [default: False]
  --help                          Show this message and exit.
```

The required files `DIFFICULTY_PARAMS` (an example included in `processed/`
folder) and `USER_SESSIONS_CSV` are produced by the `swift_to_hlr.py` script above.

The different version of the _Spaced-selection_ algorithm which can be
simulated are:

 - `SPACED_SELECTION`: Items are selected for each session based on the population average size of sessions.
 - `DYN_SPACED_RANKING`: Items are selected randomly, but keep number of items correct per session.
 - `SPACED_RANKING`: Similar to `DYN_SPACED_RANKING`, but the items are selected deterministically sorted by the probability they have been forgotten by the student.
