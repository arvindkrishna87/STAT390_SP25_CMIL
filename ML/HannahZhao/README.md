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
