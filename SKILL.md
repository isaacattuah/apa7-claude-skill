---
name: apa-7-skill
description: A skill designed to make an APA7 word document
---

# APA 7th Edition Paper Formatting Skill

## Overview
This skill enables Claude to convert raw text and research into professionally formatted APA 7th Edition documents using Python's `python-docx` library. The skill handles all standard APA formatting requirements including title pages, heading levels, references, and document structure.

## When to Use This Skill
Use this skill when users request:
- "Format this as an APA paper"
- "Create an APA 7th edition document from this research"
- "Convert this text to APA format"
- "Generate an academic paper in APA style"
- Any request involving academic paper formatting following APA guidelines

## Prerequisites
**CRITICAL**: Always read the `/mnt/skills/public/docx/SKILL.md` file FIRST before using this skill, as this skill builds upon the docx skill's capabilities.

Install required Python package:
```bash
pip install python-docx --break-system-packages
```

## APA 7th Edition Formatting Standards

### Document-Wide Settings
- **Font**: Times New Roman, 12pt (24 half-points in python-docx)
- **Line Spacing**: Double spacing (2.0 or 480 twips)
- **Margins**: 1 inch on all sides (1440 twips)
- **Page Numbers**: Top right corner of every page (including title page)
- **Alignment**: Left-aligned body text (ragged right edge)

### Title Page Format
The title page should contain (all centered):
1. **Three blank double-spaced lines at top**
2. **Paper Title** (bold, title case)
3. **One blank line**
4. **Author Name(s)**
5. **Institution/Affiliation**
6. **Course Number and Name** (for student papers)
7. **Instructor Name**
8. **Assignment Due Date**

### Five Heading Levels

#### Level 1: Centered, Bold, Title Case
Example: **Introduction** (centered)

#### Level 2: Left-Aligned, Bold, Title Case
Example: **Literature Review**

#### Level 3: Left-Aligned, Bold Italic, Title Case
Example: ***Historical Context***

#### Level 4: Indented, Bold, Title Case, Ending with Period (Run-in Heading)
Example: **Notable Study.** The text of the paragraph begins immediately after the period, on the same line as the heading.

#### Level 5: Indented, Bold Italic, Title Case, Ending with Period (Run-in Heading)
Example: ***Specific Finding.*** The text begins on the same line, following the heading and period.

### Paragraph Formatting
- **First Line Indent**: 0.5 inches (720 twips) for all body paragraphs
- **Exception**: No indent for paragraphs immediately following headings (optional style choice)
- **Line Spacing**: Double-spaced throughout

### References Section
- Starts on a new page
- Title "References" as Level 1 heading (centered, bold)
- **Hanging Indent**: 0.5 inches (720 twips) - first line flush left, subsequent lines indented
- Alphabetically ordered by author last name
- Double-spaced with no extra spacing between entries

### Special Elements

#### Math/Statistical Formulas
- Center-aligned when displayed separately
- Use italics for variables
- Example: *y* = *mx* + *b*

## Python Implementation

