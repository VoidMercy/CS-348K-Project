# CS 348K Project

**Project Title:** Piercing Rays Through Splats (TODO)

**Team Members:** Alex Lin (alexlin0), Yvette Lin (TODO), Meijin Li (TODO)

**Summary:**

**Inputs and outputs:** We list the inputs, outputs and constraints for the renderer and scene representation training separately. For the renderer, the inputs to the system are trained 3D gaussian splats scene representations. The outputs of the system will be rendered frames from novel views. The major constraint is real-time rendering performance on an Nvidia RTX A6000 GPU. For scene representation training, the inputs to the system are videos taken from different angles of scenes we want to reconstruct. The outputs of the system will be a 3D gaussian splat scene representation. The constraint is to achieve training time similar to that of the 3D-GS paper.

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

**Expected deliverables:**

**Evaluation:**

**What are the biggest risks?** The biggest risk will be working on the tasks in parallel. The BVH data structure is closely tied with the forward-pass renderer, which has to use and traverse the BVH. Additionally, the backward-pass shader computes gradients from the calculations done in the forward-pass renderer. We can mitigate these risks by carefully deciding upon the interface of the BVH data structure, and making sure everyone is on the same page about what computation the forward-pass renderer is doing exactly. We will need to plan and talk before we implement.

**What do you need help with?** RTX 4090 GPU!
