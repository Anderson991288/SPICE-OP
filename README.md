# SPICE-OP
## **Damped Miller Integrator**

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

*調整RC大小來控制cutoff frequency*
### R1 = 1k
![Screenshot from 2022-07-11 17-27-56](https://user-images.githubusercontent.com/68816726/178234206-b00e1393-44b0-453c-a87e-4a5b045af493.png)

### R1 = 100
![Screenshot from 2022-07-11 21-28-31](https://user-images.githubusercontent.com/68816726/178275259-6f353b85-be0e-4580-b273-1fbd25a35ca9.png)

理想的運放積分器使用一個電容，連接在輸出端和運放反相輸入端之間

對反相輸入端的負反饋確保節點X保持在地電位（虛擬地）。如果輸入電壓為0V，則沒有電流通過輸入電阻R1，電容不充電。

因此，理想的輸出電壓為零。

如果將恆定的正電壓 (DC) 施加到積分放大器的輸入端，則輸出電壓將以線性速率下降到負值，以試圖將反相輸入端保持在地電位。

相反，輸入端的恆定負電壓會導致輸出端呈線性上升（正）電壓。輸出電壓的變化率與施加的輸入電壓值成正比。


在輸入信號的低頻下，電路的行為通常類似於積分器。在高頻下，電容器充當短路並繞過電阻器R 2。

電容器的電抗反過來會降低放大器的增益


## differential amplifier

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

## Instrumentation Amplifier


![Screenshot from 2022-07-11 22-00-20](https://user-images.githubusercontent.com/68816726/178281592-7b29d6f9-1418-4388-a719-74c785bb8e8e.png)

netlist
```
Instrumentation Amplifier
* op amp subcircuit *
.subckt ideal_opamp 1 2 3
* node1: output terminal
* node2: noninverting input terminal
* node3: inverting terminal
Eopamp 1 0 2 3 1e7
Iopen1 2 0 0A
Iopen2 3 0 0A
.ends ideal_opamp
* main circuit *
Vcm 5 0 sin(0 20V 100Hz)
Vdc1 5 1 DC 15mV
Vdc2 2 5 DC 15mV
Xop_A1 6 1 3 ideal_opamp
Xop_A2 7 2 4 ideal_opamp
Xop_A3 10 9 8 ideal_opamp
Ra 3 4 5k
Rb1 3 6 50k
Rb2 4 7 50k
R1 6 8 15k
R2 8 10 15k
R3 7 9 15k
R4 9 0 15k
* analysis request *
.tran 10us 80ms 0 10us
.control
run
* output request *
print v(1) v(2) v(10)
plot v(2) 
plot v(1)
plot v(1) v(2) v(10)
.endc
```

![Screenshot from 2022-07-11 21-59-19](https://user-images.githubusercontent.com/68816726/178281717-5d50d4d5-e545-4160-8300-72e5414adab5.png)

