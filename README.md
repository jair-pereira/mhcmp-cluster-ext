# Cluster-based Analysis of Empirical Metaheuristics Performance
This repository contains our paper submission's code, data, and figures.

In this study, we propose a clustering-based method to characterize the performance of metaheuristic algorithms based on their performance on benchmark problems with diverse landscape characteristics.

<!-- ## Data Processing
... -->

## Running Experiments
This section contains the details to repeat the experiments from the beginning.
This experiment collects the Performance Profiles of the four PSO variants, each with six specified parameter settings, on the bbob suite on 2, 3, 5, and 10 dimensions. Check the section above for the data processing.

### Docker
Scripts are in a Docker environment. To initialize the docker:
```
docker build -t ipso .
docker run  -dt --cpus="X"  --name cpso ipso
```
where X is the number of desired CPUs

### Step 2: Performance profiling all PSO variants
Run the experiments:
```
docker exec -t -i cpso /bin/bash
nohup python3 experiment.py -e params/exp.json -a params/pso.json -p params/problems.json &
nohup python3 experiment.py -e params/exp.json -a params/pso_cauchy.json -p params/problems.json &
nohup python3 experiment.py -e params/exp.json -a params/pso_normal.json -p params/problems.json &
nohup python3 experiment.py -e params/exp.json -a params/pso_uniform.json -p params/problems.json &
exit
 ```

 ### Step 3: Accessing the data
 To process the data, first copy the data out of the docker container:
```
 docker cp cpso:/home/mhcmp_pso/exdata ./data_processing
```

 ### Step 4: Stopping Docker and Deleting the Container
```
docker stop cpso
docker rm cpso
```

### INFO: Parameter Files
Three parameter file types are inside the folder [params](https://github.com/jair-pereira/mhcmp-cluster-ext/blob/main/mhcmp_pso/params/).
1. [params/exp.json](https://github.com/jair-pereira/mhcmp-cluster-ext/blob/main/mhcmp_pso/params/exp.json): Experimental parameters containing the seed and the maximum number of functions evaluated to be used.
2. [params/pso.json](https://github.com/jair-pereira/mhcmp-cluster-ext/blob/main/mhcmp_pso/params/pso.json): Meta-parameters for the PSO variant, including pool size and velocity modifier. There is one equivalent file for each PSO variant.
3. [params/problems.json](https://github.com/jair-pereira/mhcmp-cluster-ext/blob/main/mhcmp_pso/params/problems.json): Benchmark suite. This file specifies the functions, instances, and dimensions for evaluating the PSO variants.

### Dependencies
To run these scripts, it is necessary to have installed:
* cocopp (coco processing tool)
* numpy
* pandas
* sklearn
* scipy
* plotly
