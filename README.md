# CloneProof AI

### AI-Powered Misinformation, Fake Website & Brand Impersonation Detection

CloneProof AI is a web-based risk analysis platform designed to help
users identify potentially fraudulent websites, brand impersonation, and
misleading online content. It combines machine-learning predictions with
website and brand-related signals to provide a risk assessment and
supporting indicators.

> **Important:** CloneProof AI is a decision-support prototype, not a
> guarantee that a website or news story is genuine or fraudulent.
> Always verify high-impact decisions using trusted, official sources.

------------------------------------------------------------------------

##  Key Features

-   **Website risk analysis** --- assess URL and website-related signals
    that may indicate phishing or impersonation.
-   **Brand impersonation detection** --- identify suspicious
    brand-related patterns using the available brand model.
-   **Misinformation detection direction** --- supports the broader goal
    of identifying misleading online content; enable this feature only
    where the news model and corresponding interface are integrated.
-   **Risk-oriented results** --- present a model prediction and risk
    score where supported by the application.
-   **Explainable indicators** --- surface relevant signals that
    contribute to a warning, rather than relying only on a label.
-   **Web interface and API** --- a frontend interface communicates with
    a FastAPI backend.
-   **Machine-learning model loading** --- the backend loads trained
    model artifacts from the `models/` directory.

## Problem Statement

Fraudulent websites may imitate trusted brands through lookalike
domains, copied logos, misleading product listings, suspicious contact
information, and other deceptive patterns. Misleading online content can
also spread quickly and influence user decisions.

CloneProof AI aims to make initial screening easier by analyzing
available signals and presenting a clear, understandable risk
assessment.

## How It Works

1.  The user submits a website URL or provides supported input through
    the interface.
2.  The frontend sends the input to the backend API.
3.  The backend extracts or receives the features required by the
    relevant model.
4.  The trained model generates a prediction.
5.  The application presents the result and any available risk
    indicators.
6.  The user can use those indicators to decide whether further
    verification is needed.

*The exact inputs and output fields depend on the current implementation
and the models included in the repository.*

## Technology Stack

  -----------------------------------------------------------------------
  Component                           Technology
  ----------------------------------- -----------------------------------
  Backend API                         Python, FastAPI

  Machine learning                    Scikit-learn / compatible
                                      serialized models

  Model artifacts                     Joblib (`.joblib`)

  Frontend                            Project frontend (see `frontend/`)

  Data processing                     Python, pandas and related
                                      dependencies as used by the project

  Testing                             pytest (if configured in the
                                      repository)
  -----------------------------------------------------------------------

## Project Structure

The repository has included the following project areas:

``` text
CloneProof-AI/
├── data/        # Datasets used for development and model training
├── frontend/    # User interface
├── models/      # Trained model artifacts
├── notebooks/   # Experiments and analysis
├── src/         # Backend, training and data-processing source code
├── tests/       # Tests
├── README.md
└── requirements.txt
```

The exact files may change as development continues.

## Requirements

-   Python 3.11 (the project has been developed with Python 3.11)
-   Git
-   pip
-   A modern web browser
-   Node.js and npm if the frontend has its own `package.json`

## Installation and Setup

### 1. Clone the repository

``` bash
git clone https://github.com/YASHODHA6/CloneProof-AI.git
cd CloneProof-AI
```

### 2. Create and activate a virtual environment

**Windows (PowerShell):**

``` powershell
py -3.11 -m venv .venv
.\.venv\Scripts\Activate.ps1
```

**Windows (Command Prompt):**

``` bat
py -3.11 -m venv .venv
.venv\Scripts\activate.bat
```

**Linux / macOS:**

``` bash
python3.11 -m venv .venv
source .venv/bin/activate
```

### 3. Install Python dependencies

``` bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

If installation fails, check that your Python version matches the
project requirements and review the package versions in
`requirements.txt`.

### 4. Check model files

Confirm that the model files expected by the backend are present in
`models/`. The current development setup has used:

``` text
models/
├── website.joblib
└── brand.joblib
```

If these files are not included in the repository, train the models
using the project's training scripts or obtain the artifacts from the
project maintainer. Do not commit large or sensitive datasets or model
files without checking repository policy.

### 5. Start the backend

Run the FastAPI application using the entry point configured in your
source code. For example, **if** the application object is `app` in
`src/main.py`, use:

``` bash
uvicorn src.main:app --reload
```

If your entry point or application object has a different name, adjust
the command to match it. The API documentation is usually available at:

``` text
http://127.0.0.1:8000/docs
```

### 6. Start the frontend

If `frontend/` contains a Node.js application with a `package.json`:

``` bash
cd frontend
npm install
npm run dev
```

Use the local URL printed by the frontend development server. If the
frontend is plain HTML, open the relevant HTML file or serve it using
your preferred local development server.

## 🔌 API Usage

When the FastAPI server is running, open the interactive API
documentation:

**http://127.0.0.1:8000/docs**

Use the available endpoints shown there to submit a test input and
inspect the response schema. Endpoint paths and request fields are
intentionally not hard-coded here because they must match the API
implemented in the repository.

Example request pattern (replace the endpoint and JSON fields with those
documented by your running API):

``` bash
curl -X POST "http://127.0.0.1:8000/YOUR_ENDPOINT" \
  -H "Content-Type: application/json" \
  -d '{"url":"https://example.com"}'
```

Only test URLs that you are authorized to access. Avoid submitting
private, authenticated, or sensitive URLs.

## Datasets and Model Training

The development workflow has used datasets related to:

-   Website and phishing signals
-   Brand-related impersonation patterns
-   News classification

Dataset labels, feature definitions, preprocessing, and train/test
splitting should be checked before training. In particular:

-   Use representative positive and negative examples.
-   Remove duplicates and avoid leakage between training and test sets.
-   Evaluate with precision, recall, F1-score, confusion matrix, and
    suitable class-specific metrics.
-   Keep a separate test set that is not used for model selection.
-   Do not interpret a high test score as proof of real-world
    reliability without external validation.
-   Clearly distinguish dataset labels from verified ground truth.

Run the training scripts provided in `src/train/` if present. Check each
script's arguments and expected input paths before execution.

## Interpreting Results

A prediction is a **signal for further review**, not definitive proof. A
legitimate site can look unusual, and a malicious site can evade
detection. For important decisions:

1.  Navigate to the brand's official website using a trusted source.
2.  Verify the domain spelling and contact details.
3.  Avoid entering passwords, payment details, or one-time codes on
    suspicious pages.
4.  Report suspected scams through the appropriate platform or
    authority.

## Security and Privacy

-   Do not store API keys, passwords, session cookies, or other secrets
    in source control.
-   Use environment variables for secrets and provide a `.env.example`
    file containing placeholders only, if needed.
-   Treat submitted URLs and page content as untrusted input.
-   Avoid fetching arbitrary URLs from the backend without protections
    against server-side request forgery (SSRF), private-network access,
    unsafe redirects, and resource exhaustion.
-   Add input validation, request timeouts, rate limiting, and safe
    error handling before public deployment.
-   Do not upload private datasets or personally identifiable
    information without authorization.

## Future Enhancements

-   Add verified official-brand reference data and stronger domain
    similarity analysis.
-   Add screenshot/logo similarity comparison where supported.
-   Provide clearer evidence summaries and calibrated confidence
    estimates.
-   Integrate the news misinformation classifier into the same dashboard
    if it is part of the deployed application.
-   Add model evaluation reports and tests for preprocessing
    consistency.
-   Improve multilingual support and accessibility.
-   Deploy with monitoring, rate limiting, and security controls.

## Contributing

Contributions, bug reports, and suggestions are welcome.

1.  Fork the repository.
2.  Create a feature branch.
3.  Make a focused change and add tests where appropriate.
4.  Run the available tests.
5.  Open a pull request describing the change.

## License

No license is specified here. Add a `LICENSE` file and update this
section once the project license has been selected. Until then, reuse
and redistribution permissions should not be assumed.

## Project

**CloneProof AI**\
Repository:
[YASHODHA6/CloneProof-AI](https://github.com/YASHODHA6/CloneProof-AI)

Built as an AI-powered approach to helping users recognize potentially
deceptive websites, brand impersonation, and misleading online content.
