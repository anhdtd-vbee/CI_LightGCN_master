# The code is about IEEE TRANSACTIONS ON NEURAL NETWORKS AND LEARNING SYSTEMS <Causal Incremental Graph Convolution forRecommender System Retraining>.
***This README is the guidience of reproducing CI-LightGCN results. Because the evaluation process of CI-LightGCN is a sequential scenes, if you run the code without any mid products it will cost very large time (for data processing, training at each stage, and testing at each stage). So we also provide mid products at "https://rec.ustc.edu.cn/share/16d25180-595a-11ec-b406-1baff4e05320" for quickly reproducing. Our code can also run without any mid products but with large preparation time.***

# REQUESMENT
1.torch >= 1.7.1

2.scipy >= 1.5.2

3.numpy >= 1.19.1

4.CUDA 11.1

5.tqdm

Or you can use docker to build the environment ```docker pull dsihao/3090_torch171```

# Overall:
___The necessary files must be downloaded at "https://rec.ustc.edu.cn/share/16d25180-595a-11ec-b406-1baff4e05320"___

The following parts are:

A is about how to quickly reproduce the results

B is the introduction of data

C is the introduction of mid products that for quickly reproducing

D is how to retrain the CI-LightGCN from zero with no mid products

# A. Quickly reproduce the results

1. To quickly reproduce the result of CI-LightGCN on Yelp you can use the commend 
```python CI-LightGCN.py  --dataset='finetune_yelp' --model CILightGCN --finetune_epochs 300 --conv2d_reg 1e-2 --decay 1e-4 --icl_k 61 --notactive 1 --A 0.5 --inference_k 11 --radio_loss 0.7 --icl_reg 1```

2. To quickly reproduce the result of CI-LightGCN on Gowalla you can use the commend 
```python CI-LightGCN.py  --dataset='gowalla' --model CILightGCN --finetune_epochs 200 --conv2d_reg 1e-3 --decay 1e-3 --icl_k 58 --notactive 1 --A 0.6 --inference_k 28 --radio_loss 0.02 --icl_reg 0.0005```

# B. The processed data
1. "./data/finetune_yelp" is all data you need of Yelp. Where "./data/finetune_yelp/train" is the data for train, and "./data/finetune_yelp/test" is the data for test. "./data/finetune_yelp/information.npy" is the information of this dataset.

_All data below can generate by our code, we prove them for saving your reproducing time._

2. "./data/finetune_yelp/xxx.npz" is the Adj matrix of each stage, some Adj matrixs correspond to incremental graph are for CI-LightGCN and Fine-tune LightGCN, some Adj matrixs correspond to full graph are for Fullretrain LightGCN.
3. "./data/finetune_yelp/xxx.npy" is the degree information of all nodes at each stage. 
4. "./data/finetune_yelp/rescale_vec" is the degree ratio between two adjacent stages.

__All data files of Gowalla is the same naming logic, change "/finetune_yelp/" to "/gowalla/" you can get the files for gowalla__


# C. The required pre-processing results for quickly reproducing

## a. weights files of static basic LightGCN
1. "./code/checkpoints/static_base_LightGCN_gowalla-3-64-28.npy-.pth.tar" is the __weights of basic LightGCN__ that trained with data of stages [0-30) of Gowalla. It is the input of all GCN-based mehtods such as CI-LightGCN, Full retrain LightGCN, Fine-tune LightGCN, SML-LightGCN-O, SML-LightGCN-E. Or you can train a static LightGCN model based on data of stages [0-30) of Gowalla as the input of all methods.

2. "./code/checkpoints/static_base_LightGCN_finetune_yelp-3-64-28.npy-.pth.tar" is the __weights of basic LightGCN__ that trained with data of stages [0-30) of Yelp. It is the input of all GCN-based mehtods such as CI-LightGCN, Full retrain LightGCN, Fine-tune LightGCN, SML-LightGCN-O, SML-LightGCN-E. Or you can train a static LightGCN model based on data of stages [0-30) of Yelp as the input of all methods.

## b. model weights files of CI-LightGCN(T)
By using these files you can abandon all the training phase of CI-LightGCN for quickly reproducing. These files are generated by CI-LightGCN(T).

1. "./save_for_inference/gowalla/Embeddings_at_stage__t_.pth.tar" is the embeddings of all nodes in stage _t_, and "./save_for_inference/gowalla/Weights-3-64-_t_-npy.pth.tar" is the model weights in stage _t_ of Gowalla.

2. "./save_for_inference/finetune_yelp/Embeddings_at_stage__t_.pth.tar" is the embeddings of all nodes in stage _t_, and "./save_for_inference/finetune_yelp/Weights-3-64-_t_-npy.pth.tar" is the model weights in stage _t_ of Yelp.


# D. Reproducing from zero

1. To reproduce the result of CI-LightGCN on Yelp from zero you can use the commend 
```python CI-LightGCN_from_zero.py  --dataset='finetune_yelp' --model CILightGCN --finetune_epochs 300 --conv2d_reg 1e-2 --decay 1e-4 --icl_k 61 --notactive 1 --A 0.5 --inference_k 11 --radio_loss 0.7 --icl_reg 1```

2. To reproduce the result of CI-LightGCN on Gowalla from zero you can use the commend 
```python CI-LightGCN_from_zero.py  --dataset='gowalla' --model CILightGCN --finetune_epochs 200 --conv2d_reg 1e-3 --decay 1e-3 --icl_k 58 --notactive 1 --A 0.6 --inference_k 28 --radio_loss 0.02 --icl_reg 0.0005```
