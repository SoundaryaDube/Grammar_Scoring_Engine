# grammer_scoring-_engine-NLP-

Grammar Scoring Engine
The objective is to build a model that takes a 45-60 second spoken audio file (.wav) as input and outputs a continuous grammar score from 0 to 5.

The entire solution, from preprocessing to the final report, is contained within the shl_assessment.ipynb notebook.

My Approach: A 2-Stage Pipeline
Given the very small training dataset (n=409), building a complex, end-to-end deep learning model would be at high risk of overfitting.

I implemented a more robust and interpretable 2-stage pipeline, which separates the problem into two distinct, manageable tasks:

Stage 1: Speech-to-Text (ASR): Convert all audio files into text transcripts.

Stage 2: Text-to-Score (Regression): Analyze the text to predict a grammar score.

Stage 1: Audio Transcription (ASR)
Tool: OpenAI's Whisper (base.en model).

Process: All 409 training and 197 test audio files were transcribed.

Output: The raw text transcripts were saved to train_transcripts.csv and test_transcripts.csv. This is a crucial step that makes the pipeline reproducible and fast to re-run, as the slow transcription phase only needs to be executed once.

Stage 2: Text-to-Score (Regression)
This stage treats the problem as a classic tabular regression task, where the text transcripts are used to engineer numerical features.

Feature Engineering
This is the most critical part of the model. I created features based on the SHL rubric, which mentions "sentence structure," "syntax," "grammatical mistakes," and "complex language."

Grammar Errors (language-tool-python):

error_count: The total number of grammar and spelling mistakes.

error_density: The number of errors per word (e.g., error_count / word_count).

Text Statistics (textstat):

word_count: Total words in the transcript.

sentence_count: Total sentences.

avg_sentence_length: A proxy for speech fluency and structure.

Readability & Complexity (textstat):

readability_grade: The Flesch-Kincaid grade level. This is a strong proxy for the "complex grammar" mentioned in the rubric for scores 4 and 5.

type_token_ratio: The ratio of unique words to total words, measuring lexical diversity.

Model
Model: An XGBRegressor (XGBoost) was used. Tree-based models are highly effective for small-to-medium tabular datasets and can capture non-linear relationships between features (e.g., a low error_count is only good if the word_count is high).

Validation: A 5-Fold Cross-Validation strategy was used. This provides a reliable estimate of the model's performance and helps prevent overfitting. The final test predictions are an ensemble (average) of the 5 models trained during this process.

🛠️ Installation
This project was built in a standard Python 3.10 environment.

Clone the repository:

Bash

git clone [Your-GitHub-Repo-URL]
cd [Your-Project-Folder]
Install FFmpeg: Whisper requires FFmpeg to be installed on your system. You can download it from ffmpeg.org or install it via a package manager:

Bash

# On Windows (using Chocolatey)
choco install ffmpeg

# On Mac (using Homebrew)
brew install ffmpeg

# On Linux (using apt)
sudo apt update && sudo apt install ffmpeg
Install Python dependencies:

Bash

pip install -r requirements.txt
(If you don't have a requirements.txt, you can just run pip install pandas numpy openai-whisper language-tool-python textstat xgboost scikit-learn matplotlib seaborn tqdm)

🏃 How to Run
The entire pipeline is contained in the shl_assessment.ipynb Jupyter Notebook.

Place Your Data: Ensure your data is in the correct location as referenced by the paths in the notebook (e.g., audios_train/, train.csv, etc.).

Run All Cells: Open the notebook and run all cells from top to bottom.

Phase 1 (Transcription): This step is slow and will take a significant amount of time (20-40 minutes) as it processes all 600+ audio files.

Phase 2 (Feature Engineering): This step is very fast.

Phase 3 (Model Training): This step trains the 5-fold CV model and prints the final Training RMSE.

Phase 4 (Submission): This step generates the final submission.csv file.

📈 Evaluation
The model's performance is evaluated using Root Mean Squared Error (RMSE).

Compulsory Training RMSE (from 5-Fold Cross-Validation): [INSERT YOUR RMSE SCORE HERE]

(You will get this number from the output of your training cell.)

📁 Project Structure
.
├── audios_test/                # Test audio files
├── audios_train/               # Train audio files
├── shl_assessment.ipynb        # The main Jupyter Notebook
├── train.csv                   # Original training data
├── test.csv                    # Original test data
├── sample_submission.csv       # Sample submission format
├── README.md                   # This file
│
├── train_transcripts.csv       # (Generated) Transcripts for train data
├── test_transcripts.csv        # (Generated) Transcripts for test data
└── submission.csv              # (Generated) The final submission file
