# Too Good to be True

- **Category:** Database
- **Points:** 310

## Objective

Find a Veltrexa study participant whose primary efficacy result is more than three standard deviations above the population mean.

## Investigation

I selected `aurora_trials` and inspected the `results`, `subjects`, and `studies` tables. The relevant measure was `primary_efficacy_delta`, rather than `secondary_qol_score`.

```sql
USE aurora_trials;
SHOW TABLES;
DESCRIBE results;
DESCRIBE subjects;
```

I used `AVG(value)` and `STDDEV_POP(value)` to calculate the mean and population standard deviation, then compared each subject's value with `mean + 3 * standard_deviation`. Common table expressions keep the statistical calculation separate from the subject lookup:

```sql
WITH stats AS (
    SELECT AVG(value) AS mean_val, STDDEV_POP(value) AS std_val
    FROM results
    WHERE measure = 'primary_efficacy_delta'
),
subject_results AS (
    SELECT s.subject_code, r.value
    FROM results r
    JOIN subjects s ON s.subject_id = r.subject_id
    WHERE r.measure = 'primary_efficacy_delta'
)
SELECT
    sr.subject_code,
    sr.value,
    ROUND(st.mean_val, 2) AS mean_value,
    ROUND(st.std_val, 2) AS std_deviation,
    ROUND((sr.value - st.mean_val) / st.std_val, 2) AS z_score
FROM subject_results sr
CROSS JOIN stats st
WHERE sr.value > st.mean_val + 3 * st.std_val
ORDER BY z_score DESC;
```

The query identified the outlier, and I completed the challenge.

## Technique

This was a statistical outlier query: select the correct measure first, calculate the population statistics, join measurements to subject identifiers, and filter values above the three-standard-deviation threshold.
