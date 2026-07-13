# Q2 - Human Parsing and Agnostic Representation

## Objective

Generate an agnostic person representation and garment masks as preprocessing steps for an AI-based virtual try-on pipeline.

## Models Used

### Human Parsing
- SCHP (pirocheto/schp-atr-18)

### Garment Segmentation
- rembg

## Model Selection and Comparison

| Model | Advantages | Limitations | Decision |
|--------|------------|-------------|----------|
| **SCHP (Selected)** | Specifically designed for human parsing, predicts body-part labels (upper clothes, arms, face, pants), lightweight (~267 MB), easy to run on Colab | Thin shoulder straps may occasionally be classified as arm regions | ✅ Selected |
| **SAM + Grounding DINO** | High-quality segmentation, flexible object detection | Requires multiple large models, slower inference, more complex pipeline | Not selected |
| **DensePose** | Accurate body surface and pose estimation | Does not directly provide clothing segmentation | Not selected |

## Pipeline

1. Load person image.
2. Run SCHP human parsing.
3. Generate parsing map.
4. Create clothing mask.
5. Generate agnostic image.
6. Process edge cases.
7. Generate garment masks using rembg.

## Edge Case Handling

- Crossed arms
- Side pose
- Seated person
- Sleeveless garments (confidence-based refinement)
- No-person image

## Outputs

- parsing_maps/
- agnostic/
- garment_masks/
- edge_cases/

## Libraries

- PyTorch
- Transformers
- OpenCV
- NumPy
- Pillow
- rembg
- ONNX Runtime

## Author

Nandana R
