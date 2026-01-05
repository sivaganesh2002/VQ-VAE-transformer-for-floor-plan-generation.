This repository implements a discrete latent generative modeling pipeline for floor plan synthesis using a Vector Quantized Variational Autoencoder (VQ-VAE) followed by an autoregressive Transformer operating over learned latent tokens.

The VQ-VAE consists of a fully convolutional encoder–quantizer–decoder architecture trained on resized 64×64 RGB floor plan images. The encoder maps input images to a spatial latent tensor using stacked Conv2D + ReLU layers. A vector quantization module discretizes these continuous latents by nearest-neighbor lookup against a learnable codebook of embeddings, yielding a grid of discrete latent indices. Training is stabilized via a commitment loss, codebook loss, and a straight-through estimator (STE) to preserve gradient flow through the non-differentiable quantization operation. Reconstruction is performed using a transposed convolutional decoder, optimized jointly with MSE reconstruction loss and VQ loss.

After convergence, the learned discrete latent indices are flattened into token sequences and modeled using a decoder-only Transformer trained with causal autoregressive cross-entropy loss. This stage captures global spatial dependencies, long-range room connectivity, and layout regularities that convolutional decoders alone cannot model.

During inference, the Transformer generates novel latent token sequences, which are reshaped into latent grids, mapped back to embeddings using the VQ codebook, and decoded to produce coherent and structurally consistent floor plan images.

Below are sample reconstructions and generations observed during VQ-VAE training across epochs, illustrating progressive structural stabilization and layout emergence.

<img width="850" height="385" alt="image" src="https://github.com/user-attachments/assets/d3d261c9-0ae4-4547-b953-4d06d29e5e65" />
<img width="770" height="396" alt="image" src="https://github.com/user-attachments/assets/44338704-e956-4977-a4a8-20520fd9afdc" />

