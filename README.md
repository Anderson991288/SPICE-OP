# SPICE-OP
### **Damped Miller Integrator**

![Screenshot from 2022-07-11 17-29-51](https://user-images.githubusercontent.com/68816726/178234074-6e05f332-655b-40c7-9683-538bc50016a8.png)

netlist:
```
* the Damped Miller Integrator (R2=1M) *
vi 1 0 ac 1 sin(0 1 1k)
r1 1 2 1k
r2 2 3 1Meg
c2 2 3 20uF
Eopamp 3 0 0 2 1e7
.ac dec 100 1m 1k
.control
run
plot db(v(3))

.endc
```

![Screenshot from 2022-07-11 17-27-56](https://user-images.githubusercontent.com/68816726/178234206-b00e1393-44b0-453c-a87e-4a5b045af493.png)

### differential amplifier

![Screenshot from 2022-07-11 17-32-03](https://user-images.githubusercontent.com/68816726/178234546-16d2b531-967d-423b-b579-ac78aa22306e.png)

netlist:
```
Differential Amplifier
* op amp subcircuit *
.subckt ideal_opamp 1 2 3
* node1: output terminal
* node2: noninverting input terminal
* node3: invertinf terminal
Eopamp 1 0 2 3 1e7
Iopen1 2 0 0A
Iopen2 3 0 0A
.ends ideal_opamp
* main circuit *
Vcm 3 0 SIN (0 20mV 100Hz)
Vdc1 3 1 DC 15mV
Vdc2 2 3 DC 15mV
Xop_A1 6 5 4 ideal_opamp
R1 1 4 1k
R2 4 6 9k
R3 2 5 1k
R4 5 0 1k
* analysis requests *
.tran 10us 80ms 0 10us
.control
run
* output request *
plot v(1) v(2) v(6)
.endc
```

![Screenshot from 2022-07-11 17-35-35](https://user-images.githubusercontent.com/68816726/178235200-796c93b8-0b33-4cbd-9ca2-f5c7164ba30d.png)



