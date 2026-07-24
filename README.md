# UK-Tax-Liability-Engine
A Python engine calculating UK Income Tax, National Insurance, and Dividend band erosion
# 🧮 UK Tax Liability Engine

## Overview
This Python script is an automated tax calculation engine built to process multi-source income streams (employment salary and dividends) and determine exact net take-home pay based on UK statutory tax rules. 

Built in parallel with my ACCA Taxation (TX) studies, this project translates complex tax legislation into conditional programming logic.

## ⚙️ Core Features
* **Multi-Stream Processing:** Handles simultaneous inputs for salary and dividend income.
* **Dynamic Band Erosion:** Automatically calculates remaining basic and higher rate bands when salary pushes dividends into higher tax brackets.
* **Statutory Allowances:** Implements the £12,570 Personal Allowance, the £500 Dividend Allowance, and the £100,000 threshold taper rule (losing £1 for every £2 earned).
* **National Insurance:** Calculates Class 1 Primary NICs based on current thresholds.

## 🛠️ Tech Stack
* **Language:** Python 3
* **Concepts:** Advanced conditional logic, algorithm design, financial modeling.

## 🚀 How to Run
1. Clone the repository.
2. Run `python tax_calculator.py` in your terminal.
3. Input your gross salary and dividend figures when prompted.
