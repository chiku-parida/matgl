---
layout: default
title: matgl.layers.md
nav_exclude: true
---
# matgl.layers package

This package implements the layers for M\*GNet.


## matgl.layers._activations module

Custom activation functions.


### _class_ matgl.layers._activations.SoftExponential(alpha: float | None = None)
Bases: `Module`

Soft exponential activation.
When x < 0, SoftExponential(x,alpha) = -log(1-alpha(x+alpha))/alpha
When x = 0, SoftExponential(x,alpha) = 0
When x > 0, SoftExponential(x,alpha) = (exp(alpha\*x)-1)/alpha + alpha.

References: [https://arxiv.org/pdf/1602.01321.pdf](https://arxiv.org/pdf/1602.01321.pdf)

Init SoftExponential with alpha value.


* **Parameters**

    **alpha** (*float*) – adjustable Torch parameter during the training.



#### forward(x: tensor)
Evaluate activation function given the input tensor x.


* **Parameters**

    **x** (*torch.tensor*) – Input tensor



* **Returns**

    Output tensor



* **Return type**

    out (torch.tensor)



#### training(_: boo_ )

### _class_ matgl.layers._activations.SoftPlus2()
Bases: `Module`

SoftPlus2 activation function:
out = log(exp(x)+1) - log(2)
softplus function that is 0 at x=0, the implementation aims at avoiding overflow.

Initializes the SoftPlus2 class.


#### forward(x: tensor)
Evaluate activation function given the input tensor x.


* **Parameters**

    **x** (*torch.tensor*) – Input tensor



* **Returns**

    Output tensor



* **Return type**

    out (torch.tensor)



#### training(_: boo_ )
## matgl.layers._atom_ref module

Atomic energy offset. Used for predicting extensive properties.


### _class_ matgl.layers._atom_ref.AtomRef(property_offset: array)
Bases: `Module`

Get total property offset for a system.

### Args:

property_offset (np.array): a array of elemental property offset.


#### fit(structs_or_graphs: list, element_list: tuple[str], properties: np.typing.NDArray)
Fit the elemental reference values for the properties.


* **Parameters**

    
    * **structs_or_graphs** – pymatgen Structures or dgl graphs


    * **element_list** (*tuple*) – a list of element types


    * **properties** (*np.ndarray*) – array of extensive properties



#### forward(g: dgl.DGLGraph, state_attr: torch.tensor | None = None)
Get the total property offset for a system.

Args:
g: a batch of dgl graphs
state_attr: state attributes

Returns:
offset_per_graph:


#### get_feature_matrix(structs_or_graphs: list, element_list: tuple[str])
Get the number of atoms for different elements in the structure.


* **Parameters**

    
    * **structs_or_graphs** (*list*) – a list of pymatgen Structure or dgl graph


    * **element_list** – a dictionary containing element types in the training set



* **Returns**

    a matrix (num_structures, num_elements)



* **Return type**

    features (np.array)



#### training(_: boo_ )
## matgl.layers._bond module

Generate bond features based on spherical bessel functions or gaussian expansion.


### _class_ matgl.layers._bond.BondExpansion(max_l: int = 3, max_n: int = 3, cutoff: float = 5.0, rbf_type: str = 'SphericalBessel', smooth: bool = False, initial: float = 0.0, final: float = 5.0, num_centers: int = 100, width: float = 0.5)
Bases: `Module`

Expand pair distances into a set of spherical bessel or gaussian functions.

Args:
max_l (int): order of angular part
max_n (int): order of radial part
cutoff (float): cutoff radius
rbf_type (str): type of radial basis function .i.e. either “SphericalBessel” or ‘Gaussian’
smooth (bool): whether apply the smooth version of spherical bessel functions or not
initial (float): initial point for gaussian expansion
final (float): final point for gaussian expansion
num_centers (int): Number of centers for gaussian expansion.
width (float): width of gaussian function.


#### forward(bond_dist: tensor)
Forward.

Args:
bond_dist: Bond distance

Return:
bond_basis: Radial basis functions


#### training(_: boo_ )
## matgl.layers._core module

Implementations of multi-layer perceptron (MLP) and other helper classes.


### _class_ matgl.layers._core.EdgeSet2Set(input_dim: int, n_iters: int, n_layers: int)
Bases: `Module`

Implementation of Set2Set.


* **Parameters**

    
    * **input_dim** – The size of each input sample.


    * **n_iters** – The number of iterations.


    * **n_layers** – The number of recurrent layers.



#### forward(g: DGLGraph, feat: tensor)
Defines the computation performed at every call.


* **Parameters**

    
    * **g** – Input graph


    * **feat** – Input features.



* **Returns**

    One hot vector



#### reset_parameters()
Reinitialize learnable parameters.


#### training(_: boo_ )

### _class_ matgl.layers._core.GatedMLP(in_feats: int, dims: list[int], activate_last: bool = True, use_bias: bool = True)
Bases: `Module`

An implementation of a Gated multi-layer perceptron.


* **Parameters**

    
    * **in_feats** – Dimension of input features.


    * **dims** – Architecture of neural networks.


    * **activate_last** – Whether applying activation to last layer or not.


    * **bias_last** – Whether applying bias to last layer or not.



#### forward(inputs: tensor)
Defines the computation performed at every call.

Should be overridden by all subclasses.

**NOTE**: Although the recipe for forward pass needs to be defined within
this function, one should call the `Module` instance afterwards
instead of this since the former takes care of running the
registered hooks while the latter silently ignores them.


#### training(_: boo_ )

### _class_ matgl.layers._core.MLP(dims: list[int], activation: Callable[[torch.Tensor], torch.Tensor] | None = None, activate_last: bool = False, bias_last: bool = True)
Bases: `Module`

An implementation of a multi-layer perceptron.


* **Parameters**

    
    * **dims** – Dimensions of each layer of MLP.


    * **activation** – Activation function.


    * **activate_last** – Whether to apply activation to last layer.


    * **bias_last** – Whether to apply bias to last layer.



#### _property_ depth(_: in_ )
Returns depth of MLP.


#### forward(inputs)
Applies all layers in turn.


* **Parameters**

    **inputs** – Input tensor



* **Returns**

    Output tensor



#### _property_ in_features(_: in_ )
Return input features of MLP.


#### _property_ last_linear(_: Linear | Non_ )
The last linear layer.


* **Type**

    return



#### _property_ out_features(_: in_ )
Returns output features of MLP.


#### training(_: boo_ )
## matgl.layers._embedding module

Embedding node, edge and optional state attributes.


### _class_ matgl.layers._embedding.EmbeddingBlock(degree_rbf: int, activation: nn.Module, dim_node_embedding: int, dim_edge_embedding: int | None = None, dim_state_feats: int | None = None, ntypes_node: int | None = None, include_state: bool = False, ntypes_state: int | None = None, dim_state_embedding: int | None = None)
Bases: `Module`

Embedding block for generating node, bond and state features.

Args:
degree_rbf (int): number of rbf
activation (nn.Module): activation type
dim_node_embedding (int): dimensionality of node features
dim_edge_embedding (int): dimensionality of edge features
dim_state_feats: dimensionality of state features
ntypes_node: number of node labels
include_state: Whether to include state embedding
ntypes_state: number of state labels
dim_state_embedding: dimensionality of state embedding.


#### forward(node_attr, edge_attr, state_attr)
Output embedded features.

Args:
node_attr: node attribute
edge_attr: edge attribute
state_attr: state attribute

Returns:
node_feat: embedded node features
edge_feat: embedded edge features
state_feat: embedded state features


#### training(_: boo_ )
## matgl.layers._graph_convolution module

Graph convolution layer (GCL) implementations.


### _class_ matgl.layers._graph_convolution.M3GNetBlock(degree: int, activation: Module, conv_hiddens: list[int], num_node_feats: int, num_edge_feats: int, num_state_feats: int | None = None, include_state: bool = False, dropout: float | None = None)
Bases: `Module`

A M3GNet block comprising a sequence of update operations.


* **Parameters**

    
    * **degree** – Dimension of radial basis functions


    * **num_node_feats** – Number of node features


    * **num_edge_feats** – Number of edge features


    * **num_state_feats** – Number of state features


    * **conv_hiddens** – Dimension of hidden layers


    * **activation** – Activation type


    * **include_state** – Including state features or not


    * **dropout** – Probability of an element to be zero in dropout layer



#### forward(graph: DGLGraph, edge_feat: Tensor, node_feat: Tensor, state_feat: Tensor)

* **Parameters**

    
    * **graph** – DGL graph


    * **edge_feat** – Edge features


    * **node_feat** – Node features


    * **state_attr** – State features



* **Returns**

    A tuple of updated features



#### training(_: boo_ )

### _class_ matgl.layers._graph_convolution.M3GNetGraphConv(include_states: bool, edge_update_func: Module, edge_weight_func: Module, node_update_func: Module, node_weight_func: Module, state_update_func: Module | None)
Bases: `Module`

A M3GNet graph convolution layer in DGL.

Parameters:
include_state (bool): Whether including state
edge_update_func (Module): Update function for edges (Eq. 4)
edge_weight_func (Module): Weight function for radial basis functions (Eq. 4)
node_update_func (Module): Update function for nodes (Eq. 5)
node_weight_func (Module): Weight function for radial basis functions (Eq. 5)
attr_update_func (Module): Update function for state feats (Eq. 6).


#### edge_update_(graph: DGLGraph)
Perform edge update.

Args:
graph: DGL graph

Returns:
edge_update: edge features update


#### forward(graph: DGLGraph, edge_feat: Tensor, node_feat: Tensor, state_attr: Tensor)
Perform sequence of edge->node->states updates.


* **Parameters**

    
    * **graph** – Input graph


    * **edge_feat** – Edge features


    * **node_feat** – Node features


    * **state_attr** – Graph attributes (global state)



* **Returns**

    (edge features, node features, graph attributes)



#### _static_ from_dims(degree, include_states, edge_dims: list[int], node_dims: list[int], state_dims: list[int] | None, activation: Module)
M3GNetGraphConv initialization.


* **Parameters**

    
    * **degree** (*int*) – max_n\*max_l


    * **include_states** (*bool*) – whether including state or not


    * **edge_dims** (*list*) – NN architecture for edge update function


    * **node_dims** (*list*) – NN architecture for node update function


    * **state_dims** (*list*) – NN architecture for state update function


    * **activation** (*nn.Nodule*) – activation function


Returns:
M3GNetGraphConv (class)


#### node_update_(graph: DGLGraph, state_attr: Tensor)
Perform node update.


* **Parameters**

    
    * **graph** – DGL graph


    * **state_attr** – State attributes



* **Returns**

    node features update



* **Return type**

    node_update



#### state_update_(graph: DGLGraph, state_attrs: Tensor)
Perform attribute (global state) update.


* **Parameters**

    
    * **graph** – DGL graph


    * **state_attrs** – graph features


Returns:
state_update: state_features update


#### training(_: boo_ )

### _class_ matgl.layers._graph_convolution.MEGNetBlock(dims: list[int], conv_hiddens: list[int], act: Module, dropout: float | None = None, skip: bool = True)
Bases: `Module`

A MEGNet block comprising a sequence of update operations.

Init the MEGNet block with key parameters.


* **Parameters**

    
    * **dims** – Dimension of dense layers before graph convolution.


    * **conv_hiddens** – Architecture of hidden layers of graph convolution.


    * **act** – Activation type.


    * **dropout** – Randomly zeroes some elements in the input tensor with given probability (0 < x < 1) according
    to a Bernoulli distribution.


    * **skip** – Residual block.



#### forward(graph: DGLGraph, edge_feat: Tensor, node_feat: Tensor, state_attr: Tensor)
MEGNetBlock forward pass.


* **Parameters**

    
    * **graph** (*dgl.DGLGraph*) – A DGLGraph.


    * **edge_feat** (*Tensor*) – Edge features.


    * **node_feat** (*Tensor*) – Node features.


    * **state_attr** (*Tensor*) – Graph attributes (global state).



* **Returns**

    Updated (edge features,

        node features, graph attributes)




* **Return type**

    tuple[Tensor, Tensor, Tensor]



#### training(_: boo_ )

### _class_ matgl.layers._graph_convolution.MEGNetGraphConv(edge_func: Module, node_func: Module, state_func: Module)
Bases: `Module`

A MEGNet graph convolution layer in DGL.


* **Parameters**

    
    * **edge_func** – Edge update function.


    * **node_func** – Node update function.


    * **state_func** – Global state update function.



#### edge_update_(graph: DGLGraph)
Perform edge update.


* **Parameters**

    **graph** – Input graph



* **Returns**

    Output tensor for edges.



#### forward(graph: DGLGraph, edge_feat: Tensor, node_feat: Tensor, state_attr: Tensor)
Perform sequence of edge->node->attribute updates.


* **Parameters**

    
    * **graph** – Input graph


    * **edge_feat** – Edge features


    * **node_feat** – Node features


    * **state_attr** – Graph attributes (global state)



* **Returns**

    (edge features, node features, graph attributes)



#### _static_ from_dims(edge_dims: list[int], node_dims: list[int], state_dims: list[int], activation: Module)
Create a MEGNet graph convolution layer from dimensions.


* **Parameters**

    
    * **edge_dims** (*list**[**int**]*) – Edge dimensions.


    * **node_dims** (*list**[**int**]*) – Node dimensions.


    * **state_dims** (*list**[**int**]*) – State dimensions.


    * **activation** (*Module*) – Activation function.



* **Returns**

    MEGNet graph convolution layer.



* **Return type**

    MEGNetGraphConv



#### node_update_(graph: DGLGraph)
Perform node update.


* **Parameters**

    **graph** – Input graph



* **Returns**

    Output tensor for nodes.



#### state_update_(graph: DGLGraph, state_attrs: Tensor)
Perform attribute (global state) update.


* **Parameters**

    
    * **graph** – Input graph


    * **state_attrs** – Input attributes



* **Returns**

    Output tensor for attributes



#### training(_: boo_ )
## matgl.layers._readout module

Readout layer for M3GNet.


### _class_ matgl.layers._readout.ReduceReadOut(op: str = 'mean', field: str = 'node_feat')
Bases: `Module`

Reduce atom or bond attributes into lower dimensional tensors as readout.
This could be summing up the atoms or bonds, or taking the mean, etc.

Args:
op (str): op for the reduction
field (str): Field of graph to perform the reduction.


#### forward(g: DGLGraph)

* **Parameters**

    **g** – DGL graph



* **Returns**

    torch.tensor.



#### training(_: boo_ )

### _class_ matgl.layers._readout.Set2SetReadOut(num_steps: int, num_layers: int, field: str)
Bases: `Module`

The Set2Set readout function.

Args:
num_steps (int): Number of LSTM steps
num_layers (int): Number of layers.
field (str): Field of graph to perform the readout.


#### forward(g: DGLGraph)
Defines the computation performed at every call.

Should be overridden by all subclasses.

**NOTE**: Although the recipe for forward pass needs to be defined within
this function, one should call the `Module` instance afterwards
instead of this since the former takes care of running the
registered hooks while the latter silently ignores them.


#### training(_: boo_ )

### _class_ matgl.layers._readout.WeightedReadOut(in_feats: int, dims: list[int], num_targets: int)
Bases: `Module`

Feed node features into Gated MLP as readout.

Args:
in_feats: input features (nodes)
dims: NN architecture for Gated MLP
num_targets: number of target properties.


#### forward(g: DGLGraph)

* **Parameters**

    **g** – DGL graph



* **Returns**

    torch.tensor.



* **Return type**

    atomic_properties



#### training(_: boo_ )

### _class_ matgl.layers._readout.WeightedReadOutPair(in_feats, dims, num_targets, activation=None)
Bases: `Module`

Feed the average of atomic features i and j into weighted readout layer.

Initializes internal Module state, shared by both nn.Module and ScriptModule.


#### forward(g: DGLGraph)
Defines the computation performed at every call.

Should be overridden by all subclasses.

**NOTE**: Although the recipe for forward pass needs to be defined within
this function, one should call the `Module` instance afterwards
instead of this since the former takes care of running the
registered hooks while the latter silently ignores them.


#### training(_: boo_ )
## matgl.layers._three_body module

Three-Body interaction implementations.


### _class_ matgl.layers._three_body.SphericalBesselWithHarmonics(max_n: int, max_l: int, cutoff: float, use_smooth: bool, use_phi: bool)
Bases: `Module`

Expansion of basis using Spherical Bessel and Harmonics.

Init SphericalBesselWithHarmonics.


* **Parameters**

    
    * **max_n** – Degree of radial basis functions.


    * **max_l** – Degree of angular basis functions.


    * **cutoff** – Cutoff sphere.


    * **use_smooth** – Whether using smooth version of SBFs or not.


    * **use_phi** – Using phi as angular basis functions.



#### forward(line_graph)
Defines the computation performed at every call.

Should be overridden by all subclasses.

**NOTE**: Although the recipe for forward pass needs to be defined within
this function, one should call the `Module` instance afterwards
instead of this since the former takes care of running the
registered hooks while the latter silently ignores them.


#### training(_: boo_ )

### _class_ matgl.layers._three_body.ThreeBodyInteractions(update_network_atom: Module, update_network_bond: Module, \*\*kwargs)
Bases: `Module`

Include 3D interactions to the bond update.

Init ThreeBodyInteractions.


* **Parameters**

    
    * **update_network_atom** – MLP for node features in Eq.2


    * **update_network_bond** – Gated-MLP for edge features in Eq.3


    * **\*\*kwargs** – Kwargs pass-through to nn.Module.__init__().



#### forward(graph: DGLGraph, line_graph: DGLGraph, three_basis: tensor, three_cutoff: float, node_feat: tensor, edge_feat: tensor)
Forward function for ThreeBodyInteractions


* **Parameters**

    
    * **graph** – dgl graph


    * **line_graph** – line graph.


    * **three_basis** – three body basis expansion


    * **three_cutoff** – cutoff radius


    * **node_feat** – node features


    * **edge_feat** – edge features.



#### training(_: boo_ )