# Markitdown


https://github.com/microsoft/markitdown?utm_source=tldrai




MarkItDown is a utility for converting various files to Markdown (e.g., for indexing, text analysis, etc). It supports:

    PDF
    PowerPoint
    Word
    Excel
    Images (EXIF metadata and OCR)
    Audio (EXIF metadata and speech transcription)
    HTML
    Text-based formats (CSV, JSON, XML)
    ZIP files (iterates over contents)
To install MarkItDown, use pip: pip install markitdown. Alternatively, you can install it from the source: pip install -e .

Usage
Command-Line
    markitdown path-to-file.pdf > document.md
Or use -o to specify the output file:

    markitdown path-to-file.pdf -o document.md
You can also pipe content:

cat path-to-file.pdf | markitdown
Python API
Basic usage in Python:
    
    from markitdown import MarkItDown
    
    md = MarkItDown()
    result = md.convert("test.xlsx")
    print(result.text_content)
To use Large Language Models for image descriptions, provide llm_client and llm_model:

    from markitdown import MarkItDown
    from openai import OpenAI
    
    client = OpenAI()
    md = MarkItDown(llm_client=client, llm_model="gpt-4o")
    result = md.convert("example.jpg")
    print(result.text_content)



