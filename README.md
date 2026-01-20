# Provider-Clinic Referral Linking System

An automated system for discovering healthcare providers and clinics in El Paso, TX / Las Cruces, NM, and intelligently linking individual providers to their associated clinics using Google Places API and OpenAI.

## Overview

This project consists of two main phases:

1. **Data Discovery** - Searches Google Places API for healthcare facilities across 50+ ZIP codes in El Paso and Las Cruces
2. **Provider-Clinic Linking** - Uses LLM (GPT) to classify entities and intelligently link providers to clinics at the same address

## Features

- **Google Places Integration** - Automated place discovery with pagination support
- **Address Normalization** - Standardizes addresses for accurate grouping
- **LLM-Powered Classification** - Classifies records as PROVIDER, CLINIC, or UNKNOWN
- **Intelligent Linking** - Links providers to clinics with confidence scores
- **Rate Limiting** - Built-in delays to respect API quotas

## Prerequisites

- Python 3.8+
- Google Cloud API Key with Places API enabled
- OpenAI API Key

## Installation

1. Clone this repository:
```bash
git clone https://github.com/yourusername/referral-linking.git
cd referral-linking
```

2. Create a virtual environment:
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. Install dependencies:
```bash
pip install -r requirements.txt
```

4. Create a `.env` file with your API keys:
```bash
cp .env.example .env
```

Edit `.env` and add your actual API keys:
```
GOOGLE_API_KEY=your_google_places_api_key
OPENAI_API_KEY=your_openai_api_key
```

## Usage

### Running the Full Pipeline

Open and run `AAV.ipynb` in Jupyter:

1. **Cell 1-4**: Install required packages
2. **Cell 5**: Configure API keys and search for healthcare facilities across all ZIP codes
3. **Cell 6-21**: Load data and run provider-clinic linking via LLM
4. **Cell 22**: Export results to CSV

```python
# Key outputs:
df                    # Raw places data
tagged_df             # Classified data with entity types
links_df              # Provider→Clinic mapping table
```

### Output Files

- `el_paso_google_elpaso_places_full.csv` - Raw place discovery results
- `provider_complete_linked_lascruces_elpaso.csv` - Final provider-clinic links

## Project Structure

```
.
├── AAV.ipynb                                    # Main workflow notebook
├── refferral.ipynb                              # Work-in-progress notebook
├── Physician.py                                 # Helper module (minimal)
├── el_paso_google_elpaso_places_full.csv       # Raw place data
├── existing_provider_clinic_list.csv           # Reference data
└── provider_complete_linked_lascruces_elpaso.csv # Final output
```

## Key Workflows

### Phase 1: Google Places Discovery
- Searches for 5 facility types: clinic, doctor, medical center, urgent care, dialysis center
- Covers 50+ ZIP codes in El Paso/Las Cruces/surrounding areas
- Removes duplicate places automatically

### Phase 2: LLM-Based Provider Linking
1. Normalizes addresses (standardizes street abbreviations, case, etc.)
2. Groups places by normalized address
3. For each address group, uses GPT to:
   - Classify each record as PROVIDER, CLINIC, or UNKNOWN
   - Link providers to the best matching clinic
   - Return confidence scores and reasoning

## API Costs

- **Google Places API**: ~$0.32 per place for basic info + phone/website
- **OpenAI GPT**: Charged per token for classification calls

Monitor usage in Google Cloud Console and OpenAI dashboard.

## Configuration

Edit constants in AAV.ipynb Cell 5:
- `SEARCH_QUERIES` - Terms to search for
- `EL_PASO_ZIPS` - ZIP codes to cover
- Model selection in `build_provider_clinic_table()`

## Troubleshooting

**API Key Error**: Ensure `.env` file exists with valid keys:
```bash
python -c "from dotenv import load_dotenv; import os; load_dotenv(); print(os.getenv('GOOGLE_API_KEY'))"
```

**Rate Limiting**: Built-in delays respect Google's requirements. Adjust in:
- `time.sleep(2)` - Google pagination token validation
- `time.sleep(0.1)` - Details fetching rate limit

## License

[Add your license here]

## Contributing

[Add contribution guidelines]

## Contact

[Add contact information]
