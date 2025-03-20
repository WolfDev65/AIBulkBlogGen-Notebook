# AIBulkBlogGen-Notebook

# Bulk Blog Generation Script: Documentation & Instructions

## Introduction

This script automates the generation of multiple high-quality blog articles using AI language models. It reads blog topics from a CSV file, generates complete articles with proper structure and SEO metadata, and saves each as a formatted Markdown file ready for publication.

## Installation Requirements

### Prerequisites

- Python 3.8 or higher
- API key for an LLM provider (default: Fireworks AI)

### Required Python Packages

```bash
pip install litellm tqdm pydantic pandas requests
```

## CSV File Setup

The script reads from a CSV file named `Blog_Gen.csv` in the script directory. Create this file with the following columns:

| Column | Description | Example |
|--------|-------------|---------|
| topic | Blog article title | "10 Tips for Effective Time Management" |
| category | Main category | "Productivity" |
| subcatergory | Subcategory | "Time Management" |
| pub_date_str | Publication date (YYYY-MM-DD) | "2025-03-20" |
| up_date_str | Update date (optional) | "2025-03-20" |
| status | Generation status (leave empty for new articles) | "" |

### Example CSV Content:
```
topic,category,subcatergory,pub_date_str,up_date_str,status
"How to Improve Work-Life Balance",Lifestyle,Wellbeing,2025-03-20,2025-03-20,
"10 Best Tools for Remote Work",Technology,Productivity,2025-03-21,2025-03-21,
```

## Configuration

### API Keys

1. Edit the script to insert your API key:
```python
os.environ["FIREWORKS_AI_API_KEY"] = "YOUR_API_KEY_HERE"
```

### Model Settings

The script uses Fireworks AI's DeepSeek model by default. You can modify these settings:

```python
model_config = {
    'model_name': 'fireworks_ai/accounts/fireworks/models/deepseek-v3',  # Change model
    'rate_limit_delay': (5, 6)  # Adjust delay between API calls (min, max seconds)
}
```

Alternative models can be used by changing `model_name` and adding the appropriate environment variables for their API keys.

## Running the Script

1. Prepare your `Blog_Gen.csv` file with your desired blog topics
2. Run the script:
```bash
python blog_generator.py
```

3. The script will:
   - Process each row in the CSV that doesn't have a status
   - Generate complete articles with proper structure
   - Save each article to the `generated_articles` folder
   - Update the CSV with the generation status

The script uses multithreading with 3 workers by default, allowing it to generate multiple articles simultaneously.

## Output

### Generated Files

Each article is saved as a Markdown file in the `generated_articles` folder. The filename is a lowercase, hyphenated version of the article title.

### Markdown Structure

Each generated article includes:

1. YAML front matter with:
   - Title, publication date, update date
   - Category and subcategory
   - Cover image URL (default placeholder)
   - SEO metadata (title, description, keywords)
   - Excerpt and tags

2. Article body with:
   - Opening section
   - Multiple body sections with subheadings
   - Conclusion section

### CSV Updates

The script updates the `status` column in your CSV:
- `TRUE` for successfully generated articles
- `ERROR: [error message]` for failed generations

## Troubleshooting

### Common Issues

1. **API Rate Limiting**: The script includes rate limit handling, but you may need to adjust the `rate_limit_delay` if you encounter errors.

2. **CSV Format Issues**: Ensure your CSV is properly formatted with the required columns. Use double quotes around fields that contain commas.

3. **Generation Errors**: If an article fails to generate, check the log file and the error message in the CSV's status column.

### Logging

The script creates a `blog_generation.log` file with detailed information about each step and any errors encountered.

## Advanced Usage

### Adjusting Output Length

The script generates approximately 2000-2500 word articles with 3-7 main sections. You can modify these defaults in the `generate_outline` and `generate_section` methods.

### Customizing SEO Metadata

The SEO metadata generation can be customized by editing the `generate_seo_metadata` method.

### Parallel Processing

To change the number of articles generated in parallel, adjust the `max_workers` parameter:

```python
with ThreadPoolExecutor(max_workers=3) as executor:  # Change to desired number
```

## Example Workflow

1. Create a CSV with 10 blog topics
2. Run the script
3. Monitor progress in the console and log file
4. Once complete, review the generated articles in the `generated_articles` folder
5. Edit as needed and publish
6. For the next batch, add new topics to the CSV (the script will skip rows with status)

---

This powerful script enables you to rapidly generate high-quality blog content at scale, saving considerable time while maintaining consistent structure and SEO optimization.

---
