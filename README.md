# APA 7th Edition Formatting Skill
## Claude Skill Package

This ZIP contains everything needed to add APA 7th edition formatting capabilities to Claude.

## 📦 Package Contents

```
apa-skill/
├── SKILL.md                    # Main skill file (Claude reads this)
├── README.md                   # Full documentation
├── QUICK_START.md              # Quick usage guide
├── INSTALLATION.md             # Setup instructions
├── apa_formatter.py            # Python implementation
└── examples/
    ├── example_apa_paper.docx
    └── demo_technology_education.docx
```

## 🚀 Quick Installation

### For Claude Skills

1. **Extract the ZIP**
   ```bash
   unzip apa-skill.zip
   cd apa-skill
   ```

2. **Install the skill**
   ```bash
   mkdir -p /mnt/skills/user/apa
   cp SKILL.md /mnt/skills/user/apa/
   ```

3. **Install Python dependency**
   ```bash
   pip install python-docx --break-system-packages
   ```

4. **Use with Claude**
   Simply ask: *"Format this research as an APA paper..."*

### For Standalone Use

1. **Extract and install**
   ```bash
   unzip apa-skill.zip
   cd apa-skill
   pip install python-docx --break-system-packages
   ```

2. **Use the script**
   ```python
   from apa_formatter import create_apa_document
   
   # Your code here
   ```

## 📝 What This Skill Does

Automatically formats academic papers according to APA 7th Edition guidelines:

- ✅ Title pages with proper spacing
- ✅ Five heading levels (including run-in headings)
- ✅ Page numbers on all pages
- ✅ Proper margins, font, and spacing
- ✅ References with hanging indent
- ✅ Math formulas centered and italicized

## 🎯 Input Format

Use markdown-style formatting:

```markdown
# Introduction
Your text here...

## Literature Review
More content...

### Subsection
Detailed information...

The formula is: `E = mc^2`

---
References
Author, A. (2023). Title. Journal, 10(2), 123-145.
```

## 💡 Example Usage

**Ask Claude:**
```
"Create an APA formatted paper with this content:

Title: The Impact of AI
Author: Jane Smith
Institution: Tech University
Course: CS-500
Instructor: Dr. John Doe
Date: October 16, 2025

# Introduction
Artificial intelligence is revolutionizing...

---
References
Smith, J. (2023). AI advances. Tech Journal, 5(1), 10-25.
"
```

**Or use Python:**
```python
from apa_formatter import create_apa_document

title_data = {
    'title': 'The Impact of AI',
    'author': 'Jane Smith',
    'institution': 'Tech University',
    'course': 'CS-500',
    'instructor': 'Dr. John Doe',
    'date': 'October 16, 2025'
}

raw_text = """
# Introduction
Artificial intelligence is revolutionizing...

---
References
Smith, J. (2023). AI advances. Tech Journal, 5(1), 10-25.
"""

create_apa_document(title_data, raw_text, 'output.docx')
```

## 📚 Documentation

- **SKILL.md** - Technical reference for Claude
- **README.md** - Complete documentation
- **QUICK_START.md** - Beginner-friendly guide
- **INSTALLATION.md** - Detailed setup instructions

## 🔧 Requirements

- Python 3.7+
- python-docx library
- Optional: Claude integration for automatic formatting

## ❓ Troubleshooting

### Module not found
```bash
pip install python-docx --break-system-packages
```

### Skill not working
Ensure SKILL.md is in: `/mnt/skills/user/apa/SKILL.md`

### Permission errors
Use your home directory instead:
```bash
mkdir -p ~/claude-skills/apa
cp SKILL.md ~/claude-skills/apa/
```

## 🎓 Learn More

- View example documents in `examples/` folder
- Read QUICK_START.md for usage patterns
- Check INSTALLATION.md for detailed setup
- Consult README.md for complete documentation

## 📞 Support

- **APA Style**: Visit [Purdue OWL](https://owl.purdue.edu/owl/research_and_citation/apa_style/)
- **Python-docx**: Check [official docs](https://python-docx.readthedocs.io/)

---

**Ready to create APA documents?**

1. Install the skill (copy SKILL.md to skills directory)
2. Install python-docx
3. Ask Claude or run the script

That's it! 🎉
