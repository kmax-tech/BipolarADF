# Bipolar ADFs -- Time Measurement
tl;dr This project creates bipolar Abstract Dialectical Framework (bipolar ADF) models in *bipolar disjunctive normal form* and compares the needed time for the two calculation styles of two-valued completion and three-valued logic. The comparison is made for admissible, complete, and preferred semantics.

An ADF is a tuple $A=(S,\Phi)$, where $S$ is a set of statements (arguments) and $ \Phi $ a set of acceptance functions. Because of similarities to graphs, we also refer to statements as nodes. In addition, acceptance functions can be written as logical formulas and stated as acceptance conditions. In this investigation, we are considering bipolar ADFs models which are in *bipolar disjunctive normal form*. Therefore the acceptance condition of a statement is a disjunction of conjunctions and only contains arguments with the same polarity. 
This project measures and compares two different styles of bipolar ADF calculation. The two-valued completion approach takes an interpretation $v$  and a node $s \in S$  and obtains the value of $s$ in $\Gamma(v)$  by taking all two-valued completions of $v$. After that, the consensus of all two-valued completion wrt. to the acceptance condition of $s$ is determined. In the three-valued approach, the acceptance condition of $s$ is directly evaluated with Kleene's three-valued logic. The measurement is done for admissible, complete, and preferred semantics.

## General Overview
Each *BipolarNodes_|S|* directory contains the test cases and evaluated results for a bipolar ADF of a specified number of statements $|S|$. Files starting with *BipolarTest* contain the generated models, which are indicated by the *Model:* entry. A bipolar ADF is notated as a list, which consists itself of lists. Each of these sublists consists of two parts. The first part specifies the name of the node. The second part specifies the acceptance condition here letters refer to nodes, the `,` symbol signals *and*, the  `;` symbol stands for *or* and `#` for *negation*. In addition `!`  signals *verum* and `?` stands for *falsum*.  Files beginning with *EvalBipolarTest* contain the results. The calculated semantics are specified in the filename after *Sem_ *; here *a* stands for admissible, *c* for complete, and *p* for preferred. If *tri* is found in the filename, it indicates that the results were calculated with Kleene's three-valued logic. Otherwise, the two-valued completion approach was used. In an *EvalBipolarTest* file the calculated model is notated after the  *Model:* entry.  The obtained results are stated next in the *Semantics:* entry as a tuple. Here, the first entry is the needed time in seconds for 10 calculations of the model. The second entry is a list with the calculated semantics. Each entry in the list is a list itself and refers to one semantics. The semantics are stored in the following order: `[[admissible],[complete],[preferred],[ground]]`. Each valid interpretation is notated as a dictionary with the node as key and the interpretation value. In order to better distinguish between these two styles of calculations `false, undefined, true` are notated in the two-valued completion style as `False, u, True` and in the three-valued logic style as `0.0, 0.5, 1.0`.
Note that the two-valued completion approach has a stop condition implemented. If for a node $s \in S$  we obtain two two-valued completions $v_1$ and $v_2$, which are evaluating $s$ to true, respectively false, then the value of $s$ in $\Gamma(v)$ is undefined. Therefore the checking of the remaining two-valued completions is stopped. 
 
## Python Overview
Just run `python [filename]` for executing one of the following scripts:

- `BipolarADFGeneratorGen.py` -- Generates a specified number of ADF instances (if run without modifications it creates 100 instances for each Argument Size between $1$ and $8$.
- `BipolarADFCalc.py` -- Calculates semantics for the generated models. If run as default admissible, complete and preferred semantics are calculated for the argument size between $1$ to $8$.
- `BipolarAnalyze.py` -- Analyze the previously generated files. If run as default the results of admissible and complete are compared.
- `ADFfinal.py` -- Calculates an ADF Instance. 

If you want to reproduce the calculation on your machine, move the evaluated  *EvalBipolarTest* files from the *BipolarNodes_* directories into another directory and run "BipolarADFCalc.py" for the calculation of the generated test instances. After that running, `BipolarAnalyze.py` will output a summary,  of the obtained results.
The `ADFfinal.py` file can also be used directly in order to calculate models. Therefore the variables `nodes` and `chooseinterpretations` have to be edited. Examples can be found in the file itself. The syntax for the `nodes` variable corresponds to the notation of *BipolarTest* files above. Therefore a list of lists has to be entered. In each sublist, the first entry is the node name entered as a string, which can range from "a" to "z". The second entry is the acceptance condition which also has to be entered as a string. The `chooseinterpretations` variable is a list of strings specifying the desired semantics. One or more of the following strings can be included: *'a'*, *'c'*,*'p'*,*'g'*, which refer to admissible, complete preferred, and ground semantics. In addition, the string *'tri'* can also be added to signal that three-valued logic should be used. Otherwise, the two-valued approach is chosen. 

## Model Generation
The acceptance condition for a statement $s \in S$ is generated as follows. First, we randomly decide which arguments from $S$ shall occur in the acceptance condition of $s$. After that, the polarity of the selected statements is decided. Let's denote the set of used positive or negative statements as $L$, short for literals. After that, a disjunction of conjunctions is created. The number of disjuncts in the acceptance condition can vary randomly from one to the statement size $|S|$. A disjunct is created by taking a random selection from $L$ and combining them with logical conjunction. The number of literals in a disjunct can vary from one to the number of elements in $L$. This procedure is repeated for all arguments in order to create a model. The probabilities at the involved decision steps were set to be equally probable. If the node generation process results in $|L| = 0$, either verum or falsum is used. For each argument size between $1$ and $8$, we created 100 test instances. For ADFs with more than $3$ arguments, it was checked that no duplicates exist among the instances.

## Time Measurement
We used a Python measure script with the `timeit` module for measuring calculation time. Each calculation was measured 10 times in a row, which was set with the corresponding `number` parameter. The measurement is done in `BipolarADFCalc.py` which calls `ADFfinal.py` solver as a module.

## Hardware / Software 
Models were calculated in Python Version 3.10.5  We used a laptop with Arch Linux as operating system, an Intel i7-1165G7 CPU, and 16 GB of RAM
