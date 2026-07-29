# Q2 - Human Parsing & Garment Segmentation Pipeline

## Objective

This task prepares person and garment images for a diffusion-based virtual try-on pipeline.

The pipeline performs three main operations:

1. Generate a human parsing map from the person image.
2. Create an agnostic person representation by masking the upper-body clothing region.
3. Segment the garment from its product image and generate a clean garment mask.

---

## Models Used

### Human Parsing - SCHP

Model:
`pirocheto/schp-atr-18`

SCHP is used to generate semantic human parsing information such as clothing and body regions.

The parsing output is then used to identify the upper-body clothing region required for creating the agnostic representation.

### Garment Segmentation - rembg

`rembg` is used to remove the background from garment product images.

The alpha channel of the resulting image is used to create a binary garment mask.

---

## Model Selection and Comparison

| Model | Advantages | Limitations | Decision |
|------|------------|-------------|----------|
| SCHP | Designed specifically for human parsing and provides semantic body/clothing regions | Parsing can be challenging with occlusion and unusual poses | Selected |
| SAM + Grounding DINO | Powerful general-purpose segmentation and text-guided object detection | Requires multiple models and a more complex, computationally expensive pipeline | Not selected |
| DensePose | Provides detailed body-surface and pose information | Does not directly provide the clothing segmentation needed for this task | Not selected |

SCHP was selected because the task specifically requires human parsing and upper-body clothing identification. It provides the semantic information needed to construct the clothing mask while remaining practical to run in Google Colab.

---

## Person Processing Pipeline

The person preprocessing workflow is:

Person Image
    |
    v
SCHP Human Parsing
    |
    v
Parsing Map
    |
    v
Upper-Body Clothing Mask
    |
    v
Mask Refinement
    |
    v
Agnostic Person Representation

The clothing mask is generated from the SCHP probability output for the relevant clothing classes.

A fixed confidence threshold and morphological refinement are applied to obtain a more consistent mask.

The detected upper-body clothing pixels are replaced with a neutral gray region to create the agnostic representation.

---

## Garment Processing Pipeline

The garment preprocessing workflow is:

Garment Product Image
    |
    v
rembg Background Removal
    |
    v
RGBA Garment Image
    |
    v
Extract Alpha Channel
    |
    v
Binary Garment Mask

This separates the garment from the product-image background while retaining its shape.

---

## Edge Case Handling and Testing

The supplied edge cases were explicitly tested through the same processing pipeline.

### Hair Over Shoulders

`person_02` and `person_03` contain hair overlapping the shoulder/clothing region.

These images were processed using the same SCHP human parsing pipeline to evaluate whether clothing regions can still be separated when hair overlaps the upper body.

### Strappy Sleeveless Garment

`garment_03` contains a strappy sleeveless garment.

The garment is processed using rembg so that the garment silhouette, including thin strap regions, can be retained in the garment mask.

### Crossed Arms

`edge_cases/person_crossed_arms.jpg` tests human parsing when the arms overlap the upper-body clothing.

The image is processed using SCHP and both the required parsing map and agnostic representation are generated.

### Side Pose

The supplied side-pose image was additionally passed through the SCHP pipeline to test parsing and clothing-mask robustness under a non-front-facing body orientation.

### Seated Pose

The supplied seated-person image was additionally tested to observe the behavior of the parsing and agnostic-generation pipeline on a seated posture.

### No-Person Image

The supplied no-person image was also tested diagnostically.

SCHP is a human parsing model rather than a dedicated person detector. Therefore, no-person rejection should not rely only on SCHP output. In the complete web-demo pipeline, person validation is handled as a guardrail before try-on processing.

---

## No Filename-Specific Hardcoding

The edge-case outputs are not manually created or hardcoded for individual filenames.

The supplied images are passed through the same SCHP/rembg processing pipeline used for the normal inputs.

The implementation uses fixed processing hyperparameters such as:

- SCHP semantic clothing classes
- Confidence thresholds
- Morphological operations

These are general pipeline parameters and are not manually changed based on a particular test-image filename.

---

## Outputs

The pipeline generates:

### Person Outputs

- Human parsing maps
- Upper-body clothing masks
- Agnostic person representations

### Garment Outputs

- Background-removed garment images
- Binary garment masks

### Edge-Case Outputs

The crossed-arms case includes the required:

- Parsing map
- Agnostic person representation

Additional supplied edge cases were also tested for robustness.

---

## Folder Structure

Q2/
├── person/
├── garment/
├── edge_cases/
├── parsing_maps/
├── agnostic/
├── garment_masks/
├── Q2.ipynb
└── README.md

---

## Libraries Used

- PyTorch
- Transformers
- OpenCV
- NumPy
- Pillow
- rembg
- ONNX Runtime

---

## Implementation Notes

- SCHP performs semantic human parsing.
- Clothing masks are derived from SCHP probability outputs.
- Morphological processing is used to refine clothing-mask regions.
- Agnostic images replace the detected upper-body clothing region with a neutral gray region.
- rembg performs garment background removal.
- Garment masks are extracted from the resulting alpha channel.
- Edge cases are processed through the same general pipeline rather than using manually generated masks.

---

## Author

Nandana R
