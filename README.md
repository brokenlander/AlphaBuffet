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

## Model Training
Fine-tuning performed using Unsloth's optimized implementation:

### Base Model
- Microsoft Phi-4 base model
- Max sequence length: 8192 tokens
- Full precision training (no 4-bit quantization)

### LoRA Configuration
- Rank: 32
- Alpha: 32
- Target modules: attention and feed-forward layers
- Dropout: 0
- Gradient checkpointing: enabled with Unsloth optimization

### Training Parameters
- Batch size: 2 per device
- Gradient accumulation: 4 steps
- Learning rate: 2e-4
- Weight decay: 0.01
- Scheduler: Linear
- Epochs: 3
- Mixed precision: bfloat16
- Optimizer: AdamW 8-bit

### Chat Template
- Using Phi-4 template formatting with instruction-response pairs
- Training focused on response generation only, masking instruction tokens to preserve the model's instruction-following capabilities.

## Model Quantization
Post-training quantization using LLM-Compressor:

### Quantization Strategy
- Method: FP8 Dynamic quantization
- Target: All linear layers except LM head
- Weight quantization: Static, per-channel
- Activation quantization: Dynamic, per-token
- No calibration data required

### Implementation
- Applied to LoRA-merged model
- Maintains model quality with reduced precision
- Optimized for inference deployment
- Weights and tokenizer saved in HuggingFace format

Final model available at HuggingFace Hub: `brokenlander/AlphaBuffet-Phi-FP8-Dynamic`
