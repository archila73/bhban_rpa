# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the companion code repository for the Korean textbook **"6개월 치 업무를 하루 만에 끝내는 업무 자동화"** (2020, 생능출판사) by Byunghyun Ban. It contains standalone Python automation example scripts organized by textbook chapter. There is no build system, test suite, or package — each lesson is an independent script run directly.

## Running Scripts

Each lesson directory contains a standalone `main.py` as its entry point. Run any script with:

```bash
python main.py
# or with arguments (some scripts require them):
python main.py <username> <password>
python main.py <keyword> <num_images> <output_dir> <resolution>
```

Some Part 2 lessons use differently named entry points (e.g., `merge.py`, `noise.py`, `resize.py`). Check the lesson directory for the appropriate file.

## Installing Dependencies

There is no central `requirements.txt`. Install per-domain:

```bash
# Excel/CSV automation (Part 2 Chapter 4)
pip install pyexcel pyexcel-xlsx

# Image processing (Part 2 Chapter 5, Part X)
pip install numpy pillow

# Web/headless automation (Parts 4–5)
pip install selenium

# PDF image extraction (Part X)
pip install pymupdf

# Macro automation (Parts 3–4, Windows-only)
# See: https://github.com/needleworm/pymacro
```

Parts 3 and 4 also depend on `pywinmacro.py`, which is copied directly into each lesson directory — it is **Windows-only** (uses `win32api`, `win32con`). Part 5 replaces this with Selenium, which is cross-platform.

Selenium scripts require a matching **ChromeDriver** binary on PATH. ChromeDriver version must match the installed Chrome version.

## Repository Architecture

```
[Part 2] 컴퓨터 자동화 기초/          ← File, Excel, and image batch automation
    [Chapter 3]/                        ← Text/CSV file merging and conversion
    [Chapter 4]/                        ← CSV/XLSX bulk operations
    [Chapter 5]/                        ← Image batch processing (PIL)

[Part 3] 매크로를 활용한 자동화/        ← Windows macro automation (pywinmacro)
    [Chapter 7]/

[Part 4] 인터넷 활용 자동화/            ← Internet automation via macros (Windows)
    [Chapter 9]/

[Part 5] 매크로는 잊어라! 헤드리스.../  ← Selenium/headless automation (cross-platform)
    [Chapter 10]/                       ← Selenium basics: Twitter/Instagram bots
    [Chapter 11]/                       ← Advanced: multi-account bots, image crawlers

[Part X]/                               ← Bonus: PDF extraction, royalty-free image crawling
```

## Code Patterns

**Part 2 (Basic automation):** Procedural scripts. Each file performs one batch task directly — no classes. Common pattern: loop over files → transform → write output.

**Parts 3–4 (Macro-based):** `pywinmacro.py` is copied into each lesson and provides cursor control, screen capture, and clipboard access via Windows APIs. Business logic lives in a separate module (e.g., `login_macro.py`, `twitter_bot_tweet.py`); `main.py` just calls into it.

**Parts 4–5 (Web automation):** OOP design. Bot logic is encapsulated in classes (`TwitterBot`, `LikeBot`, `CaptureBot`, `ReplyBot`, etc.) in a dedicated module file. `main.py` is kept minimal — it instantiates the bot and calls its methods. This is the intended architectural pattern the book teaches.

**Part 5 (Headless/Selenium):** Same OOP pattern as Part 4 but without `pywinmacro`. Selenium `webdriver.Chrome()` with `Options` replaces macro-based browser control. ChromeDriver is managed via `webdriver.Chrome(executable_path=...)` or PATH lookup.

## File Encoding

Some files (particularly Part 2 Chapter 3) use **EUC-KR** encoding due to Korean sample data files. When reading or writing such files, specify `encoding='euc-kr'`. Most source `.py` files are UTF-8.

## Lesson Numbering

Directory names follow the pattern `<part>_<chapter>_<lesson>_<Korean description>`, e.g., `2_3_1_회원 개인정보 파일 1천 개, 1초만에 만들기`. The numeric prefix is the canonical lesson ID; the Korean suffix is the lesson title.
