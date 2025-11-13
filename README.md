# Automated Testing Tool for LLM Chatbots
> ⚠️ Ongoing project – planning, design and development stage, no production code pushed yet for security reasons.

This project is a small Python toolkit to **automate running and testing prompts against LLM-based chatbots**.

Right now, it focuses on:

- Reading prompts from a **CSV/Excel file**
- Automatically sending each prompt to:
  - **HKChat** (primary model)
  - **Perplexity** (comparison model)
- Collecting the answers into new output columns and saving a **results sheet**

In the future, an **LLM-based evaluator** will also be added to analyse the responses and **mark each test as PASS or FAIL** based on expected behaviour.

---

## What It Does Today

### 1. Spreadsheet-driven test runs

You prepare a `.csv` or `.xlsx` file with columns such as:

- `Prompt` – the user message / question to send  
- Optional mode columns:
  - `HKChat Mode` – e.g. “Thinking”, “Weather”, “Legal” (maps to HKChat buttons)
  - `Perplexity Mode` – e.g. “Research” / “Search` (can be auto-inferred from HKChat mode)

The tool will:

1. Detect the **prompt column** automatically (supports names like `Prompt`, `prompt`, `Question`, etc.).
2. Add or reuse output columns:
   - `HKChat Output`
   - `Perplexity Output`
3. Optionally add `Perplexity Mode` if it does not exist.

### 2. Automated browser runs (HKChat + Perplexity)

Using **Playwright**, the script opens:

- A persistent browser session for **HKChat**
- A persistent browser session for **Perplexity**

For each row in the spreadsheet, it:

1. Starts a **new chat** in HKChat.
2. Sets the requested **mode** (e.g. “Thinking”, “Weather”, “Legal”) when provided.
3. Types the prompt into HKChat and waits until the full response is finished.
4. Captures the reply (preferably via the **copy** button; otherwise by reading the final rendered text).
5. Writes the cleaned reply into `HKChat Output`.

Then, for the same row, it:

1. Starts a **new chat** in Perplexity.
2. Selects the **Search** or **Research** mode (based on the HKChat mode if available).
3. Sends the same prompt.
4. Waits for the final answer (copy button or stable text).
5. Writes the cleaned reply into `Perplexity Output`.
6. Records the Perplexity mode into `Perplexity Mode`.

The tool also:

- Adds a simple `S/N` column when none exists, for easier row tracking.
- Saves screenshots into an `out/` folder when errors occur for later debugging.

---

## Planned: LLM-based PASS / FAIL Judging

The current version only **runs prompts and captures outputs**.  
The next major feature will be an **automatic evaluator**, powered by an LLM, that will:

1. Read a test specification (for each prompt), including:
   - The **prompt** itself
   - Optional **expected keywords** or behaviour description
2. Inspect the **HKChat Output** (and possibly the Perplexity answer).
3. Use an LLM to decide whether the response:
   - Contains the required information
   - Follows style or safety constraints
4. Mark each row with a simple result, e.g.:
   - `Result`: `PASS` / `FAIL`
   - Optional `Notes`: short explanation from the evaluator model

Planned extra columns in the spreadsheet might include:

- `Expected Keywords`
- `Evaluation Result` (PASS / FAIL)
- `Evaluation Notes`

This will turn the project from a “prompt runner” into a more complete **regression testing tool for LLM chatbots**.

---

## Tech Stack

- **Language**: Python 3
- **Automation**: Playwright (Chromium)
- **Input/Output**: CSV / Excel via `pandas`
- **Data processing**:
  - Auto-detect prompt / mode / output columns
  - Normalisation and cleanup of model outputs
- **Planned evaluation**:
  - LLM-based scoring for PASS / FAIL (e.g. using an external LLM API)
  - `pytest` for internal unit tests
  - GitHub Actions for basic CI

---

## High-Level Workflow

1. Place one or more `.csv` / `.xlsx` files in the working directory.
2. Run the script (example):

   ```bash
   python web_excel_runner.py
