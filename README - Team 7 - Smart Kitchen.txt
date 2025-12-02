# Smart Kitchen AI – AI-Powered Smart Home Kitchen Assistant

An AI-powered smart kitchen assistant that:
- Detects ingredients from fridge images using **MobileNetV2 + OpenCV**
- Tracks inventory and prioritizes items near expiration
- Generates **personalized recipes** using **GPT-2 + Retrieval-Augmented Generation (RAG)**
- Models recipe selection as a **Constraint Satisfaction Problem (CSP)** to minimize food waste
- Provides a friendly **Streamlit UI** with a chef avatar, YouTube links, and audio recipe narration

This project was built as part of the **AI-Powered Smart Home Kitchen** system at NITK Surathkal.

---

## Key Features

- **Ingredient Detection**
  - Uses a MobileNetV2-based model to detect fruits and vegetables from fridge images.
  - Robust to different lighting conditions, cluttered scenes, and varied arrangements.

- **Smart Recipe Planning (CSP)**
  - Recipes are chosen only if:
    - All required ingredients are available.
    - Ingredients are not expired.
  - An urgency utility function prioritizes recipes that consume ingredients closest to expiry, reducing food waste.

- **Hybrid Recipe Generation (GPT-2 + RAG)**
  - Fine-tuned GPT-2 on culinary data to generate coherent, diverse recipes.
  - RAG layer retrieves factual recipes (e.g., via Spoonacular API) and grounds GPT-2 outputs in real recipes.
  - Ensures the recipes are both creative **and** feasible with the current ingredient set.

- **User Interaction**
  - Streamlit web UI with:
    - Chef avatar animation
    - Image upload for fridge photos
    - Text-based interaction
    - YouTube video links for tutorials
    - Audio narration of generated recipes (via gTTS)

- **Impact**
  - Aims to reduce household food waste by making it easier to:
    - See what’s in the fridge
    - Use ingredients before they spoil
    - Plan meals efficiently

---

## System Architecture (High-Level)

1. **Image Input**
   - User uploads a fridge image (sample in `assets/sample_fridge.jpg`).

2. **Ingredient Detection (Vision Module)**
   - Images are preprocessed with OpenCV (resize, normalization).
   - MobileNetV2 detects objects and outputs class labels + confidence scores.
   - Non-Maximum Suppression (NMS) filters overlapping boxes.
   - Labels are mapped into a standardized ingredient taxonomy.

3. **Inventory & Expiration Tracking (CSP Formulation)**
   - Ingredient set: \(I = \{i_1, i_2, ..., i_n\}\)
   - Expiration set: \(E = \{e_1, e_2, ..., e_n\}\)
   - Recipe set: \(R = \{r_1, r_2, ..., r_m\}\)
   - Constraints:
     - **Availability:** all ingredients required by a recipe must be in \(I\)
     - **Not expired:** all ingredients must have expiration date > current date
   - **Urgency objective:** prioritize recipes that use ingredients closer to expiry.

4. **RAG + GPT-2 Recipe Generation**
   - Retrieve relevant recipes from external API / vector store (FAISS + BERT embeddings).
   - Condition GPT-2 on:
     - Detected ingredients
     - Retrieved recipes
     - User preferences (diet, cuisine, etc.).
   - Generate step-by-step recipe instructions.

5. **Presentation Layer (Streamlit)**
   - Displays:
     - Ingredient list
     - Suggested recipes
     - Step-by-step instructions
     - YouTube video links
     - Optional audio narration for accessibility.

---

## Project Structure

```text
smart-kitchen-ai/
├── app/
│   ├── app7.py              # Streamlit app entrypoint
│   └── model/
│       └── mymodel.h5       # Trained MobileNetV2 model (NOT committed)
│
├── assets/
│   ├── avatar.gif           # Chef avatar animation
│   └── sample_fridge.jpg    # Sample image for demo/testing
│
├── docs/
│   └── AI_Project_Report.pdf  # Full technical paper & methodology
│
├── README.md
├── requirements.txt
├── .gitignore
└── LICENSE
