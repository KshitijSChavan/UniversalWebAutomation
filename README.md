# UniversalWebAutomation

# Algorithm: Universal Web Automation Tool

The code has been removed due to potential legal and ethical concerns.

## Overview
This tool demonstrates how to automate interactions on **any website** by combining:
- **OCR (Optical Character Recognition)** for detecting text on screen.
- **Selenium + PyAutoGUI** for performing actions like clicking, typing, and scrolling.
- **LLM integration** (OpenAI/Anthropic APIs) for flexible instruction parsing and extracting required information.
- watch demo at - https://youtu.be/xj15TDoTX-U

The system allows users to define a sequence of actions (click, type, scroll, extract) in plain language, which are then executed on the target webpage.

---

## Table of Contents
1. [What this project does](#what-this-project-does)  
2. [Disclaimer & responsible usage](#disclaimer--responsible-usage)  
3. [Algorithm / How it works (step-by-step)](#algorithm--how-it-works-step-by-step)  
4. [Main components / functions map](#main-components--functions-map)  
5. [Requirements & installation](#requirements--installation)  
6. [How to run (safe, minimal example)](#how-to-run-safe-minimal-example)  
7. [Limitations & known issues](#limitations--known-issues)  
8. [Suggestions & safer alternatives](#suggestions--safer-alternatives)  
9. [License & attribution](#license--attribution)

---

## What this project does
This project demonstrates an approach for automating user-like interactions on web pages when no formal API or structured feed is available. Specifically it shows:

- How to locate **visible UI text** using OCR on screenshots (EasyOCR/Tesseract) and convert those detections into coordinates and action targets.  
- How to reliably perform actions using a hybrid strategy: prefer DOM-driven methods (Selenium) when possible, fall back to desktop-level events (PyAutoGUI) when elements are not directly selectable.  
- How to extract **required information** from pages (e.g., titles, dates, short text blocks) using OCR + optional LLM refinement (LLM helps clean noisy OCR text into structured fields).  
- A safe demo for recruiters: run against public sites (Wikipedia, your GitHub Pages) to show capability without exposing a universal “weaponized” bot.

Why this matters: many business workflows still rely on UI-only systems or legacy portals. An OCR-first RPA approach provides a pragmatic bridging strategy where APIs are not available and human-like automation is required.

---

## Disclaimer & Responsible Usage
**Read before using.**  
- This repository is **educational**. Use it responsibly and only against websites that explicitly permit automation or where you own the content.  
- Respect `robots.txt`, site Terms of Service, and local laws. Do NOT use this for scraping personal data, bypassing protections, or automating actions that would cause harm (spam, credential stuffing, fraud).  
- The demo intentionally excludes any code that automates sign-in to private accounts or accesses restricted data. If you adapt the approach, add consent checks and rate-limiting.  
- The author is not responsible for misuse. If an employer requests evidence of technical skill, provide the safe demo and algorithm — not the full universal automation engine.

---

## Step-by-Step Algorithm

### 1. Initialization
1. Start the automation tool.
2. Configure and launch Selenium Chrome browser with anti-bot options.
3. Initialize OCR engine (EasyOCR or PyTesseract).

---

### 2. Website Handling
1. Accept a website URL from user input or arguments.
2. Open the website in Selenium browser.
3. Maximize browser window and wait for page load.

---

### 3. Screenshot Capture
1. Capture current webpage as a screenshot.
2. Store screenshot in a temporary directory for OCR processing.

---

### 4. OCR-Based Text Detection
1. Preprocess screenshot (resize, grayscale, thresholding).
2. Apply OCR to detect text elements and their bounding boxes.
3. Search for target text (e.g., “Login”, “Search”, “Submit”).
4. Return coordinates of the text (center of bounding box).

---

### 5. Action Execution
Actions supported:
- **Click**:  
  - Locate text coordinates via OCR.  
  - Move mouse and click at detected position.  
- **Type**:  
  - Type given text into the active input field with small delays for reliability.  
- **Scroll**:  
  - Scroll down by one window height.  
- **Extract**:  
  - Extract required information from page text and refine results using LLM.

---

### 6. Workflow Sequencing
1. Define a sequence of actions (predefined or user-driven).  
   Example:  
   - Open website → Click "Login" → Type username → Type password → Click "Search".  
2. Execute actions step by step, taking a screenshot before each step.  
3. Provide feedback if text is not found.

---

### 7. Information Extraction (Optional)
1. Capture relevant sections of webpage.  
2. Apply OCR to extract raw text.  
3. Use keyword rules or LLM to filter and find required information.  
4. Save extracted results to file or database.

---

### 8. Intelligent Parsing (Optional with LLMs)
1. Send extracted raw text to LLM.  
2. Request structured output (e.g., only the required fields).  
3. Refine automation loop by interpreting high-level user commands (e.g., “navigate to reports section and extract data”).

---

### 9. Cleanup
1. Close browser session.  
2. Remove temporary screenshots.  
3. Save logs/results.

---

## Main components / functions map
> This is a safe, deconstructed map — no executable site-specific code included.

- **`ocr_utils.py`**  
  - `capture_screenshot()` — capture and save full-window image.  
  - `preprocess_image(image)` — upscale, denoise, threshold.  
  - `run_ocr(image)` — call EasyOCR/Tesseract to return boxes, text, confidence.  
  - `build_indexes(ocr_data)` — produce `word_index` and `line_index`.

- **`matcher.py`**  
  - `find_matches(index, phrase)` — exact + fuzzy search across lines/words.  
  - `rank_candidates(candidates, last_bbox)` — composite score using confidence, proximity, and recency.

- **`actions.py`**  
  - `click_screen(x, y)` — PyAutoGUI click (human-like delays).  
  - `selenium_click(element)` — prefer DOM click when available.  
  - `type_text_via_selenium(element, text)` / `type_text_via_pyautogui(text)`.  
  - `scroll_down()` — JS scroll by viewport height.

- **`extractor.py`**  
  - `select_regions_for_extraction()` — heuristics for list vs detail pages.  
  - `ocr_region(region)` — OCR on cropped region.  
  - `llm_refine(raw_text, schema)` — (optional) call LLM to return structured fields.

- **`executor.py`**  
  - `execute_instruction(instruction)` — orchestrates match → execute → verify → retry.  
  - `verify_outcome(expected)` — checks for DOM changes, visible banners, or OCR-rechecked text.  
  - `retry_logic()` — exponential backoff and alternate strategies.

- **`main.py`**  
  - CLI interface, demo sequence definitions, debug flags, logging setup, and safe-run wrapper.

---

## Example Use Cases
- Automating websites that **do not provide APIs**.  
- Automating repetitive form-filling workflows.  
- Extracting structured information from dynamic pages.  
- Demonstrating OCR-driven Robotic Process Automation (RPA).

---

# Requirements & installation

> Minimal demo stack (safe demo). Use a virtual environment.

```bash
# Clone (replace URL with your repo)
git clone https://github.com/yourusername/universal-web-automation-demo.git
cd universal-web-automation-demo

# Create & activate venv
python -m venv venv

# Linux/macOS
source venv/bin/activate

# Windows (PowerShell)
venv\Scripts\Activate.ps1

# Install dependencies
pip install -r requirements.txt
```

## Example requirements.txt (demo-only)

```bash
selenium>=4.0
pyautogui
opencv-python
numpy
pytesseract
easyocr   # optional; pick one engine in real run
Pillow
```

## Notes

- You will need a ChromeDriver compatible with your Chrome version (put it in PATH) if you run Selenium locally.
- On Linux, ensure `scrot` or equivalent is available for PyAutoGUI screenshot capture.
- For Tesseract use, install system Tesseract and point `TESSERACT_CMD` as needed.

## How to run (safe, minimal example)

This example demonstrates a non-sensitive workflow: searching Wikipedia. No logins, no private data.

Start the demo (predefined sequence) that runs only public actions:

```bash
python main.py --demo wikipedia-search
```

### What wikipedia-search does (step-by-step):

1. Opens https://wikipedia.org via Selenium.
2. Waits for page load and takes a screenshot.
3. Finds the "English" language button via OCR (line matching) and clicks it (Selenium preferred if DOM element resolvable).
4. Focuses the search bar (Selenium `.find_element()` or PyAutoGUI click on OCR bbox).
5. Types "Python (programming language)" and triggers the search.
6. Saves a final screenshot and extracts the first paragraph as a demo "required information" extraction (OCR → optional LLM cleanup).
7. Saves a small `demo_result.png` and `demo_output.json` describing what it did.

### Examining results:

- Check `demo_result.png` for proof-of-run GIF (convert sequence of screenshots to gif if desired).
- Check `demo_output.json` or `demo.log` for step-by-step logs and timings.

## Limitations & known issues

Be explicit about what could fail and why — recruiters respect honesty.

### OCR inaccuracies

Small fonts, anti-aliased text, images-as-text, or decorative fonts reduce recognition accuracy.

### Coordinate mapping fragility

Multiple monitor setups, DPI scaling, and headless vs headful differences can misalign screen coordinates.

### Dynamic & JS-heavy UIs

Single-page apps and dynamically-rendered buttons often require DOM-based selectors (Selenium) rather than pure OCR. OCR-only approaches can be brittle.

### Timing & race conditions

Pages that lazy-load content require robust wait/verification logic; naive fixed sleeps cause flakiness.

### Ethical/legal constraints

Even if technically possible, automation may violate a site's Terms of Service or local law. Always verify permissions.

### Not production-ready

This is an educational demo — not hardened for scale, concurrency, or adversarial inputs.

## Suggestions & safer alternatives

When possible, prefer these options over a UI-driven OCR bot:

- **Official APIs**: Always the most reliable and lawful route — prefer JSON responses over scraping.
- **Structured scraping**: For public data, use `requests` + `BeautifulSoup` or `Scrapy` (faster, more robust).
- **Enterprise RPA**: For business-grade automation with compliance/audit features, use UiPath / Automation Anywhere / Blue Prism.
- **Consent-first automation**: If automating on behalf of a client, obtain written permission and add rate limits, error handling, and logging.
- **Hybrid approach**: Prefer Selenium DOM interactions first; fallback to OCR-only when DOM interaction isn't possible.

## Notes
- This algorithm is a **demo framework** and should only be used on websites where automation is legally and ethically permitted.  
- No sensitive credentials or site-specific logic is included here.
