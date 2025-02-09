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
The preprocessing pipeline (`DataPreprocessing.ipynb`) transforms our source materials into a format suitable for fine-tuning. The pipeline preserves Buffett's unique insights and communication style while creating high-quality training examples.

### Preprocessing Strategy
#### 1. Intelligent Chunking
- Uses spaCy for sentence-based text segmentation
- Creates overlapping chunks to maintain context and coherence:
  - Maximum chunk size: 1500 characters
  - Minimum chunk size: 500 characters
  - 3-sentence overlap between chunks

#### 2. Content Validation & Conversation Generation
- Utilizes a local Llama 3.3 70B - 8bit model for:
  - Validating chunks for meaningful content (filters out tables, headers, etc.)
  - Transforming validated content into natural question-answer pairs
- Maintains Buffett's distinctive communication style
- Focuses on investment principles, business decisions, and market insights

### Implementation Details
- Generates separate JSON files for each source document in ShareGPT format
- Maintains clear links between training data and source documents
- Allows selective reprocessing of specific documents
- Filters out non-instructive content while preserving key insights