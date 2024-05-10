---
layout: post
title: "How The Rendering Works Part 1."
category: blog
tags: rendering
---

## How The Rendering Works - Part 1

This article may be useful not only for `Technical Artists`, but also to `3D Artists` who want to understand, how to make fairly optimized maps and models.
It will be easy to understand, because each article have `gif` images with animation.

* # Overview
{:toc}

#### Intro

A computer views, it's assets as collections of vertex and texture data. Transforming this raw data into high-quality images primarily involves the collaboration between the system's central processing unit `CPU` and its graphics processing unit `GPU`.

![Full-width image](https://via.placeholder.com/600x300){:.lead width="600" height="300" loading="lazy"}

#### Transfer the data into system memory to facilitate quick access

Initially, essential data is fetched from the hard drive `HDD` and transferred into the system memory `RAM` to enhance accessibility. Subsequently, crucial meshes and textures are loaded into the graphics card's memory `VRAM` due to its significantly faster access capabilities.

![Full-width image](https://via.placeholder.com/600x300){:.lead width="600" height="300" loading="lazy"}

Once a texture is no longer required following its transfer into `VRAM`, it can be removed from `RAM`. However, it's crucial to ensure that the texture won't be needed again soon since reloading it from `HDD` incurs significant time costs. Meshes, on the other hand, should remain in `RAM` since the `CPU` often requires access to them, such as for collision detection purposes.

![Full-width image](https://via.placeholder.com/600x300){:.lead width="600" height="300" loading="lazy"}

With the data now residing in the `VRAM` on the graphics card, the transfer speed from `VRAM` to `GPU` remains insufficiently fast. The `GPU's` processing capability far exceeds the rate at which data can be delivered to it.

To address this issue, hardware engineers integrate small memory modules directly inside the processor chips, commonly referred to as on-chip caches. Although the amount of memory allocated for this purpose is limited due to the exorbitant costs associated with embedding it within the processor chip, the `GPU` copies small portions of currently necessary data into these caches.

![Full-width image](https://via.placeholder.com/600x300){:.lead width="600" height="300" loading="lazy"}

The replicated data now resides within what's known as the `L2 Cache`. This is essentially a compact memory space (on NVIDIA GM204: 2048 KB) located directly on the `GPU`, enabling faster access compared to the `VRAM`.

However, even this isn't sufficiently swift for efficient operations! Thus, there exists an even tinier `L1 cache` (on NVIDIA GM204: 384 KB (4 x 4 x 24 KB)), situated not just on the `GPU` but even closer to its processing cores!

![Full-width image](https://via.placeholder.com/600x300){:.lead width="600" height="300" loading="lazy"}

Additionally, there's a separate memory allocation designated for input and output data for the GPU Cores: registers or a register file. Here, the GPU Cores fetch two values, perform calculations, and store the results into a register (essentially a memory address within the register file).

Subsequently, the outcomes stored in the registers are retrieved and placed back into L1/L2/VRAM, clearing room for new values in the register file. Typically, as a programmer, you don't need to concern yourself too deeply with these processes.

![Full-width image](https://via.placeholder.com/600x300){:.lead width="600" height="300" loading="lazy"}

What's the reason for all this complexity? As noted earlier, it all boils down to access times! When you compare the access times of, say, a HDD and the L1 cache, the difference is enormous—truly significant!

Prior to the render-party kicking off, the CPU configures certain global values that dictate how the meshes will be rendered. This collection of values is referred to as the Render State.

#### Set the Render State

A render state serves as a comprehensive definition outlining how meshes are to be rendered. It encompasses details such as:
`vertex and pixel shader, texture, material, lighting, transparency, etc.`

Once the preparation is complete, the `CPU` can at last summon the `GPU` and instruct it on what to draw. This instruction is commonly referred to as a `Draw Call`.
