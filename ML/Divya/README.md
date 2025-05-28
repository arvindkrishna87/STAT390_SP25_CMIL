What I did this week (May 28):
Presentation on Multiple Instance Learning


I created a presentation analyzing a review of how MIL has been used for digital pathology: “Multiple Instance Learning for Digital Pathology” by Michael Gadermayr & Maximilian Tschuchnig (2024) in Computerized Medical Imaging and Graphics. This paper is from March 2024 and highlights advances made in the three years prior to that. It mentions that there has been an increase in recent years in MIL for digital pathology. 

MIL is useful for digital pathology mainly since it can directly use the whole slide image (WSI) and does not need pixel or region annotations. A WSI is treated as a “bag” of image patches (instances), and the label is assigned to the bag, not to individual patches. 

In the first step, MIL takes the whole image as an input and extracts patches (either randomly over the whole image, randomly within a region of interest, or based on feature representation, a combination of the prior ones, or another selection method). For our project, we have already created a method of patching, so we can pick up with the second step of MIL: patch processing. 

Typically, pretrained CNNS (e.g., DenseNet, ImageNet) are used to process the patches into features. Then, in the next step, aggregation, patch features are pooled into a single bag vector. Lastly, a label is predicted for the whole bag (i.e., case). 


One way to refine the MIL approach is by analyzing pooling strategies. Basic pooling strategies are average and max pooling, as I presented on earlier, but there are more complex strategies that have been found to yield better results. One is attention-based pooling, which learns patch importance weights based on features and therefore focuses on key patches. However, this does not capture the relationships between patches. For that, there is another method called transformer-based pooling, which uses self-attention to understand how patches influence each other. It can therefore capture the gobal context and patterns across multiple patches, but it is more complex and computationally expensive. 

There are also different types of MIL: instance-based and embedding-based. Instance-based MIL computes a score/probability per patch. In our case, it would be the likelihood if it being high-grade vs. benign. Then, it pools the scores, with one of the more basic methods, usually max or average, to obtain the overall classification. While this method is more simple, it does allow us to see predictions at the patch level. It gives us a very intiuitive heatmap of which patches are high probability of being high grade. This would allow us to see if it is identifying the lesions correctly. On the other hand, attention-based pooling gives us a heatmap of which patches were seen as most important, which is not necessarily the same as which had high probabilities of being high grade. 

Embedding-based MIL creates a feature vector for each patch and then pools the vectors into a single bag-level vector. This vector is then fed into a classifier (typically a small neural network called a Multi-Layer Perceptron or MLP) which gives a sigmoid output to predict the case-level label. This is more complex than instance-based by typically more accurate.

For next steps, I think what Hannah has been working on is an embedding-based MIL with attention pooling. We could generate heatmaps to see which patches were most important for preduction to get a more global sense of which patches are driving predictions and if they are indeed the benign/high grade lesions. I think we could also try instance-based MIL, since as described above, this would generate probabilities of benign/high-grade on a patch level and would allow us to see if it is correctly identifying high-grade/benign regions. I think transformer pooling could also be an avenue to explore (there is a TransMIL framework described in a paper by Shao et al. in Computer Vision and Pattern Recognition), since the information about the interactions/relationships between patches could be informative for our project, since there could be patterns that are split up among many patches that indicate a high-grade or benign case. For example, one patch by itself may not be informative/appears benign, but perhaps with its surrounding patches it creates a pattern that is known to be high-grade. Transformer pooling could help us recognize such instances.   






