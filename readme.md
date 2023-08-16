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
 Tools installation 
</details>

<details>
<summary>Day 3 </summary>
 Tools installation 
</details>

<details>
<summary>Day 4 </summary>
 Tools installation 
</details>

<details>
<summary>Day 5 </summary>
 Tools installation 
</details>
