
# UNet colored polygon

## Overview

This project involves building a conditional UNet model using PyTorch to generate colored polygons based on an input polygon image and a given color name. The final model produces an output image where the polygon is filled with the specified color.

This assignment challenged me to combine visual input (the polygon) with textual input (color name), and ensure the model could correctly render the colorized shape. 

## Model Architecture

I used a UNet architecture implemented from scratch in PyTorch. To condition the model on both image and color input:

- The input polygon image (grayscale, 1 channel) was passed through the UNet encoder.

- The color input, converted to a 3D RGB tensor from the hex code, was broadcasted and concatenated to the encoder’s output before passing through the decoder.

- Implementation of this mechanism allowed the model to "know" what color to apply during generation.

No pretrained weights were used. The model was trained end-to-end on the dataset provided.

## Key Implementation

One of the most critical implementation choices I made was converting the color names provided in the dataset JSON to their corresponding hexadecimal color codes using the Python webcolors library.

Instead of relying on textual color names directly, I realized that converting them to standardized RGB tuples via hex codes would give the model a more precise and numerically consistent representation of the target color.

This decision significantly improved model performance and consistency during training, as it removed ambiguity in color mapping and allowed the model to learn direct pixel-wise RGB targets instead of linguistic representations.

## Hyperparameters and Training

I tested with several hyperparameters, including variations in batch size, learning rate, and optimizer settings. The following is what worked best for me:

- Optimizer: Adam

- Learning rate: 1e-4

- Batch size: 8

- Loss Function: Mean Squared Error (MSE)

- Epochs trained: 50

## Experiment Tracking

All training experiments were tracked using wandb. I logged the training loss, validation loss, and occasional output images to visually track how the model improved over time.

The Wandb URL does not allow sharing the link publicly hence I have provided my dashboards in a report and shared it in the form for scrutinizing.


## Inference

Inference was implemented directly in the same `.ipynb` notebook. The notebook includes functions to:

- Loading the trained UNet model

- Loading a sample grayscale polygon image

- Specifying a color name

- Converting the color name to RGB using webcolors

- Displaying both the input and the generated colored polygon image

## Key Learnings

- Color conditioning is not trivial — I initially tried using color names as raw strings or indices, but switching to RGB hex codes greatly improved accuracy and consistency.

- UNet is flexible — It adapts well to low-level tasks like pixel-wise colorization when properly conditioned.

- Experiment tracking matters — Using wandb allowed me to quickly spot issues like training plateaus and incorrect color predictions.
