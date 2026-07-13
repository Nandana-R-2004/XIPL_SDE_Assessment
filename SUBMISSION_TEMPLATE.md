# Virtual Try-On Assessment - Submission

**Candidate name:** Nandana R  
**Email:** nandanaremeshm@gmail.com  
**Date:** 13 July 2026  
**Demo video link (max 5 min):** https://drive.google.com/drive/folders/1ke5BDm3Bzz3fHIZoPs_8X6qCysA7IWZH?usp=sharing 
**Colab notebook links (if used):** https://drive.google.com/drive/folders/16H3wUwfTsx7UaGiAO3iqL5nUoOHaVas0?usp=sharing

---

# Q1 - Garment & Body Understanding

### VLM chosen and why

I used a Vision Language Model (VLM) to extract garment and person attributes from the input images. The model identifies clothing type, color, sleeve style, neckline, fit, and other visual attributes. These attributes are used as metadata for the virtual try-on pipeline.

### How to run

1. Open the **Q1** notebook.
2. Run all cells sequentially.
3. Place the person and garment images in the required input folders.
4. The notebook generates garment and person attributes in JSON format.

### Known limitations

- Performance depends on image quality.
- Occluded garments may produce incomplete attributes.
- Very unusual clothing styles may not be recognized accurately.

---

# Q2 - Human Parsing & Segmentation

### Models used

- SCHP Human Parsing
- AutoMasker
- Rembg (background removal)

### How to run

1. Open the **Q2** notebook.
2. Run all cells.
3. The notebook generates:
   - Human parsing maps
   - Agnostic person image
   - Clothing mask
   - Garment mask

### Edge cases handled

- Images with complex backgrounds
- Partial body visibility
- Clothing mask refinement

### Known failures

- Heavy occlusions
- Multiple people in the image
- Very low-resolution images

---

# Q3 - End-to-End Try-On

### Try-on model chosen and why

**CatVTON**

CatVTON was selected because it is an open-source diffusion-based virtual try-on model that produces realistic garment transfer while preserving the person's identity.

### Hardware used

- Google Colab
- NVIDIA T4 GPU

### Constraints hit and workarounds

- Large model downloads
- GPU memory limitations
- Dependency version conflicts (NumPy, Diffusers, Transformers)

Resolved by:

- Using compatible package versions
- Running inference in Google Colab
- Using pre-generated masks from Q2

### How to run

1. Open the **Q3** notebook.
2. Run all cells.
3. Load person image, garment image, and generated mask.
4. CatVTON generates the try-on result.
5. Results are saved in the output folder.

---

# Q4 - Automated Quality Evaluation

### Metrics implemented

- CLIP Similarity (Garment Fidelity)
- SSIM (Identity Preservation)
- VLM-as-Judge Score

### VLM-as-Judge Prompt

The generated image is evaluated based on:

- Garment fidelity
- Identity preservation
- Overall realism
- Fit consistency
- Visual quality

The VLM assigns a quality score based on these criteria.

### Results

The evaluation results are recorded in:

```
evaluation_template_q4.csv
```

---

# Q5 - Mini Web Demo

### Framework

Gradio

### Features

- Upload person image
- Upload garment image
- Generate virtual try-on
- Display garment attributes
- Display evaluation scores
- Display processing time

### Guardrails implemented

- Reject image if no person is detected.
- Warn if the person is seated.
- Warn if the person is in a side pose.

### How to launch

1. Open the **Q5** notebook.
2. Run all cells.
3. Launch the Gradio interface.
4. Upload a person image and a garment image.
5. Click **Generate Try-On**.
6. View the generated try-on result along with garment attributes, evaluation scores, and guardrail messages.

---

# Repository Structure

```
XIPL_SDE_Assessment
│
├── SUBMISSION_TEMPLATE.md
├── Q1
├── Q2
├── Q3
├── Q4
├── Q5
└── evaluation_template_q4.csv
```

---

# Honest Failure Log

During development, several challenges were encountered:

- Dependency conflicts between NumPy, Diffusers, Transformers, and Gradio in Google Colab.
- CatVTON required multiple package version adjustments.
- Hugging Face model downloads occasionally failed due to version mismatches.
- GPU runtime resets required reinstallation of dependencies.
- Initial Gradio integration displayed placeholder outputs instead of the generated try-on images. This was resolved by connecting the application to the actual CatVTON output pipeline.
- Some edge-case images (multiple people or extreme poses) produced less accurate segmentation or garment transfer.

All identified issues were documented and addressed to produce a working end-to-end virtual try-on pipeline.
