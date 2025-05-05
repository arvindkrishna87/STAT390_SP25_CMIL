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