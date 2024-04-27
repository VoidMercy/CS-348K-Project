# CS 348K Project

**Project Title:** Real-Time Raytracing for 3D Gaussian Splats

**Team Members:** Alex Lin (alexlin0), Yvette Lin (yvelin), Meijin Li (meijin)

**Summary: **We aim to develop a high-performance raytracer for real-time rendering of scenes represented with 3D Gaussian splats on an Nvidia RTX A6000 GPU. To create end-to-end training process, from video input through scene reconstruction to novel view synthesis, we are going to start with existing 3D gaussian splatting scene representations, modify the forward-pass renderer in [the surface splatting paper](https://www.cs.umd.edu/~zwicker/publications/SurfaceSplatting-SIG01.pdf) to account for overlapping 3D gaussians, and integrate with nerfstudio's Splatfacto codebase. The system's efficacy will be validated by comparing its rendering quality and speed against traditional rasterization methods.

**Inputs and outputs:** We list the inputs, outputs and constraints for the renderer and scene representation training separately. 

- For the renderer, the inputs to the system are trained 3D gaussian splats scene representations. The outputs of the system will be rendered frames from novel views. The major constraint is real-time rendering performance on an Nvidia RTX A6000 GPU. 
- For scene representation training, the inputs to the system are videos taken from different angles of scenes we want to reconstruct. The outputs of the system will be a 3D gaussian splat scene representation. The constraint is to achieve training time similar to that of the 3D-GS paper.

**Task list:**

1. Find and download existing 3D gaussian splatting scene representations.
1. Write bounded volume hierarchy construction algorithm in cuda c++ on the CPU side.
1. Forward-pass renderer using ray tracing in cuda c++ compute shader. The renderer should shade each pixel in parallel, and each pixel computes ray BVH intersections and accumulates pixel color. We will first attempt to reproduce the results in the paper by using the same rendering method presented in the paper: splatting 3D gaussians into 2D and alpha composing by depth order. Alpha composing in order is implemented by traversing ray intersections in order of closest first.
1. Modify forward-pass renderer to account for overlapping 3D gaussians using either a closed-form expression or ray marching to approximate (Professor Kayvon's idea). We will have to do the math to compute the density of overlapping gaussians.
1. Backward-pass to compute gradients in cuda c++ compute shader (both with and without overlapping 3D gaussians).
1. Integrate the above components using nerfstudio's Splatfacto codebase for an end-to-end 3D gaussian splatting training pipeline.
1. Evaluation

Nice to haves:

1. Implement the forward-pass renderer in Slang and get gradients automatically differentiated
2. Implement hardware accelerated ray-tracing for RTX GPUs

**Expected deliverables:** Our main deliverable is a raytracer for scenes represented with Gaussian splats, which we will demonstrate can be used to train scenes end-to-end for novel view synthesis (by slotting in the raytracer to replace existing rasterization pipelines used in rendering). Qualitatively, we will demonstrate our results by showing real-time rendering on a collection of scenes reconstructed from sets of 2D images (from existing datasets and possibly from additional datasets we collect ourselves).

**Evaluation:** We expect our method to have comparable quality to the baseline rasterization method, which we will validate by reporting the traditional quantitative image quality metrics (PSNR, SSIM, LPIPS) on a set of 3D scenes. x We also expect our method to exhibit faster rendering and training than the baseline rasterization method on certain types of scenes and views, namely scenes with a high Gaussian splat count with views rendered at a low resolution. We will validate the expected speedup by reporting training and rendering times for our method compared to the baseline on the same hardware.

**What are the biggest risks?** The biggest risk will be working on the tasks in parallel. The BVH data structure is closely tied with the forward-pass renderer, which has to use and traverse the BVH. Additionally, the backward-pass shader computes gradients from the calculations done in the forward-pass renderer. We can mitigate these risks by carefully deciding upon the interface of the BVH data structure, and making sure everyone is on the same page about what computation the forward-pass renderer is doing exactly. We will need to plan and talk before we implement.

**What do you need help with?** RTX 4090 GPU!
