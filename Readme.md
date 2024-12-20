
# FastAPI OCR Extraction Service

This project provides an OCR-based text extraction service using FastAPI. It processes uploaded PDF files to extract Hindi text from table grids and outputs the result in multiple formats: Excel, DOCX, and plain text.

## Key Features

- **OCR Text Extraction**: Uses Tesseract OCR to extract Hindi text from scanned images or PDFs.
- **Grid Detection**: Identifies and extracts text from specific grid-like structures in the PDF pages.
- **Multiple Output Formats**: Supports generating output files in Excel (.xlsx), DOCX (.docx), and plain text (.txt).
- **FastAPI Service**: The service is built with FastAPI for efficient handling of requests and responses.

## Technologies Used

- **FastAPI**: For creating the API.
- **Tesseract OCR**: For optical character recognition (OCR).
- **OpenCV**: For image processing and grid detection.
- **PDF2Image**: For converting PDF pages into images.
- **Openpyxl**: For generating Excel files.
- **python-docx**: For creating DOCX files.

## Installation Instructions

1. **Clone the repository**:
   ```
   git clone <repository_url>
   cd <repository_directory>
   ```

2. **Create a virtual environment**:
   ```
   python3 -m venv myenv
   source myenv/bin/activate  # On Windows: myenv\Scripts\activate
   ```

3. **Install required dependencies**:
   ```
   pip install -r requirements.txt
   ```

4. **Install Tesseract OCR** (if not already installed):
   - On Ubuntu/Debian:
     ```
     sudo apt-get install tesseract-ocr
     ```
   - On Windows, download from [Tesseract's official page](https://github.com/tesseract-ocr/tesseract).

5. **Start the FastAPI server**:
   ```
   uvicorn pipeline:app --reload
   ```

   The API will be available at `http://localhost:8000`.

## API Endpoints

### `/extract-text/` (POST)
Extract text from the uploaded PDF file, with an option to select page range and output format.

**Parameters**:
- `file`: PDF file to upload.
- `page_start`: Start page number (inclusive).
- `page_end`: End page number (inclusive).
- `output_format`: Output format (`excel`, `docx`, `text`).

**Example Request**:
```bash
curl -X 'POST' \
  'http://localhost:8000/extract-text/' \
  -F 'file=@your_file.pdf' \
  -F 'page_start=1' \
  -F 'page_end=2' \
  -F 'output_format=excel'
```

**Response**:
Returns a list of URLs to download the output files.

```json
{
  "file_paths": [
    "http://localhost:8000/download/output_file.xlsx"
  ]
}
```


## Process Flow

1. **File Upload**: The user uploads a PDF file along with the desired page range and output format (Excel, DOCX, or text).
2. **PDF to Image Conversion**: The uploaded PDF is converted into images for processing.
3. **Grid Detection**: The pages are processed to detect grid-like structures.
4. **OCR Text Extraction**: Text is extracted from each detected grid cell using Tesseract OCR.
5. **Output Generation**: The extracted text is saved into the selected format (Excel, DOCX, or text).
6. **File Download**: The generated files are available for download via unique URLs.

## Example Use Case

- Upload a PDF file that contains tabular data in Hindi.
- Extract the text and save it into an Excel, DOCX, or plain text file.
- Download the resulting file(s) through the provided link. You can paste the link in web then it will download.

## Troubleshooting

- Ensure Tesseract OCR is correctly installed and its path is configured in the script (`pytesseract.pytesseract.tesseract_cmd`).
- If OCR is not detecting text properly, check the image preprocessing settings in the `preprocess_image` function.
- Ensure the page numbers provided in the request are within the bounds of the PDF.
