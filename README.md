# IIITB ASIC CLASS

<details>

<summary> Day 0 </summary>

### Installations

For Linux (Debian-based):

Installation of yosys. gtkwave and iverilog can be done via ``apt``

```
sudo apt-get install iverilog gtkwave yosys
```

To check whether the tools have been successfully installed use :

```
iverilog -v

```


![Screenshot from 2023-07-31 09-53-55](https://github.com/hypnotic2402/iiitb_asic_class/assets/75616591/67fe51ce-aa9e-4eee-97a0-d2fbc7471197)

```
gtkwave -V
```

![Screenshot from 2023-07-31 09-54-04](https://github.com/hypnotic2402/iiitb_asic_class/assets/75616591/f1505e23-a40a-43ef-8e62-1a922af53227)

```
yosys -V
```

![Screenshot from 2023-07-31 09-54-19](https://github.com/hypnotic2402/iiitb_asic_class/assets/75616591/ea745e6e-5884-4e18-a052-61aa990f8d1b)

</details>

<details>

<summary> Day 1 </summary>

### Simulation of MUX design

Clone the sky130RTLDesignAndSynthesisWorkshop repo.

```
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
```

Compile the good_mux.v design using iverilog and simulate it using GTKWave.

```
cd sky130RTLDesignAndSynthesisWorkshop/
cd verilog_files

iverilog good_mux.v tb_good_mux.v
./a.out
gtkwave tb_good_mux.vcd
```
![Screenshot from 2023-08-08 23-54-18](https://github.com/hypnotic2402/iiitb_asic_class/assets/75616591/a6d638d2-c0b1-4b02-aa11-ffd098936cd3)


### Synthesis of MUX

Run yosys to begin synthesis. We will be using the sky130_fd_sc_hd__tt_025C_1v80.lib standard cell libray to generate netlist.

```
yosys
```

```
> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> read_verilog good_mux.v
> synth -top good_mux
> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

We can use the ``show`` command to see the netlist in form of a graph.

```
> show
```
![Screenshot from 2023-08-09 00-21-44](https://github.com/hypnotic2402/iiitb_asic_class/assets/75616591/6533cf59-3c00-4fcc-8c56-6c511d3bfcb5)

Finally, generate the netlist in form of a verilog file:

```
> write_verilog -noattr good_mux_netlist.v
```

</details>

## Day 2

In case the module has multiple sub-modules inside, yosys can be used to synthsize the design in various styles:

### Hierarchical Synthesis

```
> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> read_verilog multiple_modules.v
> synth -top multiple_modules 
> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
```
We can view this netlist as a diagram by :
```
> show multiple_modules
```

![Screenshot from 2023-08-12 04-08-30](https://github.com/hypnotic2402/iiitb_asic_class/assets/75616591/ccf1d0ea-eb2d-4da7-b59a-e684574651c8)
Save in form of a verilog file by:
```
> write_verilog -noattr multiple_modules_hier.v
```

### Flattened Synthesis

```
> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> read_verilog multiple_modules.v
> synth -top multiple_modules
> flatten
> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

We can view this netlist as a diagram by :
```
> show multiple_modules
```
![Screenshot from 2023-08-12 04-26-29](https://github.com/hypnotic2402/iiitb_asic_class/assets/75616591/4cdcd0e8-7b7e-46e2-b124-bc3355fb9374)

### Sub-Module Level

```
> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> read_verilog multiple_modules.v
> synth -top sub_module1
> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> show
```

![Screenshot from 2023-08-12 04-32-36](https://github.com/hypnotic2402/iiitb_asic_class/assets/75616591/9e013461-3eff-4486-a7fe-9b29f4f161d5)

### Flip-Flop Styles

#### Async-Reset FF

```
iverilog dff_asyncres.v tb_dff_asyncres.v
./a.out
gtkwave tb_dff_asyncres.vcd
```


![Screenshot from 2023-08-14 22-28-56](https://github.com/hypnotic2402/iiitb_asic_class/assets/75616591/55307920-442a-49a7-a242-794cfa602dd9)

```
> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> read_verilog dff_asyncres.v
> synth -top dff_asyncres
> dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> show
```

![Screenshot from 2023-08-14 22-40-08](https://github.com/hypnotic2402/iiitb_asic_class/assets/75616591/f8774935-cc7f-47f4-ab20-5de711ed5dcb)


#### Async-Set FF

```
iverilog dff_async_set.v tb_dff_async_set.v
./a.out
gtkwave tb_dff_async_set.vcd
```

![Screenshot from 2023-08-14 22-31-23](https://github.com/hypnotic2402/iiitb_asic_class/assets/75616591/48ce9633-aedd-42f9-ae90-eef8eaf1ff97)

```
> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> read_verilog dff_async_set.v
> synth -top dff_async_set
> dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> show
```
![Screenshot from 2023-08-14 22-42-25](https://github.com/hypnotic2402/iiitb_asic_class/assets/75616591/33f166e6-9375-431a-8052-dcfea0c284bc)



#### Synch-Reset FF

```
iverilog dff_syncres.v tb_dff_syncres.v
./a.out
gtkwave tb_dff_syncres.vcd
```
![Screenshot from 2023-08-14 22-33-33](https://github.com/hypnotic2402/iiitb_asic_class/assets/75616591/9aeea405-f7d7-4ef1-bab2-f4bf7dbc469d)

```
> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> read_verilog dff_dff_syncres.v
> synth -top dff_dff_syncres
> dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> show
```
![Screenshot from 2023-08-14 22-46-15](https://github.com/hypnotic2402/iiitb_asic_class/assets/75616591/4b5d0de7-6498-432d-b089-d47ae7405b81)


### Special cases for synthesis (No Cells used)

```
> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> read_verilog mult_2.v
> synth -top mul2
> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> show
```

![Screenshot from 2023-08-14 22-56-02](https://github.com/hypnotic2402/iiitb_asic_class/assets/75616591/58874f4b-b760-4484-b0fe-e4d7ba1a7533)


```
> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> read_verilog mult_8.v
> synth -top mult8
> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> show
```
![Screenshot from 2023-08-14 22-57-54](https://github.com/hypnotic2402/iiitb_asic_class/assets/75616591/05d28f36-85dc-4720-8ce7-42eedabbb04e)


## Day 3

### Combinatorial Optimizations

```
> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> read_verilog opt_check.v
> synth -top opt_check
> opt_clean -purge
> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> show
```
![Screenshot from 2023-08-14 23-27-22](https://github.com/hypnotic2402/iiitb_asic_class/assets/75616591/66970fe3-1d2e-43e4-b306-78b7b7a93a22)

```
> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> read_verilog opt_check2.v
> synth -top opt_check2
> opt_clean -purge
> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> show
```
![Screenshot from 2023-08-14 23-28-55](https://github.com/hypnotic2402/iiitb_asic_class/assets/75616591/3d39347b-d21f-41cb-9aec-659cd2088e54)

```
> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> read_verilog opt_check3.v
> synth -top opt_check3
> opt_clean -purge
> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> show
```
![Screenshot from 2023-08-14 23-30-19](https://github.com/hypnotic2402/iiitb_asic_class/assets/75616591/ea6f5260-f9df-47b7-a727-4889c3d24a28)


```
> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> read_verilog opt_check4.v
> synth -top opt_check4
> opt_clean -purge
> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> show
```
![Screenshot from 2023-08-14 23-31-10](https://github.com/hypnotic2402/iiitb_asic_class/assets/75616591/199b59e1-795c-40d4-854b-c138958e0be8)

```
> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> read_verilog multiple_module_opt.v
> synth -top multiple_module_opt
> flatten
> opt_clean -purge
> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> show
```

![Screenshot from 2023-08-14 23-33-23](https://github.com/hypnotic2402/iiitb_asic_class/assets/75616591/d2de9d6f-8a7a-4628-975c-932085276dd5)

```
> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> read_verilog multiple_module_opt2.v
> synth -top multiple_module_opt2
> flatten
> opt_clean -purge
> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> show
```
![Screenshot from 2023-08-14 23-34-42](https://github.com/hypnotic2402/iiitb_asic_class/assets/75616591/f2a578bd-bb52-4803-8915-0fc4366c7daf)


### Sequential Logic Optimizations

```
> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> read_verilog dff_const1.v
> synth -top dff_const1
> dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> show
```
![Screenshot from 2023-08-14 23-44-47](https://github.com/hypnotic2402/iiitb_asic_class/assets/75616591/4e3dc337-df9c-4707-8966-e02aef2cab2d)

```
> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> read_verilog dff_const2.v
> synth -top dff_const2
> dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> show
```
![Screenshot from 2023-08-14 23-46-18](https://github.com/hypnotic2402/iiitb_asic_class/assets/75616591/e5e501d1-f42e-4f48-86f0-b6a6015b8f14)

```
> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> read_verilog dff_const3.v
> synth -top dff_const3
> dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> show
```

  
![Screenshot from 2023-08-14 23-47-11](https://github.com/hypnotic2402/iiitb_asic_class/assets/75616591/c10b6742-d7b1-43b4-a305-07fe9cb7b3de)

```
> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> read_verilog dff_const4.v
> synth -top dff_const4
> dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> show
```
![Screenshot from 2023-08-14 23-47-49](https://github.com/hypnotic2402/iiitb_asic_class/assets/75616591/90fde018-9746-40cf-b5d2-2aeea5cfa72c)

```
> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> read_verilog dff_const5.v
> synth -top dff_const5
> dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> show
```

![Screenshot from 2023-08-14 23-49-02](https://github.com/hypnotic2402/iiitb_asic_class/assets/75616591/1eaadb48-1523-4fc3-a3eb-81a8e0328738)

```
> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> read_verilog counter_opt.v
> synth -top counter_opt
> dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> show
```
![Screenshot from 2023-08-14 23-50-44](https://github.com/hypnotic2402/iiitb_asic_class/assets/75616591/9c48dfbe-333d-4ea1-8924-35f7be430b0b)

```
> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> read_verilog counter_opt2.v
> synth -top counter_opt2
> dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> show
```
![Screenshot from 2023-08-14 23-51-45](https://github.com/hypnotic2402/iiitb_asic_class/assets/75616591/c881ae66-f62e-4ba6-a43d-be0767488209)

## Day 4

### Gate Level Simulation

#### ternary_operator_mux

Before Synthesis :

```
iverilog ternary_operator_mux.v tb_ternary_operator_mux.v
./a.out
gtkwave tb_ternary_operator_mux.vcd
```
![Screenshot from 2023-08-15 00-17-50](https://github.com/hypnotic2402/iiitb_asic_class/assets/75616591/53e9e39b-57ba-4f86-9339-2809392b2b9d)


Synthesis:
```
> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> read_verilog ternary_operator_mux.v
> synth -top ternary_operator_mux
> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> write_verilog -nnoattr ternary_operator_mux_net.v
```

Post Synthesis:
```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v
./a.out
gtkwave tb_ternary_operator_mux.vcd
```

![Screenshot from 2023-08-15 00-22-57](https://github.com/hypnotic2402/iiitb_asic_class/assets/75616591/2354c57d-22a7-469d-b083-2305a8b2d1da)

#### bad_mux

Before Synthesis:

```
iverilog bad_mux.v tb_bad_mux.v
./a.out
gtkwave tb_bad_mux.vcd
```

![Screenshot from 2023-08-15 00-26-22](https://github.com/hypnotic2402/iiitb_asic_class/assets/75616591/e567a922-89b3-4b8c-9b6e-b3caa8f317de)

Synthesis:
```
> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> read_verilog bad_mux.v
> synth -top bad_mux
> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> write_verilog -nnoattr bad_mux_net.v
```

Post Synthesis:
```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v bad_mux_net.v tb_bad_mux.v
./a.out
gtkwave tb_bad_mux.vcd
```

![Screenshot from 2023-08-15 00-28-52](https://github.com/hypnotic2402/iiitb_asic_class/assets/75616591/3bd41465-d373-4b4e-8f76-907ec0ab1e59)

#### blocking_caveat

Before Synthesis:

```
iverilog blocking_caveat.v tb_blocking_caveat.v
./a.out
gtkwave tb_blocking_caveat.vcd
```
![Screenshot from 2023-08-15 00-33-32](https://github.com/hypnotic2402/iiitb_asic_class/assets/75616591/7aebbbcb-3883-4a99-bf2a-004498dddbdc)


Synthesis:
```
> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> read_verilog blocking_caveat.v
> synth -top blocking_caveat
> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
> write_verilog -nnoattr blocking_caveat_net.v
```

Post Synthesis:
```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v blocking_caveat_net.v tb_blocking_caveat.v
./a.out
gtkwave tb_blocking_caveat.vcd
```
![Screenshot from 2023-08-15 00-35-24](https://github.com/hypnotic2402/iiitb_asic_class/assets/75616591/18a0e941-d92b-4538-b7df-cd677fe4c7f0)




