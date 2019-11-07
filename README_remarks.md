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