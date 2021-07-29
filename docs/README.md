---
title:  'Convert OneNote to MarkDown'
author:
- Sjoerd de Valk, SPdeValk Consultancy
- modified by nixsee, a guy, @theohbrothers
date: 2020-07-15 13:00:00
keywords: [migration, tooling, onenote, markdown, powershell]
abstract: |
  This document is about converting your OneNote data to Markdown format.
permalink: /index.html
---

Credit for this script goes to the wizard @SjoerdV who created the original script [here](https://github.com/SjoerdV/ConvertOneNote2MarkDown). I've taken it and made a variety of modifications and improvements.

# Convert OneNote to MarkDown

[![github-actions](https://github.com/theohbrothers/ConvertOneNote2MarkDown/workflows/ci-master-pr/badge.svg)](https://github.com/theohbrothers/ConvertOneNote2MarkDown/actions)
[![github-release](https://img.shields.io/github/v/release/theohbrothers/ConvertOneNote2MarkDown?style=flat-square)](https://github.com/theohbrothers/ConvertOneNote2MarkDown/releases/)

## Summary

Ready to make the step to Markdown and saying farewell to your OneNote, EverNote or whatever proprietary note taking tool you are using? Nothing beats clear text, right? Read on!

The powershell script 'ConvertOneNote2MarkDown-v2.ps1' will utilize the OneNote Object Model on your workstation to convert all OneNote pages to Word documents and then utilizes PanDoc to convert the Word documents to Markdown (.md) format. It will also:

* Create a **folder structure** for your Notebooks and Sections
* Process pages that are in sections at the **Notebook, Section Group and up to 5 Nested Section Group levels**
* Allow you to choose between **converting a specific notebook or all notebooks**
* Allow you to **choose between creating subfolders for subpages** (e.g. Page\Subpage.md) or **appending prefixes** (e.g. Page_Subpage.md)
* Allow you you choose between putting all **Images** in a central '/media' folder for each notebook, or in a separate '/media' folder in each folder of the hierarchy
* Fix image references in the resulting .md files, generating *relative* references to the image files within the markdown document
* Extract all **File Objects** to the same folder as Images and fix references in the resulting .md files. Symbols in file names removed for link compatibility.
* Can choose between **discarding or keeping intermediate Word files**. Intermedia Word files are stored in a central notebook folder.
* Allow to choose between converting from existing docx (90% faster) and creating new ones - useful if just want to test differences in the various processing options without generating new docx each time
* Allow user can **select which markdown format will be used**, defaulting to Pandoc's standard format, which strips any HTML from tables along with other desirable (for me) formatting choices.
   * markdown (Pandoc’s Markdown)
   * commonmark (CommonMark Markdown)
   * gfm (GitHub-Flavored Markdown), or the deprecated and less accurate markdown_github; use markdown_github only if you need extensions not supported in gfm.
   * markdown_mmd (MultiMarkdown)
   * markdown_phpextra (PHP Markdown Extra)
   * markdown_strict (original unextended Markdown)
* See more details on these options here: https://pandoc.org/MANUAL.html#options
* Remove double spaces and "\" escape symbol that are created when converting with Pandoc
* Improved file headers, with title now as a # heading, standardized DateTime format, and horizontal line to separate from rest of document

## Known Issues

1. If there are any collapsed paragraphs in your pages, the collapsed/hidden paragraphs will not be exported
    * You can use the included Onetastic Macro script to automatically expand all paragraphs in each Notebook
    * [Download Onetastic here](https://getonetastic.com/download) and, once installed, use New Macro-> File-> Import to install the attached .xml macro file within Onetastic
1. Password protected sections should be unlocked before continuing, the Object Model does not have access to them if you don't
1. You should start by 'flattening' all pen/hand written elements in your onennote pages. Because OneNote does not have this function you will have to take screenshots of your pages with pen/hand written notes and paste the resulting image and then remove the scriblings. If you are a heavy 'pen' user this is a very cumbersome. **If you have an automated solution for this, please let me know**
1. Relative paths can not be used as input for the target folder. Always use an absolute path (ex. 'c:\temp\notes').
1. While running the conversion OneNote will be unusable and it is recommended to 'walk away' and have some coffee as the Object Model might be interrupted if you do anything else.
1. Linked file object in .md files are clickable in VSCode, but do not open in their associated program, you will have to open the files directly from the file system.
1. Anything I did not catch... please submit an issue.


## Requirements

* Windows >= 10

  * I have only tested this on Windows...

* Microsoft OneNote >= 2016 (To be clear, this is the Desktop version NOT the Windows Store version. Can be downloaded for FREE here - https://www.onenote.com/Download)

* Microsoft Word >= 2016

* PanDoc >= 2.7.2

  * TIP: Use [Chocolatey](https://chocolatey.org/docs/installation#install-with-powershellexe) to install Pandoc on Windows, this will also set the right path (environment) statements. (https://chocolatey.org/packages/pandoc)


## Installation

Clone this repository to acquire the powershell script.

## Usage

1. Start the OneNote application. All notebooks currently loaded in OneNote will be converted
2. . It is advised that you install Onetastic and the attached macro, which will automatically expand any collapsed paragraphs in the notebook. They won't be exported otherwise.
    * To install the macro, click the New Macro Button within the Onetastic Toolbar and then select File -> Import and select the .xml macro included in the release.
    * Run the macro for each Notebook that is open
3. It is highly recommended that you use VS Code, and its embedded Powershell terminal, as this allows you to edit and run the script, as well as check the results of the .md output all in one window.
4. Start by editing the configuration at the top of the script to your liking
5. Open a PowerShell terminal and navigate to the folder containing the script and run it: `.\ConvertOneNote2MarkDown-v2.ps1`

    * if you have trouble, try running both Onenote and Powershell as an administrator.
    * if you receive an error, try running this line to bypass security: `Set-ExecutionPolicy Bypass -Scope Process`

6. Sit back and wait until the process completes
7. To stop the process at any time, press Ctrl+C.
8. If you like, you can inspect some of the .md files prior to completion. If you're not happy with the results, stop the process, delete the .md and media folders and re-run with different parameters.

## Results

The script will log any errors encountered at the end of its run, so please review, fix and run again if needed.
If you are satisfied check the results with a markdown editor like VSCode. All images should popup just right in the Preview Pane for Markdown files.

## Recommendations

1. I'd like to strongly recommend the [VS Code Foam extension](https://github.com/foambubble/foam-template), which pulls together a selection of markdown-related extensions to become a comprehensive knowledge management tool.
1. I'd also like to recommend [Obsidian.md](http://obsidian.md), which is another fantastic markdown knowledge management tool.
1. Some other VSCode markdown extensions to check out are:

```powershell
    .\code `
    --install-extension davidanson.vscode-markdownlint `
    --install-extension ms-vscode.powershell-preview `
    --install-extension jebbs.markdown-extended `
    --install-extension telesoho.vscode-markdown-paste-image `
    --install-extension redhat.vscode-yaml `
    --install-extension vscode-icons-team.vscode-icons `
    --install-extension ms-vsts.team
```

> NOTE: The bottom three are not really markdown related but are quite obvious.

## Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

### [2.3] - 2020-07-16
#### Added
* Improved file header with title as # heading, standardized DateTime format and horizontal line to separate from document
* Option to re-use existing docx file - 90% faster - for when testing different processing options

#### Changed
* Tweaks to filenames and other formatting

#### Removed
* Nothing

### [2.2] - 2020-07-16
#### Added
* Symbols removed from attached file names
* Files stored in same folder as images
* Double spaces and "\" escape symbols removed after conversion with Pandoc
* Tables use piped formatting for compatibility with Obsidian

#### Changed
* User prompt layouts

#### Removed
* Nothing

### [2.1] - 2020-07-15
#### Added
* Prompt for keep or discard .docx files
* Prompt to have images in central folder or separate ones for each folder in hierarchy

#### Changed
* User prompt layouts

#### Removed
* Nothing


### [2.0] - 2020-07-14
#### Added
* Consolidated prior scripts into one
* Prompt for markdown format selection
* Prompt to choose between prefix and subfolders for subpages

#### Changed
* Now produces relative references to images (e.g ../../media
* Each notebook has a centralized images/media folder

#### Removed
* Extraneous code

### [1.1] - 2020-07-11
#### Added
 * two new scripts to allow for Section Groups, as well as Section Groups + Subfolders for Subpages

#### Changed
* Pandoc instead of gfm set as default format

#### Removed
* Nothing

### [1.0.0] - 2019-05-19

#### Added

* Initial Release

#### Changed

* Nothing

#### Removed

* Nothing


## Credits
* Avi Aryan for the awesome [VSCodeNotebook](https://github.com/aviaryan/VSCodeNotebook) port
* @SjoerdV for the original script
