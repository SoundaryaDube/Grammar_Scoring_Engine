# Healthcare Prescription Decoding

The objective is to build a Natural Language Processing (NLP) system that takes raw photos of medical prescriptions as input and decodes them into structured, usable text.

This tool is designed to reduce manual data entry errors and streamline medication management for healthcare providers.

## My Approach: A 2-Stage Pipeline

Given the complexity of handwriting analysis and the critical nature of medical data, I implemented a robust 2-stage pipeline that separates visual processing from semantic understanding.

### Stage 1: Image Preprocessing (Computer Vision)

**Tool:** OpenCV

**Process:**
Raw prescription images often contain noise, shadows, or poor lighting.

* **Thresholding & Enhancement:** Utilized OpenCV to apply adaptive thresholding and image preprocessing techniques.
* **Goal:** To significantly enhance text visibility and separate the handwritten text from the background noise, preparing a clean input for the extraction phase.

### Stage 2: Entity Extraction (NLP)

**Tool:** spaCy & Scikit-learn

**Process:**
Once the text is visible, the system treats it as a sequence of tokens to be classified.

* **Tokenization:** Employed spaCy to break down the processed text into individual meaningful units.
* **Named Entity Recognition (NER):** utilized classification models to recognize and extract critical medical entities. The model specifically targets:
* **Drug Names:** (e.g., "Paracetamol")
* **Dosages:** (e.g., "500mg")
* **Instructions:** (e.g., "Twice a day after meals")



## Impact

* **Streamlined Workflow:** Automates the information extraction process, allowing doctors and pharmacists to digitize records instantly.
* **Enhanced Safety:** Reduces the risk of human error in interpreting handwriting, ensuring patients receive the correct medication details.

## Installation

This project was built in a standard Python environment.

**Clone the repository:**

```bash
git clone [Your-GitHub-Repo-URL]
cd [Your-Project-Folder]

```

**Install Python dependencies:**

```bash
pip install -r requirements.txt

```

*(If you don't have a requirements.txt, you can install the key libraries directly based on the tech stack used)*:

```bash
pip install opencv-python spacy scikit-learn numpy

```

**Download spaCy Model:**
You will need to download the English language model for spaCy:

```bash
python -m spacy download en_core_web_sm

```

## How to Run

The core logic is contained in the main script (e.g., `main.py` or `notebook.ipynb`).

1. **Place Your Data:** Ensure your raw prescription images are in the `input_images/` directory.
2. **Run the Script:**
```bash
python main.py

```


3. **View Results:** The structured text (JSON or CSV) will be saved to the `output/` directory.

## Project Structure

```text
├── input_images/               # Folder for raw prescription photos
├── output/                     # Folder for decoded structured text results
├── main.py                     # Main script for the extraction pipeline
├── preprocessing.py            # OpenCV functions for image thresholding
├── extraction.py               # spaCy logic for NER and tokenization
├── requirements.txt            # Python dependencies
└── README.md                   # This file

```

## Technologies Used

* **Python:** Core programming language.
* **OpenCV:** For image preprocessing and thresholding.
* **spaCy:** For NLP, tokenization, and entity extraction.
* **Scikit-learn:** For classification tasks.
