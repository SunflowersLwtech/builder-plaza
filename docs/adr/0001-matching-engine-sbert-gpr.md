# Matching engine: SBERT embeddings + cosine retrieval + Gaussian-process scoring

The proposal promises "Gaussian-process regression over GitHub code vectors and business-need vectors" with an 80/20 exploit/explore split, but a production-grade GP recommender is infeasible in the three-week build window, and pure rule-based scoring was rejected as too simple. We decided on a pipeline that keeps the documented claim honest at demo scale: sentence-transformers (all-MiniLM-L6-v2) embeds each builder's skill text (GitHub repo topics, languages, README extracts) and each Project Card's needs text; cosine similarity over pgvector retrieves candidates; scikit-learn `GaussianProcessRegressor`, fitted on a small set of labelled example matches, scores them (GPs are well suited to tens of samples — exactly our seeded-demo scale); an epsilon-greedy layer returns 80% top-scored and 20% exploratory matches. Top overlapping skill terms are surfaced as the Match Reason.

## Considered Options

- **LightFM hybrid recommender** — citable and pip-installable, but interaction-driven; the platform launches with near-zero interaction data and LightFM is reported to overfit small datasets. Named in the report as the upgrade path once interactions accumulate.
- **Academic models (TIMA, CODER)** — cited as related work; research-grade code, not directly reusable in the window.
- **Pure rule-based scoring** — rejected by the team as too simple for the module's ambitions.
