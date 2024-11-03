# Clinical Trial Recruitment Analysis Platform

## Overview

This project implements an ethical data analysis pipeline for understanding public sentiment around clinical trials and healthcare research participation. It uses publicly available Reddit data to analyze user engagement and sentiment towards clinical trials and generates personalized messages for engagement.

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

- Python 3.12+
- Reddit API credentials
- OpenAI API key with credits available for LLM usage
- Required Python packages (see `requirements.txt`)

### Installation

1. Clone the repository:

   ```bash
   git clone https://github.com/CarolusSolis/turmerik-reddit-scraping.git
   cd turmerik-reddit-scraping
   ```

2. Install dependencies:

   ```bash
   pip install -r requirements.txt
   ```

3. Configure credentials:

   - Copy `config/credentials.json.example` to `config/credentials.json`
   - Add your credentials to the `credentials.json` file

### Running the Pipeline

1. **Data Collection**:

   ```bash
   jupyter notebook notebooks/1_reddit_scraping.ipynb
   ```

   - Adjust subreddit list and keywords in the notebook
   - Monitor rate limits and data volume

2. **Sentiment Analysis**:

   ```bash
   jupyter notebook notebooks/2_sentiment_analysis.ipynb
   ```

3. **User Group Analysis and Message Generation**:

   ```bash
   jupyter notebook notebooks/3_message_generation.ipynb
   ```

   - Review topic modelling results and message templates
   - Create message for each user if desired

## Sample Output

Please refer to the `data/` folder for sample outputs. The `generated_messages.csv` file contains the output of the final message generation step with mock trial information.


## Design Considerations

### Data Collection

- Ethical scraping of public Reddit posts from health-related subreddits
- Keyword-based filtering for clinical trial-related discussions
- Rate limiting and proper API usage compliance
- Data anonymization to ensure privacy during collection
- Metadata tracking for research integrity

### Analysis Pipeline

1. **Data Collection** (`1_reddit_scraping.ipynb`)

   - PRAW-based Reddit data collection
   - Configurable subreddit and keyword lists to allow for customization
   - Code structure in place to support privacy-preserving data collection

2. **Sentiment Analysis** (`2_sentiment_analysis.ipynb`)

   - Utilization of pre-trained sentiment models on HuggingFace for quality and maintainability
   - Cost-effective processing for large datasets

3. **User Group Analysis and Message Generation** (`3_message_generation.ipynb`)

   - Topic modeling (LDA) to identify common themes across users
   - Analysis of frequent questions and concerns by displaying the top words in each topic
   - Generation of evidence-based communication templates, without too much user-specific information in order to avoid over-personalization
   - Human in the loop to review and revise templates

### Technical Decisions

#### Sentiment Analysis Strategy

- **Why pre-trained sentiment analysis models instead of LLMs?**
  - Cost efficiency: LLMs would be overkill for straightforward sentiment classification
  - Scalability: The actual volume of data would be much higher in production
  - Performance: Faster processing time for large-scale analysis

#### Message Generation Approach

- **Hybrid approach for cost-effectiveness:**
  1. First identify interested and neutral patients using traditional sentiment analysis
  2. Use topic modeling (LDA) to group users into distinct segments
  3. Utilize LLMs (e.g., GPT) only for generating template messages for each user group
  - This approach balances personalization with cost efficiency while adhering to ethical guidelines and Reddit's terms of service
  4. Human in the loop: review and revise the template messages as needed, ensuring the messages are not only personalized but also ethical and compliant with clinical trial guidelines

### Ethical Framework

- Strict adherence to Reddit's API Terms of Service and Content Policy
- Focus on aggregate sentiment analysis rather than individual targeting
- All data collection is limited to public posts and comments
- Implementation of robust privacy protection measures
- No direct user targeting or identification

### Challenges

- **Data Privacy and Compliance:** Reddit's API does not allow user-specific information, so we cannot directly target individuals. This limits the effectiveness of personalized messaging.
- **Keyword-based filtering:** This approach may include too many irrelevant discussions if the keywords are not specific enough or there are too many of them. If this proves to be insufficient, we can use a vector-search or LLM-based approach to reduce false positives.
- **User Engagement Limitations:** Since the generated messages cannot be tailored to individual users, the level of engagement might be limited. Ensuring that the generated messages are general enough to be applicable but still engaging remains a balancing act.
- **Sentiment Model Limitations:** Pre-trained sentiment models might not fully capture the nuances of healthcare discussions, leading to inaccurate sentiment categorization. Fine-tuning or supplementing these models with domain-specific data could help, but would add complexity and cost.
