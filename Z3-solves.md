CustomCrypto - 

```python 
from z3 import *

encrypted_values = [28, 24, 1, 9, 9, 19, 93, 93, 94, 2, 26, 13, 6, 92, 61, 11, 15, 39, 91, 91, 20, 28, 54, 8, 17, 89, 23, 61]

solver = Solver()

enc = [BitVec(f"enc_{i:02}", 8) for i in range(len(encrypted_values))]
dec = [BitVec(f"dec_{i:02}", 8) for i in range(len(encrypted_values))]
key = [BitVec(f"key_{i:02}", 8) for i in range(4)]

prefix = 'pwned'
for i, v in enumerate(prefix):
    solver.add(dec[i] == ord(v))

for i in range(len(encrypted_values)):
    chunk = i // 4
    offset = i % 4
    solver.add(enc[i] == encrypted_values[i])
    solver.add((dec[i] + chunk) ^ key[offset] == enc[i])
    solver.add(dec[i] >= 32)
    solver.add(dec[i] <= 126)

if solver.check() == sat:
    model = solver.model()
    decrypted_message = ''.join(chr(model[dec[i]].as_long()) for i in range(len(encrypted_values)))
    decryption_key = [model[key[i]].as_long() for i in range(4)]
    
    print("Decrypted message:", decrypted_message)
    print("Decryption key:", decryption_key)
else:
    print("No solution")

```

lock-code : 

```python
from z3 import *

solver = Solver()

code = [Int(f'code_{i}') for i in range(3)]

for i in range(3):
    solver.add(And(code[i] >= 0, code[i] <= 9))

# Clue 1: 291 
solver.add(Sum([If(code[i] == 2, 1, 0) for i in range(3)]) +
           Sum([If(code[i] == 9, 1, 0) for i in range(3)]) +
           Sum([If(code[i] == 1, 1, 0) for i in range(3)]) == 1)
solver.add(Or(code[0] == 2, code[1] == 9, code[2] == 1))

# Clue 2: 245 
solver.add(Sum([If(code[i] == 2, 1, 0) for i in range(3)]) +
           Sum([If(code[i] == 4, 1, 0) for i in range(3)]) +
           Sum([If(code[i] == 5, 1, 0) for i in range(3)]) == 1)
solver.add(And(code[0] != 2, code[1] != 4, code[2] != 5))


# Clue 3: 463 
solver.add(Sum([If(code[i] == 4, 1, 0) for i in range(3)]) +
           Sum([If(code[i] == 6, 1, 0) for i in range(3)]) +
           Sum([If(code[i] == 3, 1, 0) for i in range(3)]) == 2)
solver.add(And(code[0] != 4, code[1] != 6, code[2] != 3))
           

# Clue 4: 578 
solver.add(Sum([If(code[i] == 5, 1, 0) for i in range(3)]) +
           Sum([If(code[i] == 7, 1, 0) for i in range(3)]) +
           Sum([If(code[i] == 8, 1, 0) for i in range(3)]) == 0)

# Clue 5: 569 
solver.add(Sum([If(code[i] == 5, 1, 0) for i in range(3)]) +
           Sum([If(code[i] == 6, 1, 0) for i in range(3)]) +
           Sum([If(code[i] == 9, 1, 0) for i in range(3)]) == 1)
solver.add(And(code[0] != 5, code[1] != 6, code[2] != 9))


if solver.check() == sat:
    model = solver.model()
    solution = [model[code[i]].as_long() for i in range(3)]
    print(f"Solulu: {solution}")
else:
    print("Skibidi Toilet")

```

sonda 

Sussy Script won't work with my set of random numbers generated through C script

```python
from z3 import *

seed = 17
rand = [1804289383, 846930886, 1681692777, 1714636915, 1957747793, 424238335, 719885386,
        1649760492, 596516649, 1189641421, 1025202362, 1350490027, 783368690, 1102520059,
        2044897763, 1967513926, 1365180540]

solver = Solver()

ptr = [BitVec(f"ptr{i}", 32) for i in range(seed)]
s = [BitVec(f"s{i}", 8) for i in range(seed)]

solver.add(len(s) <= seed)
solver.add(ptr[0] == 2 * seed + rand[0] % (5 * seed))

for i in range(1, seed):
    solver.add(ptr[i] == ptr[i - 1] + rand[i] % 94 + 33)

for j in range(seed):
    solver.add(Sum([ZeroExt(24, s[k]) for k in range(j + 1)]) == ptr[j])

if solver.check() == sat:
    model = solver.model()
    print("Solution :",model)
    flag = ""
    for i in range(seed):
        flag += chr(model[s[i]].as_long()) 
    print("Flag:", flag)
else:
    print("Skibidi")

```

Sweet 

```python
from z3 import *

for input_len in range(1, 32):
    solve = Solver()
    input = [BitVec(f"i_{i}", 8) for i in range(input_len)]
    output = BitVecVal(0, 64)

    for i in range(input_len):
        output += ZeroExt(64 - 8, input[i])
        output <<= 1

    solve.add(output == 0x2D64A)

    if solve.check() == sat:
        m = solve.model()
        solution = sorted([(d, m[d]) for d in m], key=lambda x: str(x[0]))
        flag = "".join([f"{int(str(x[1]), 10):x}" for x in solution])

        print(flag)
        break
```

mathgenme 

```python
from z3 import *

a1 = Int('a1')
a2 = Int('a2')
a3 = Int('a3')
a4 = Int('a4')

solver = Solver()

solver.add((3 * a4 + a3 + 4 * a2 - 10 * a1 == 28),
           (9 * a2 - 8 * a1 + 6 * a3 - 2 * a4 == 72),
           (a4 + -3 * a2 - 2 * a1 - 8 * a3 == 29),
           (a3 + 5 * a1 + 7 * a2 - 6 * a4 == 88))

if solver.check() == sat:
    model = solver.model()
    print("Solution:")
    print("a1 =", model[a1])
    print("a2 =", model[a2])
    print("a3 =", model[a3])
    print("a4 =", model[a4])
else:
    print("No solution")

```

