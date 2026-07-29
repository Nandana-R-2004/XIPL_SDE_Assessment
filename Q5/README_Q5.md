# Q5 - Mini Virtual Try-On Web Application

## Objective
This task integrates the outputs of **Q1, Q2, Q3, and Q4** into a single Gradio-based Virtual Try-On application.

## Features
- Upload person image
- Upload garment image
- Generate virtual try-on result
- Display garment attributes
- Display evaluation metrics
- Guardrails for invalid inputs
- Show processing time

## Models Used
- MiniCPM-V 2.6 (INT4)
- CatVTON
- OpenCLIP
- Face SSIM
- MiniCPM-VLM

## Running in Google Colab
1. Open **Q5.ipynb**.
2. Enable GPU:
   Runtime → Change runtime type → GPU.
3. Run all notebook cells from top to bottom.
4. Wait for the Gradio URL.
5. Open the URL.
6. Upload a person image and a garment image.
7. Click **Generate Try-On**.

## Guardrails
- Rejects images with no person detected.
- Warns for seated poses.
- Warns for side-view poses.

## Outputs
- Virtual Try-On Image
- Garment Attributes
- Garment Fidelity Score
- Identity Preservation Score
- VLM-as-Judge Score
- Processing Time

## Libraries
- Gradio
- PyTorch
- Transformers
- Diffusers
- OpenCV
- Pillow
- NumPy
- OpenCLIP

## Author
**Nandana R**
B.Tech Computer Science (Artificial Intelligence)
