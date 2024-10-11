# PDF Generation App

A Python application that generates PDFs using the `FPDF` library. This app includes custom headers, footers, and chapters, and reads chapter content from text files to format and output PDF documents.

## Description

This Python application uses the `FPDF` library to generate a multi-page PDF document. It supports custom header and footer styling, adding chapters, and formatting chapter bodies with multi-column text from external files. You can easily adapt it for creating PDF books, reports, or other documents.

## Installation 
1. Clone this repository: 
```bash 
git clone https://github.com/JordanVF/PDF-Generation-Basics.git
```
2. Navigate to the project directory: 
```bash
cd PDF-Generation-Basics
```
3. Install the required dependencies, FPDF:
```bash
pip install fpdf
```
4. Ensure the text files for chapter content (e.g., para.txt) are available in the appropriate directory.

## Usage
1. Modify the pdf.print_chapter() calls in the script to customize the chapters or text file paths.
2. Run the script
3. The generated PDF will be saved to the specified output file (e.g., PDF Generation Basics/sample.pdf).

## How it works
### Sections
- Header: A custom header with styling (font, colors, alignment) is created using the header() method of the PDF class.
```python
def header(self):
    self.set_font("helvetica", "B", 15)
    width = self.get_string_width(self.title) + 6
    self.set_x((210 - width) / 2)
    self.set_draw_color(0, 80, 180)
    self.set_fill_color(230, 230, 0)
    self.set_text_color(220, 50, 50)
    self.set_line_width(1)
    self.cell(width, 9, self.title, new_x="LMARGIN", new_y="NEXT", align="C", fill=True)
    self.ln(10)
```
- Footer: A custom footer shows the current page number using the footer() method.
```python
def footer(self):
    self.set_y(-15)
    self.set_font("helvetica", "I", 10)
    self.set_text_color(128)
    self.cell(0, 10, f"Page {self.page_no()}/{{nb}}", align="C")
```
- Chapter Layout: Each chapter has a title and body, with content loaded from a text file. The chapter body is displayed with multi-column formatting using the multi_cell() method.
```python
def chapter_title(self, num, label):
    self.set_font("helvetica", "", 12)
    self.set_fill_color(200, 220, 255)
    self.cell(0, 6, f"Chapter {num}: {label}", new_x="LMARGIN", new_y="NEXT", align="L", fill=True)

def chapter_body(self, filepath):
    with open(filepath, "rb") as fh:
        txt = fh.read().decode("latin-1")
    self.set_font("Times", size=12)
    self.multi_cell(0, 5, txt)
    self.ln()
    self.set_font(style="I")
    self.cell(0, 5, "(End of Excerpt)")
```
### PDF Creation
1. Create the PDF Object:
```python
pdf = PDF()
```
2. Set Document Properties: Set the title and author.
```python
pdf.set_title("100 Ways to Learn Programming")
pdf.set_author("Jordan Frankish")
```
3. Add Chapters: Add chapters by specifying the chapter number, title, and the file path of the content.
```python
pdf.print_chapter(1, "Getting started with programming", "PDF Generation Basics/para.txt")
pdf.print_chapter(2, "Which Programming language to learn", "PDF Generation Basics/para.txt")
```
4. Output PDF: Finally, output the PDF to a file.
```python
pdf.output("PDF Generation Basics/sample.pdf")
```

## License

This project is licensed under the MIT License. See the LICENSE file for more details.

