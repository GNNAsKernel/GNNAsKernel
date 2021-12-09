# GNNAsKernel
Official code for **From Stars to Subgraphs: Uplifting Any GNN with Local Structure Awareness**

On the way to release! 

## Visualizations
### GNN-AK(+)
![GNN-AK](./figs/GNN-AK.png)
### GNN-AK(+) with SubgraphDrop 
![GNN-AK-S: GNN-AK with SubgraphDrop](./figs/GNN-AK-S.png)

## Results 
### GNN-AK boosts expressiveness
![GNN-AK boosts expressiveness](./figs/simulation.png)
### GNN-AK boosts practical performance
![GNN-AK boosts practical performance](./figs/real-world.png)


## Setup 

```
# params
# 10/6/2021, newest packages. 
ENV=gnn_ak
CUDA=11.1
TORCH=1.9.1
PYG=2.0.1

# create env 
conda create --name $ENV python=3.9 -y
conda activate $ENV

# install pytorch 
conda install pytorch=$TORCH torchvision torchaudio cudatoolkit=$cuda -c pytorch -c nvidia -y

# install pyg2.0
conda install pyg=$PYG -c pyg -c conda-forge -y

# install ogb 
pip install ogb

# install rdkit
conda install -c conda-forge rdkit -y

# update yacs and tensorboard
pip install yacs==0.1.8 --force  # PyG currently use 0.1.6 which doesn't support None argument. 
pip install tensorboard
pip install matplotlib

```

## Hyperparameters 

See ``core/config.py`` for all options. 

## Run normal GNNs 

See ``core/model_utls/pyg_gnn_wrapper.py`` for more options.   

Custom new GNN convolutional layer 'X' can be plugged in ``core/model_utls/pyg_gnn_wrapper.py``, and use 'X' as model.gnn_type option. 

```
# Run different normal GNNs 
python -m train.zinc model.mini_layers 0 model.gnn_type GINEConv
python -m train.zinc model.mini_layers 0 model.gnn_type SimplifiedPNAConv
python -m train.zinc model.mini_layers 0 model.gnn_type GCNConv
python -m train.zinc model.mini_layers 0 model.gnn_type GATConv
python -m train.zinc model.mini_layers 0 model.gnn_type ...

python -m train.zinc model.num_layers 6 model.mini_layers 0 model.gnn_type GCNConv # 6-layer GCN
```
## Run different datasets
See all available datasets under train folder.

``` 
# Run different datasets
python -m train.zinc 
python -m train.cifar10 
python -m train.counting 
python -m train.graph_property 
python -m ...
```

## Run GNN-AK 

Fully theoretically explained by Subgraph-1-WL*. 

Use:    ``model.mini_layers 1 (or >1) model.embs "(0,1)" model.hops_dim 0 ``

```
python -m train.zinc model.mini_layers 1 model.gnn_type GINEConv model.embs "(0,1)" model.hops_dim 0  
```

## Run GNN-AK+

At least as powerful as GNN-AK (or more powerful).

Use:    ``model.mini_layers 1 (or >1) model.embs "(0,1,2)" model.hops_dim 16 ``   
These are set as default. See ``core/config.py``.

```
# Run GNN-AK+ with different normal GNNs
python -m train.zinc model.mini_layers 1 model.gnn_type GINEConv            # 1-layer base model
python -m train.zinc model.mini_layers 1 model.gnn_type SimplifiedPNAConv   # 1-layer base model
python -m train.zinc model.mini_layers 2 model.gnn_type GINEConv            # 2-layer base model
python -m train.zinc model.mini_layers 2 model.gnn_type SimplifiedPNAConv   # 2-layer base model
```
## Run with different number of GNN-AK(+) iterations 
Changing number of outer layers.
```
python -m train.zinc model.num_layers 4 
python -m train.zinc model.num_layers 6 
python -m train.zinc model.num_layers 8 
```

## Run with different subgraph patterns 
See ``core/transform.py`` for detailed implementation.

```
python -m train.zinc subgraph.hops 2      # 2-hop egonet
python -m train.zinc subgraph.hops 3      # 3-hop egonet

# Run with random-walk subgraphs based on node2vec 
python -m train.zinc subgraph.hops 0 subgraph.walk_length 10 subgraph.walk_p 1.0 subgraph.walk_q 1.0  
```


# Cite
Please cite our work if you use our code!

```
@inproceedings{
anonymous2022from,
title={From Stars to Subgraphs: Uplifting Any {GNN} with Local Structure Awareness},
author={Anonymous},
booktitle={Submitted to The Tenth International Conference on Learning Representations },
year={2022},
url={https://openreview.net/forum?id=Mspk_WYKoEH},
note={under review}
}
```


<!--
**GNNAsKernel/GNNAsKernel** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->
