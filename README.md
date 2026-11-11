# Maktabah Syamilah — Arabic Book Search Engine

A semantic search engine for classical Arabic books (*kitab*). Type an Arabic word and it finds the most relevant pages across 41 books, even when the page doesn't contain that exact word, by expanding the query with words that have similar meanings before ranking the pages.

The same search engine ships in three forms: a **desktop app** (Tkinter, packaged as a standalone executable), a **Flask** web app, and a **Django** web app.

<!-- TODO: add a screenshot of the search results, e.g. docs/screenshot.png -->
<!-- ![Search results](docs/screenshot.png) -->

## How it works

```
query ──► Arabic normalization ──► FastText query expansion ──► TF-IDF + cosine similarity ──► ranked pages
```

1. **Normalization**: unifies Arabic letter variants (alef forms إ أ ٱ آ → ا, ى → ي, hamza carriers → ء) and strips diacritics (*tashkeel*) and punctuation, so spelling variations still match.
2. **Query expansion**: a FastText word-embedding model trained on the corpus finds the words closest in meaning to the query, and the top three are used as search terms.
3. **Ranking**: every page is vectorized with TF-IDF, and the pages with the highest cosine similarity to each expanded term are returned with the book title, page number, and page text.

## Corpus

| Category | Topic | Books |
|---|---|---|
| أصول الفقه والقواعد الفقهية | Principles and legal maxims of Islamic jurisprudence | 21 |
| الأنساب | Arab genealogy | 20 |

## Tech stack

| Area | Tools |
|---|---|
| NLP / IR | Gensim FastText, scikit-learn (TF-IDF, cosine similarity), NLTK, PyArabic |
| Data | NumPy, pandas |
| Interfaces | Tkinter + ttkthemes, Flask, Django |
| Packaging | PyInstaller |

## Project structure

```
desktop/        Tkinter desktop app, packaged with PyInstaller
web/flask/      Flask web app (search, results, and page detail views)
web/django/     Django web app (search and results views)
```

## Getting started

The dataset and trained model are **not** included in this repository. Each app expects a `data/` folder:

```
data/
├── dataset/   # kitabsave.npy, sentence_clearsave.npy, kategori.npy, namakitab.npy
└── model/     # modelFT.model (FastText)
```

The dependencies are pinned to 2020 versions (numpy 1.14, scikit-learn 0.19), so use **Python 3.6 or 3.7** in a virtual environment.

**Flask**

```bash
cd web/flask
pip install -r requirements.txt
# put data/ next to app.py
python app.py              # http://localhost:5000
```

**Django**

```bash
cd web/django
pip install -r requirements.txt
# put data/ inside main/
python manage.py runserver # http://localhost:8000
```

**Desktop**

```bash
cd desktop
pip install -r requirements.txt   # Python must include Tkinter
# put data/ next to app.py
python app.py

# build a standalone executable into dist/
pyinstaller --onefile app.spec
```

## Background

Built in 2020. <!-- TODO: add context, e.g. which course or research project this was for and what your role was -->
