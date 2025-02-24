This is a partial implementation of the paper [On-demand high-capacity ride-sharing via dynamic trip-vehicle assignment](https://www.pnas.org/doi/abs/10.1073/pnas.1611675114) without the rebalancing module.

## Prepare the environment
You will need to have a working gurobi installation and license to run this program. Gurobi offers free academic license. You can find the installation instruction [here](https://support.gurobi.com/hc/en-us/articles/4534161999889-How-do-I-install-Gurobi-Optimizer). Instruction on acquiring academic license and installing it on your computer is [here](https://www.gurobi.com/features/academic-named-user-license/).

After successfully installed gurobi, you will need to install required packages. It is recommended to create a virtual environment before installing the packages. If you are using Anaconda or miniconda, run
```bash
conda create --name {env_name}
conda activate {env_name}
pip install -r requirements.txt
```
Now you should be ready to run the program.
## Run the simulation
```bash
Key arguments:
        --THREADS number of threads to run the simulation. 
        --REQUEST_DATA_FILE request data file to load
        --VEHICLE_DATA_FILE initial vehicle location to load
        --VEHICLE_LIMIT number of vehicles
        --PRUNING_RR_K maximum number of conneted request for each request on RV graph
        --PRUNING_RV_K maximum number of conneted vehicles for each request on RV graph
        --INITIAL_TIME simulation start time
        --FINAL_TIME simulation end time
        --CARSIZE capacity of each vehicle
        --LOG_FILE result log file
```
An example to run the simulation from 0 am to 1 am for 500 vehicles with capacity 4:
```bash
python main.py --THREADS 6 --REQUEST_DATA_FILE requests/requests.csv --VEHICLE_DATA_FILE vehicles/vehicles.csv --VEHICLE_LIMIT 500 --PRUNING_RR_K 10 --PRUNING_RV_K 30 --INITIAL_TIME 00:00:00 --FINAL_TIME 01:00:00 --CARSIZE 4  --LOG_FILE results.log
```
The example result log file is saved as `results/results.log`. An example file is provided under the folder.

## Contents
*`data/`: data file to run the algorithm.
*`main.py`: entrance file for the simulation.

*`src/algo/`: include files about the RV generation, RTV generation, and assignment optimization.

*`src/env/`: simulation-related files.

*`src/utils/global_var.py`: include global arguments for the algorithm and explanation.

*`src/utils/parser.py`: include available customizable arguments when running the program. 

*`src/utils/helper.py`: file that includes helper functions. 

## Notes for @Natasha
- You WILL NOT need to run simulation for your project. I include the simulation files in this repository just to help you understand the whole structure of this algorithm.
- `src/algo/rvgenerator` should be the file that you will mainly focus on and it is for building the RV graph in the paper.You will need to adapt this part to use the data you get from Alex. That dataset is a bit different from the data files I put as example under the `data/` folder but it contains all information to build the RV graph.
- The `threads` argument is for multi-thread processing to accelerate computation. If you do not know how to set it, just use 1 and it makes no difference regarding to the algorithm itself.
- To quickly understand the structure of the code, I would recommend that you start from the following line in the `main.py` file:
```bash
ilp_assignement_full(active_vehicles,active_requests,current_time,network,args.THREADS)
```
and step into this function and the nested functions to get an idea.