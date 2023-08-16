<details>
<summary>Day 0 </summary>
 
## Tools installation 

### 1. iverilog

installed iverilog using below command

``` sudo apt-get install iverilog ```
![iverilog](https://github.com/dillibabuporlapothula/ASIC/assets/141803312/22533fbf-21b1-46de-95c0-e1399d7dd77b)



### 2. gtkwave

installed gtkwave using below command

``` sudo apt install gtkwave ```


![iverilog](https://github.com/dillibabuporlapothula/ASIC/assets/141803312/43d71236-7a63-4f4d-ba76-f43541190d8b)

### 3. YOSYS

commands to install yosys
``` git clone https://github.com/YosysHQ/yosys.git![yosis](https://github.com/dillibabuporlapothula/ASIC/assets/141803312/fb924020-6bf1-4ef2-8387-34a5b6aae18a)

cd yosys 
sudo apt install make (if not installed)
sudo apt-get install build-essential clang bison flex \
   libreadline-dev gawk tcl-dev libffi-dev git \
   graphviz xdot pkg-config python3 libboost-system-dev \
   libboost-python-dev libboost-filesystem-dev zlib1g-dev
 make 
 sudo make install
``` 
![yosis](https://github.com/dillibabuporlapothula/ASIC/assets/141803312/00c32b98-0539-4639-b359-00254b92588a)


</details>
 
<details>
<summary>Day 1 </summary>
 
 ## overview
  Here we have taken 2*1 mux and we synthesized it using iverilog and simulated it using gtkwave to view waveforms and yosys to generate the netlist.

 ## iverilog
   First clone the ``` git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git ``` repository which has all the required verilog codes and library.

Now execute below commands to generate vcd file.

```
cd sky130RTLDesignAndSynthesisWorkshop/
cd verilog_files/
iverilog good_mux.v tb_good_mux.v
./a.out
```
![iverilog_op](https://github.com/dillibabuporlapothula/ASIC/assets/141803312/bd399863-960e-4d08-8cdf-6b91626dfa31)


 ## gtkwave

 execute below command to view the vcd file as waveform.

 ``` gtkwave tb_good_mux.vcd ```
 
![gtkwave_op](https://github.com/dillibabuporlapothula/ASIC/assets/141803312/3f4aadae-41bb-4a2a-83f8-7b1b7004e30a)

 ## yosys

 To generate a logic block and to genarate a netlist for our RTL code execute below commands.

```
yosys
read_liberty -lib VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files/good_mux.v
synth -top good_mux
abc -liberty VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
 ![yosys-ckt](https://github.com/dillibabuporlapothula/ASIC/assets/141803312/b3d3ed97-300a-47e7-bf3a-f7b8a7793d80)

 Netlist :
 
![yosys-code1](https://github.com/dillibabuporlapothula/ASIC/assets/141803312/f1ef6a72-377f-4194-b94d-6083731bfe44)


</details>

<details>
<summary>Day 2 </summary>
 
 ## overview
 As part of this section ,we have gone through the lib files, Hierarchial synthesis, sub-module synthesis Flat synthesis, efficient Flop coding styles and optimizations.

 ## multiple modules synthesis
 To synthesize multiple modules execute below commands

 ```
yosys
read_verilog multiple_modules.v
synth -top multiple_modules
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
show multiple_modules
write_verilog -noattr multiple_modules_hier.v
```
![day-2 multiple modules graph](https://github.com/dillibabuporlapothula/ASIC/assets/141803312/d5ef3ebc-21a3-47ae-8896-48e672e03ab7)
![multi modu -netlist](https://github.com/dillibabuporlapothula/ASIC/assets/141803312/6e81b8a5-4851-4a30-bdfd-860f80fcf842)

 ## sub-module synthesis 
we need to specify in the synth command which submodule to use 

```
synth -top sub_module1
show
```
![sub module synthesis](https://github.com/dillibabuporlapothula/ASIC/assets/141803312/a1d80dc6-fc08-40cd-8ae1-ddfd1863c3d3)

 ## Flat synthesis
Flat synthesis will synthesise the entire design including sub-modules. execute below commands.

```
yosys
read_verilog multiple_modules.v
synth -top multiple_modules
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
flatten
show
write_verilog -noattr multiple_modules_flat.v
!gvim multiple_modules_flat.v
```

![flatten -graph](https://github.com/dillibabuporlapothula/ASIC/assets/141803312/650a660b-5bdb-40ad-a05f-8bc0c19db677)

![flatten -netlist](https://github.com/dillibabuporlapothula/ASIC/assets/141803312/07e61ef0-d235-4e44-a405-3fb602041275)

 ## Different Flipflops coding styles and optimisations
 
 ### D-FlipFlop with asynchronous reset
execute below commands 

```
iverilog dff_asyncres.v tb_dff_asyncres.v
./a.out
gtkwave tb_dff_asyncres.vcd

yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
read_verilog ../verilog_files/dff_asyncres.v
synth -top dff_asyncres
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
show
```
![asynres - gtk](https://github.com/dillibabuporlapothula/ASIC/assets/141803312/5716e622-382f-47be-9fd5-6c81b8c499a7)

![asynres - yosy](https://github.com/dillibabuporlapothula/ASIC/assets/141803312/9c1da0cc-deab-483d-932a-45ef05d942a9)

 ### D-FlipFlop with synchronous reset

 ```
iverilog dff_syncres.v tb_dff_syncres.v
./a.out
gtkwave tb_dff_syncres.vcd

yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
read_verilog ../verilog_files/dff_syncres.v
synth -top dff_syncres
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
show
```

![synres - gtk1](https://github.com/dillibabuporlapothula/ASIC/assets/141803312/dddad6ed-3c41-4883-88b9-858c9d2d738e)

![synres - yos](https://github.com/dillibabuporlapothula/ASIC/assets/141803312/68c01cfd-817a-40fc-bb60-2d0fe4701d19)

 ### D-FlipFlop with asynchronous set

 ```
iverilog dff_async_set.v tb_dff_async_set.v
./a.out
gtkwave tb_dff_async_set.vcd

yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
read_verilog ../verilog_files/dff_async_set.v
synth -top dff_async_set
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
show
```
![async set - gtk](https://github.com/dillibabuporlapothula/ASIC/assets/141803312/799d7360-1813-4611-969c-32a291522f77)

![async set - yosy](https://github.com/dillibabuporlapothula/ASIC/assets/141803312/c4fd95c7-b3e5-43cb-ad67-df8b047fab97)

## Multipliers
### mult2

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog mult_2.v
synth -top mul2
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show mul2
write_verilog -noattr mul2_net.v
```
![mult 2 -](https://github.com/dillibabuporlapothula/ASIC/assets/141803312/c2289666-672e-4471-b587-c9c631f17477)

### mult8

```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog mult_8.v
synth -top mul8
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show mul8
write_verilog -noattr mul8_net.v
```
![mult 8](https://github.com/dillibabuporlapothula/ASIC/assets/141803312/3a7b299e-c655-49db-a2cd-0ee1d9baba72)

</details>

<details>
<summary>Day 3 </summary>
 
 ## overview 
  The optimisation is important to increase the speed,efficiency and reduce area & power of a logic circuit.

 ## Combinational logic optimisation

 ```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
read_verilog ../verilog_files/opt_check4.v
synth -top opt_check4
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
show
```
![comb - opt check](https://github.com/dillibabuporlapothula/ASIC/assets/141803312/818d4efe-f701-4ea0-b120-2ca1783e30b0)

![comb - opt check2](https://github.com/dillibabuporlapothula/ASIC/assets/141803312/d41a3e94-479b-4de9-8a37-f784eb440d6f)

![comb - opt check3](https://github.com/dillibabuporlapothula/ASIC/assets/141803312/e0944865-7bd8-43c9-a4b5-71856307be09)

![comb - opt check4](https://github.com/dillibabuporlapothula/ASIC/assets/141803312/2ae22cea-b29a-4644-8f33-8d590a765b27)

 ### multi module optimisation

 ```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
read_verilog ../verilog_files/multiple_module_opt.v
synth -top multiple_module_opt
opt_clean -purge
flatten
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
show

```

![comb multi-module optimi](https://github.com/dillibabuporlapothula/ASIC/assets/141803312/627bd5c0-abb8-4265-8ea4-872203563e10)

![multi module opt 2](https://github.com/dillibabuporlapothula/ASIC/assets/141803312/2a2431b9-cdac-4bfb-bb88-71520fcffaee)

 ## sequential logic optimisation
 
</details>

<details>
<summary>Day 4 </summary>
 Tools installation 
</details>

<details>
<summary>Day 5 </summary>
 Tools installation 
</details>
