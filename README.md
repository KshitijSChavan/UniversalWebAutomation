# UniversalWebAutomation

# Algorithm: Universal Web Automation Tool

## Overview
This tool demonstrates how to automate interactions on **any website** by combining:
- **OCR (Optical Character Recognition)** for detecting text on screen.
- **Selenium + PyAutoGUI** for performing actions like clicking, typing, and scrolling.
- **LLM integration** (OpenAI/Anthropic APIs) for flexible instruction parsing and extracting required information.

The system allows users to define a sequence of actions (click, type, scroll, extract) in plain language, which are then executed on the target webpage.

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

## Example Use Cases
- Automating websites that **do not provide APIs**.  
- Automating repetitive form-filling workflows.  
- Extracting structured information from dynamic pages.  
- Demonstrating OCR-driven Robotic Process Automation (RPA).

---

## Notes
- This algorithm is a **demo framework** and should only be used on websites where automation is legally and ethically permitted.  
- No sensitive credentials or site-specific logic is included here.
