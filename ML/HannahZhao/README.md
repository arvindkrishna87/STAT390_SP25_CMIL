# Week 5 - Hannah

## What I did this week

1. Presentation on Multiple Instance Learning
2. research next steps for final project (slice level MIL)

# Week 4 - Hannah

## What I did this week

1. trained and tested densenet MIL model with a filtered folder of the updated patches, incorporating David's image size recommendations (week4_MIL_updated_patches_h&e)
2. research, trained, and tested a first version of FocusMIL as an attempt to address the incorrect focus MIL had on less informative patches -- at this stage, don't think it's worth to pursue since it's a very different approach and doesn't show promising results to continue exploring. (week4_MIL_exploration)
3. trained and tested densenet MIL model using the densenet patch classifier trained on Krish's curated list of cases (week4_MIL_goodpatchesdensenet)
4. 5_21_Presentation slides 12 & 13 for in class presentation

### Conclusions

To address the issue where the attention weights are focusing on blurred, zoomed-in, uninformative patches, I filtered out patches that are smaller than 60 pixels, per David's analysis and recommendation. After that, we could see that the case level prediction improved, but only due to the fact that it's now predicting high grade for every case. So, filtering helped MIL detect high-grade more confidently. Also, after visualizing the highest and lowest attention weight patches for the two models, it's clear that, while the model is still focusing on more zoomed in and less informative/dense patches, it's still assigning higher weights to patches a lot more clear and informative than last week's model.

This week, I also implemented FocusMIL, which is a model architecture that addresses the limitation of MIL which is the overfitting on wrong correlations/patterns, rather than truly informative features at the patch level. FocusMIL does this by using Max-pooling aggregation and also variational encoding (which is usig a distribution rather than fixed embeddings for patch level features). In my implementation, at the case level, the model achieved 67% accuracy but does not correctly classify any benign cases. This suggests that FocusMIL can identify high-gradeCMIL, however, it doesn't classify benign cases at the case level. I also visualized the patches that FocusMIL scored the highest for, and this showed that, in contrast to the earlier MIL models that focused on visually more uninformative and blurred regions, FocusMIL consistely focused on patches with more clear cell structures and dense patterns. However, despite the promising qualitative improvements in patch focus, the case-level performance of FocusMIL is still too unbalanced to justify continued exploration in this project cycle given that it's a very different approach to MIL that would need more understanding and research.

In order to investigate whether or not improved patch embeddings might lead to better MIL performance, I trained a MIL model using the densenet patch classifier that had previously achieved 73% accuracy when trained on Krish's manually curated list of cases. This indeed helped, and showed that the model was now placing higher attention weights on clear, cell rich aptches rather than the blurry, uninformative ones. However despite this improvement, the case-level accuracy is still poor.

I think the next steps for MIL would be to improve the attention mechanism. After visualizing the attention for the patches, we could also observe that the weights are all very low (~0.2) across the top 5 patches, which shows that attention may not be able to capture enough complexity enough to determine truly which patches are more important than others.

# Week 3 - Hannah

## What I did this week

1. trained and tested last week's densenet and MIL models on new patches
2. visualized attention weights on the 5 highest weighted patches and 5 lowest weighted patches of each case

### Conclusions

My densenet model and MIL models did not outperform last week's models (which were trained on the curated list of cases that Krish gave us). Furthermore, Just from seeing the patches that MIL is assigning more weight to, I think the issue was that I’m feeding the MIL model all patches from each case without any filtering in the new patches folder. Due to the limited time we had for this week's presentation, I was only able to download the new patches folder and run 2 models. So moving forward, I’m going to go back and filter the patches from the raw patches folder based on David’s size analysis, and also I plan to branch out from the densenet classifier for my patch level training, and use faster, more high accuracy ones like resnet or even just David’s CNN, just because densenet is slow at running and it isn’t giving a very significant boost in accuracy.

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

The biggest breakthrough was when I manually curated the training, validation, and test splits to address class imbalance at the patch level and not just at the case level. Specifically I balanced the training set to include 108 patches each for Benign and High grade cases. I also carefully selected my validation cases so that each class had between 180-200 patches. Then, I tested on the remaining high-quality cases given to us by Krish. These changes led to great improvement.

My model shows clear learning progress across the epochs, which I visualized by plotting confusion matrices per epoch on the validation set, and the final results confirms this. In the end, for my last densenet model that I trained on 10 epochs, I got a true 75% accuracy, which is not the result of biased predictions toward a majority class. Of course, this still isn't a very good accuracy, but more exploration and adjustments can still be made.

### 5. Next Steps

Now that I've validated that the patch-level backbone can learn meaningful patterns, I aim to reintegrate this into my MIL model by injecting the pretrained DenseNet features into my MIL model for bag(case)-level classification. Eventually I aim to build back up to the TransMIL architecture, where we could use attention and learn inter-patch relatinoships.

# Week 1 - Hannah

## What I did this week

1. Explored/Researched other ways to resize/unify the patch images without losing information.
2. Current class imbalance issue & research on best balance.
3. Address the issue of training on noisy labels (i.e. labels that are labeled high grade but not actually showing relevant information) -- first attempt at Multiple Instance Learning

## 1. Padding, no more centercrop

### Problem & Solution

Currently, preprocessing is applying resize and centercrop, which may lead to loss of information and low resolution of images. To prevent this from influencing the quality of our data, we can address this through padding and then resizing. This preserves as much of the image data as possible while also unifying all the patch data.

## 2. Class Imbalance

### Problem & More Exploration Needed

One of the reasons why our models is having a hard time predicting benign cases could be that we are training with 9070 patches, 6750 of which were high-grade patches. Because we are training our model on every patch with its case-level label, this could be negatively impacting our results.

However, once I started looking at the numbers based on cases, I noticed that the imbalance was not as bad: 17 benign cases to 23 high-grade cases. The point of confusion is that, in the class labels for all our cases worksheet, there are 28 benign cases and 37 high-grade cases. Even if we end up doing patching for all of them, there is still a bit of an imbalance. Our goal would be to have a 1:1 class balance to train on for the best results.

Another point of concern is that our preprocessing includes filtering out patches that are too small or ones that don't contain relevant information. It is also a possibility that there are a greater number of benign patches that are being filtered out than high-grade patches.

I think it would be best if we completed the patching for all benign cases that we have available to us so that we could include it in our models. We should also consider training by cases instead of patches -- which leads me into my first attempt at Multiple Instance Learning.

## 3. Multiple Instance Learning

### Progress

I suspect that there are lots of noisy labels, or labels that may be labeled as high grade or benign but not actually showing relevant information that can help our model learn. So, instead of forcing our model to predict benign vs. high grade on every patch (many of which might not show malignant features even in a high grade case), we could treat each case as a bag of patches. Then, with Max-pooling MIL, if any patch scores "high grade" strongly, the whole bag becomes "high-grade".

I've attempted this on a densenet model using only H&E stained patches. However, the model still struggles to learn benign cases. This may be due to class imbalance (training on 23 high-grade cases out of 31 total cases), or it may be due to the Max-pooling MIL pooling I'm using.

Next week, I plan to train MIL for the two other stainings. I will also explore other types of bag-level pooling such as Mean pooling (taking the average for the bag prediction) or attention-based MIL (which assigns weights).
