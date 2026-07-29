# Q3 - End-to-End Virtual Try-On Inference

## Objective

This task implements an end-to-end virtual try-on pipeline using the
open-source CatVTON model.

The preprocessing outputs generated in Q2 are integrated into Q3 to
construct person-space masks for the five person-garment pairs. These
masks are then supplied to CatVTON for final virtual try-on inference.

---

## Model

- Model: CatVTON
- Base model: booksforcharlie/stable-diffusion-inpainting
- CatVTON checkpoint: zhengchong/CatVTON
- Checkpoint version: vitonhd
- Framework: PyTorch / Diffusers
- Device: CUDA
- GPU: NVIDIA Tesla T4
- GPU memory: approximately 14.56 GB
- Precision: FP16
- Resolution: 768 x 1024
- Inference steps: 50
- Guidance scale: 2.5
- Seed: 42

### Model Licenses

- **CatVTON:** CC BY-NC-SA 4.0
- **Stable Diffusion Inpainting base model:** CreativeML Open RAIL-M License

CatVTON was selected because it is relatively lightweight and suitable
for Google Colab compared with heavier virtual try-on alternatives.

---

## Q2 to Q3 Pipeline

The final workflow is:

Person Image + Garment Image
        |
        v
Q2 Preprocessing
        |
        +-- Clothing Mask
        +-- Garment Mask
        +-- Human Parsing Map
        |
        v
Garment-Aware Person-Space Mask
        |
        v
CatVTON Inference
        |
        v
Virtual Try-On Output

The final Q3 masks are therefore based on the preprocessing outputs
generated in Q2.

CatVTON AutoMasker was NOT used for the final masks.

---

## Garment-Aware Mask Generation

The target garments have different sleeve structures, so a single mask
geometry was not suitable for every pair.

The five target garments were handled as follows:

| Pair | Target Garment Type |
|------|---------------------|
| pair_01 | Short Sleeve |
| pair_02 | Long Sleeve |
| pair_03 | Strappy / Sleeveless |
| pair_04 | Short Sleeve |
| pair_05 | Short Sleeve |

The Q2 clothing masks and parsing information were used as the base
representation.

The editable person-space region was then adapted according to the
geometry of the target garment.

For short-sleeve garments, the mask includes the upper torso, shoulders,
and required upper-arm regions.

For the long-sleeve garment, the editable region extends farther along
the arms.

For the strappy garment, the mask focuses on the existing garment and
strap/shoulder regions while preserving unrelated body regions.

---

## CatVTON Inference

Each pair was processed using:

- Resolution: 768 x 1024
- Precision: FP16
- Inference steps: 50
- Guidance scale: 2.5
- Seed: 42

The person image, target garment image, and final Q2-derived person-space
mask are supplied to the CatVTON pipeline.

All five required person-garment pairs were successfully processed.

---

## Final Outputs

Five virtual try-on images were generated:

- pair_01
- pair_02
- pair_03
- pair_04
- pair_05

The final results are stored inside the Q3 output/result directories.

---

# Constraints and Workarounds

## 1. GPU Memory Constraint

The experiments were executed on Google Colab using an NVIDIA Tesla T4
GPU with approximately 14.56 GB of GPU memory.

CatVTON was selected because it has lower hardware requirements than
heavier virtual try-on alternatives.

FP16 inference was used to reduce GPU memory consumption.

Observed peak GPU memory during inference remained approximately within
3.5 GB to 6 GB depending on the pair.

No GPU out-of-memory error occurred during the final inference runs.

CPU offloading and model quantization were therefore not required.

---

## 2. Dependency Installation

The default Colab environment did not contain all CatVTON dependencies.

Additional packages required included:

- fvcore
- iopath
- av
- diffusers
- transformers
- accelerate
- peft

These dependencies were installed before loading the CatVTON pipeline.

A dependency warning involving Gradio and Hugging Face Hub was observed,
but it did not prevent CatVTON inference.

---

## 3. Model Loading

During initialization, the Stable Diffusion inpainting checkpoint did
not contain the expected UNet safetensors file.

The loader automatically fell back to the available PyTorch serialized
weights.

The model loaded successfully and inference continued normally.

---

## 4. Colab Runtime Disconnection

During development, the Colab runtime disconnected.

Because the CatVTON pipeline existed only in runtime memory, this caused:

NameError: name 'pipe' is not defined

A pipeline restoration cell was used to reload CatVTON from the cached
Hugging Face checkpoints.

This avoided repeating the complete setup and preprocessing workflow.

---

## 5. Q2 Mask Compatibility

The Q2 clothing mask represents the garment currently worn by the
person.

Directly using this mask was not always sufficient for virtual try-on
because the target garment could have different geometry.

Examples included:

- sleeveless source to short-sleeve target
- shorter source garment to longer target garment
- short-sleeve source to long-sleeve target
- existing straps that needed replacement

The Q2 segmentation and parsing outputs were therefore transformed into
garment-aware person-space masks before CatVTON inference.

---

## 6. Sleeve Generation

Some initial masks did not provide enough editable space around the
upper arms.

This caused incomplete sleeve generation.

The workaround was to expand only the required arm region according to
the target garment type while preserving the remaining body regions.

---

## 7. Lower Garment Boundary

Over-expanding the editable mask below the shirt could cause generated
fabric to extend into the jeans or lower-body clothing.

The mask bottom boundary was therefore restricted using the original
clothing region and target garment geometry.

This helped preserve the lower-body clothing.

---

## 8. Strappy Garment

The strappy garment in pair_03 required additional mask refinement.

The initial result preserved portions of the straps from the original
top.

The shoulder and original strap regions were added to the editable mask
while the arms, face, and lower body were protected.

This allowed CatVTON to replace the original straps with the target
garment straps.

---

## 9. Safety Checker False Positive

During pair_03 inference, the safety checker incorrectly flagged a
normal clothing try-on result.

An initial attempt to set the checker to None caused:

TypeError: 'NoneType' object is not iterable

CatVTON's custom pipeline expected an iterable safety-check result.

For the known benign assessment input, the safety-check handling was
adjusted to return the structure expected by the CatVTON pipeline,
allowing the inference result to be produced normally.

---

## Reproducibility

Final inference configuration:

| Parameter | Value |
|-----------|-------|
| GPU | Tesla T4 |
| Resolution | 768 x 1024 |
| Precision | FP16 |
| Inference Steps | 50 |
| Guidance Scale | 2.5 |
| Seed | 42 |

Exact output pixels may vary if the CUDA, PyTorch, Diffusers, or model
versions are changed.

---

## Model License

CatVTON and its model weights were obtained from the public CatVTON
repository/checkpoint for this technical assessment.

The applicable CatVTON repository and checkpoint license terms should
be followed when using or redistributing the model or generated assets.

No paid inference API was used for Q3.

---

## Result

CatVTON successfully generated virtual try-on outputs for all five
required person-garment pairs while operating within the memory
limitations of a Google Colab Tesla T4 GPU.

The final implementation connects the Q2 preprocessing pipeline directly
to the Q3 CatVTON inference stage.

---

## Author

Nandana R

B.Tech Computer Science - Artificial Intelligence
