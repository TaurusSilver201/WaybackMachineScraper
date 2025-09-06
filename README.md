# 🌐 Wayback Machine Snapshot Scraper

This Python script scrapes the **Wayback Machine (Internet Archive)** for snapshots of given domains. It extracts metadata (title, description, keywords), summarizes homepage text, and saves the results into a CSV file.

---

## 🚀 Features
- Fetches snapshots of domains from the **Wayback Machine**.
- Extracts:
  - Page **title**
  - **Meta description**
  - **Meta keywords**
- Detects whether a snapshot is a **redirection**.
- Summarizes homepage text using **Sumy LSA**.
- Supports proxies (`ip:port:user:pass`).
- Multithreaded scraping with `ThreadPoolExecutor`.
- Exports results into a **timestamped CSV**.

---

## 📂 Input Files
- `DOMAINS_FILE` → List of domains (one per line).
- `PROXIES_FILE` → List of proxies in `ip:port:user:pass` format.
- `NUM_THREADS` → Number of concurrent workers.

Example `domains.txt`:
example.com
openai.com
github.com


Example `proxies.txt`:
127.0.0.1:8080:user:pass
192.168.0.10:9090:user:pass


---

## 📅 Snapshot Dates
For each domain, snapshots are requested from:
- **Today**
- **1 year ago**
- **2.5 years ago**
- **4 years ago**

---

## ⚙️ How It Works
1. Reads domain list and proxy list.
2. For each domain, builds Wayback Machine URL: `http://web.archive.org/web/<snapshot_date>/<domain>`
3. Downloads page using a random proxy.
4. Extracts metadata and homepage summary.
5. Saves structured results into a CSV file.

---

## 📊 Output
The script generates a CSV file: `output_YYYYMMDD_HHMMSS.csv`

### Example Row

| domain | title | desc | kw | content_summary | date | redirection |
|---|---|---|---|---|---|---|
| example.com | Example Co | Best example site. | examples | Short summary here… | 20230906 | yes |

---

## ▶️ Usage
Run the script with:
```bash
python snapshot_scraper.py
📦 Dependencies
requests

beautifulsoup4

sumy

pandas (optional if exporting differently)

Install requirements:

Bash

pip install requests beautifulsoup4 sumy pandas
⚠️ Notes
The script uses random proxies to avoid blocking.

If you don’t have proxies, you can remove the proxy logic and call requests.get(url) directly.
