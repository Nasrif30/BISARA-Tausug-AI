# BISSARA AI — Phase 2, Version 1.0

**An Artificial Intelligence System for the Preservation of the Tausug Language, History, and Culture**

---

## Overview

BISSARA AI Phase 2 Version 1.0 represents the first production-grade fine-tuned language model built specifically for the Tausug-speaking community. Built on the Qwen2.5-1.5B architecture and fine-tuned using the Unsloth framework, this release marks the culmination of the Phase 2 dataset consolidation effort — merging lexical, grammatical, historical, cultural, and identity-based knowledge into a single unified training corpus.

The model is capable of translating between Tausug (Bahasa Sug), English, and Filipino, explaining cultural concepts, narrating historical accounts, and engaging in natural conversations rooted in Tausug heritage.

---

## System Architecture

<img width="8192" height="7783" alt="final tausug phase 2 final" src="https://github.com/user-attachments/assets/806f7708-4e3c-4357-befd-d8d2ca72edde" />


---

## Model Information

| Property             | Details                                      |
|----------------------|----------------------------------------------|
| Base Model           | Qwen2.5-1.5B (Instruct)                      |
| Fine-Tuning Method   | LoRA via Unsloth                             |
| Training Steps       | 3,090                                        |
| Training Epochs      | 3                                            |
| Dataset Size         | 8,238 examples                               |
| Quantization Format  | GGUF Q4_K_M                                  |
| Model Size           | ~986 MB                                      |
| Inference Engine     | Ollama / llama.cpp                           |
| HuggingFace Model    | alnasrifhaliddin/BISSARA-v1.0                |
| HuggingFace GGUF     | alnasrifhaliddin/BISSARA-v1.0-GGUF           |

---

## Phase 2 Dataset Composition

The Phase 2 master dataset was assembled from the following primary sources:

- **Tausug Dictionary** — Lexical entries with example sentences and English translations
- **Tausug Grammar Reference** — Grammatical rules, verb focus systems, and sentence structures
- **Tausug Alphabet and Phonology** — Character mappings, pronunciation guides, and orthographic conventions
- **Tausug History and Narratives** — Historical accounts of the Sulu Sultanate and Bangsamoro heritage
- **Tausug Folklore and Stories** — Cultural stories, folktales, and oral tradition transcriptions
- **Pangalay and Sulu Sea Studies** — Documentation of the traditional Pangalay dance and maritime culture
- **Identity and Reflections** — Curated conversational data establishing BISSARA AI's persona and mission

All entries were processed through a master pipeline that stripped system prompts, removed duplicates, validated message structure, and ensured dataset integrity before training.

---

## How to Run Locally

**Requirements:** Ollama installed on your machine.

**Step 1 — Pull the model:**
```bash
ollama run hf.co/alnasrifhaliddin/BISSARA-v1.0-GGUF:Q4_K_M
```

**Step 2 — Create the Modelfile:**

Create a file named `Modelfile` in your working directory with the exact content below. This file tells Ollama how to format prompts for BISSARA AI and where to stop generating output.

```
# BISSARA AI v1.0 — Ollama Modelfile
# Model: Qwen2.5-1.5B fine-tuned on Tausug Phase 2 Dataset
# Developer: Alnasrif Jal-usman Haliddin

FROM hf.co/alnasrifhaliddin/BISSARA-v1.0-GGUF:Q4_K_M

# Chat template — must match the training data format exactly.
# BISSARA AI was trained without system prompts, so only user/assistant turns are used.
TEMPLATE """<|im_start|>user
{{ .Prompt }}<|im_end|>
<|im_start|>assistant
"""

# Stop tokens — Ollama will cut generation immediately when any of these appear.
# These cover the Qwen2.5 special token set to prevent trailing garbage output.
PARAMETER stop "<|im_end|>"
PARAMETER stop "<|im_start|>"
PARAMETER stop "<|endoftext|>"

# Generation parameters
PARAMETER temperature 0.7
PARAMETER top_p 0.9
PARAMETER repeat_penalty 1.1
```

**Step 3 — Build and run:**

```bash
ollama create bissara -f Modelfile
ollama run bissara
```

**Important notes on the Modelfile:**

| Parameter          | Value   | Purpose                                                              |
|--------------------|---------|----------------------------------------------------------------------|
| `temperature`      | 0.7     | Controls creativity. Lower = more precise. Higher = more varied.     |
| `top_p`            | 0.9     | Limits token sampling to the most probable 90% at each step.         |
| `repeat_penalty`   | 1.1     | Discourages the model from repeating the same phrase in a loop.      |
| `stop` tokens      | 3 rules | Forces the model to stop at the correct end-of-turn boundaries.      |

---

## Known Limitations (v1.0)

- The base Qwen2.5 model may occasionally append residual tokens from its pretraining corpus at the tail end of a response. This is a known artifact of the base model and will be resolved in v1.1 with improved tokenizer configuration.
- Identity responses may vary in specificity across sessions due to the generative nature of large language models.
- The model is optimized for Tausug-English interaction and is not designed for general-purpose tasks.

---

## Roadmap

| Version  | Target                                                                 |
|----------|------------------------------------------------------------------------|
| v1.0     | Initial production release — Qwen2.5-1.5B LoRA fine-tune (current)    |
| v1.1     | Retrain with clean dataset, correct tokenizer template, stop token fix |
| v2.0     | Expanded dataset, improved multi-turn dialogue, GGUF optimization      |

---

## Acknowledgments

BISSARA AI was developed by **Alnasrif Jal-usman Haliddin** as a personal initiative to bridge the gap between artificial intelligence and the Tausug linguistic and cultural heritage.

This project would not exist without the exceptional support, mentorship, and contributions of the following individuals:

---

### Khalid Usman
**Tausug Developer and Mentor**

Khalid Usman provided the foundational technical guidance that shaped the development direction of BISSARA AI from its earliest stages. As a developer deeply rooted in both the Tausug community and the discipline, his mentorship as a teacher gave this project its technical foundation and cultural grounding. His portfolio and work can be found at [kusman.me](https://kusman.me/).

---

### Dr. Ahmad Sampang Ibn Hajiri
**Academic Adviser and Data Resource Provider**

Dr. Ahmad Sampang Ibn Hajiri contributed significantly to the integrity of this project by providing access to essential Tausug language data resources and offering academic guidance throughout the research and development process. His scholarly expertise ensured that the linguistic and historical knowledge embedded in BISSARA AI is grounded in authentic and credible sources. (https://www.facebook.com/asmusahari)

---

## License

- **LICENSE** – Apache License 2.0  
  https://github.com/Nasrif30/BISSARA-Tausug-AI/blob/main/LICENSE
  See the **LICENSE** file for the complete license terms.

---

*BISSARA AI — Preserving the language, history, and identity of the Tausug people through artificial intelligence.*
