# Xssaqr 🦅

**A Specialized Reflected Cross-Site Scripting (RXSS) Testing Tool**

Xssaqr is a Python-based security testing tool designed specifically for identifying potential Reflected Cross-Site Scripting (RXSS) vulnerabilities in web applications.

The tool focuses on testing URL parameters with payloads and analyzing HTTP responses to identify reflections that may indicate potential RXSS vulnerabilities.

## Features

* **RXSS-Focused Testing:** Dedicated to Reflected Cross-Site Scripting detection.
* **Payload Testing:** Tests parameters using multiple XSS payloads.
* **Response Analysis:** Examines server responses for reflected payloads.
* **Parameter-Based Testing:** Focuses on URL parameters that may be vulnerable to RXSS.
* **Concurrent Scanning:** Uses multithreading to improve scanning speed.
* **Readable Output:** Presents findings in a clear format to help security researchers review results.
* **Command-Line Interface:** Provides options for configuring scans from the terminal.

## Requirements

* Python 3.10+
* pip
* Internet connection to test authorized web targets.
* requests
colorama
urllib3


## Installation

```bash
git clone https://github.com/uevilz/Xssaqr.git
cd Xssaqr
pip install -r requirements.txt
chmod +x xssaqr.py
```

## Usage

Display the available options:

```bash
python3 xssaqr.py -h
```

Run the tool using the options supported by your version.

## How It Works

1. Accepts target URLs or input according to the supported command-line options.
2. Identifies and tests URL parameters.
3. Sends payloads and analyzes the resulting HTTP responses.
4. Reports reflections that may require further investigation.

## Limitations

* A reflected payload does not automatically mean an exploitable XSS vulnerability exists.
* Results depend on the payloads used, response context, and application behavior.
* RXSS testing does not cover every form of XSS, including all DOM-based XSS vulnerabilities.
* Findings should be manually verified before being reported as confirmed vulnerabilities.

## Intended Use

Xssaqr is intended for authorized security testing, penetration testing, and educational research.

**Only test websites and applications that you own or have explicit permission to assess.**

## Author
Far7an

Project: Xssaqr
