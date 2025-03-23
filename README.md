# IA-Embarque

This is an artificial intelligence project focused on the prediction of failures in industrial machines. The model is trained with a dataset containing 10,000 samples, where data preprocessing and balancing techniques are applied, since the number of machines with failures is significantly lower than those operating normally.

Initially, the dataset presents five types of failures: TWF, HDF, PWF, OSF, and RNF. However, failures of the RNF type are removed, as they are random failures, which makes it unfeasible to create a reliable prediction pattern.

One of the main challenges of the project is the low number of samples for the TWF failure type, even after removing the random failures. To handle this imbalance, techniques such as undersampling and class weighting are applied during training.

After finalizing and validating the model, the proposal is to deploy it on an STM32 board, completing the main goal of the project, from model development to embedded hardware implementation.