```python
from docx import Document
from docx.shared import Pt, Inches
from docx.enum.text import WD_ALIGN_PARAGRAPH, WD_LINE_SPACING
from docx.oxml.ns import qn
from docx.oxml import OxmlElement

def create_apa_document(title_data, body_content, references):
    """
    Create an APA 7th edition formatted document.
    
    Parameters:
    -----------
    title_data : dict
        {'title': str, 'author': str, 'institution': str, 
         'course': str, 'instructor': str, 'date': str}
    body_content : list of dict
        [{'type': 'heading', 'level': int, 'text': str},
         {'type': 'paragraph', 'text': str},
         {'type': 'math', 'formula': str}]
    references : list of str
        ['Author, A. A. (Year). Title...', ...]
    """
    doc = Document()
    
    # Set default font and spacing
    style = doc.styles['Normal']
    font = style.font
    font.name = 'Times New Roman'
    font.size = Pt(12)
    
    paragraph_format = style.paragraph_format
    paragraph_format.line_spacing_rule = WD_LINE_SPACING.DOUBLE
    paragraph_format.space_before = Pt(0)
    paragraph_format.space_after = Pt(0)
    
    # Set margins
    for section in doc.sections:
        section.top_margin = Inches(1)
        section.bottom_margin = Inches(1)
        section.left_margin = Inches(1)
        section.right_margin = Inches(1)
    
    # Add page numbers
    add_page_numbers(doc)
    
    # Create title page
    create_title_page(doc, title_data)
    
    # Add page break before body
    doc.add_page_break()
    
    # Body content
    create_body(doc, body_content)
    
    # References on new page
    doc.add_page_break()
    create_references(doc, references)
    
    return doc

def add_page_numbers(doc):
    """Add page numbers to the top right of every page."""
    section = doc.sections[0]
    header = section.header
    header_para = header.paragraphs[0] if header.paragraphs else header.add_paragraph()
    header_para.alignment = WD_ALIGN_PARAGRAPH.RIGHT
    
    run = header_para.add_run()
    fldChar1 = OxmlElement('w:fldChar')
    fldChar1.set(qn('w:fldCharType'), 'begin')
    
    instrText = OxmlElement('w:instrText')
    instrText.set(qn('xml:space'), 'preserve')
    instrText.text = "PAGE"
    
    fldChar2 = OxmlElement('w:fldChar')
    fldChar2.set(qn('w:fldCharType'), 'end')
    
    run._r.append(fldChar1)
    run._r.append(instrText)
    run._r.append(fldChar2)

def create_title_page(doc, title_data):
    """Create APA-formatted title page."""
    # Three blank lines
    for _ in range(3):
        doc.add_paragraph()
    
    # Title (bold, centered)
    title_para = doc.add_paragraph()
    title_para.alignment = WD_ALIGN_PARAGRAPH.CENTER
    title_run = title_para.add_run(title_data['title'])
    title_run.bold = True
    
    # Blank line
    doc.add_paragraph()
    
    # Other info (centered)
    for key in ['author', 'institution', 'course', 'instructor', 'date']:
        if key in title_data:
            para = doc.add_paragraph(title_data[key])
            para.alignment = WD_ALIGN_PARAGRAPH.CENTER

def create_body(doc, body_content):
    """Create document body with proper heading levels."""
    i = 0
    while i < len(body_content):
        block = body_content[i]
        
        if block['type'] == 'heading':
            level = block['level']
            text = block['text']
            next_block = body_content[i + 1] if i + 1 < len(body_content) else None
            
            # Handle run-in headings (levels 4-5)
            if level in [4, 5] and next_block and next_block['type'] == 'paragraph':
                para = doc.add_paragraph()
                para.paragraph_format.left_indent = Inches(0.5)
                
                heading_run = para.add_run(text + '. ')
                heading_run.bold = True
                if level == 5:
                    heading_run.italic = True
                
                para.add_run(next_block['text'])
                i += 2
                continue
            else:
                # Regular headings
                para = doc.add_paragraph(text)
                if level == 1:
                    para.alignment = WD_ALIGN_PARAGRAPH.CENTER
                    para.runs[0].bold = True
                elif level == 2:
                    para.runs[0].bold = True
                elif level == 3:
                    para.runs[0].bold = True
                    para.runs[0].italic = True
        
        elif block['type'] == 'paragraph':
            para = doc.add_paragraph(block['text'])
            para.paragraph_format.first_line_indent = Inches(0.5)
        
        elif block['type'] == 'math':
            para = doc.add_paragraph()
            para.alignment = WD_ALIGN_PARAGRAPH.CENTER
            formula_run = para.add_run(block['formula'])
            formula_run.italic = True
        
        i += 1

def create_references(doc, references):
    """Create APA-formatted references section."""
    heading = doc.add_paragraph('References')
    heading.alignment = WD_ALIGN_PARAGRAPH.CENTER
    heading.runs[0].bold = True
    
    for ref in references:
        if ref.strip():
            para = doc.add_paragraph(ref.strip())
            para.paragraph_format.left_indent = Inches(0.5)
            para.paragraph_format.first_line_indent = Inches(-0.5)
```

