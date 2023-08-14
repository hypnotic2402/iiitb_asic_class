# IIITB ASIC CLASS

## Day 0

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

## Day 1

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

## Flip-Flop Styles

### Async-Reset FF

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


### Async-Set FF

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



### Synch-Reset FF

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








