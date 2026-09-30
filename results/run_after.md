# Run log — after

- Produced by: `store.py::search` and `gate.py::check`
- Corpus: `campus_life`
- Improvement: `config.py` changed top-k from 5 to 8
- Runs: 3 deterministic retrieval/gate passes
- Generation: not run; `.env` with `GEMINI_API_KEY` was absent

| Criterion | Run 1 | Run 2 | Run 3 |
|---|---|---|---|
| Retrieved chunks contain expected facts | 5/5 | 5/5 | 5/5 |
| Every answer names a source | N/A | N/A | N/A |
| Gate refuses out-of-scope questions | 5/5 | 5/5 | 5/5 |
| Sampled chunks are complete | 5/5 | 5/5 | 5/5 |
| Expected fact appears in answer | N/A | N/A | N/A |

The same five in-corpus questions passed and the same five out-of-scope
questions were refused. The top-k change could not be compared at generation
time because the API key was unavailable.
