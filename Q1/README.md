# Q1 - Garment & Body Understanding with a Vision-Language Model

## Objective

This task uses an open Vision-Language Model (VLM) to analyze a person image and a garment image and produce structured JSON attributes.

For each person-garment pair, the system extracts:

### Garment Attributes
- Type
- Sleeve length
- Neckline
- Primary color
- Pattern

### Person Attributes
- Pose category: `front-facing`, `side`, `seated`, or `unknown`
- Upper-body visibility
- Lower-body visibility

The final outputs follow the structure provided in `sample_output_q1.json`.

---

## Model Used

**MiniCPM-V 2.6 (INT4)**

- Framework: Hugging Face Transformers
- Inference: PyTorch
- Quantization: INT4
- Runtime: Google Colab
- Hardware: NVIDIA Tesla T4 GPU

### Why MiniCPM-V 2.6?

MiniCPM-V 2.6 was selected because it provides strong image understanding and instruction-following capabilities while remaining practical for Google Colab GPU environments.

The INT4 quantized version reduces GPU memory requirements, making inference feasible on a Tesla T4 while retaining the multimodal capabilities required for garment attribute extraction and human pose understanding.

It was suitable for this task because the same model can analyze both garment appearance and person/body information and return structured responses through carefully designed prompts.

---

## Pipeline

The Q1 pipeline follows these stages:

1. Load the person and garment images.
2. Resize large images while preserving aspect ratio.
3. Analyze the garment image using MiniCPM-V.
4. Extract:
   - garment type
   - sleeve length
   - neckline
   - primary color
   - pattern
5. Analyze the person image using MiniCPM-V.
6. Extract body orientation, posture, visible body parts, and visibility information.
7. Apply pose decision logic to obtain:
   - front-facing
   - side
   - seated
   - unknown
8. Apply additional checks for difficult pose cases.
9. Convert the model response into structured JSON.
10. Save the results in the `output/` directory.

---

## Pose Classification and Edge-Case Handling

The person-analysis pipeline uses structured VLM reasoning together with lightweight post-processing rules.

### Seated Detection

Seated posture has the highest priority. A focused second-pass check is used for difficult seated images.

If a seated person is detected:

`pose_category = "seated"`

This takes priority over body orientation.

### Side-Pose Detection

For potentially ambiguous body orientations, a focused torso-orientation check is performed.

The system focuses on torso and shoulder orientation rather than only face direction. Multiple orientation predictions are used for difficult cases to improve robustness.

### No-Person Detection

If the VLM reports no visible person and both upper- and lower-body visibility are false, the result is:

- `pose_category = "unknown"`
- `upper_body_visible = false`
- `lower_body_visible = false`

This output is also useful for the no-person guardrail used later in Q5.

---

## Edge Cases

The supplied edge-case images were tested separately.

Important required cases include:

- `person_side_pose.jpg` -> `side`
- `person_seated.jpg` -> `seated`
- `no_person.jpg` -> `unknown` with no upper/lower body detected

`person_crossed_arms.jpg` was also processed as part of the supplied edge-case set.

---

## Output Format

Example:

{
  "person_image": "person_01.png",
  "garment_image": "garment_01.jpg",
  "garment_attributes": {
    "type": "t-shirt",
    "sleeve_length": "short",
    "neckline": "crew",
    "primary_color": "purple",
    "pattern": "solid"
  },
  "person_attributes": {
    "pose_category": "front-facing",
    "upper_body_visible": true,
    "lower_body_visible": true
  },
  "model_used": "MiniCPM-V-2.6",
  "confidence_notes": ""
}

---

## Folder Structure

Q1/
├── person/
│   ├── person_01.png
│   ├── ...
│   └── person_05.png
├── garment/
│   ├── garment_01.jpg
│   ├── ...
│   └── garment_05.jpg
├── edge_cases/
├── output/
│   ├── pair_01.json
│   ├── pair_02.json
│   ├── pair_03.json
│   ├── pair_04.json
│   ├── pair_05.json
│   ├── all_test_pairs_q1.json
│   └── edge-case JSON outputs
├── Q1.ipynb
└── README.md

---

## Libraries Used

- PyTorch
- Transformers
- Pillow
- Pydantic
- SentencePiece
- Accelerate
- BitsAndBytes

---

## Implementation Notes

- Images are resized while preserving their aspect ratio.
- Model responses are parsed into structured JSON.
- JSON extraction includes fallback handling for malformed VLM responses.
- Deterministic inference is used where possible.
- INT4 quantization reduces GPU memory usage.
- Additional pose checks improve robustness on the supplied seated and side-pose edge cases.
- Final pair outputs were validated against the structure of `sample_output_q1.json`.

---

## Author

Nandana R  
B.Tech Computer Science (Artificial Intelligence)
