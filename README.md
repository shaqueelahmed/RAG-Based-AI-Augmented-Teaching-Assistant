# 🎥 Video Course AI Assistant (RAG Pipeline)

An end-to-end Retrieval-Augmented Generation (RAG) application designed to make video courses fully searchable. This system processes raw video lectures, transcribes and translates the audio, and uses a Large Language Model (LLM) to accurately answer user queries by pointing them to the exact video and timestamp containing the information.

## 🚀 Overview
This project demonstrates a complete data engineering and AI pipeline. It takes raw `.mp4` video files, converts them to audio, and uses OpenAI's Whisper model to generate timestamped text chunks. These chunks are embedded using a local vector model and stored for semantic search. When a user asks a question, the system retrieves the most relevant context and generates a precise, human-like response using Llama 3.2.

[cite_start]The system is currently configured to act as an AI teaching assistant for a web development course[cite: 1].

## 🛠️ Tech Stack
* **Language:** Python
* **Media Processing:** FFmpeg (`subprocess`)
* **Speech-to-Text:** OpenAI Whisper (`large-v2`)
* **Embeddings:** Ollama (`bge-m3`)
* **Vector Search:** Scikit-learn (Cosine Similarity), Pandas, NumPy
* **LLM Integration:** Ollama (`llama3.2`)

## ⚙️ Architecture & Pipeline

1.  **Audio Extraction (`video_to_mp3.py`):** Scans the `videos/` directory and uses FFmpeg to extract `.mp3` audio files, parsing tutorial numbers and titles from the filenames.
2.  **Transcription (`mp3_to_json.py`):** Processes the audio using Whisper. It translates Hindi audio to English and creates structured JSON files containing text chunks with their corresponding start and end timestamps.
3.  **Vectorization (`preprocess_json.py`):** Reads the JSON chunks and interfaces with a local Ollama API to generate high-dimensional embeddings using the `bge-m3` model. The data is structured into a Pandas DataFrame and serialized via `joblib` for rapid access.
4.  **Inference & Retrieval (`process_incoming.py`):** * Accepts a user query and vectorizes it.
    * Calculates cosine similarity against the `joblib` database to retrieve the top 5 most relevant video chunks.
    * [cite_start]Constructs a detailed prompt containing the video title, number, start time, end time, and text[cite: 2].
    * [cite_start]Instructs the Llama 3.2 model to provide a conversational response indicating the exact video and timestamp to watch[cite: 3].
    * [cite_start]Implements guardrails to prevent the AI from answering questions unrelated to the course material[cite: 4].
