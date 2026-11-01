# ZentroMart VoiceOps — Multilingual Customer Support Assistant

A Google Colab–based capstone prototype for a multilingual customer-support assistant for the fictional ZentroMart business. The project combines retrieval-augmented generation (RAG), structured customer/order data, tool-based order lookup and ticket creation, and a Gradio chat/voice interface.

> **Project status:** This is a development/capstone prototype. The notebook has not been independently verified end-to-end in a fresh Colab runtime. Treat the live demo as temporary and session-dependent. Do not use this prototype for real customer data or production authentication.

## Live demo

[Open the Gradio demo](https://f5cbcd221af0eac553.gradio.live/)

The `gradio.live` address is a temporary share URL. It may stop working when the Colab runtime disconnects, the Gradio process stops, or the share session expires. The link being accessible does not by itself establish that every feature has been tested.

## Project objectives

- Provide customer support through a chat-style interface.
- Support Hindi, English, Hinglish, Marathi, and Gujarati prompts.
- Retrieve relevant information from the supplied ZentroMart knowledge base.
- Look up order information only after the dataset's verification flow.
- Create support tickets through the supplied mock support tool.
- Demonstrate customer registration and conversation/session handling.
- Evaluate responses against the supplied practice questions and conversations.

## Technology stack

- Google Colab (Free runtime)
- Python
- Hugging Face Transformers and Qwen2.5-1.5B-Instruct
- Sentence Transformers (`paraphrase-multilingual-MiniLM-L12-v2`)
- FAISS for vector similarity search
- Gradio for the user interface
- Pandas for structured CSV data

Actual runtime availability and compatibility depend on the Colab session, installed package versions, and whether a GPU is allocated.

## Dataset

The project uses the supplied synthetic ZentroMart capstone dataset. Expected extracted directory:

```text
/content/zentromart_extract/Capstone_Project_Dataset_FTTT_2026/
```

The dataset contains:
- Knowledge-base Markdown documents and policy material
- Customer, order, order-item, product, pincode, holiday, and support-log CSV files
- Practice questions and sample conversations
- Mock support tools in `tools/tools.py`

Keep identifier columns such as order IDs, customer IDs, SKUs, phone numbers, and pincodes as strings. Follow the dataset README and schema documentation as the source of truth for field definitions and business rules.

## Run in Google Colab

1. Open [Google Colab](https://colab.research.google.com/).
2. Upload and open the project `.ipynb` notebook.
3. In the left **Files** panel, upload the dataset ZIP.
4. Extract the ZIP using a notebook cell. Example:

```python
import os
import zipfile

ZIP_PATH = "/content/Capstone_Project_Dataset_FTTT_2026(2).zip"
EXTRACT_PATH = "/content/zentromart_extract"

os.makedirs(EXTRACT_PATH, exist_ok=True)

with zipfile.ZipFile(ZIP_PATH, "r") as archive:
    archive.extractall(EXTRACT_PATH)

DATA_DIR = os.path.join(
    EXTRACT_PATH,
    "Capstone_Project_Dataset_FTTT_2026"
)

print("Dataset path:", DATA_DIR)
print("Dataset exists:", os.path.isdir(DATA_DIR))
```

5. Confirm the printed dataset path exists. The expected path is:

```python
DATA_DIR = "/content/zentromart_extract/Capstone_Project_Dataset_FTTT_2026"
```

6. Select **Runtime → Change runtime type** and choose a GPU if one is available. A free GPU is not guaranteed.
7. Run notebook cells sequentially from top to bottom. Do not use **Run all** on the first attempt. Check each cell's output before continuing.
8. Complete the dataset validation cell before starting model downloads, embedding generation, or the Gradio interface.
9. Run the Gradio launch cell near the end of the notebook. Open the generated share link to use the interface.

If your uploaded ZIP has a different filename, update `ZIP_PATH`. If the extracted folder name differs, update `DATA_DIR` to the actual folder containing the dataset files.

## Main workflow

1. Load and validate the supplied dataset.
2. Parse knowledge-base Markdown documents and their section metadata.
3. Generate multilingual embeddings and build a FAISS retrieval index.
4. Retrieve relevant knowledge-base sections for a user query.
5. Generate a response with the selected Qwen instruct model, using retrieved context.
6. Use the supplied mock tools for order lookup and support-ticket creation.
7. Present the assistant through Gradio.

## Safety and limitations

- The dataset is synthetic and intended for education.
- The order-verification mechanism is a mock demonstration, not production-grade identity verification.
- Never reveal verification values, private customer details, or internal secrets in responses.
- Order details should only be returned after the dataset's required verification step.
- Create-ticket success should only be reported when the mock tool confirms that a ticket was created.
- Knowledge-base policy precedence and superseded-document status must be respected.
- Voice/STT/TTS functionality may require a separate provider account, API key, or compatible SDK. No claim is made that a live voice provider integration has been configured or tested.
- Model output can be incorrect. Retrieved evidence and tool results should be checked before relying on an answer.
- No benchmark scores or production-readiness claim are included in this README.

## Evaluation

The dataset includes practice questions and sample conversations that can be used to assess retrieval and response quality. Run the notebook's evaluation cells and report actual measured results only after they have completed successfully. Do not describe planned evaluation as completed evaluation.

## Troubleshooting

**`FileNotFoundError` for the dataset**
- Check that the ZIP is uploaded in Colab's Files panel.
- Confirm `ZIP_PATH` matches the exact uploaded filename.
- Confirm `DATA_DIR` points to the extracted dataset root.

**GPU unavailable**
- Continue only if the notebook's CPU path is supported; model loading and generation may be significantly slower. Free Colab GPU availability varies.

**Package/version conflicts**
- Restart the runtime after changing package versions, then rerun setup cells in order.
- Avoid mixing incompatible package versions from separate notebook sessions.

**Gradio link does not open**
- Confirm the notebook runtime is still connected and the Gradio launch cell is still running.
- Re-run the launch cell to create a new share URL if the previous temporary link has expired.

## Repository structure

The exact contents depend on the supplied project bundle. The primary project artifact is the Colab notebook, with the dataset supplied separately as a ZIP.

```text
ZentroMart-VoiceOps/
├── notebooks/
│   └── ZentroMart_Project_Notebook.ipynb
├── dataset/
│   └── Capstone_Project_Dataset_FTTT_2026/   # supplied separately
└── README.md
```

## Credits and source material

The capstone was developed using the supplied ZentroMart dataset and the course materials at:
- https://caciitg.com/llm/course/

The dataset's own README, schema documentation, and tool implementation should be consulted for exact data and business-rule details.

## Author / submission details

Add your name, institution, course, GitHub repository URL, and submission date here before submitting. The Colab notebook and temporary Gradio link are not a substitute for a public source-code repository or a permanently hosted deployment.
