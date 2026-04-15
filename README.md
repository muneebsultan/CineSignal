# CineSignal

**Content-based movie recommendations with review sentiment analysis.**

CineSignal suggests films similar to one you already like and summarizes audience tone by analyzing IMDb user reviews. Movie metadata (title, genres, runtime, ratings, posters, and more) comes from [The Movie Database (TMDB)](https://www.themoviedb.org/documentation/api). Review text is collected from IMDb with [Beautiful Soup](https://www.crummy.com/software/BeautifulSoup/) and scored with NLP-based sentiment analysis.

![Python](https://img.shields.io/badge/Python-3.8+-blueviolet)
![Framework](https://img.shields.io/badge/Framework-Flask-red)
![Frontend](https://img.shields.io/badge/Frontend-HTML%20%7C%20CSS%20%7C%20JS-green)
![API](https://img.shields.io/badge/Data-TMDB-fcba03)

---

## Features

- **Similar titles** — Recommendations from a content-based model using text features and **cosine similarity** between movies.
- **Rich details** — Posters, cast, and metadata via the TMDB API.
- **Review signal** — Scraped IMDb reviews with sentiment analysis so users see more than a single score.
- **Search UX** — Autocomplete and AJAX-driven flows in the browser; if a title is not suggested, type the name and press **Enter** (minor typos are often fine).

---

## Tech stack

| Layer | Technologies |
|--------|----------------|
| Backend | [Flask](https://flask.palletsprojects.com/), [Gunicorn](https://gunicorn.org/) (deployment) |
| ML / NLP | [scikit-learn](https://scikit-learn.org/), [NLTK](https://www.nltk.org/), [NumPy](https://numpy.org/), [SciPy](https://scipy.org/), [pandas](https://pandas.pydata.org/) |
| Data | [TMDB API](https://www.themoviedb.org/documentation/api), [tmdbv3api](https://github.com/Api-Wrappers/tmdbv3api), [requests](https://requests.readthedocs.io/), Beautiful Soup, [lxml](https://lxml.de/) |
| Frontend | HTML, CSS, JavaScript (AJAX) |

Full dependency pins are in [`requirements.txt`](requirements.txt).

---

## Quick start

### Prerequisites

- **Python 3.8+** (3.8 matches the original project; newer 3.x versions usually work—verify in your environment).
- A **TMDB API key** ([how to obtain one](#tmdb-api-key)).

### 1. Clone and install

```bash
git clone <your-repo-url>
cd movie_recommendation
python -m venv .venv
```

Activate the virtual environment (Windows PowerShell):

```powershell
.\.venv\Scripts\Activate.ps1
```

Then:

```bash
pip install -r requirements.txt
```

### 2. Configure the TMDB key

Do **not** commit real keys to git. Replace the placeholder in **`static/recommend.js`** wherever `my_api_key` is set to `'YOUR_API_KEY'` (currently at **lines 15 and 29**).

### 3. Run locally

```bash
python main.py
```

Open [http://127.0.0.1:5000/](http://127.0.0.1:5000/) in your browser.

---

## TMDB API key

1. Create an account at [themoviedb.org](https://www.themoviedb.org/).
2. In account settings, open the **API** section from the sidebar.
3. Complete the application form for an API key. If a website URL is required and you do not have one, you can use a placeholder such as `NA` unless TMDB’s current policy says otherwise.
4. After approval, copy the key into `static/recommend.js` as described above.

---

## How it works

### Similarity and recommendations

The engine compares movies using **similarity scores** between text-derived features (for example, tags and descriptions used in the model). Each score is a value typically interpreted between **0** and **1**: higher means more alike. In this project, **cosine similarity** measures alignment between vectors even when documents differ in length—similar texts can sit far apart in raw Euclidean distance but still point in nearly the same direction in embedding space.

Further reading: [Understanding the math behind cosine similarity](https://www.machinelearningplus.com/nlp/cosine-similarity/).

### Architecture (high level)

![Architecture diagram](https://user-images.githubusercontent.com/36665975/110212434-597bb700-7ec1-11eb-9ffa-7ac319e33123.jpg)

### Cosine similarity (concept)

![Cosine similarity illustration](https://user-images.githubusercontent.com/36665975/70401457-a7530680-1a55-11ea-9158-97d4e8515ca4.png)

---

## Project layout (main paths)

| Path | Role |
|------|------|
| `main.py` | Flask application entrypoint |
| `templates/` | `home.html`, `recommend.html` |
| `static/` | `recommend.js`, `autocomplete.js`, `style.css` |
| `*.csv`, `reviews.txt` | Processed data and review inputs used by the pipeline |
| `*.ipynb` | Notebooks for preprocessing and experiments |
| `Procfile` | Process type for Heroku-style hosts |

---

## Demos and media (upstream)

These links come from the original project; availability depends on third-party hosting.

- Demo: [mrswsa.herokuapp.com](https://mrswsa.herokuapp.com/) — alternate: [the-movie-buff.herokuapp.com](https://the-movie-buff.herokuapp.com/)
- Video walkthrough: [YouTube](https://www.youtube.com/watch?v=dhVePtyECFw)
- Featured session: [Krish Naik — YouTube](https://www.youtube.com/watch?v=A_78fGgQMjM)  
  [![Krish Naik session](https://github.com/kishan0725/AJAX-Movie-Recommendation-System-with-Sentiment-Analysis/blob/master/static/krish-naik.PNG)](https://www.youtube.com/watch?v=A_78fGgQMjM)

---

## Related project: The Movie Cinema

The original author also maintains **[The Movie Cinema](https://github.com/kishan0725/The-Movie-Cinema)** ([live app](https://the-movie-cinema.herokuapp.com/)), which supports more languages and uses **TMDB’s own recommendation engine** instead of this repo’s custom content-based path. **CineSignal** builds its own similarity matrix; scaling that to **700k+** TMDB titles was not practical on small hosts (for example, high RAM use for a full **CountVectorizer** matrix on Heroku). For multi-language discovery at scale, compare that project’s approach.

---

## Datasets and sources

1. [IMDb 5000 Movie Dataset (Kaggle)](https://www.kaggle.com/carolzhangdc/imdb-5000-movie-dataset)
2. [The Movies Dataset (Kaggle)](https://www.kaggle.com/rounakbanik/the-movies-dataset)
3. [List of American films of 2018 (Wikipedia)](https://en.wikipedia.org/wiki/List_of_American_films_of_2018)
4. [List of American films of 2019 (Wikipedia)](https://en.wikipedia.org/wiki/List_of_American_films_of_2019)
5. [List of American films of 2020 (Wikipedia)](https://en.wikipedia.org/wiki/List_of_American_films_of_2020)

---

## Credits

This repository extends ideas and materials from the **AJAX Movie Recommendation System with Sentiment Analysis** lineage by [kishan0725](https://github.com/kishan0725). Upstream reference: [Movie-Recommendation-System-with-Sentiment-Analysis](https://github.com/kishan0725/Movie-Recommendation-System-with-Sentiment-Analysis). **CineSignal** is the product-style name used here for clarity and branding; replace it in this file if you ship under another trademark.
