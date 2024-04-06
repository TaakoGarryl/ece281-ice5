# ICE 5: Basic Elevator Controller
DOC Statement: I have run into an odd error in my next state logic. ERROR: [XSIM 43-4187] File "C:/Users/Noah.Webster/Desktop/281/ece281-ice5/src/hdl/elevator_controller_fsm.vhd" Line 109 : The "Vhdl 2008 Condition Operator" is not supported yet for simulation. I have checked it on google and chat gpt, which only suggested a change that would take the code from concurrent to sequential, and also didn't get rid of the error when tested. I am at a loss. No syntax error exists that is being displayed.

This is the offending code. 
f_Q_next <= s_floor2 when (i_up_down and o_floor(0) and not o_floor(1)) else
             s_floor3 when (i_up_down and o_floor(1) and not o_floor(0)) else
            s_floor4 when (i_up_down and o_floor(1) and o_floor(0)) else
            s_floor4 when (i_up_down and o_floor(2)) else
            s_floor3 when (not i_up_down and o_floor(2)) else -- going down
            s_floor2 when (not i_up_down and o_floor(1) and o_floor(0)) else
            s_floor1 when (not i_up_down and o_floor(1) and not o_floor(0)) else
            s_floor1 ;

If possible, can I meet with you on Monday to figure this out.
VHDL for ECE 281 [ICE 5](https://usafa-ece.github.io/ece281-book/ICE/ICE5.html)

Targeted toward Digilent Basys3. Make sure to install the [board files](https://github.com/Xilinx/XilinxBoardStore/tree/2018.2/boards/Digilent/basys3).

Tested on Windows 11.

---

## Build the project

You can simply open `elevatorController.xpr` and Vivado will do the rest!

## GitHub Actions Testbench

The workflow uses the [setup-ghdl-ci](https://github.com/ghdl/setup-ghdl-ci) GitHub action
to run a *nightly* build of [GHDL](https://ghdl.github.io/ghdl/).

The workflow uses GHDL to analyze, elaborate, and run the entity specified in the `.github/workflows/testbench.yml`.

```yaml
env:
  TESTBENCH_ENTITY: elevator_controller_fsm
```

If successful then GHDL will quietly exit with a `0` code.
If any of the `assert` statements fail **with** `severity failure` then GHDL will cease the simulation and exit with non-zero code; this will also cause the workflow to fail.
Assert statements of other severity levels, such as "error" w