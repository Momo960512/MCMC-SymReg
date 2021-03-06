# MCMC-SymReg
Bayesian symbolic regression using mcmc sampling. 

Paper here: https://arxiv.org/abs/1910.08892

## API and Usage

### Codes location

`codes/BSR.py`: API interface of BSR class

`codes/bsr_class.py`: definition of BSR class

`codes/simulations.py`: part of simulation settings in the paper

`codes/funcs.py`: basic sampling functions

### Usage Example

```python
K = 3 # number of trees
MM = 50 # number of iterations
# set hyperparameters alternatively
hyper_params = [{'treeNum': 3, 'itrNum':50, 'alpha1':0.4, 'alpha2':0.4, 'beta':-1}]
# initialize BSR object
my_bsr = BSR(K,MM)
# train (need to fill in parameters)
# train_X is dataframe with each row a datapoint
# train_y is series with default index
my_bsr.fit(train_X,train_y)
# fit new values
# new_X is dataframe of new data
fitted_y = my_bsr.predict(new_X)
# display fitted trees
my_bsr.model()
# complexity, including complexity of each tree & total
complexity = my_bsr.complexity()
```

>>>>>>> 

## Pdf files

`bsr_paper.pdf`: paper for Bayesian Symbolic Regression

`Symbolic_Regression_Tree_MCMC.pdf`: note for proposed algorithm

## Archived Versions

### archive/demo.py
The first version, implementing the mcmc method for symbolic regression.

### archive/demo2.py
In the ReassignOperator transition, unary and binary operators are able to change to each other.

### archive/demo3.py
Let the input operators be stored in a list and can assign weight to the specified operators.

The probability of Grow is $\frac{1-p_0}{2}\cdot \min \{ 1,\frac{5}{N+d+2} \}$.

### archive/demo4.py
Add new actions, transform and detransform.

#### Trans: 
Uniformly pick a node and add a unary nonlinear node between it and its parent.
#### Detrans: 
Uniformly pick a unary nonlinear node and substitute it with its child.
#### Proposal:
P_grow = (1-p_0)/3 * min {1,5/(N_nt+2)}, N_nt is the number of non-terminal nodes

P_prune = (1-p_0)/3 - P_grow

P_detrans = (1-p_0)/3 * (n1 / 2+ n1), n1 is the number of unary nonlinear nodes

P_trans = (1-p_0)/ [3*(N+5)], N is the number of all nodes

P_reop = P_refeat = (1-P_grow-P_prune-P_detrans-P_trans)/2

### archive/demo5.py
Modify the definition of transform and detransform.

#### Insert:
Uniformly take a candidate node and place some operator as its parent
#### Delete:
Uniformly take a candidate node and delete it; the candidate node should be non-terminal, and should not be the root if its child nodes are all terminal.

if it has two child nodes, randomly preserve one child (need to be non-terminal).
#### Proposal:
P_grow = (1-p_0)/3 * min {1,4/(N_nt+2)}, N_nt is the number of non-terminal nodes

P_prune = (1-p_0)/3 - P_grow

P_delete = (1-p_0)/3 * (Nc / Nc+3), Nc is the number of candidates for deletion.

P_insert = (1-p_0)/ 3 - P_delete; The probs added nodes should be proportional to the preset weights.

P_reop = P_refeat = (1-p_0)/ 6

consider the proportions of (p_grow+p_prune), (p_delete+p_insert), (p_reop+p_refeat)
