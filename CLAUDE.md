# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A face recognition system built around the **AT&T face database** — 40 subjects × 10 grayscale PGM images each (92×112 px, 8-bit). The technical specification lives in `docs/Face Recognition.pdf`. This is a Python project (evidenced by `.gitignore`) in early development; source code has not yet been added.

## Dataset

`dataset/` contains the AT&T face database organized as:

```
dataset/sX/Y.pgm   # X = subject 1–40, Y = image 1–10
```

Images vary in lighting, facial expression (open/closed eyes, smiling), and facial details (glasses). The directory is gitignored — use `dataset.zip` to restore it.

## Development Setup

No `requirements.txt` or `pyproject.toml` exists yet. When adding dependencies, common libraries for this domain include:

- `numpy`, `opencv-python` or `Pillow` for image loading/processing
- `scikit-learn` for classical ML approaches (PCA/Eigenfaces, LDA, SVM)
- `torch` / `tensorflow` for deep learning approaches
- `pytest` for tests

## Commands

> To be filled in once source code and tooling are added (e.g., `python main.py`, `pytest`, linting commands).

## Architecture Notes

- The project is on branch `devel`; `main` is the integration branch.
- `docs/Face Recognition.pdf` contains the full project specification — read it before implementing.
- The dataset is gitignored; `dataset.zip` (tracked) is the canonical source.
