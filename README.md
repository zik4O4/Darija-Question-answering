# Darija Medical Question Answering

A research portfolio for single-turn medical question answering in Moroccan Darija, with synthetic sample data, preprocessing utilities, reported model comparisons and a Streamlit inference interface.

## Project status

This repository documents an academic project and provides a lightweight demonstration scaffold. It does not include the original MedQA-MA corpus, trained model weights or a complete executable fine-tuning pipeline.

| Available component | Current behavior |
|---|---|
| Data utilities | Clean, normalize and split synthetic QA examples |
| Training notebooks | Display configurations/plans for Atlas-Chat, Llama 3.1, Mistral, Phi and AraBART |
| Demo interface | Display an explanatory fallback message without a model |
| Local inference client | Call an externally configured llama.cpp `/completion` endpoint |
| Evaluation utilities | Recompute composite scores from reported tables; provide optional metric examples |

The fallback message is not a generated medical answer. Training plans are not completed training runs.

## Problem and approach

The project studies answer generation for Darija medical questions, including text variability and evaluation across lexical and semantic metrics. Research documentation describes SFT/QLoRA for decoder-only models and full fine-tuning for AraBART. The published code separates preprocessing, training configuration, inference and evaluation.

**Implemented demo stack:** Python, pandas, scikit-learn, Streamlit and requests. **Research/training ecosystem:** PyTorch, Transformers, PEFT, TRL and associated evaluation packages listed in the full requirements; availability in requirements is not proof of a runnable training pipeline.

## Quick start without model downloads

Python 3.10+ is required by current source annotations.

```bash
git clone https://github.com/zik4O4/Darija-Question-answering.git
cd Darija-Question-answering
python -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements-demo.txt
streamlit run app/app.py
```

Select the demonstration mode to inspect the interface. On Windows activate `.venv\Scripts\Activate.ps1`. The full `requirements.txt` includes optional GPU/training packages and is unnecessary for this fallback demo.

## Local model inference

Run your own compatible llama.cpp server with suitable model weights, then configure:

```bash
export LLAMA_CPP_SERVER_URL="http://127.0.0.1:8080"
streamlit run app/app.py
```

Choose the llama.cpp server mode. The client posts a prompt to `/completion` and reads the response's `content` field. Server setup, model conversion, model-specific prompting and weights are external prerequisites, not supplied by the repo.

## Utilities and layout

```bash
python -m src.data.prepare_demo_data
python -m src.data.cleaning
python -m src.evaluation.evaluate
```

Run from the repository root. Cleaning writes the synthetic processed CSV locally; evaluation reads the existing results table. `notebooks/` contains demonstrations and configuration plans; `docs/` describes methodology and models; `src/` contains utilities; `data/` contains synthetic examples; `results/` and `figures/` hold reported artifacts.

## Reported research results

| Model | BERTScore F1 | chrF | Accuracy@0.5 | ROUGE-L | Composite |
|---|---:|---:|---:|---:|---:|
| Atlas-Chat | 0.7501 | 0.2391 | 0.5300 | 0.0693 | 0.4652 |
| Llama 3 | 0.7307 | 0.2020 | 0.5100 | 0.0685 | 0.4440 |
| AraBART | 0.6457 | 0.1052 | 0.4300 | 0.0467 | 0.3668 |

Source: [`results/final_results.csv`](results/final_results.csv). Composite = `0.35 * BERTScore_F1 + 0.25 * chrF + 0.25 * Accuracy@0.5 + 0.15 * ROUGE-L`. The rounded composites are consistent with this formula. Underlying training runs and full evaluation are not reproducible from this snapshot alone. Exact dataset provenance, run metadata and original training artifacts are still needed for independent reproduction. Scores are preserved as reported, not newly measured.

## Existing demonstration asset

![Existing Darija QA demonstration screenshot](figures/demo.png)

The image is a project asset; the current fallback interface does not establish working model inference. Other figures illustrate the documented pipeline and reported scores.

## Data, safety and limitations

- Included CSVs are synthetic demonstration data, not MedQA-MA.
- No model weights are included. Store checkpoints and private tokens outside Git.
- This is single-turn question answering; no RAG or production conversational system is claimed.
- Arabic normalization and optional metrics must be matched to the original experimental protocol before reproducing research scores.
- This academic prototype does not replace professional medical advice, diagnosis or treatment.

## Author and license

Zakariya Ben Kassi — academic final-year project in Artificial Intelligence. No repository-level license has been selected; dataset and model terms remain separate.
