# Challenge 1A: PDF Structure Extraction

## Overview

This solution extracts structured outlines from PDF documents, including titles and hierarchical headings (H1, H2, H3) with page numbers. The system is designed for speed, accuracy, and multilingual support.

## Features

- **PDF Processing**: Handles documents up to 50 pages
- **Title Extraction**: Intelligent title detection using multiple methods
- **Heading Detection**: Hierarchical heading extraction (H1, H2, H3)
- **Multilingual Support**: Japanese and CJK script detection (bonus feature)
- **Document Classification**: Adaptive processing for different document types
- **JSON Output**: Clean, structured output format

## Technical Approach

### Core Architecture

The system uses a multi-stage approach:

1. **Document Profiling**: Automatically detects document type (form, report, TOC, flyer)
2. **Language Detection**: Identifies CJK vs Latin scripts for specialized processing
3. **Text Extraction**: Uses PyMuPDF for robust text and metadata extraction
4. **Feature Analysis**: Analyzes font sizes, positions, formatting, and layout
5. **Statistical Classification**: Uses adaptive thresholds for heading detection

### Multilingual Capabilities

The system includes specialized support for Japanese documents:
- Detection of CJK characters and script diversity
- Recognition of Japanese structural markers (第, 章, 節, 項)
- Cultural keyword patterns (概要, 序論, 結論, 参考)
- Adaptive confidence thresholds for different languages

### Document Type Specialists

- **Form Processor**: Strict filtering for application forms
- **Report Processor**: Comprehensive extraction for dense documents  
- **TOC Processor**: Specialized handling for table of contents
- **Flyer Processor**: Fragment merging for visual layouts

## Dependencies

- Python 3.11
- PyMuPDF (fitz) - PDF processing
- NumPy - Mathematical operations
- Standard library modules (pathlib, json, logging, re)

## Docker Usage

### Build the Container

```bash
docker build --platform linux/amd64 -t challenge1a .
```

### Run the Container

```bash
docker run --rm \
  -v $(pwd)/input:/app/input \
  -v $(pwd)/output:/app/output \
  --network none \
  challenge1a
```

The container will:
- Process all PDF files from `/app/input` directory
- Generate corresponding JSON files in `/app/output`
- Work completely offline (no network access)

## Output Format

```json
{
  "title": "Document Title",
  "outline": [
    {"level": "H1", "text": "Introduction", "page": 1},
    {"level": "H2", "text": "Background", "page": 2},
    {"level": "H3", "text": "Methodology", "page": 3}
  ]
}
```

## Performance

- **Speed**: Processes 50-page PDFs in under 10 seconds
- **Memory**: Efficient processing with minimal memory footprint
- **Accuracy**: High precision and recall for heading detection
- **Reliability**: Robust fallback mechanisms for different document types

## Testing

The system has been tested with:
- Academic papers and research documents
- Government forms and applications
- Technical reports and manuals
- Marketing flyers and brochures
- Table of contents documents

Sample test files demonstrate successful extraction across various document types and languages.

## Architecture Highlights

### Adaptive Confidence Thresholds
Different document types use optimized confidence thresholds:
- Forms: 0.90 (strict to avoid noise)
- Reports: 0.30 (inclusive for comprehensive extraction)
- TOC Documents: 0.50 (balanced approach)
- Flyers: 0.60 (visual distinctiveness focus)

### Multi-Feature Scoring
Heading detection combines multiple signals:
- Font size relative to document average
- Spatial position and isolation
- Text formatting (bold, italic, font family)
- Structural patterns (numbering, capitalization)
- Language-specific markers

### Quality Assurance
- Duplicate detection and filtering
- Length-based content validation
- Context-aware text assembly
- Page-based distribution limits

## File Structure

```
Challenge_1a/
├── Dockerfile              # Container configuration
├── process_pdfs.py         # Main processing script
├── requirements.txt        # Python dependencies
├── README.md              # This documentation
├── .gitignore             # Git ignore rules
├── outputs/               # Generated results
└── sample_dataset/        # Test data
    ├── pdfs/              # Input PDF files
    ├── outputs/           # Expected outputs
    └── schema/            # Output format schema
```

## Competition Compliance

This solution meets all Adobe Hackathon Challenge 1A requirements:
- CPU-only processing (no GPU dependencies)
- AMD64 architecture compatibility
- Offline operation (no internet access)
- 10-second processing limit for 50-page PDFs
- Valid JSON output format
- Docker containerization
- Multilingual bonus feature implementation

The system is ready for evaluation and demonstrates robust PDF structure extraction capabilities across diverse document types and languages.
