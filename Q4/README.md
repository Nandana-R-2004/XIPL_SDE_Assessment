# Q4 - Automated Quality Evaluation of Try-On Results

## Objective

Evaluate the quality of the virtual try-on results generated in Q3 using three complementary evaluation metrics.

---

## Evaluation Metrics

### 1. Garment Fidelity

**Model Used:** OpenCLIP (ViT-B-32)

Measures semantic similarity between the original garment image and the garment appearing in the generated try-on result.

---

### 2. Identity Preservation

**Method Used:** Structural Similarity Index (SSIM)

Measures how well the generated try-on image preserves the appearance of the original person.

---

### 3. VLM-as-Judge

**Vision Language Model:** MiniCPM-V-2_6-int4 (Q1)

Evaluation Rubric:

- Garment realism
- Fit realism
- Texture transfer
- Visual artifacts

Returns:

- Overall score (1–10)
- Short explanation

---

## Judge Prompt

You are evaluating a virtual try-on result.

Evaluate the generated image based on:

1. Garment realism
2. Fit realism
3. Texture transfer
4. Visible artifacts

Return:

{
  "score": <1-10>,
  "reason": "<short explanation>"
}

---

## Output

Generated file:

evaluation_template_q4.csv

---

## Technologies Used

- OpenCLIP
- PyTorch
- scikit-image (SSIM)
- pandas
- Pillow
- MiniCPM-V-2_6-int4

---

Author: Nandana R
