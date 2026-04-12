---
title: "Engineering Essential: Automated Technical Documentation Workflow from JSON to PDF"
date: 2025-04-07T09:59:35Z
description: "In modern software development environments, managing and maintaining technical documentation is often viewed as a tedious task. As a…"
canonicalURL: "https://medium.com/@chrislee8613/engineering-essential-automated-technical-documentation-workflow-from-json-to-pdf-0c4c22f3fdd5"
cover:
  image: "https://cdn-images-1.medium.com/max/800/1*NOkUwKcevr-3zzBM-b6mnQ.png"
  relative: false
---
![](https://cdn-images-1.medium.com/max/800/1*NOkUwKcevr-3zzBM-b6mnQ.png)

In modern software development environments, managing and maintaining technical documentation is often viewed as a tedious task. As a developer, I’ve struggled with writing and updating various professional specification documents (such as Error Code Tables, SOPs, Protocol documents, etc.). These documents share common characteristics: high repetitiveness and frequent version changes.

In this article, I’ll share a complete document automation solution: using JSON as a data source, processing data with Python, generating beautifully formatted PDF documents using LaTeX, and implementing version control and automatic publishing through Git.

## Why Choose the JSON + LaTeX + Git Combination?

After exploring various technical documentation solutions, I found this combination offers the following advantages:

1. **JSON**: Structured data storage, easily readable and modifiable by programs
2. **LaTeX**: Professional typesetting system, producing high-quality PDF documents
3. **Git**: Comprehensive version control, supporting collaboration and publishing workflows

While HTML is also an option, PDF format is often preferred for formal documents due to its fixed layout and more professional appearance.

## Workflow Overview

The entire automation workflow can be summarized in the following steps:

1. Create JSON files as data sources
2. Process JSON data using Python
3. Convert processed data to LaTeX format
4. Compile LaTeX files into PDFs using MiKTeX
5. Utilize Git for version control and publishing

Let’s dive into the specific implementation of each step.

## Step 1: Creating JSON Files

First, we need to design a JSON structure that suits our documentation needs. Taking an error code table as an example:

```css
{
  "document_info": {
    "title": "System Error Code Table",
    "version": "1.2.0",
    "last_updated": "2025-04-07",
    "author": "Engineering Team"
  },
  "error_codes": [
    {
      "code": "E001",
      "severity": "ERROR",
      "message": "Database connection failed",
      "description": "Unable to establish connection with the database",
      "solution": "Check database connection string and network status"
    },
    {
      "code": "E002",
      "severity": "WARNING",
      "message": "Configuration file incomplete",
      "description": "System configuration file missing necessary parameters",
      "solution": "Refer to documentation to complete required parameters in the configuration file"
    }
    // More error codes...
  ]
}
```

This structured format makes updates and maintenance very simple. Need to add a new error code? Just add a new object to the array.

## Step 2: Processing JSON with Python

Next, we write Python scripts to process the JSON data. The purpose of this step is to:

1. Read the JSON file
2. Validate data integrity
3. Perform necessary data transformations
4. Prepare for LaTeX generation

Here’s an example Python code for processing JSON:

```cpp
import json
import datetime
import os
```

```
def process_error_code_table(json_file_path):
    # Read JSON file
    with open(json_file_path, 'r', encoding='utf-8') as f:
        data = json.load(f)
    
    # Validate data integrity
    required_fields = ['document_info', 'error_codes']
    for field in required_fields:
        if field not in data:
            raise ValueError(f"JSON missing required field: {field}")
    
    # Data processing and transformation
    # For example: sort by error code
    data['error_codes'] = sorted(data['error_codes'], key=lambda x: x['code'])
    
    # Update version information
    if 'auto_update_date' in data['document_info'] and data['document_info']['auto_update_date']:
        data['document_info']['last_updated'] = datetime.datetime.now().strftime('%Y-%m-%d')
    
    return data
```

```
# Use the function
error_data = process_error_code_table('error_codes.json')
```

## Step 3: Converting JSON to LaTeX

Now it’s time to convert the processed JSON data into LaTeX format. We can use Python string templates or specialized template engines to achieve this:

```ruby
def generate_latex(data):
    # LaTeX document header
    latex_content = r'''\documentclass{article}
\usepackage[utf8]{inputenc}
\usepackage{booktabs}
\usepackage{longtable}
\usepackage{xcolor}
\usepackage{hyperref}
\usepackage{geometry}
```

```
\geometry{a4paper, margin=1in}
```

```
\title{''' + data['document_info']['title'] + r'''}
\author{''' + data['document_info']['author'] + r'''}
\date{Version ''' + data['document_info']['version'] + r''' - Last Updated: ''' + data['document_info']['last_updated'] + r'''}
```

```
\begin{document}
```

```
\maketitle
\tableofcontents
\newpage
```

```
\section{Error Code Table}
```

```
\begin{longtable}{|p{0.08\textwidth}|p{0.10\textwidth}|p{0.22\textwidth}|p{0.3\textwidth}|p{0.3\textwidth}|}
\hline
\textbf{Code} & \textbf{Severity} & \textbf{Error Message} & \textbf{Description} & \textbf{Solution} \\
\hline
\endhead
'''
    
    # Add a row for each error code
    for error in data['error_codes']:
        # Set color based on severity
        severity_color = 'red' if error['severity'] == 'ERROR' else 'orange' if error['severity'] == 'WARNING' else 'black'
        
        latex_content += f"{error['code']} & "
        latex_content += f"\\textcolor{{{severity_color}}}{{{error['severity']}}} & "
        latex_content += f"{error['message']} & "
        latex_content += f"{error['description']} & "
        latex_content += f"{error['solution']} \\\\\n\\hline\n"
    
    # Document end
    latex_content += r'''\end{longtable}
```

```
\end{document}
'''
    
    return latex_content
```

```
# Generate LaTeX document
latex_content = generate_latex(error_data)
```

```
# Save LaTeX file
with open('error_codes.tex', 'w', encoding='utf-8') as f:
    f.write(latex_content)
```

## Step 4: Compiling LaTeX to PDF Using MiKTeX

Now that we have the LaTeX file, we’ll use MiKTeX (or another LaTeX distribution) to compile it into a PDF. We can do this by calling system commands from our Python script:

```cpp
import subprocess
```

```
def compile_latex_to_pdf(tex_file_path):
    try:
        # Call pdflatex twice to ensure table of contents and references are correctly generated
        subprocess.run(['pdflatex', tex_file_path], check=True)
        subprocess.run(['pdflatex', tex_file_path], check=True)
        
        # Get PDF filename (remove .tex extension)
        pdf_file = tex_file_path.replace('.tex', '.pdf')
        
        if os.path.exists(pdf_file):
            print(f"PDF generated successfully: {pdf_file}")
            return pdf_file
        else:
            print("PDF generation failed")
            return None
    except subprocess.CalledProcessError as e:
        print(f"LaTeX compilation error: {e}")
        return None
```

```
# Compile PDF
pdf_file = compile_latex_to_pdf('error_codes.tex')
```

## Step 5: Using Git for Version Control and Publishing

Finally, we integrate the entire process with Git to implement version control and automated publishing:

```python
def git_publish_document(pdf_file, version):
    try:
        # Add all files to Git
        subprocess.run(['git', 'add', '.'], check=True)
        
        # Commit changes
        commit_message = f"Update documentation to version {version}"
        subprocess.run(['git', 'commit', '-m', commit_message], check=True)
        
        # Add version tag
        tag_name = f"v{version}"
        subprocess.run(['git', 'tag', '-a', tag_name, '-m', f"Version {version}"], check=True)
        
        # Push to remote repository
        subprocess.run(['git', 'push', 'origin', 'master'], check=True)
        subprocess.run(['git', 'push', 'origin', tag_name], check=True)
        
        print(f"Document published, version {version}")
        return True
    except subprocess.CalledProcessError as e:
        print(f"Git operation error: {e}")
        return False
```

```
# Publish document
if pdf_file:
    git_publish_document(pdf_file, error_data['document_info']['version'])
```

## Integrating All Steps

Finally, we can integrate all the above steps into a complete Python script, enabling one-click document generation and publishing:

```cpp
import json
import datetime
import os
import subprocess
```

```
def main():
    # 1. Process JSON data
    error_data = process_error_code_table('error_codes.json')
    
    # 2. Generate LaTeX
    latex_content = generate_latex(error_data)
    with open('error_codes.tex', 'w', encoding='utf-8') as f:
        f.write(latex_content)
    
    # 3. Compile PDF
    pdf_file = compile_latex_to_pdf('error_codes.tex')
    
    # 4. Git publishing
    if pdf_file and input("Publish document? (y/n): ").lower() == 'y':
        git_publish_document(pdf_file, error_data['document_info']['version'])
```

```
if __name__ == "__main__":
    main()
```

## Advanced Automation Options

Beyond the basic document generation workflow, we can further optimize the level of automation:

1. **Using GitHub Actions or Jenkins**: Set up CI/CD pipelines to automatically trigger document generation and publishing when JSON changes
2. **Adding review process**: Incorporate a document review step before publishing
3. **Multi-language support**: Add multilingual data in JSON to generate document versions in different languages
4. **Document difference comparison**: Implement functionality to compare differences between old and new document versions

## Conclusion

Through this JSON → Python → LaTeX → PDF → Git automated workflow, we have successfully simplified technical documentation maintenance to just updating JSON data. This not only improves efficiency but also ensures document consistency and professionalism.

For those professional specification documents that require frequent updates, such as error code tables, SOPs, protocol documents, etc., this automation method is particularly effective. It allows teams to focus on the content itself rather than document formatting and publishing processes.

You can adjust this workflow according to your own needs or extend it to support more types of technical documentation. The important thing is to find a document automation strategy that works for your team and stick with it.
