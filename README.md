# APA 7th Edition Formatting Skill
## Claude Skill Package

This skill adds APA 7th edition formatting capabilities to Claude, allowing you to transform raw research text into professionally formatted Word documents.

**Learn more about Claude Skills:**
- 📰 [Anthropic Skills Announcement](https://www.anthropic.com/news/skills)
- 📖 [Skills Documentation](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview)

## 📦 Package Contents

```
apa-skill-package/
├── SKILL.md                    # Main skill file (Claude reads this)
├── README.md                   # This file
├── DOCUMENTATION.md            # Full technical documentation
├── QUICK_START.md              # Quick usage guide
├── INSTALLATION.md             # Detailed setup instructions
├── apa_formatter.py            # Python implementation
└── examples/
    ├── example_apa_paper.docx
    └── demo_technology_education.docx
```

## 🚀 Installation

### Method 1: Via Claude.ai (Recommended - Coming Soon)

Skills will be available through Claude.ai's interface:

1. Go to **claude.ai**
2. Navigate to **Skills** in the sidebar
3. Search for "APA Formatting"
4. Click **Install**

*Note: Public skill library coming soon. For now, use Method 2.*

### Method 2: Upload via Claude.ai (Current)

1. **Download and extract this ZIP file**
   - Save `apa-skill.zip` to your computer
   - Extract the ZIP file
   - Locate the `SKILL.md` file inside

2. **Upload the skill to Claude**
   - Go to [claude.ai](https://claude.ai)
   - Click your profile → **Settings**
   - Navigate to **Capabilities** → **Skills**
   - Click **Upload skill** button
   - Select the `SKILL.md` file from the extracted folder
   - Toggle the skill **ON** (blue)

3. **Install Python dependency** (required for document generation)
```bash
   pip install python-docx --break-system-packages
```

4. **Start using the skill**
   
   Open any chat with Claude and say:
   
   *"Format this research as an APA paper..."*
   
   Claude will automatically use the APA skill when you mention formatting or academic papers.

**Note:** You can enable/disable the skill anytime in Settings > Capabilities > Skills.

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

## 🧠 How Claude Skills Work

**Skills extend Claude's capabilities** by providing specialized instructions and tools:

1. **You install a skill** by placing `SKILL.md` in `/mnt/skills/user/{skill-name}/`
2. **Claude automatically detects** when a skill is relevant to your request
3. **Claude reads the skill file** to learn how to help you
4. **Claude follows the instructions** to complete your task

With this APA skill installed, Claude will:
- Recognize when you need APA formatting
- Parse your markdown-formatted text
- Apply all APA 7th edition rules automatically
- Generate a properly formatted Word document

**You just chat naturally** - no special commands needed!

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

## 💡 Using the Skill

### With Claude (Recommended)

Once installed, **just chat naturally with Claude**. The skill activates automatically when you mention APA formatting or academic papers.

**Example conversation:**

**You:**
```
Format this as an APA paper:

Title: The Impact of AI in Education
Author: Jane Smith
Institution: Tech University
Course: EDU-500
Instructor: Dr. John Doe
Date: October 16, 2025

# Introduction
Artificial intelligence is transforming educational practices through
personalized learning and automated assessment tools.

## Literature Review
Previous research has demonstrated mixed results...

---
References
Smith, J. (2023). AI in modern classrooms. Ed Tech Journal, 5(1), 10-25.
```

**Claude:**
*Claude will read the APA skill, parse your text, apply all formatting rules, generate a Word document, and provide a download link.*

---

**More natural examples:**

```
"Create an APA formatted paper from my research notes"
"I need this essay in APA 7th edition format"
"Convert this to an academic paper with proper APA formatting"
```

Claude understands what you need and uses the skill automatically!

### Standalone Python Script (Without Claude)

**Or use Python:**
You can also use the included script without Claude:

```python
from apa_formatter import create_apa_document

title_data = {
    'title': 'The Impact of AI in Education',
    'author': 'Jane Smith',
    'institution': 'Tech University',
    'course': 'EDU-500',
    'instructor': 'Dr. John Doe',
    'date': 'October 16, 2025'
}

raw_text = """
# Introduction
Artificial intelligence is transforming education...

## Literature Review
Previous research has shown...

---
References
Smith, J. (2023). AI in classrooms. Ed Tech Journal, 5(1), 10-25.
"""

create_apa_document(title_data, raw_text, 'my_paper.docx')
print("APA document created!")
```

This generates the same properly formatted Word document.

## 📚 What Gets Formatted Automatically

When you use this skill, Claude automatically applies:

| Element | APA Format |
|---------|-----------|
| **Title Page** | 3 blank lines, centered bold title, author info |
| **Page Numbers** | Top right corner, all pages including title |
| **Margins** | 1 inch on all sides |
| **Font** | Times New Roman, 12pt throughout |
| **Line Spacing** | Double spacing (2.0) everywhere |
| **Heading 1** | Centered, Bold, Title Case |
| **Heading 2** | Left-aligned, Bold, Title Case |
| **Heading 3** | Left-aligned, Bold Italic, Title Case |
| **Heading 4** | Indented, Bold, run-in (text on same line) |
| **Heading 5** | Indented, Bold Italic, run-in |
| **Paragraphs** | 0.5 inch first-line indent |
| **References** | New page, hanging indent (0.5 inch) |
| **Math Formulas** | Centered, italicized |

## 📚 Additional Documentation

- **SKILL.md** - Technical reference for Claude (the main skill file)
- **DOCUMENTATION.md** - Complete technical documentation
- **QUICK_START.md** - Beginner-friendly guide with examples
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

## 🎓 Learn More About Claude Skills

- 📰 [Skills Announcement](https://www.anthropic.com/news/skills) - Official blog post from Anthropic
- 📖 [Skills Documentation](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview) - Complete guide to using skills
- 💡 [Example Documents](./examples/) - See what this skill produces

---

## ✨ Ready to Start?

**Quick Start (3 steps):**

1. **Install the skill:**
   ```bash
   mkdir -p /mnt/skills/user/apa
   cp SKILL.md /mnt/skills/user/apa/
   ```

2. **Install dependency:**
   ```bash
   pip install python-docx --break-system-packages
   ```

3. **Chat with Claude:**
   ```
   "Format this as an APA paper: [paste your text]"
   ```

**That's it!** Claude will handle all the formatting automatically. 🎉

---

**Questions?** Check out:
- `QUICK_START.md` for beginner tutorials
- `INSTALLATION.md` for troubleshooting
- `DOCUMENTATION.md` for advanced features