## Text Parsing

Parse markdown-style text into structured content:

```python
def parse_markdown_text(raw_text):
    """Parse markdown text into body_content and references."""
    body_content = []
    references = []
    in_references = False
    current_paragraph = []
    
    for line in raw_text.split('\n'):
        stripped = line.strip()
        
        # Check for references section
        if stripped.lower() == 'references' or stripped == '---':
            in_references = True
            if current_paragraph:
                body_content.append({
                    'type': 'paragraph',
                    'text': ' '.join(current_paragraph)
                })
                current_paragraph = []
            continue
        
        if in_references:
            if stripped:
                references.append(stripped)
            continue
        
        # Parse headings (#, ##, ###, etc.)
        if stripped.startswith('#'):
            if current_paragraph:
                body_content.append({
                    'type': 'paragraph',
                    'text': ' '.join(current_paragraph)
                })
                current_paragraph = []
            
            level = len(stripped) - len(stripped.lstrip('#'))
            text = stripped.lstrip('#').strip()
            body_content.append({
                'type': 'heading',
                'level': min(level, 5),
                'text': text
            })
        
        # Parse math (in backticks)
        elif '`' in stripped:
            if current_paragraph:
                body_content.append({
                    'type': 'paragraph',
                    'text': ' '.join(current_paragraph)
                })
                current_paragraph = []
            
            import re
            match = re.search(r'`(.+?)`', stripped)
            if match:
                body_content.append({
                    'type': 'math',
                    'formula': match.group(1)
                })
        
        # Regular paragraph
        elif stripped:
            current_paragraph.append(stripped)
        elif current_paragraph:
            body_content.append({
                'type': 'paragraph',
                'text': ' '.join(current_paragraph)
            })
            current_paragraph = []
    
    return body_content, references
```

## Workflow

1. **Extract title page data** from user input
2. **Parse body content** using markdown structure
3. **Identify references section** (after "---" or "References")
4. **Generate document** with proper APA formatting
5. **Save to outputs** directory

## Quality Checklist

Before delivering, verify:
- [ ] Title page has 3 blank lines at top
- [ ] Page numbers on all pages (top right)
- [ ] All paragraphs have 0.5" first-line indent
- [ ] Heading levels follow APA format exactly
- [ ] References have 0.5" hanging indent
- [ ] Double spacing throughout
- [ ] 1-inch margins on all sides
- [ ] Times New Roman 12pt font
- [ ] Run-in headings (levels 4-5) formatted correctly

## Common Pitfalls to Avoid

1. **Wrong spacing**: Use `WD_LINE_SPACING.DOUBLE`, not `2.0`
2. **Incorrect indents**: Use `Inches()` for measurements
3. **Run-in headings**: Levels 4-5 must be on same line as paragraph
4. **Page numbers**: Must appear on ALL pages including title page
5. **Hanging indent**: References need negative first_line_indent

## Example Usage

```python
# User provides:
title_data = {
    'title': 'The Impact of AI in Healthcare',
    'author': 'Dr. Jane Smith',
    'institution': 'Medical University',
    'course': 'MED-500',
    'instructor': 'Prof. John Doe',
    'date': 'October 16, 2025'
}

raw_text = """
# Introduction
AI is transforming healthcare delivery.

## Background
Historical context of AI in medicine.

---
References
Smith, J. (2023). AI in medicine. Journal, 10(2), 123-145.
"""

# Parse and create
body_content, references = parse_markdown_text(raw_text)
doc = create_apa_document(title_data, body_content, references)
doc.save('/mnt/user-data/outputs/apa_paper.docx')
```

## Tips for Best Results

1. Always ask for missing title page information
2. Don't rewrite content, only apply formatting
3. Validate references follow APA format
4. Use consistent markdown for headings
5. Save final output to `/mnt/user-data/outputs/`

This skill ensures all APA 7th edition requirements are met automatically.
