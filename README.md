# README

## Overview

This project demonstrates the use of Qiskit to create and manipulate quantum circuits. The code prepares a quantum state, measures it, and calculates expectation values using Qiskit's StatevectorSampler and StatevectorEstimator primitives. Additionally, it shows how to transpile a quantum circuit for a specific set of basis gates and coupling map.

This project demonstrates the use of Qiskit to create and manipulate quantum circuits. It prepares a quantum state, measures it, and calculates expectation values using Qiskit's StatevectorSampler and StatevectorEstimator primitives. Additionally, it shows how to transpile a quantum circuit for specific basis gates and coupling maps.

## Prerequisites

- Python 3.7+
- Qiskit
- NumPy

## Installation

1. Install Python 3.7 or higher.
2. Install the required packages using pip:

```sh
pip install qiskit numpy
```

## Usage

1. **Prepare the Quantum State**

   The code creates a quantum circuit for preparing the quantum state \(|000\rangle + i |111\rangle\):

   ```python
   qc_example = QuantumCircuit(3)
   qc_example.h(0)          # generate superposition
   qc_example.p(np.pi/2, 0) # add quantum phase
   qc_example.cx(0, 1)      # 0th-qubit-Controlled-NOT gate on 1st qubit
   qc_example.cx(0, 2)      # 0th-qubit-Controlled-NOT gate on 2nd qubit
   ```

2. **Add Classical Output**

   Add classical measurement to all qubits:

   ```python
   qc_measured = qc_example.measure_all(inplace=False)
   ```

3. **Execute Using the Sampler Primitive**

   Use the StatevectorSampler to execute the circuit and get the measurement counts:

   ```python
   sampler = StatevectorSampler()
   job = sampler.run([qc_measured], shots=1000)
   result = job.result()
   print(f" > Counts: {result[0].data['meas'].get_counts()}")
   ```

4. **Define the Observable**

   Define the observable to be measured:

   ```python
   operator = SparsePauliOp.from_list([("XXY", 1), ("XYX", 1), ("YXX", 1), ("YYY", -1)])
   ```

5. **Execute Using the Estimator Primitive**

   Use the StatevectorEstimator to calculate the expectation values:

   ```python
   estimator = StatevectorEstimator()
   job = estimator.run([(qc_example, operator)], precision=1e-3)
   result = job.result()
   print(f" > Expectation values: {result[0].data.evs}")
   ```

6. **Transpile the Circuit**

   Transpile the quantum circuit for a specific set of basis gates and coupling map:

   ```python
   qc_transpiled = transpile(qc_example, basis_gates=['cz', 'sx', 'rz'], coupling_map=[[0, 1], [1, 2]], optimization_level=3)
   print(f" > Transpiled circuit: {qc_transpiled}")
   ```

## Running the Code

To run the code, execute the test_qiskit.py script:

```sh
python test_qiskit.py
```

This will output the measurement counts, expectation values, and the transpiled circuit.

## License

This project is licensed under the MIT License.
