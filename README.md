# Automated Testing Tool for LLM Chatbots

> ⚠️ Concept repo only – planning and design stage, no production code pushed yet for security reasons.

This project is a small Python toolkit to **automate testing for some certain LLM-based chatbots**.

The idea is:

- You define a set of test cases (prompts + expectations).
- The tool sends these prompts to a chatbot (or a mock client).
- It checks the responses for:
  - Required keywords or phrases
  - Basic quality rules (e.g. not empty, not too short)
  - Optional latency checks (how long the model takes to respond)
- It then prints a **test report** so you can quickly see which prompts fail.

---

## Status

> 🔧 Work in progress – setting up the core structure and basic test runner.

Right now, the focus is on:

- A simple test format (e.g. JSON file with prompts and expected keywords)
- A Python test runner that:
  - Reads the test file
  - Calls a chatbot client
  - Evaluates and prints pass/fail for each test

---

## Planned Features

- ✅ Load test cases from a JSON or YAML file  
- ✅ Keyword-based checks for correctness  
- ⏳ Optional latency measurement for each response  
- ⏳ Pluggable clients:
  - Mock client (for local testing)
  - Real LLM providers (e.g. OpenAI, Hugging Face) via adapters
- ⏳ Summary report (number of passed/failed tests)

---

## Tech Stack (Planned)

- **Language**: Python 3
- **Testing**: `pytest` for internal unit tests
- **Config**: JSON or YAML for test case definitions
- **CI**: GitHub Actions to run tests on every push (planned)

---

## Example Test Case Format (planned)

An example of how a simple test file might look:

```json
[
  {
    "name": "Greet politely",
    "prompt": "Say hello to the user.",
    "expected_keywords": ["hello", "hi"],
    "max_latency_ms": 3000
  },
  {
    "name": "Explain concept simply",
    "prompt": "Explain what a neural network is to a 12-year-old.",
    "expected_keywords": ["neurons", "connections", "brain", "simple"],
    "max_latency_ms": 5000
  }
]
