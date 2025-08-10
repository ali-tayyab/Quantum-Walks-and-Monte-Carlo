## Algorithm: Quantum Galton Board Circuit Construction

**Input:** Number of levels `n` (integer, `n >= 1`)  
**Output:** A quantum circuit simulating a Quantum Galton Board of `n` levels

---

### Step 1: Initialize Parameters
- Set `total_qubits = 2n + 2`
- Set `middle = n + 1`  ← (initial ball position)
- Create quantum register `q[0...total_qubits - 1]`
- Create classical register `c[0...n]`
- Initialize quantum circuit `qc` with `q` and `c`

---

### Step 2: Initial Superposition and Ball Placement
- Apply Hadamard gate: `H(q[0])`  
  ← (Put control qubit in superposition)
- Apply Pauli-X gate: `X(q[middle])`  
  ← (Place the ball in the middlemost qubit)

- Initialize list: `ball_positions = [middle]`

---

### Step 3: Simulate Each Level
**For** each `step` in `0` to `n - 1`:
1. Apply:
   - `RESET(q[0])`
   - `H(q[0])`

2. Let `positions_this_step = sorted(ball_positions)`  
   Let `new_positions = {}`

3. **For** each index `i` and `pos` in `positions_this_step`:
   - Set `left = pos - 1`
   - Set `right = pos + 1`
   - **If** `1 <= left < total_qubits` and `right < total_qubits`:
     - Apply:
       - `CSWAP(q[0], q[left], q[pos])`
       - `CX(q[pos], q[0])`
       - `CSWAP(q[0], q[pos], q[right])`
     - **If** `step >= 1` and `i` is **not the last index**:
       - Apply: `CX(q[right], q[0])`
     - Add `left` and `right` to `new_positions`

4. Set `ball_positions = sorted(new_positions)`

---

### Step 4: Measure Output
- For `j = 0` to `n`:
  - Let `measure_qubit = 2j + 1`  
  - Apply: `MEASURE(q[measure_qubit]) → c[j]`

---
**End of Algorithm**