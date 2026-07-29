# AI Virtual Try-On Pipeline
## XIPL SDE Intern Technical Assessment

An end-to-end AI-powered Virtual Try-On system built using open-source vision-language and computer vision models. This project covers garment understanding, human parsing, virtual try-on generation, automated evaluation, and an interactive Gradio web application.

---

## Project Overview

This repository contains solutions for all five questions of the XIPL SDE Intern Technical Assessment.

The pipeline performs:

- Garment attribute extraction using a Vision Language Model
- Human parsing and garment segmentation
- Agnostic person image generation
- Virtual try-on using CatVTON
- Automated quality evaluation
- Interactive Gradio demo with guardrails

The project is implemented using only open-source models and was developed primarily in Google Colab.

---

# Repository Structure

```
XIPL_SDE_Assessment/

├── Q1/
│   └── Q1.ipynb
│
├── Q2/
│   └── Q2.ipynb
│
├── Q3/
│   └── Q3.ipynb
│
├── Q4/
│   └── Q4.ipynb
│
├── Q5/
│   ├── app.py
│   ├── pipeline.py
│   ├── requirements.txt
│   └── Q5.ipynb
│
└── README.md
```

---

# Project Workflow

```
Person Image
        │
        ▼
 MiniCPM-V Attribute Extraction
        │
        ▼
SCHP Human Parsing
        │
        ▼
Agnostic Image Generation
        │
        ▼
Garment Background Removal
        │
        ▼
CatVTON Virtual Try-On
        │
        ▼
Generated Try-On Image
        │
        ▼
OpenCLIP + Face SSIM Evaluation
        │
        ▼
Gradio Web Application
```

---

# Models Used

## Q1 – Vision Language Model

**Model:** MiniCPM-V-2.6 (INT4)

Purpose:

- Garment understanding
- Person understanding
- Structured JSON generation

Generated Attributes:

- Garment Type
- Sleeve Length
- Neckline
- Primary Color
- Pattern
- Person Pose
- Upper Body Visibility
- Lower Body Visibility

---

## Q2 – Human Parsing

**Model:** SCHP (Self-Correction Human Parsing)

Purpose:

- Human segmentation
- Parsing map generation
- Clothing region extraction
- Agnostic image creation

---

## Garment Segmentation

Background removal is performed to generate clean garment masks suitable for virtual try-on preprocessing.

---

## Q3 – Virtual Try-On

**Model:** CatVTON

Purpose:

- Generate realistic virtual try-on images
- Preserve person identity
- Transfer garment appearance

---

## Q4 – Automated Evaluation

Evaluation metrics include:

- Garment Fidelity using OpenCLIP
- Identity Preservation using Face SSIM
- Overall quality assessment

---

## Q5 – Gradio Web Application

The Gradio application integrates the complete pipeline into an interactive interface.

Features include:

- Upload person image
- Upload garment image
- Automatic preprocessing
- Virtual try-on generation
- Garment attribute display
- Evaluation score display
- Processing time estimation

Guardrails:

- Rejects images without a detectable person
- Warns when side-view images are uploaded
- Warns when seated poses are detected

---

# Question-wise Summary

## Q1 – Garment & Body Understanding

Implemented:

- Vision-language based image understanding
- Automatic attribute extraction
- JSON output generation

---

## Q2 – Human Parsing & Garment Segmentation

Implemented:

- Human parsing maps
- Agnostic person generation
- Garment segmentation
- Edge-case handling

---

## Q3 – End-to-End Virtual Try-On

Implemented:

- Image preprocessing
- CatVTON inference
- Final try-on image generation

---

## Q4 – Automated Quality Evaluation

Implemented:

- Garment similarity scoring
- Identity preservation scoring
- Automated evaluation pipeline

---

## Q5 – Interactive Demo

Implemented:

- Complete Gradio interface
- End-to-end inference
- Guardrails
- Result visualization

---

# Technologies Used

- Python
- Google Colab
- PyTorch
- OpenCV
- NumPy
- Pillow
- Transformers
- Gradio

---

# Installation

Clone the repository:

```bash
git clone https://github.com/<your-username>/<repository-name>.git

cd <repository-name>
```

Install dependencies:

```bash
pip install -r requirements.txt
```

---

# Running the Project

Launch the Gradio application:

```bash
python app.py
```

or

```bash
python Q5/app.py
```

depending on your directory structure.

---

# Outputs

The project generates:

- Garment attribute JSON
- Human parsing maps
- Agnostic person images
- Garment masks
- Virtual try-on images
- Automated evaluation scores

---

# GPU Constraints

The project was developed and tested using Google Colab GPU.

During implementation:

- Memory usage was managed through sequential execution.
- Intermediate outputs were reused wherever possible.
- Lightweight open-source models were selected to improve compatibility with Colab environments.

---

# Open-Source Models

| Model | Purpose |
|--------|---------|
| MiniCPM-V-2.6 | Vision Language Model |
| SCHP | Human Parsing |
| CatVTON | Virtual Try-On |
| OpenCLIP | Garment Fidelity Evaluation |

---

# Future Improvements

- Support multiple garment categories
- Higher-resolution inference
- Faster preprocessing
- Batch processing
- Cloud deployment
- Improved quality evaluation metrics

---

# Author

**Nandana R**

B.Tech Computer Science (Artificial Intelligence)

GitHub: https://github.com/Nandana-R-2004

---

# Acknowledgements

This project was developed as part of the **XIPL SDE Intern Technical Assessment** using publicly available open-source research models and libraries.

Special thanks to the developers and researchers behind:

- MiniCPM-V
- SCHP
- CatVTON
- OpenCLIP
- Gradio
