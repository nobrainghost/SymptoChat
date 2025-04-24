# SymptoChat
An AI-powered Conversational Symptom Assessor tool designed for preliminary symptom assessment. Users describe their symptoms in natural language, answer a targeted clarifying question, and receive a ranked list of potential ailments for informational exploration. Note: This is not a diagnostic tool and does not replace professional medical advice

# Conversational AI for Preliminary Medical Assessment

## Overview

This project focuses on developing r a conversational AI bot designed to offer preliminary insights into potential medical conditions based on user-described symptoms. The system engages users in a simplified dialogue, utilizing a disease-symptom knowledge base enhanced by Natural Language Processing (NLP) and vector embeddings to identify and rank potential ailments, refining its suggestions through targeted questioning.

## Core Objective

The primary goal of this phase is to build and validate a system that can:

1.  Accept user-reported symptoms described in natural language.
2.  Interpret these symptoms and map them to a structured knowledge base.
3.  Leverage semantic understanding (via text embeddings) to identify an initial set of potential matching diseases.
4.  Intelligently select and ask a single, differentiating question to help narrow down possibilities.
5.  Present a refined, ranked list of potential ailments based on the user's input and response.

## Methodology & Workflow

The approach integrates a structured knowledge base with modern NLP techniques to enable semantic search and targeted refinement:

1.  **Knowledge Base Foundation:** The process begins with a structured dataset mapping diseases to associated symptoms (initially represented as binary indicators).

2.  **Symptom Description Enhancement via LLM:** Recognizing that binary indicators lack nuance, a key step involves using a Large Language Model (LLM). The LLM generates richer, more descriptive text for each disease based on its associated symptoms (e.g., transforming "Disease A: Symptom X=1, Symptom Y=1" into "Disease A is often characterized by symptoms such as [description of X] and [description of Y]"). This creates a more semantically meaningful representation of each disease.

3.  **Embedding Generation:** The enhanced, text-based disease descriptions are converted into high-dimensional vector representations (embeddings) using a pre-trained sentence embedding model. This captures the semantic meaning of the descriptions.

4.  **User Symptom Input & Interpretation:** The user describes their symptoms in natural language (e.g., "I have a high fever and a really bad cough"). Basic NLP techniques (like keyword matching, synonym handling, and potentially embedding comparison) are used to understand the input and identify relevant symptom concepts from the knowledge base.

5.  **Initial Semantic Search & Ranking:**
    * The user's symptom description is embedded using the same model used for diseases.
    * Cosine similarity (or a similar metric) is calculated between the user's symptom embedding and all disease embeddings.
    * Diseases are ranked based on this similarity, providing an initial list of potential candidates. Let's say the top candidates based on "high fever and a really bad cough" are:
        * Flu
        * Common Cold
        * Pneumonia
        * Bronchitis

6.  **Identifying Differentiating Symptoms:** The system analyzes the characteristic symptoms associated with these top-ranked potential diseases *from the original structured dataset* to find key differences.
    * Flu: (Fever, Cough, **Body Aches**, Fatigue, ...)
    * Common Cold: (Fever, Cough, **Runny Nose**, Sneezing, ...)
    * Pneumonia: (Fever, Cough, **Shortness of Breath**, Chest Pain, ...)
    * Bronchitis: (Cough, **Mucus Production**, Fatigue, Mild Fever, ...)

    The system identifies symptoms that are present in some of these conditions but typically absent or less prominent in others. For instance, `Shortness of Breath` is a key indicator for Pneumonia but less common or severe in the initial stages of Flu, Common Cold, or Bronchitis. Similarly, `Body Aches` strongly suggest Flu.

7.  **Targeted Clarification & Narrowing Down (Example):**
    Based on the analysis above, the system selects a high-value differentiating symptom to ask about. The goal is to choose a symptom that can effectively rule in or rule out a significant portion of the current candidates.

    * **Question Formulation:** The AI might ask: *"Have you also experienced any shortness of breath?"*

    * **User Response & Logic:**
        * **If the user answers "Yes":** The system significantly increases the likelihood score for diseases where `Shortness of Breath` is a key symptom (like Pneumonia). Diseases where this symptom is uncommon (like Common Cold, potentially early Flu/Bronchitis) would be deprioritized or removed from the top considerations. The potential list narrows significantly towards conditions matching this positive confirmation.
        * **If the user answers "No":** The system reduces the likelihood score for diseases strongly associated with `Shortness of Breath` (like Pneumonia). Conditions where this symptom is typically absent (like Common Cold, potentially Flu/Bronchitis) remain as stronger possibilities relative to Pneumonia. The potential list narrows by excluding conditions strongly indicated by the absent symptom.

8.  **Refined Ranking:** The user's response ("yes" or "no") is used to update the relevance scores or re-rank the potential diseases. Conditions consistent with the *entire* interaction (initial symptoms + clarifying answer) are ranked higher.

9.  **Presenting Preliminary Suggestions:** A final, refined list of top potential ailments (e.g., after the "No" to shortness of breath, perhaps Flu and Bronchitis are ranked highest) is presented to the user, accompanied by the prominent disclaimer emphasizing this is not a diagnosis.

## Knowledge Base Source

* **Dataset:** "Stark, Bran (2025), “Disease and symptoms dataset 2023”, Mendeley Data, V1, doi: 10.17632/2cxccsxydc.1 "
* **Source:** [https://data.mendeley.com/datasets/2cxccsxydc/1](https://data.mendeley.com/datasets/2cxccsxydc/1)
* **Enhancement:** Utilizes LLMs to generate descriptive text from binary symptom associations for improved embedding quality.





