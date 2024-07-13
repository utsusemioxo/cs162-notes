`x0` register is 'hard-wired' to value 0.
e.g. 
```risc-v
add x3,x4,x0       # same as f=g in C
```
below instruction is non-sence:
```risc-v
add x0,x3,x4.      # will not do anything
```
