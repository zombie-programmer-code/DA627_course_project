# DA627_course_project
# Multimodal Retrieval-Augmented Generation (RAG) for Image Captioning

This project implements a **multimodal Retrieval-Augmented Generation (RAG) pipeline** for image captioning by combining image-text embeddings (CLIP) with vision-language models (Phi-3.5, LLaVA).

---

## Overview

Given an input image, the system:
1. Computes image embeddings using CLIP
2. Retrieves semantically similar captions from a dataset
3. Uses these captions as context for a vision-language model
4. Generates a grounded caption

The goal is to evaluate whether **retrieval improves caption generation quality**.

---

## Key Insight

> **Retrieval quality is the primary bottleneck in multimodal RAG systems.**

From experiments:
- RAG improves performance across all metrics
- Stronger models (Phi-3.5) outperform older ones (LLaVA)
- Encoder choice has minimal impact
- Retrieval success (Hit@K) strongly determines output quality

---

## Pipeline
Image → CLIP Embedding → Caption Retrieval → Prompt Construction → LLM → Output Caption


Steps:
- Precompute caption embeddings
- Compute image embedding
- Retrieve top-k captions (cosine similarity)
- Construct prompt with retrieved captions
- Generate output using vision-language model

---

## Datasets

### Flickr30k
- ~31K images, ~158K captions (~5 per image)
- Diverse descriptions for robust embedding space

### MS COCO (val2017 subset)
- ~5K images, ~5 captions per image
- More complex and diverse real-world scenes
- Subset used due to Kaggle GPU/memory limits

---

## Evaluation Metrics

- **BLEU** → n-gram precision
- **ROUGE-1/2/L** → overlap and fluency
- **CLIPScore** → image-text semantic alignment

---

## Results

### RAG vs No RAG
- RAG improves all metrics across both datasets
- Larger gains on MS COCO → due to higher semantic diversity

### Model Comparison (Phi-3.5 vs LLaVA)
- Phi-3.5 consistently outperforms LLaVA
- Better multimodal alignment despite fewer parameters (~4.2B vs ~7B)

### Encoder Comparison (ViT-B/16 vs ViT-B/32)
- Minimal difference in BLEU/ROUGE
- Small differences in CLIPScore → semantic alignment effect

### Recall Study (Most Important Result)
- If **Hit@K = 1 → large performance boost**
- If **Hit@K = 0 → poor outputs**
- Increasing K introduces **noise vs coverage tradeoff**

---

## Key Finding

> **RAG effectiveness depends more on retrieval quality than model size.**

---

## Tech Stack

- PyTorch
- HuggingFace Transformers
- CLIP (OpenCLIP)
- FAISS (optional for retrieval)
- Kaggle GPU environment

---

## Repository Structure
├── notebooks/
│ ├── da627-project-rag-no-rag.ipynb
│ ├── da627-project-rag-no-rag-mscoco.ipynb
│ ├── da627-project-llm-study.ipynb
│ ├── da627-project-llm-study-mscoco.ipynb
│ ├── da627-project-encoder-study.ipynb
│ ├── da627-project-encoder-study-mscoco.ipynb
│ ├── da627-project-recall-study.ipynb
│ ├── da627-project-recall-study-mscoco.ipynb
│ └── hello.py
│
├── results/
│ ├── *.csv (experimental results)
│
├── DA627 Final presentation-220150019.pdf
├── DA627_220150019_course_project_report.pdf
├── LICENSE
└── README.md

---

## How to Run

1. Install dependencies as given in the notebooks
2. Run notebooks in order:
- rag-no-rag
- llm-study
- encoder-study
- recall-study

---

## Future Work

- Improve retrieval (reranking)
- Adaptive top-k selection
- Fine-grained (entity-level) retrieval
- Test stronger retrievers
- Scale to full MS COCO
- Add human evaluation

---

## Report & Slides

- Report: `DA627 Final presentation-220150019.pdf`
- Slides: `DA627_220150019_course_project_report.pdf`

---

## Author

Saptarshi Mukherjee  
B.Tech Data Science & AI, IIT Guwahati
