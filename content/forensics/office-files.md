---
title: "Office Files"
description: "Analyze Microsoft Office documents for embedded macros, metadata, and hidden objects using oletools, olevba, and related utilities."
date: 2025-10-05
draft: false
weight: 25
categories: ["Forensics"]
tags: ["office files", "oletools", "vba", "macros", "docx", "xls", "ppt"]
authors: ["CTF.Support Team"]
toc: true
summary: "Learn to analyze Office documents for macros, metadata, and hidden objects using olevba and other oletools utilities in CTF forensics challenges."
aliases: ["/forensics/office-files/"]
slug: "office-files"
---

## Introduction

Office documents (like `.doc`, `.docx`, `.xls`, `.ppt`, etc.) often contain embedded macros, metadata, or hidden objects that may store flags or malicious code.  
In CTF challenges, analyzing these files can reveal hidden strings, encoded data, or VBA macros.

## Quick Reference

- Extract macros: `olevba sample.doc`
- Extract metadata: `olemeta sample.doc`
- List OLE streams: `oledir sample.doc`

## Tools

| Tool                                              | Purpose                                        |
|---------------------------------------------------|------------------------------------------------|
| [oletools](https://github.com/decalage2/oletools) | Analyze OLE and OOXML files                    |
| `olevba` / `olevba3`                              | Extract VBA macros from Office documents       |
| `oledir`                                          | List all streams and storages inside OLE files |
| `olemeta`                                         | Extract metadata                               |

## Extracting Macros (VBA)

To extract VBA macros from Office documents:

```bash
olevba sample.doc
```

Look for:

- Encoded strings or obfuscated code
- Auto-executing macros (e.g., `AutoOpen`, `Document_Open`)
- Suspicious commands like Shell, CreateObject, or Base64Decode

Example output:

```text
$ olevba suspicious.doc
+-----------+--------------------+-----------------------------------------+
| Type      | Keyword            | Description                             |
+-----------+--------------------+-----------------------------------------+
| AutoExec  | AutoOpen           | Runs when the document is opened        |
| Suspicious| CreateObject       | May create external COM objects         |
| Suspicious| Shell              | Executes command-line instructions      |
+-----------+--------------------+-----------------------------------------+
```

## OOXML Files (DOCX, XLSX, PPTX)

Modern Office files (e.g., `.docx`, `.xlsx`) are ZIP archives containing XML data.

Inspect their contents using standard tools:

```bash
unzip sample.docx -d output/
```

Look inside the extracted directories for:

- `word/document.xml` — contains the main text
- `word/vbaProject.bin` — stores VBA macros
- `docProps/core.xml` — contains metadata (author, timestamps)

You can analyze `vbaProject.bin` separately with `olevba`:

```bash
olevba output/word/vbaProject.bin
```
