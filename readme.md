# Fine-tuning LLaMA 3.2 1B for SQL Generation

This project is about fine-tuning a small LLaMA model (1B) to generate SQL queries from natural language. I'm using a dataset that contains examples of how people ask questions and how those get translated into SQL.

## What I'm Doing

* I'm starting with a pre-trained LLaMA 3.2 1B model.
* I use a dataset called `synthetic_text_to_sql-ShareGPT` which has examples of prompts and the corresponding SQL queries.  
  Dataset URL: [https://huggingface.co/datasets/mlabonne/synthetic_text_to_sql-ShareGPT](https://huggingface.co/datasets/mlabonne/synthetic_text_to_sql-ShareGPT)
* I fine-tune the model using Unsloth libary with LoRA Adapters. This allows me to train only parts of the model, which makes it much faster and memory-efficient.

## Evaluation Process

The evaluation pipeline is implemented in [`Evaluate_LLM.ipynb`](./Evaluate_LLM.ipynb):

1. **SQL Question Generation** : Groq’s `llama3-8b-8192` model generates 10 SQL question blocks, each with table creation, inserts, and a natural language SQL question.

2. **Model Answering** : Each question is passed to a local fine-tuned LLaMA model (using `llama-cpp-python`) to generate SQL queries and explanations.

3. **Automated Evaluation** : Groq’s `gemma2-9b-it` model acts as an expert tutor to score each (question, answer) pair on correctness and completeness (1–10 scale) and provide feedback.

4. **Summary** : Average scores and detailed feedback for all questions are output.

*Note:* 
- The question generation and evaluation both use Groq's hosted models (Llama 3_8b for question generation, Gemma 2_9b for evaluation).  
- The local LLaMA_3.2_1b fine tuned model is only used for generating answers.  
- Normally, I use Gemini for evaluation, but due to Gemini being slow today, I used Groq for both question generation and evaluation in this run.

## Why I’m Doing This

I want to build a model that can understand plain English and generate accurate SQL queries. This can be useful for tools where people want to ask questions about their data without writing SQL themselves.

## Where to Find the Model & Notebooks

You can find the fine-tuned model, including the .gguf file format for easy local use, on my Hugging Face repository:

👉 https://huggingface.co/Adhishtanaka/llama_3.2_1b_SQL/tree/main

You can find the Jupyter Notebook files used in this project directly in this repository:

- `Evaluate_LLM.ipynb`: The evaluation pipeline for the fine-tuned model.
- `Llama3.2_1B-SQL.ipynb`: The main notebook for fine-tuning and experimentation.

👉 Browse these files in the [GitHub repository](https://github.com/Adhishtanaka/llama3.2_1.b-SQL) for full code and documentation.



