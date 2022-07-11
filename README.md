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
