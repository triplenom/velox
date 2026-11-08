# Local Library

Place shared documents and subfolders here. Velox searches UTF-8 `.txt` and `.md` directly. PDF, DOCX, PPTX, XLSX and XLS use the existing offline MarkItDown converter; install `markitdown[pdf,docx,pptx,xlsx,xls]==0.1.5` for these formats. Scanned/image-only PDFs need text supplied by the user: no OCR is performed.

Generated Markdown is cached in `.cache/`; sources are never modified. Symlinks, Windows junctions and `.cache/` cannot be read as source documents. This directory is shared across chats and is independent of the working/data directory. Do not put secrets here unless every enabled chat should be able to read them.
