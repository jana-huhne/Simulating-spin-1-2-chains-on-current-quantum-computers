# Simulating spin-1/2 chains on current quantum computers
This program simulates the behaviour of a chain of spin-1/2 particles over time on real and simulated quantum computers. The implementation follows the methods suggested by [1] Smith, A., Kim, M.S., Pollmann, F. et al. in [Simulating quantum many-body dynamics on a current digital quantum computer](https://doi.org/10.1038/s41534-019-0217-0). npj Quantum Inf 5, 106 (2019).

## Requirements
- Jupyter notebook
- [Quiskit](https://qiskit.org/)

## Usage
Run all cells in the jupyter notebook in order to start the GUI
You can use it to configure your own simulations, run them on real and simulated quantum computers and plot the results.

First, enter your IBM account token if you wish to run simulations on real hardware, click on "activate" and wait until "success" is displayed next to the button. This step isn't required.
Enter your desired physical and simulation parameters in the sections "Physical Properties" and "Simulation". Give your simulation a name in the "name" field and click "run" in order to run the simulation. Depending on the input, this may take a while. After the simulation has finished, its name should appear in the box under "select results to plot")
In order to plot the results, select the observable fo which the values will be calculated and plotted and, if required, the qubit indices for which to perform the calculation. Choose one of the two display methods. Finally, select the results you wish to plot and click on "show" to open a plot window. You can select multiple results at once, if the display method is "graph", all selected results will be put together into one figure.


Explanations of the input fields:
##### IBM token:
If you have an IBM account, you can copy your user token here (Find it under Account settings -> Account overview -> API token). You can also create an account [here](https://quantum-computing.ibm.com/) for free. This will give the program access to real IBM quantum computers, they will appear in the list of simulators. Note that the simulator menu will take some time to load after an IBM account was activated each time it is clicked because it has to get the available hardware options online.

##### Physical properties
specify the parameters of the hamiltonian $$\hat{H}=-J \sum_{j=1}^{N-1}\left(\hat{\sigma}_{j}^{x} \hat{\sigma}_{j+1}^{x}+\hat{\sigma}_{j}^{y} \hat{\sigma}_{j+1}^{y}\right)+U \sum_{j=1}^{N-1} \hat{\sigma}_{j}^{z} \hat{\sigma}_{j+1}^{z}+\sum_{j=1}^{N} h_{j} \hat{\sigma}_{j}^{z}$$, which describes the simulated physical system

- ` number of qubits`: number of particles to simulate
- `J,U`: Strength of nearest-neighbor interactions
- `h`: potential acting on the spin-1/2 chain. If `slope` is 0, the value is sampled randomly for each grid point from the interval [-h, h]. If `slope` is not zero, the potential starts at value h at the first qubit and increases linearly with the specified slope 
- `initial state`: the state the simulation starts with. Either "domain wall": |...🠗🠗🠗🠕🠕🠕...> or "neel": |...🠕🠗🠕🠗🠕...>

##### Simulation
- `time`: total time span that will be simulated
- `data points`: number of datapoints within the specified timespan
- `symmetric trotter decomposition`: if selected, a symmetric trotter decomposition of order 3 is performed instead of a regular trotter decomposition of order 2
- `error mitigation`: if selected, physically impossible states are discarded in the result (states where the conservation of total spin is violated) 
- `simulator`:  backend used to perform the simulation
	- numerical exact: reference solution using the time propagator of the system
	- numerical trotter: reference solution using the trotterized time propagator of the system
	- numerical reduced trotter: reference solution using the trotterized time propagator of the system with fewer trotter steps and intermediate datapoints in order to achieve shallower circuits ("reduced trotter decomposition")
	- statevector simulator: run the simulation on quiskit's statevector simulator with regular trotter decomposition
	- error free: run the simulation on a simulated quantum computer without hardware error and reduced trotter decomposition
	- fake_*name*: run the simulation on a simulated quantum computer mimicking the error of the real IBM quantum computer with the same name (using the reduced trotter decomposition)
	- ibmq_*name*: run the simulation on a real quantum computer (only if an IBM API token was activated) using the reduced trotter decomposition. note: simulations on real hardware will take significantly longer than local simulations. Status updates can be found at the bottom of the jupyter notebook, directly under the "GUI" cell.
- `name`: the name of the simulation to be run. It will be displayed in the plot legend or title and can be used to select the simulations to plot.

##### Data Processing
- `observable`: the observable used to calculate the data which is later plotted
- `qubit 1`, `qubit 2`: specify the indices of the particles/qubits that should be used in the calculation if required
- `display method`: display the data either as regular graph or as heatmap
