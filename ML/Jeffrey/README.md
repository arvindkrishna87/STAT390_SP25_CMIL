# Week 2 - Jeffrey

## What I did this week

1. Attempted training 3 seperate models on the 3 different set of patches and ensembleing them together

2. Researched and implemented an attention-based CNN (SE-ResNet)

## This week's progress summary

### 1. Implemented the same resnet model with 3 different sets of patches

I trained 3 of the same Resnet50 models on the 3 different sets of patches (H&E, Melan, and SOX10) and then ensemble them together. I used the same resnet model for all 3 sets of patches. I only trained the final classifier on the full set of patches. The results were not very as expected. The H&E model was the best performing model, but the other two models were not very good. I think this is because the patches are not very informative as they don't stain as much of the cells. The issue of predicting high-grade patches is not present in a few of the models but not all of them.

### 2. Stacked the 3 models together

I stacked the 3 models together and trained them on the full set of patches. The results were good. The model was able to predict patches with a good accuracy 71%. The case-level accuracy was good, if a threshold of 0.5 is used.

### 3. Researched and implemented an attention-based CNN (SE-ResNet)

Researched the mathematical implementation of attention. Added to the presentation and added additional resources for further reading. 

I implemented an attention-based CNN (SE-ResNet) and trained it on the full set of H&E patches for comparison. The accuracy of the attention-based CNN was higher than that of the normal ResNet model. The attention-based CNN was able to predict patches with a good accuracy but the case-level accuracy was not very good. The is likely to be solved by ensambling all 3 patches together.

### 5. Next Steps

Now that I've validated ensembling the 3 different patches together does increase the performance, I will try to implement different ensembling methods that can increase the performacne of the model. I'll also try to implement these techniques on the new patches.

Now that I've validated the attention based CNNs may be able to increase the performance of the model, I will try to implement different attention based CNNs and see if they can increase the performance of the model. I'll also try to implement these techniques on the new patches. Furthermore, I will try to implment a simple attention based CNN to develop weight maps for inference on what sections of the image are important for the model. 

# Week 3

## What I did this week

1. Repeated the previous experiments (Splitting up the patches into 3 different stains, and then simple CNN for inference VS attention based CNN) on the new patches


## This week's progress summary

### 1. Repeated the previous experiments on the new patches

I repeated the two main experiments on the new patches. I trained 3 of the same Resnet50 models on the 3 different sets of patches (H&E, Melan, and SOX10). I used the same resnet model for all 3 sets of patches. I only trained the final classifier on the full set of patches. The results were not  as expected. The H&E model was the worst performing model, but the other two models were fairly good. In the second experiment, I trained an attention based CNN (CoAtNet) and the Simple CNN on the 3 set patches. The results were better for the Melan and SOX10 patches, but the H&E model was not very good. 

### 2. Next Steps

After confirming that the new slides improve the performance of the models of Melan and SOX10, I will try to implement different  methods that can increase the performance of the model for H&E. I found a specific ResNet50 based model that is specifically designed for H&E patches. I will try to implement this model and see if it can increase the performance of the model. I'll also try ensembling the 3 different patches together and see if it can increase the performance of the model.


# Week 4

## What I did this week

1. Performed EDA on the new H&E patches to see why the model was not performing well after training on the new patches. Attempted to train a the same model on the new middle 60% of the H&E patches so see if I could increase the performance of the model by changing the training set.
2. Attempted to train a new model on the new patches using a different architecture (DeiT) and see if it could increase the performance of the model. This was due to the fact that more complex models were perfroming better on the new patches, so I thought a more complex model would be better.

## This week's progress summary

### 1. EDA on the new H&E patches

I visualized the distribution of image size for the new patches compared to the old patches. The new patches have much larger proportion patches that are on the smaller side and the larger side of the distribution. I also visualized the distribution of the number of patches and cases in the train, test, and validation sets. There were some class imbalances in the training and validation sets, so we had Harvey look into resampling methods to see if we could increase the performance of the model. 
### 2. Model perfromance on new subset of H&E patches

I trained the same model on the new middle 60% of the H&E patches. The model was able to predict patches with a higher accuracy across all models. However, the model was not able to predict the patches with a good accuracy. During this comparison, I also noticed that more complex models were performing better on the new patches, so I thought a more complex model would be better.

### 3. Attempted to train a new model on the new patches using a different architecture (DeiT)

I trained a new model on the new patches using a different architecture (DeiT). The model was able to predict patches with a good accuracy. Additionally, I trained a CoAtNet model with the new patches, unfreezing all layers. The model looks promising, but I need to train it for a longer time as Colab disconnects after 12 hours.

### 4. Next Steps

I will continue to train the CoAtNet model on the new patches and see if it can increase the performance of the model. I will also try to implement different methods that can increase the performance of the model for H&E. I will also try to ensemble the 3 different patches together and see if it can increase the performance of the model.

# Week 5

## What I did this week

1. Outlines plans for the final project and presentation. Developed a series of experiments to run for the final project to determine the best patch level classifier for each of the 3 different stains. Outlined ensemble methods to use for the final project, and potential utilizations of Akhils SVM model for case level classification.