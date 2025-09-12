# Wayback Machine Historical Website Scraper

A Python tool that retrieves and analyzes historical snapshots of websites using the Internet Archive's Wayback Machine. This scraper collects website metadata, content summaries, and tracking information across multiple time periods for research and analysis purposes.

## Overview

This script systematically fetches historical snapshots of domains from the Wayback Machine at different time intervals (current, 1 year ago, 2.5 years ago, and 4 years ago) to analyze how websites have evolved over time.

## Features

- **Multi-Temporal Analysis**: Captures snapshots from current date and 1, 2.5, and 4 years ago
- **Concurrent Processing**: Uses ThreadPoolExecutor for efficient parallel scraping
- **Proxy Support**: Implements proxy rotation to avoid rate limiting
- **Content Summarization**: Generates intelligent summaries of webpage content using LSA
- **Redirection Detection**: Identifies when snapshots contain redirections
- **CSV Export**: Outputs data in structured CSV format with timestamps

## Data Collected

For each domain and time period, the scraper extracts:

| Field | Description |
|-------|-------------|
| **Domain** | Target website domain |
| **Title** | HTML page title |
| **Description** | Meta description content |
| **Keywords** | Meta keywords (if available) |
| **Content Summary** | AI-generated summary of page content |
| **Date** | Snapshot date (YYYYMMDD format) |
| **Redirection** | Whether the snapshot contains redirects |

## Installation

### Dependencies

```bash
pip install requests beautifulsoup4 sumy nltk
```

### Additional Setup

Download required NLTK data:
```python
import nltk
nltk.download('punkt')
nltk.download('stopwords')
```

## Configuration

Create a `config.py` file with the following variables:

```python
# File containing list of domains (one per line)
DOMAINS_FILE = 'domains.txt'

# Number of concurrent threads
NUM_THREADS = 10

# File containing proxy list (format: ip:port:username:password)
PROXIES_FILE = 'proxies.txt'
```

### File Formats

**domains.txt**:
```
example.com
company.org
business.net
```

**proxies.txt**:
```
192.168.1.1:8080:username:password
10.0.0.1:3128:user:pass
```

## Usage

### Basic Execution

```bash
python wayback_scraper.py
```

### Output

The script generates a timestamped CSV file (e.g., `output_20240101_123045.csv`) containing all collected data.

### Sample Output

```csv
domain,title,desc,kw,content_summary,date,redirection
example.com,Example Company,Leading provider of...,business,We provide innovative solutions...,20240101,
example.com,Example Corp,Technology solutions...,tech,Our company specializes in...,20230101,yes
```

## Time Periods Analyzed

The scraper automatically calculates and fetches snapshots from:

- **Current Date**: Most recent available snapshot
- **1 Year Ago**: Historical comparison point
- **2.5 Years Ago**: Medium-term historical data
- **4 Years Ago**: Long-term historical perspective

## Technical Details

### Architecture

- **Concurrent Processing**: Utilizes ThreadPoolExecutor for parallel execution
- **Error Handling**: Graceful handling of missing snapshots and network errors
- **Content Analysis**: Uses LSA (Latent Semantic Analysis) for content summarization
- **Proxy Rotation**: Random proxy selection for each request

### Snapshot Validation

The scraper includes logic to:
- Detect valid vs. invalid snapshots
- Identify error pages (404s)
- Flag redirected content
- Handle missing metadata gracefully

### Content Summarization

Uses the `sumy` library with LSA algorithm to generate intelligent summaries:
- Analyzes full webpage content
- Extracts key sentences
- Provides configurable summary length

## Error Handling

The script handles common issues:
- **Network timeouts**: Continues processing other domains
- **Missing snapshots**: Records empty fields for unavailable data
- **Proxy failures**: Attempts with different proxies
- **Invalid HTML**: Graceful parsing failures

## Use Cases

### Research Applications
- **Website Evolution Analysis**: Track how companies change their messaging
- **Digital Archaeology**: Study historical web content
- **Brand Monitoring**: Analyze competitor positioning over time
- **Academic Research**: Historical web content studies

### Business Intelligence
- **Competitor Analysis**: Historical positioning and messaging
- **Market Research**: Industry evolution tracking
- **Brand History**: Documentation of company evolution

## Legal and Ethical Considerations

### Internet Archive Usage
- **Respect Rate Limits**: The Internet Archive is a non-profit resource
- **Attribution**: Credit the Internet Archive in research publications
- **Fair Use**: Ensure usage complies with fair use principles

### Best Practices
- Implement reasonable delays between requests
- Use proxies responsibly
- Respect the Internet Archive's terms of service
- Consider the research value vs. resource consumption

## Performance Optimization

### Threading Configuration
```python
# Adjust based on system resources and respect for IA servers
NUM_THREADS = 5  # Conservative approach
NUM_THREADS = 20  # More aggressive (use carefully)
```

### Memory Management
- Process large domain lists in batches
- Implement periodic garbage collection for long runs
- Monitor memory usage with extensive historical data

## Troubleshooting

### Common Issues

1. **Empty Results**: 
   - Check if domains existed during specified time periods
   - Verify Wayback Machine has snapshots for those dates

2. **Proxy Errors**:
   - Validate proxy credentials and connectivity
   - Implement proxy health checking

3. **Slow Performance**:
   - Reduce thread count
   - Add delays between requests
   - Check network connectivity

### Debug Mode

Add debug logging to troubleshoot issues:
```python
import logging
logging.basicConfig(level=logging.DEBUG)
```

## Contributing

When contributing to this project:
- Test with small domain lists first
- Respect the Internet Archive's resources
- Document any new features thoroughly
- Consider the ethical implications of large-scale scraping

## Dependencies

```
requests>=2.25.0
beautifulsoup4>=4.9.0
sumy>=0.8.0
nltk>=3.6.0
```

## License

This project is intended for educational and research purposes. Users must comply with:
- Internet Archive's Terms of Use
- Applicable copyright laws
- Fair use principles for research

---
**⚠️ Important**: The Internet Archive is a valuable non-profit resource. Please use this tool responsibly and consider making a donation to support their mission if you find this data valuable for your research.
