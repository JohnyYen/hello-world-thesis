# AGENTS.md - LaTeX Thesis Development Guidelines

## Overview

This document provides comprehensive guidelines for agentic coding assistants working on this LaTeX-based academic thesis project. The thesis focuses on "Arquitectura de software adaptativa para la personalización del aprendizaje mediante videojuegos educativos" (Adaptive Software Architecture for Learning Personalization Through Educational Games).

## Build Commands

### Primary Build Pipeline
```bash
# Full compilation with bibliography (3-pass process)
pdflatex main.tex && biber main && pdflatex main.tex && pdflatex main.tex

# Continuous compilation with auto-rebuild on changes
latexmk -pdf -pvc main.tex

# Clean all auxiliary files
latexmk -c
```

### Single File/Chapter Compilation
```bash
# Compile individual chapter for testing
pdflatex chapters/chapter_one.tex

# Check bibliography compilation separately
biber main
```

### Validation & Error Checking
```bash
# Halt on first error for debugging
pdflatex -halt-on-error main.tex

# Validate bibliography datamodel
biber --validate-datamodel main

# Check for undefined references
pdflatex -interaction=nonstopmode main.tex | grep -i "undefined\|missing\|error"
```

### Development Environment Commands
```bash
# Install missing LaTeX packages (in container)
tlmgr install <package-name>

# Update bibliography after changes
biber main && pdflatex main.tex
```

## LaTeX Code Style Guidelines

### File Structure & Organization

**Document Hierarchy:**
- `main.tex`: Entry point, includes all components
- `package.tex`: Package imports and global configurations
- `variables.tex`: Document variables and metadata
- `chapters/`: Individual chapter files
- `src/`: Images and resources directory

**Inclusion Pattern:**
```latex
% Use \input{} for configuration files
\input{./package.tex}
\input{./variables.tex}

% Use \include{} for chapters
\include{./chapters/chapter_one.tex}
```

### Formatting Conventions

**Document Setup:**
```latex
\documentclass[12pt, a4paper]{report}
\usepackage[spanish]{babel}  % Spanish language support
\usepackage[utf8]{inputenc}  % UTF-8 encoding
\onehalfspacing            % Academic standard spacing
```

**Page Layout:**
```latex
\geometry{a4paper, margin=1in, headsep=1cm}
\pagestyle{fancy}
\fancyhead[L]{\leftmark}  % Chapter name in header
\fancyhead[R]{\thepage}   % Page number
```

**Font Configuration:**
```latex
\usepackage{helvet}       % Helvetica font family
\renewcommand{\rmdefault}{phv}
\renewcommand{\sfdefault}{phv}
```

### Bibliography Management

**BibLaTeX Configuration:**
```latex
\usepackage[
    backend=biber,
    style=ieee,
    sorting=none,
    natbib=true,
    url=false,
    doi=false,
    isbn=false
]{biblatex}

\addbibresource{bibliography.bib}
```

**Citation Styles:**
```latex
% In-text citations
\cite{author_year}        % IEEE numeric style
\citep{author_year}       % Parenthetical
\citeauthor{author_year}  % Author only
\citeyear{author_year}    % Year only

% Bibliography generation
\printbibliography[title={Bibliografía}]
```

**Bibliography Entry Standards:**
- Use UTF-8 encoding for Spanish characters
- Include DOI when available (even if not displayed)
- Consistent field formatting across entries
- Validate entries with `biber --validate-datamodel`

### Code Style Standards

**Indentation & Spacing:**
```latex
% Consistent indentation for nested environments
\begin{itemize}
    \item First level item
    \begin{itemize}
        \item Nested item with proper indentation
    \end{itemize}
\end{itemize}

% Space after commands and before content
\section{Section Title}  % Space after \section

\textbf{text}           % No space before content
```

**Command Usage:**
```latex
% Use custom commands for reusable content
\newcommand{\getTitle}{Thesis Title}
\newcommand{\getAuthor}{Author Name}

% Consistent naming: camelCase for custom commands
\newcommand{\chapterOne}{Chapter One Title}
```

**Environment Standards:**
```latex
% Figure environment
\begin{figure}[H]
    \centering
    \includegraphics[width=0.8\textwidth]{image.png}
    \caption{Caption text with proper punctuation.}
    \label{fig:image_label}
\end{figure}

% Table environment
\begin{table}[H]
    \centering
    \begin{tabular}{|c|c|}
        \hline
        Column 1 & Column 2 \\
        \hline
        Data & Data \\
        \hline
    \end{tabular}
    \caption{Table caption.}
    \label{tab:table_label}
\end{table}
```

### Naming Conventions

**Files:**
- Lowercase with underscores: `chapter_one.tex`
- Descriptive names: `introduction.tex`, `conclusion.tex`
- Consistent numbering: `first_chapter.tex`, `second_chapter.tex`

**Labels & References:**
```latex
% Figures: fig:prefix
\label{fig:uml_diagram}
\label{fig:system_architecture}

% Tables: tab:prefix
\label{tab:results_comparison}
\label{tab:performance_metrics}

% Sections: sec:prefix
\label{sec:methodology}
\label{sec:results}

% Equations: eq:prefix
\label{eq:performance_formula}
```

