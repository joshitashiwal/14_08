1. ONLINE TESTING SYSTEM.

has online data base, contains a unique key for every student to ensure different questions for everyone. has voice input answer, fill in the blanks or upload your answers as objective and subjective question evalution for every student. limits paper usage and ensures equal level testing for every student with the added advantage of limiting the copying of answers possible in the system. 
<img width="2560" height="1440" alt="Screenshot (145)" src="https://github.com/user-attachments/assets/bfaceac8-560e-4abb-87fb-69279cae4d5a" />

https://replit.com/@sauravshiwal/ExamMaster
flowchart LR
A[Instructor creates blueprint\n(topics, LO tags, difficulty, constraints)] --> B[Question Bank\n(seed items + metadata)]
B --> C[AI Variant Generator\n(LLM + templates)]
C --> D[Equivalence Checker\n(IRTb/metrics)]
D --> E[Exam Assembler\n(per-student paper)]
E --> F[Delivery Service\n(web/app, login)]
F --> G[Submission Collector]
G --> H[Auto-Grader\n(MCQ exact, SA rubric+LLM, Code tests)]
H --> I[Bias & Difficulty Monitor\n(item stats, DIF checks)]
I --> J[Marks Release + Audit Logs]




2. ONLINE EVALUATION.
helps the prof/ evaluator correct the papers. with plag checking by the ai software. ensures proper grading of a student to ensure limited error in the evaluting of scores achieved by a student and proper result decleartion. also shares proper data with the professor of the achieved scores or class statistics of the performance by the students.
<img width="2560" height="1440" alt="Screenshot (147)" src="https://github.com/user-attachments/assets/287f4cd2-3c6a-4346-8500-70d41c024da8" />

https://replit.com/@sauravshiwal/PaperlessExams
flowchart TB
A[Admin/Instructor RBAC] --> B[Question Authoring + Bank]
B --> C[AI Generator & Reviewer]
C --> D[Secure Exam Builder\n(rand. order, time windows)]
D --> E[Distribution & Proctoring\n(PWA + device checks)]
E --> F[Student Responses\n(text/code/MCQ/uploads)]
F --> G[Scoring Engine\n(LLM + rules + tests)]
G --> H[Moderation & Regrade Queue]
H --> I[Results + Analytics\n(item stats, DIF, leakage scan)]
I --> J[Archive & Compliance]


3. GITHUB CODE RE-WRITE.
The “MLA” (Multi-Level Attention) repository implements an evidence-based fact-checking model introduced in ACL-IJCNLP 2021. It includes scripts for claim verification and sentence selection, allowing end-to-end processing: from preprocessing a dataset (e.g., FEVER claims), training the model, to running predictions and evaluating outcomes. The dependency stack is clearly specified (Python 3.7, PyTorch, Transformers), and the repository supports reproducibility via provided experiments/ and links to model checkpoints. The toy run demonstrates how a base language model (like BERT) can be used to predict veracity indicators based on input claims.
#!/usr/bin/env bash
set -e  # Exit on error

# 1. Clone the repo
git clone https://github.com/nii-yamagishilab/mla.git
cd mla

# 2. Print commit hash
echo "Running commit:" $(git rev-parse --short HEAD)

# 3. Create and activate conda environment
echo "Creating Conda environment 'mla-env' (Python 3.7)"
conda create -n mla-env python=3.7 -y
# Activate it
echo "Activating conda env 'mla-env'"
# The activation command depends on shell; here’s for bash:
source "$(conda info --base)/etc/profile.d/conda.sh"
conda activate mla-env

# 4. Install dependencies
echo "Installing dependencies..."
pip install -r requirements.txt

# 5. Sanity check, show PyTorch version and whether CUDA is available
echo "PyTorch version and CUDA available:"
python - << 'PYCODE'
import torch
print("torch.__version__:", torch.__version__)
print("CUDA available:", torch.cuda.is_available())
PYCODE

# 6. Run a quick help check to ensure scripts load
echo "Testing script help screens..."
for script in preprocess_claim_verification.py preprocess_sentence_selection.py train.py predict.py eval_fever.py; do
  echo "  -> $script --help"
  python "$script" --help >/dev/null
done

# 7. Run toy prediction on sample claims
echo "Running toy end-to-end (preprocess + predict)..."
mkdir -p runs/toy_claims
printf "id\tclaim\n1\tThe Eiffel Tower is in Paris.\n2\tThe Sun rises in the west.\n" > runs/toy_claims/toy.tsv

python preprocess_claim_verification.py \
  --input_file runs/toy_claims/toy.tsv \
  --output_dir runs/toy_claims/pre

python predict.py \
  --data_dir runs/toy_claims/pre \
  --output_dir runs/toy_claims/pred \
  --model_name_or_path bert-base-uncased \
  --max_seq_length 128 \
  --per_device_eval_batch_size 8

# 8. List prediction outputs
echo "Prediction outputs:"
ls -R runs/toy_claims/pred

echo "Finished successfully. Environment: mla-env"

<img width="2560" height="1440" alt="Screenshot (148)" src="https://github.com/user-attachments/assets/28313afe-674c-45a4-8a32-b009b884e438" />
