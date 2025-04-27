# team_7

# Project Readme

This repository contains notebooks exploring the use of OpenAI's GPT models for clinical decision support tasks related to sepsis management, developed as part of the Duke Datathon 2025.

## Notebook Descriptions

### `Duke datathon 2025.ipynb`

*   **Purpose:** Evaluates the baseline performance of `gpt-4-turbo` in recommending initial fluid resuscitation volumes for synthetic sepsis patient cases. 
*   **Methodology:**
    *   Generates 10 sets of synthetic patient data based on randomized "baseline" sepsis profiles.
    *   Within each set, creates 10 patient variations where only the `body weight` (ranging from 40kg to 130kg) is systematically changed.
    *   Queries `gpt-4-turbo` with a structured prompt containing the patient data for each of the 100 cases, asking *only* for the numerical fluid volume in liters. **Note:** No external guideline text is explicitly injected into the prompt in this notebook.
    *   Stores the patient data, prompts, and GPT responses in a Pandas DataFrame.
    *   Visualizes the relationship between patient body weight and the GPT-recommended fluid volume for each baseline set, fitting linear regression lines to observe trends.

### `RAG_Pipeline_Team-7.ipynb`

*   **Purpose:** Implements and evaluates a Retrieval-Augmented Generation (RAG) pipeline to improve GPT's fluid resuscitation recommendations by grounding them in specific clinical guidelines. Compares `gpt-4o` and `gpt-4`.
*   **Methodology:**
    *   Loads and processes the "Surviving Sepsis Campaign: International Guidelines 2021" PDF (`surviving_sepsis_campaign__international.21.pdf`).
    *   Splits the guideline text into chunks.
    *   Generates text embeddings for each chunk using `sentence-transformers` (`all-MiniLM-L6-v2`).
    *   Builds a FAISS vector index for efficient similarity searching (retrieval).
    *   Generates synthetic patient cases similar to the first notebook (10 baselines, 10 weight variations each).
    *   **RAG Implementation:** For each patient case:
        *   Retrieves the top 3 most relevant guideline chunks based on a query ("fluid resuscitation initial amount sepsis").
        *   Constructs a prompt containing *both* the patient data *and* the retrieved guideline context.
        *   Queries `gpt-4o` using this augmented prompt, asking for the numerical fluid volume.
    *   Stores results and visualizes the weight vs. recommended fluid relationship *after* guideline injection.
    *   **Comparison:** Repeats the experiment using `gpt-4` instead of `gpt-4o`.
    *   Visualizes the comparison between `gpt-4o` and `gpt-4` performance with RAG.

### `Duke datathon 2025 (Anti).ipynb`

*   **Purpose (Inferred):** Evaluates and potentially compares different GPT models or configurations on their ability to recommend appropriate *antibiotics* for synthetic sepsis patient cases.
*   **Methodology (Inferred from outputs and filename):**
    *   Likely generates synthetic patient cases similar to the other notebooks.
    *   Queries one or more GPT models with prompts asking for *antibiotic* recommendations (distinct from the fluid task).
    *   The code suggests a function `run_experiment_multiple_models()` was used.
    *   Saves results to `gpt_antibiotic_recommendation_multiple_models.csv`.
    *   Includes a function `plot_correct_rates(df)`, indicating the analysis focuses on the *correctness* of the antibiotic recommendations rather than just the output value or its relation to weight.