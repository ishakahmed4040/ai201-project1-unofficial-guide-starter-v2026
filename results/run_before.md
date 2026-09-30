# Run log — before

- Produced by: `store.py::search` and `gate.py::check`
- Corpus: `campus_life`
- top-k: 5
- Runs: 3 deterministic retrieval/gate passes
- Generation: not run; `.env` with `GEMINI_API_KEY` was absent

| Criterion | Run 1 | Run 2 | Run 3 |
|---|---|---|---|
| Retrieved chunks contain expected facts | 5/5 | 5/5 | 5/5 |
| Every answer names a source | N/A | N/A | N/A |
| Gate refuses out-of-scope questions | 5/5 | 5/5 | 5/5 |
| Sampled chunks are complete | 5/5 | 5/5 | 5/5 |
| Expected fact appears in answer | N/A | N/A | N/A |

| Question | Best distance | Gate | Supporting source |
|---|---:|---|---|
| Housing lottery random for juniors and seniors? | 0.135 | passed | `admin_housing_lottery.txt` |
| Kestrel Commons lunch wait times? | 0.167 | passed | `dining_kestrel_commons.txt` |
| Weekday shuttle frequency? | 0.425 | passed | `transit_shuttle.txt` |
| CS 340 exams open-book and curved? | 0.427 | passed | `course_cs_340_exams.txt` |
| Health-centre walk-in hours? | 0.216 | passed | `health_center.txt` |

Out-of-scope best distances were 0.825, 0.934, 0.886, 0.844, and 0.896;
the gate refused all five.
