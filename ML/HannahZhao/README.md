# Week 2 - Hannah

## What I did this week

1. Implemented Adaptive Pooling & Attention to my MIL model
2. Trained and Tested on Reduced number of cases on MIL and then a patch classifier densenet model

## This week's progress summary

### 1. Started with a TransMIL model

I initially implemented a full transformer-based MIL architecture with a DenseNet backbone. A transformer-based MIL is basically an extension of the traditional MIL through using self-attention layers in order to model interactions between the patches within a bag (or case). So, instead of assuming that each patch is independent, the model can learn relationships between patches, which is extremely helpful in suppressing noisy patches and capturing global patterns across the entire case. The TransMIL I've constructed includes a DenseNet backbone to extract the patch features, a transformer encoder (which handles the self-attention), and attention layers to condense the patch representations into a bag-level prediction.

I chose to use TransMIL because some patches may show benign elements while others high-grade, so context would matter. TransMIL can capture dependences between patches and avoids treating each patch in isolation.

However, I was training and testing on a very small number of cases and my kernel was frequently crashing. Since, the results weren't good either I decided that I needed to debug and isolate the issue before trying this complex model.

### 2. Simplified to a standard MIL (with attention)

I removed the transformer layers and just built a simple attention-based MIL model. I wanted to see whether or not the MIL model can even learn the bag level predictions on a small number of patches, but I realized that I may have to simplify it down more to get to the core of the issue.

### 3. Further reduced to a Patch Classifier

As you can see in my simpleMIL_H&E_base copy.ipynb, I tried to further reduce and compare a patch classifier (just the DenseNet classifier) with the MIL model, and the patch classifier model was predicting everything pretty much as high-grade. I realized that before I worry about MIL and TransMIL I need to focus on just the patch level by training a Densenet classifier directly on individual patches to confirm if the DenseNet (which is the backbone of the MIL and transMIL) is even capable of learning patterns in the data.

### 4. Isolated the issue by training Densenet Classifier

As you can see in the simpledensenet_H&E.ipynb and the simpledensenet_H&E_moreepochs.ipynb, I was finally able to get a good performing model that is actually learning the patterns. After lots of struggling with MIL and TransMIL and then with densenet, I realized that the issue wasn't in the model archetecture itself necessarily -- it was in my data splits.

The biggest breakthrough was when I manually curated the training, validation, and test splits to address class imbalance at the patch level and not just at the case level. Specifically I balanced the training set to include 108 patches each for Benign and High grade cases. I also carefully selected my validation cases so that each class had between 180-200 patches. Then, I tested on the re