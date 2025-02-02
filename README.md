# SeargeSDXL
Custom nodes for easier use of SDXL in [ComfyUI](https://github.com/comfyanonymous/ComfyUI) including workflows for SDXL 1.0 txt2img that utilize both the base and refiner checkpoints.

# Install:

### Recommended Installation:
- Navigate to your `ComfyUI/custom_nodes/` folder
- Run `git clone https://github.com/SeargeDP/SeargeSDXL.git`
- Restart ComfyUI

### Alternative Installation:
- Drop the `SeargeSDXL` folder into the `ComfyUI/custom_nodes` directory and restart ComfyUI.

# Information for Example Workflows
Now 3 workflows are included in the examples directory. They are called *simple*, *advanced*, and *reborn*.

### Simple and Advanced
The only difference between *simple* and *advanced* is how the prompts are used through the CLIP models.
The *advanced* workflow also processes the base sampler at half the speed compared to the *simple* workflow,
due to the way the conditioning is combined.

### Reborn
The *reborn* workflow is a new workflow, created from scratch. It requires the latest additions to the
SeargeSDXL custom node extension, because it makes use of some new node types.

The interface for using this new workflow is also designed in a different way, with all parameters that
are usually tweaked to generate images tighly packed together. This should make it easier to have every
important element on the screen at the same time without scrolling.

Prompting has also been revised, once more. The main prompt and secondary prompt influence the two CLIP models
similar to the way my original workflow worked. A third positive prompt was added for style description and
artist references. This one influences both CLIP models.

For the negative prompt the way it is now designed expects objects and subjects you don't want in the negative
prompt and styles that you don't want in the negative style.

# Custom Nodes

<img src="https://github.com/SeargeDP/SeargeSDXL/blob/main/example/Searge-SDXL-Nodetypes.png" width="768">

## SDXL Sampler Node
<img src="https://github.com/SeargeDP/SeargeSDXL/blob/main/example/Searge-SDXL-Node.png" width="407">

### Inputs
- **base_model** - connect the SDXL base model here, provided via a `Load Checkpoint` node 
- **base_positive** - recommended to use a `CLIPTextEncodeSDXL` with 4096 for `width`, `height`, `target_width`, and `target_height`
- **base_negative** - recommended to use a `CLIPTextEncodeSDXL` with 4096 for `width`, `height`, `target_width`, and `target_height`
- **refiner_model** - connect the SDXL refiner model here, provided via a `Load Checkpoint` node 
- **refiner_positive** - recommended to use a `CLIPTextEncodeSDXLRefiner` with 2048 for `width`, and `height`
- **refiner_negative** - recommended to use a `CLIPTextEncodeSDXLRefiner` with 2048 for `width`, and `height`
- **latent_image** - either an empty latent image or a VAE-encoded latent from a source image for img2img
- **noise_seed** - the random seed for generating the image
- **steps** - total steps for the sampler, it will internally be split into base steps and refiner steps
- **cfg** - CFG scale (classifier free guidance), values between 3.0 and 12.0 are most commonly used
- **sampler_name** - the noise sampler _(I prefer dpmpp_2m with the karras scheduler, sometimes ddim with the ddim_uniform scheduler)_
- **scheduler** - the scheduler to use with the sampler selected in `sampler_name`
- **base_ratio** - the ratio between base model steps and refiner model steps _(0.8 = 80% base model and 20% refiner model, with 30 total steps that's 24 base steps and 6 refiner steps)_
- **denoise** - denoising factor, keep this at 1.0 when creating new images from an empty latent and between 0.0-1.0 in the img2img workflow

### Outputs
- **LATENT** - the generated latent image

## SDXL Prompt Node
<img src="https://github.com/SeargeDP/SeargeSDXL/blob/main/example/Searge-SDXL-Node2.png" width="434">

### Inputs
- **base_clip** - connect the SDXL base CLIP here, provided via a `Load Checkpoint` node 
- **refiner_clip** - connect the SDXL refiner CLIP here, provided via a `Load Checkpoint` node 
- **pos_g** - the text for the positive base prompt G 
- **pos_l** - the text for the positive base prompt L
- **pos_r** - the text for the positive refiner prompt
- **neg_g** - the text for the negative base prompt G
- **neg_l** - the text for the negative base prompt L
- **neg_r** - the text for the negative refiner prompt
- **base_width** - the width for the base conditioning
- **base_height** - the height for the base conditioning
- **crop_w** - crop width for the base conditioning
- **crop_h** - crop height for the base conditioning
- **target_width** - the target width for the base conditioning
- **target_height** - the target height for the base conditioning
- **pos_ascore** - the positive aesthetic score for the refiner conditioning
- **neg_ascore** - the negative aesthetic score for the refiner conditioning
- **refiner_width** - the width for the refiner conditioning
- **refiner_height** - the height for the refiner conditioning

### Outputs
- **CONDITIONING** 1 - the positive base prompt conditioning
- **CONDITIONING** 2 - the negative base prompt conditioning
- **CONDITIONING** 3 - the positive refiner prompt conditioning
- **CONDITIONING** 4 - the negative refiner prompt conditioning

# Examples

- simple prompt processing

<img src="https://github.com/SeargeDP/SeargeSDXL/blob/main/example/Searge-SDXL-1.0-workflow-1.png" width="768">

- advanced prompt processing

<img src="https://github.com/SeargeDP/SeargeSDXL/blob/main/example/Searge-SDXL-1.0-workflow-2.png" width="768">

- reborn workflow

<img src="https://github.com/SeargeDP/SeargeSDXL/blob/main/example/Searge-SDXL-1.0-workflow-3.png" width="768">

### Results

- simple prompt processing

<img src="https://github.com/SeargeDP/SeargeSDXL/blob/main/example/Searge-SDXL-1.0-simple.png" width="768">

- advanced prompt processing

<img src="https://github.com/SeargeDP/SeargeSDXL/blob/main/example/Searge-SDXL-1.0-advanced.png" width="768">

- reborn workflow

<img src="https://github.com/SeargeDP/SeargeSDXL/blob/main/example/Searge-SDXL-1.0-reborn.png" width="768">
