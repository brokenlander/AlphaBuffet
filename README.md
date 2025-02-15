# AlphaBuffet
This project aims to create a fine-tuned language model that captures Warren Buffet's investment philosophy and communication style. The model is built using QLoRA fine-tuning on the Llama 3.3 70B base model, incorporating decades of Buffet's written and spoken wisdom.

## Dataset
The training data draws from three primary sources that capture Buffett's investment philosophy and decision-making process.

### 1. Berkshire Hathaway Shareholder Letters (1977-2023)
- Annual letters written by Warren Buffet to Berkshire Hathaway shareholders
- Contains detailed investment rationale, business principles, and market insights

### 2. Berkshire Annual Meeting Q&A Transcripts (1994-2022)
- Transcribed questions and answers from annual shareholder meetings
- Features Buffet's direct responses to shareholder inquiries
- Includes valuable insights on market conditions, investment decisions, and business philosophy

### 3. "The Essays of Warren Buffett: Lessons for Corporate America"
- Curated collection edited by Lawrence Cunningham
- Thematically organized writings that highlight key principles and teachings
- Provides structured context to Buffet's investment and management philosophy

## Data Preprocessing
The preprocessing pipeline (`DataPreprocessing.ipynb`) transforms source materials into high-quality training data while preserving Buffett's unique insights and communication style.

### Document Processing Strategy
Two optimized chunking strategies are implemented:

#### 1. Numbered Section Strategy
- Splits text based on numbered Q&A sections (e.g., "1.", "2.") 
- Designed for meeting transcripts to preserve dialogue context
- Splits long sections while maintaining Q&A pairs

#### 2. Sentence Overlap Strategy
- Uses spaCy for sentence-based segmentation with overlap
- Max chunk: 1600 chars, min: 500 chars, 2-sentence overlap
- Optimized for narrative documents like letters

### Content Processing Pipeline
The pipeline uses Claude 3.5 Sonnet for sophisticated content processing:

#### 1. Content Validation
Filters content to ensure quality:
- Approves broadly applicable business philosophy and market insights
- Removes transaction specifics, isolated decisions, and raw financial data
- Focuses on enduring principles over temporal details

#### 2. Conversation Generation
Transforms validated content into training pairs:
- Generates contextual questions about key themes
- Constructs answers using Buffett's exact words and ideas
- Maintains his characteristic Q&A style
- Outputs in ShareGPT format for training compatibility
- Maintains source provenance
