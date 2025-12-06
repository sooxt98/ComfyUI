# LongCat-Image ComfyUI Integration Summary

## Overview

This integration enables the use of LongCat-Image models within ComfyUI, providing access to:
- Text-to-image generation with excellent Chinese text rendering
- Image editing capabilities with instruction-based prompts
- Efficient inference with a 6B parameter model

## What Was Done

### 1. Custom Node Implementation

Created a custom node package at `custom_nodes/comfyui_longcat_image/` with:

- **LongCatImageModelLoader**: Loads LongCat-Image models from the filesystem
- **LongCatImageTextToImage**: Generates images from text prompts
- **LongCatImageEdit**: Edits images based on instruction prompts

### 2. Integration Features

- **Flexible Model Loading**: Supports loading models from various paths (absolute or relative to ComfyUI models directory)
- **Data Type Support**: Configurable dtype (bfloat16, float16, float32) for different hardware capabilities
- **Automatic Model Type Detection**: Distinguishes between text-to-image and edit models based on path
- **ComfyUI Compatibility**: Full integration with ComfyUI's node system and image format
- **Chinese Language Support**: Native support for Chinese prompts and text rendering

### 3. Documentation

- Comprehensive README with installation instructions
- Example workflow JSON files for both text-to-image and image editing
- Detailed parameter descriptions for all nodes
- Usage tips and best practices

## Installation Instructions

### Prerequisites

1. ComfyUI installed and working
2. Python 3.10 or later
3. CUDA-compatible GPU (recommended) or CPU

### Installation Steps

```bash
# 1. Install dependencies
cd custom_nodes/comfyui_longcat_image
pip install -r requirements.txt

# 2. Install LongCat-Image package
pip install git+https://github.com/meituan-longcat/LongCat-Image.git

# 3. Download models
pip install "huggingface_hub[cli]"

# For text-to-image
huggingface-cli download meituan-longcat/LongCat-Image \
    --local-dir models/diffusion_models/LongCat-Image

# For image editing
huggingface-cli download meituan-longcat/LongCat-Image-Edit \
    --local-dir models/diffusion_models/LongCat-Image-Edit
```

## Usage Examples

### Text-to-Image Generation

```python
# In ComfyUI workflow:
# 1. LongCat-Image Model Loader
#    - model_path: "LongCat-Image"
#    - dtype: "bfloat16"
#
# 2. LongCat-Image Text to Image
#    - prompt: "A young woman wearing a yellow sweater..."
#    - width: 1344
#    - height: 768
#    - steps: 50
#    - guidance_scale: 4.5
#
# 3. Save Image
```

### Image Editing

```python
# In ComfyUI workflow:
# 1. LongCat-Image Model Loader
#    - model_path: "LongCat-Image-Edit"
#
# 2. Load Image (input image)
#
# 3. LongCat-Image Edit
#    - prompt: "将猫变成狗" (change cat to dog)
#    - steps: 50
#    - guidance_scale: 4.5
#
# 4. Save Image
```

## Technical Details

### Model Architecture
- Based on DiT (Diffusion Transformer) architecture
- 6B parameters (highly efficient)
- Supports resolutions up to 768x1344 (and variations)
- Built-in prompt rewriting capability using text encoder

### Pipeline Components
- Text Encoder: Qwen2.5-VL based
- Transformer: LongCat-Image custom architecture
- VAE: Autoencoder for latent space operations
- Scheduler: Flow Matching Euler Discrete

### Performance Characteristics
- Competitive with models several times larger
- Excellent Chinese text rendering (90.7% on ChineseWord benchmark)
- State-of-the-art image editing quality among open-source models
- Remarkable photorealism in generated images

## Files Modified/Added

```
.gitignore                                           # Modified to allow custom node
custom_nodes/comfyui_longcat_image/
├── __init__.py                                      # Main node implementation
├── README.md                                        # Documentation
├── requirements.txt                                 # Python dependencies
├── example_workflow_t2i.json                       # Text-to-image example
└── example_workflow_edit.json                      # Image editing example
```

## Dependencies

Core dependencies (from requirements.txt):
- accelerate>=1.11.0
- diffusers>=0.35.2
- transformers>=4.57.1
- safetensors>=0.6.2
- peft>=0.18.0

Plus the LongCat-Image package itself from GitHub.

## Security Review

- All dependencies checked for known vulnerabilities: ✅ No issues found
- CodeQL security scan: ✅ No alerts
- Code review completed: ✅ All suggestions addressed

## Testing Recommendations

1. **Basic Functionality Test**:
   - Load the text-to-image model
   - Generate an image with a simple English prompt
   - Verify the output is generated correctly

2. **Chinese Text Test**:
   - Generate an image with Chinese characters in the prompt
   - Verify text rendering quality

3. **Image Editing Test**:
   - Load the edit model
   - Edit an image with an instruction
   - Verify the edit maintains consistency

4. **Different Resolutions**:
   - Test various resolution combinations
   - Verify memory usage is reasonable

## Known Limitations

1. **Model Size**: Models are ~12GB each, requiring significant disk space and VRAM
2. **VRAM Requirements**: Recommended minimum 8GB VRAM for bfloat16, more for higher precision
3. **Installation Complexity**: Requires installing from GitHub repo (not on PyPI)
4. **CPU Performance**: While CPU inference is supported, it will be significantly slower

## Future Enhancements

Potential improvements that could be made:
1. Add LoRA support for fine-tuned models
2. Implement batch processing for multiple images
3. Add model quantization options for lower VRAM usage
4. Support for LongCat-Image-Dev (fine-tuning checkpoint)
5. Integration with ComfyUI Manager for easier installation

## References

- [LongCat-Image GitHub Repository](https://github.com/meituan-longcat/LongCat-Image)
- [LongCat-Image on Hugging Face](https://huggingface.co/meituan-longcat/LongCat-Image)
- [Technical Report](https://github.com/meituan-longcat/LongCat-Image/blob/main/assets/LongCat_Image_Technical_Report.pdf)
- [ComfyUI Documentation](https://docs.comfy.org/)

## Support

For issues specific to this integration:
- Check the README in `custom_nodes/comfyui_longcat_image/`
- Verify installation steps were followed correctly
- Ensure models are downloaded to the correct location

For issues with LongCat-Image models:
- Refer to the [official repository](https://github.com/meituan-longcat/LongCat-Image)
- Contact: longcat-team@meituan.com

## License

This integration follows the licenses of:
- ComfyUI: GPL-3.0
- LongCat-Image: Apache 2.0

The integration code itself is provided under the same license as ComfyUI (GPL-3.0).