**Custom Commands:**
- camelCase: `\getTitle`, `\getAuthor`, `\chapterOne`
- Descriptive: `\university`, `\faculty`, `\supervisorName`

### Error Handling & Validation

**Compilation Errors:**
- Always run 3-pass compilation: LaTeX → Biber → LaTeX → LaTeX
- Check for undefined citations after bibliography changes
- Validate cross-references before final compilation
- Use `\graphicspath{{./src/}}` for consistent image paths

**Bibliography Validation:**
```bash
# Check for missing fields
biber --validate-datamodel main

# Verify citation consistency
pdflatex -interaction=nonstopmode main.tex | grep "Citation"
```

**File Organization Checks:**
- Ensure all `\input{}` and `\include{}` paths are correct
- Verify `\graphicspath` matches actual directory structure
- Check for duplicate labels across chapters

### Domain-Specific Guidelines (Educational Games/Software Architecture)

**Diagram Creation:**
```latex
% Use tikz for UML diagrams
\usepackage{tikz}
\usepackage{tikz-uml}

\begin{tikzpicture}
    \umlclass{ClassName}{
        +attribute : Type
        +method() : ReturnType
    }
\end{tikzpicture}
```

**Code Listings:**
```latex
\usepackage{listings}
\usepackage{xcolor}

\lstset{
    language=Java,
    basicstyle=\ttfamily\footnotesize,
    keywordstyle=\color{blue},
    commentstyle=\color{green},
    numbers=left,
    numberstyle=\tiny,
    frame=single,
    breaklines=true
}

\begin{lstlisting}[caption=Code example]
public class Example {
    // Code implementation
}
\end{lstlisting}
```

**Architecture Diagrams:**
- Use `\usepackage{graphicx}` for imported diagrams
- Consistent sizing: `width=0.8\textwidth`
- PNG/PDF formats preferred over EPS
- Descriptive captions following academic standards

### Development Environment

**Devcontainer Configuration:**
- Uses `danteev/texlive` base image
- Pre-installed packages: arxiv-latex-cleaner, pdflinkchecker-cli
- Node.js available for additional tooling

**VS Code Extensions (Required):**
- LaTeX Workshop (james-yu.latex-workshop)
- LaTeX Formatter (nickfode.latex-formatter)
- LaTeX Utilities (mathematic.vscode-latex)
- LTeX (valentjn.vscode-ltex) - Spanish spell checking

**Auto-Compilation Settings:**
```json
{
    "latex-workshop.latex.autoBuild.onSave.enabled": true,
    "latex-workshop.view.pdf.viewer": "tab",
    "ltex.language": "es-ES"
}
```

### Testing & Validation Procedures

**Pre-Commit Checks:**
1. Full compilation without errors
2. Bibliography validation
3. Cross-reference verification
4. Spell check in Spanish
5. PDF generation and visual inspection

**Automated Validation:**
```bash
# Comprehensive build test
#!/bin/bash
pdflatex -halt-on-error main.tex
if [ $? -ne 0 ]; then
    echo "LaTeX compilation failed"
    exit 1
fi

biber main
pdflatex -halt-on-error main.tex
pdflatex -halt-on-error main.tex

# Check for warnings
grep -i "warning\|undefined\|missing" main.log || echo "No warnings found"
```

**Manual Validation Steps:**
- Review PDF for formatting consistency
- Verify all figures/tables have captions and labels
- Check bibliography formatting and completeness
- Validate page numbering and headers
- Confirm Spanish language formatting

### Best Practices

**Version Control:**
- Commit all `.tex`, `.bib`, and image files
- Use meaningful commit messages
- Track build artifacts in `.gitignore`
- Maintain separate branches for major revisions

**Collaboration:**
- Use relative paths for all includes/graphics
- Document custom commands in comments
- Maintain consistent coding style across chapters
- Regular compilation checks before pushing changes

**Performance Optimization:**
- Use `\includeonly{}` during development for faster compilation
- Separate large figures to avoid memory issues
- Optimize image sizes before inclusion

### No Cursor/Copilot Rules Found

This project does not contain Cursor rules (`.cursorrules` or `.cursor/rules/`) or Copilot instructions (`.github/copilot-instructions.md`). All coding guidelines are documented in this AGENTS.md file.

### Troubleshooting Common Issues

**Bibliography Problems:**
```bash
# Clear bibliography cache
rm main.bbl main.bcf
biber main --cache  # Rebuild cache
```

**Missing Packages:**
```bash
# Install missing LaTeX packages
tlmgr install tikz-uml listings xcolor
```

**Encoding Issues:**
- Ensure all files are saved as UTF-8
- Use `\usepackage[utf8]{inputenc}` consistently
- Verify Spanish characters display correctly

This document serves as the comprehensive guide for maintaining code quality and development workflows in this LaTeX thesis project. Follow these guidelines to ensure consistency, maintainability, and successful compilation.</content>
<parameter name="filePath">AGENTS.md