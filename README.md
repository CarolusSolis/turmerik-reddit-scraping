# Clinical Trial Recruitment Analysis Platform

## Overview
This project implements an ethical data analysis pipeline for understanding public sentiment around clinical trials and healthcare research participation. It uses publicly available Reddit data to analyze general attitudes and concerns about clinical trials, helping researchers better understand and address common questions and hesitations about trial participation.

### Technical Decisions

#### Sentiment Analysis Strategy
- **Why pre-trained models instead of LLMs?**
  - Cost efficiency: LLMs would be overkill for straightforward sentiment classification
  - Scalability: The actual volume of data would be much higher in production
  - Consistency: Pre-trained sentiment models provide more consistent results for this specific task
  - Performance: Faster processing time for large-scale analysis

#### Message Generation Approach
- **Hybrid approach for cost-effectiveness:**
  1. First identify interested and neutral patients using traditional sentiment analysis
  2. Use topic modeling to group users into distinct segments
  3. Utilize LLMs (e.g., GPT) only for generating template messages for each user group
  - This approach balances personalization with cost efficiency while adhering to ethical guidelines and Reddit's terms of service


## Design Considerations

### Ethical Framework
- Strict adherence to Reddit's [API Terms of Service](https://www.reddit.com/wiki/api) and [Content Policy](https://www.reddit.com/wiki/contentpolicy)
- Focus on aggregate sentiment analysis rather than individual targeting
- All data collection is limited to public posts and comments
- Implementation of robust privacy protection measures
- No direct user targeting or identification

### Data Collection
- Ethical scraping of public Reddit posts from health-related subreddits
- Keyword-based filtering for clinical trial related discussions
- Rate limiting and proper API usage compliance
- Data anonymization at collection time
- Metadata tracking for research validity

### Analysis Pipeline
1. **Data Collection** (`1_reddit_scraping.ipynb`)
   - PRAW-based Reddit data collection
   - Configurable subreddit and keyword lists
   - Privacy-preserving data structure
   - Built-in rate limiting and error handling

2. **Sentiment Analysis** (`2_sentiment_analysis.ipynb`)
   - Efficient pre-trained model utilization
   - Focus on understanding general attitudes and concerns
   - Aggregate-level analysis to maintain privacy
   - Cost-effective processing for large datasets

3. **Message Analysis** (`3_message_analysis.ipynb`)
   - Topic modeling for identifying common themes
   - Understanding frequent questions and concerns
   - Analysis of effective communication strategies
   - Generation of evidence-based communication guidelines

## Project Structure
```
project/
├── notebooks/
│   ├── 1_reddit_scraping.ipynb
│   ├── 2_sentiment_analysis.ipynb
│   └── 3_message_analysis.ipynb
├── data/
│   ├── raw_reddit_data.json
│   ├── sentiment_results.json
│   └── analysis_results.json
├── config/
│   └── credentials.json
└── README.md
```

## Setup Instructions

### Prerequisites
- Python 3.8+
- Reddit API credentials
- Access to Hugging Face's sentiment analysis models
- Required Python packages (see requirements.txt)

### Installation
1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/clinical-trial-analysis.git
   cd clinical-trial-analysis
   ```

2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

3. Configure credentials:
   - Copy `config/credentials.json.example` to `config/credentials.json`
   - Add your Reddit API credentials

### Running the Pipeline

1. Data Collection:
   ```bash
   jupyter notebook notebooks/1_reddit_scraping.ipynb
   ```
   - Adjust subreddit list and keywords in the notebook
   - Monitor rate limits and data volume

2. Sentiment Analysis:
   ```bash
   jupyter notebook notebooks/2_sentiment_analysis.ipynb
   ```
   - Review and adjust sentiment thresholds if needed
   - Monitor processing resource usage

3. Message Analysis:
   ```bash
   jupyter notebook notebooks/3_message_analysis.ipynb
   ```
   - Review topic modeling results
   - Analyze communication patterns
