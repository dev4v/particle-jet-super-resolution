# particle-jet-super-resolution


This project uses a Generative Adversarial Network (GAN) for the task of jet image super-resolution. GANs are well suited for image reconstruction problems because they can learn to generate high-resolution outputs while preserving important spatial features.

A GAN consists of two neural networks:

Generator – generates high-resolution jet images from low-resolution inputs.

Discriminator – distinguishes between real high-resolution images and generated images.

The generator learns to produce realistic images while the discriminator provides feedback to improve the generated outputs. This adversarial training helps recover high-frequency details that traditional interpolation or regression-based methods may fail to capture.

Convolutional neural networks were used in both the generator and discriminator because they are effective for extracting spatial features from image data. Upsampling layers in the generator progressively increase the resolution of the input image.


## Dataset Used and Specifics

The dataset used in this project consists of particle jet images used for studying super-resolution in high-energy physics. Each sample in the dataset contains a pair of images:

- Low-resolution (LR) jet image – used as the model input

- High-resolution (HR) jet image – used as the ground truth target

The dataset is stored in Parquet format, where each entry contains arrays representing the pixel intensities of the jet images.

## Dataset Structure

Each record in the dataset includes:

| Column Name | Description |
|-------------|-------------|
| X_jets_LR | Low resolution jet image used as input |
| X_jets | High resolution jet image used as ground truth |
| y | Label associated with the jet sample |

| Scalar Property | Description          |
|-----------------|--------------------|
| pt              | Transverse momentum |
| m0              | Particle mass       |

- X_jets_LR represents the low-resolution input image.

- X_jets represents the corresponding high-resolution image.

- y contains associated labels for the jet sample.


## Data Preprocessing 

Before training the model, the dataset was processed as follows:

- The Parquet file was loaded using Python data processing libraries.

- Image arrays were converted into PyTorch tensors.

- Pixel values were normalized to improve training stability.

- Low-resolution and high-resolution image pairs were prepared for GAN training.

## Optimization Strategy

The model was trained using the Adam optimizer, which is commonly used in deep learning due to its stable and efficient convergence.

Key training choices:

| Parameter | Value |
|-----------|-------|
| Optimizer | AdamW |
| Learning Rate | 1e-4  |
| Epochs | 200 |
| Batch Size | 16 |

Loss functions: Adversarial loss for GAN training and reconstruction loss to maintain similarity with the ground truth high-resolution images.

The adversarial loss encourages the generator to produce realistic images, while the reconstruction loss ensures that the generated output remains close to the true high-resolution target.

During training, the generator and discriminator are updated iteratively. This process allows the generator to progressively improve its ability to reconstruct detailed jet images.

## Training Observations

During training of the SRGAN for 200 epochs,  discriminator loss  consistently reached zero, indicating it became very confident in distinguishing generated images from real ones. The generator loss ranged from ~0.05 to 0.07, showing gradual adjustment as it learned to improve image details.  
