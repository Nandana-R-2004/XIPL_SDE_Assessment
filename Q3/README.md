# Q3 - End-to-End Virtual Try-On Inference

## Objective

This notebook performs virtual try-on using the open-source **CatVTON** model. The pipeline generates realistic try-on images for five person-garment pairs by integrating the preprocessing pipeline developed in Q2.

---

## Model

- Model: CatVTON
- Framework: PyTorch + Diffusers
- GPU: Tesla T4 (Google Colab)
- Precision: FP16

---

## Pipeline

1. Load person and garment images.
2. Generate upper-body mask using CatVTON AutoMasker.
3. Perform virtual try-on using CatVTON.
4. Save the generated output image.

---

## Output

The notebook generates:

Q3/
├── outputs/
│   ├── output_01.png
│   ├── output_02.png
│   ├── output_03.png
│   ├── output_04.png
│   └── output_05.png

---

## Constraints Encountered

### 1. Missing Python Dependencies

Errors encountered:
- fvcore
- iopath
- av

Workaround:
Installed the required packages using pip.

---

### 2. Safety Checker

Issue:
The Stable Diffusion safety checker incorrectly classified generated try-on images as NSFW and replaced them with a placeholder image.

Workaround:
Enabled CatVTON's built-in safety bypass using:

pipeline.skip_safety_check = True

---

### 3. Mask Compatibility

Issue:
Using the clothing masks generated in Q2 produced weak garment transfer where only textures/logos were transferred.

Workaround:
Used CatVTON's AutoMasker to generate the upper-body mask expected by the model during inference.

---

### 4. GPU Memory

GPU:
Tesla T4 (Google Colab)

No out-of-memory errors were encountered after using FP16 inference.

---

## Libraries

- torch
- diffusers
- transformers
- accelerate
- peft
- fvcore
- iopath
- av
- Pillow

---

## Author

Nandana R
B.Tech Computer Science (AI)
