<div align="center">
<h1> WebPriceCompare 
<img src="./assets/icon.png" width="45px">
<br> Multi-Site Price Comparison Web Agent </h1>
</div>

<div align="center">

![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)
![Python 3.10+](https://img.shields.io/badge/python-3.10.13-green.svg)
![Selenium](https://img.shields.io/badge/Selenium-4.15.2-red)

</div>

<div align="center">
<img src="./assets/overall_process_crop.png" width="90%">
</div>

## Introduction

This repository contains the code for **WebPriceCompare**, an AI-powered web agent designed to compare product prices across multiple e-commerce websites. This project is based on **WebVoyager** ([original repo](https://arxiv.org/abs/2401.13919)) and has been modified to enable automated price comparisons.

### Key Features
- **Multi-Site Price Comparison**: The agent browses multiple e-commerce websites and extracts product prices for comparison.
- **Automated Web Interaction**: Uses Selenium to navigate, search for products, and extract price information.
- **AI-Powered Decision Making**: Uses GPT-4o-mini to determine the lowest price and generate a final decision.
- **Support for Dynamic Pages**: Handles pages with AJAX loading, pop-ups, and accessibility tree-based navigation.

## Setup Environment

### Prerequisites
1. Ensure that **Google Chrome** is installed. (The latest Selenium version does not require ChromeDriver installation.)
2. If running on a **Linux server**, install Chromium (e.g., for CentOS: `yum install chromium-browser`).

### Installation
Create a Conda environment and install dependencies:
```bash
conda create -n webpricecompare python=3.10
conda activate webpricecompare
pip install -r requirements.txt
```

## Data

### Task Format
Each task specifies a product to search for and a list of e-commerce websites to check. The dataset format follows:
```json
{
    "id": 1,
    "product": "Apple iPhone 12 Pro Max (256GB, Pacific Blue)",
    "websites": [
        "https://www.amazon.com",
        "https://www.bestbuy.com",
        "https://www.walmart.com"
    ],
    "ques": "Find the lowest price for this product."
}
```
The dataset is stored in `data/tasks_test.jsonl`.

## Running

### Running WebPriceCompare
1. Add product queries in `data/tasks_test.jsonl`.
2. Set your OpenAI API key in `run.sh`.

#### **Method 1: Using Bash Script (`run.sh`)**
Run the agent:
```bash
bash run.sh
```

#### `run.sh` Example
```bash
#!/bin/bash
nohup python -u run.py \
    --test_file ./data/tasks_test.jsonl \
    --api_key YOUR_OPENAI_API_KEY \
    --headless \
    --max_iter 15 \
    --max_attached_imgs 3 \
    --temperature 1 \
    --fix_box_color \
    --seed 42 > test_tasks.log &
```

#### **Method 2: Windows Direct Execution**
For **Windows users**, you can run the agent directly using the following command:  
(Replace `"C:\Users\user\AppData\Local\Programs\Python\Python310\python.exe"` with your actual Python installation path.)
```powershell
"C:\Users\user\AppData\Local\Programs\Python\Python310\python.exe" run.py --temperature 0.0 --test_file data/tasks_test.jsonl --api_key "YOUR-OPENAI-API-KEY" --api_model gpt-4o-mini
```

### Output
- Screenshots and interaction logs are stored in the `results/` directory.
- The final decision on the **lowest price** is printed and logged.

## Parameters

- `--test_file`: JSON file with product queries.
- `--max_iter`: Maximum number of interactions per task.
- `--api_key`: OpenAI API key for processing.
- `--output_dir`: Directory for storing results.
- `--download_dir`: Directory for downloading files (if needed).
- `--headless`: Run without opening a visible browser.
- `--max_attached_imgs`: Number of screenshots to retain for context.
- `--text_only`: Enable text-based navigation (without images).
- `--temperature`: Control randomness of AI responses.

## Results and Evaluation

After execution, the system selects the lowest-priced product and generates a final report. Example output:

```
Product: Apple iPhone 12 Pro Max (256GB, Pacific Blue)
Website: Amazon
Price: $999.99
```

## Citation

If you use or modify this project, please also consider citing the original WebVoyager paper:
```
@article{he2024webvoyager,
  title={WebVoyager: Building an End-to-End Web Agent with Large Multimodal Models},
  author={He, Hongliang and Yao, Wenlin and Ma, Kaixin and Yu, Wenhao and Dai, Yong and Zhang, Hongming and Lan, Zhenzhong and Yu, Dong},
  journal={arXiv preprint arXiv:2401.13919},
  year={2024}
}
```

## Disclaimer

This project is **not** an official product and does not guarantee accurate results due to the dynamic nature of web pages, AI decision-making, and API changes. Users should verify extracted data before use.
