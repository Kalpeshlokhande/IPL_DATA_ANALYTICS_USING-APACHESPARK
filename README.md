# IPL Data Analytics with Apache Spark (PySpark)

An end-to-end data analysis of **Indian Premier League (IPL) cricket data up to 2017** using **PySpark and Spark SQL**. Raw CSV files stored in **Amazon S3** are loaded with explicit schemas, cleaned and transformed with the DataFrame API, analysed with Spark SQL joins and aggregations, and visualised with Matplotlib and Seaborn.

## Dataset

Five related CSV files (ball-by-ball, match, player, player-match and team data), read from an S3 bucket:

| File | Contents |
|---|---|
| `Ball_By_Ball.csv` | Every delivery: runs, extras, wickets, dismissal type, batter, bowler |
| `Match.csv` | Teams, venue, toss winner and decision, match winner, win margin, season |
| `Player.csv` | Player name, batting hand, bowling skill, country |
| `Player_match.csv` | Player's team, role and age for each match |
| `Team.csv` | Team names and IDs |

A copy is in [`IPL_DATASETS_TILL_2017/`](IPL_DATA_ANALYTICS_USING_SPARK/IPL_DATASETS_TILL_2017).

## Tech stack

PySpark · Spark SQL · Amazon S3 · pandas · Matplotlib · Seaborn · Jupyter Notebook

## Pipeline

1. **Ingest** – create a `SparkSession` and read each CSV from S3 with an explicit `StructType` schema (no schema inference, so types are correct and loading is faster).
2. **Clean and transform**
   - Keep only valid deliveries (exclude wides and no-balls) for batting analysis.
   - **Window function:** running total of runs per match and innings (`Window.partitionBy("match_id","innings_no").orderBy("over_id")`).
   - Derived columns with `when()`: high-impact balls, win-margin category (High / Medium / Low), whether the toss winner also won the match, batting style, veteran status.
   - Date features: year, month and day from the match date.
   - Normalise player names with `regexp_replace` and fill missing values with `na.fill`.
3. **Analyse with Spark SQL** – register temp views and run multi-table joins and aggregations.
4. **Visualise** – convert results to pandas and plot with Matplotlib and Seaborn.

## Analyses

- Total and average runs per match and innings
- Top-scoring batsmen per season (4-table join)
- Most economical bowlers in powerplay overs (overs 1–6)
- Impact of winning the toss on the match result
- Average runs scored by batsmen in winning matches
- Average and highest scores by venue
- Most frequent dismissal types
- Team performance after winning the toss

## Visualisations

The notebook contains 6 charts, including:
- Top 10 most economical powerplay bowlers
- Match outcome after winning the toss
- Top 10 batsmen by average runs in wins
- Scores by venue
- Most frequent dismissal types
- Team wins after winning the toss

## How to run

**On Databricks or AWS EMR (as built):** upload the CSV files to an S3 bucket, update the `s3://...` paths in the notebook, attach it to a Spark cluster and run all cells.

**Locally:**

```bash
pip install pyspark pandas matplotlib seaborn jupyter
jupyter notebook IPL_DATA_ANALYTICS_USING_SPARK/IPL_DATA_ANALYSIS_SPARK.ipynb
```

Then replace the S3 paths with local paths, for example:

```python
spark.read.schema(ball_by_ball_schema).option("header", "true").csv("IPL_DATASETS_TILL_2017/Ball_By_Ball.csv")
```

## Skills demonstrated

Spark DataFrame API · explicit schemas · window functions · conditional transformations · Spark SQL joins and aggregations · reading data from S3 · data cleaning · data visualisation

## Author

**Kalpesh Lokhande** – [LinkedIn](https://www.linkedin.com/in/kalpesh-lokhande-493517283/) · [GitHub](https://github.com/Kalpeshlokhande)
