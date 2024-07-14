---
title: "Polars vs Pandas: A Real-World Performance Comparison (Part 1/2)"
date: 2024-07-13
categories:
  - blog
  - data engineering
  - python
  - polars
  - pandas
  - performance
---

Recently, I decided to experiment with the Python library Polars at work. I've been hearing a lot about it, and I wanted to see how it compares to Pandas, which we currently use for our data preprocessing. Here's what I learned!

# Why Polars?

Polars has been trending in the data engineering world, and I was curious to see if it lived up to the hype. Plus, I'm a firm believer in learning by doing, so I thought this would be a great opportunity to get hands-on experience with a new tool.

It is a "blazingly fast" [1] DataFrame library written in Rust, which:
- Utilizes all available cores on your machine.
- Optimizes queries to reduce unneeded work/memory allocations.
- Handles datasets much larger than your available RAM.
- A consistent and predictable API.
- Adheres to a strict schema (data-types should be known before running the query).

What it promises seem really impressive, and is backed up by existing benchmarks like the most recent one from DuckDB. [2]

Finally, it is easy to migrate to Polars from Pandas, as it provides us with a user guide for people coming from Pandas. [3]

# The Experiment

To get a fair comparison, I took an existing project we have in production and replaced the preprocessing pipeline. This pipeline, originally built with Pandas, does the following:

1. Loads parquet data
2. Performs data type casting
3. Filters the data
4. Adds new columns/features
5. Saves the result back to parquet

I implemented the same pipeline using Polars and compared the performance.

For context, the dataset I used had:
- 2.4 million rows
- 121 columns

So it is somewhat a small dataset.

# The Results

I ran each version of the pipeline 10 times and took the average runtime. The results were impressive:

- Polars: 27.25 seconds
- Pandas: 123.7 seconds

That's a **78%** improvement in speed! 🚀

Those results are very encouraging, as the Polars implementation was done without much knowledge of the library, and I expect that you could squeeze out even better performances by writing better queries.

# Challenges

It wasn't all smooth sailing. Some functions I was used to in Pandas, like `index_of` on a List or Array, don't exist in Polars yet. However, the Polars Discord community was incredibly helpful and helped me find workarounds quickly.

# Feedback and Next Steps

When I presented these results to my colleagues, they were impressed by the numbers. However, since this particular pipeline doesn't run daily and isn't too time-consuming, they didn't see an immediate need to switch.

To build a stronger case, I'm planning two more experiments:

1. Test the improvements for our inference pipeline
2. Try Polars on a larger project with more than 10 million rows

Stay tuned for part 2 of this series, where I'll share the results of these additional tests!

Have you tried Polars? What has your experience been? Let me know!

# Sources

[1] [https://docs.pola.rs/#key-features](https://docs.pola.rs/#key-features)

[2] [https://duckdblabs.github.io/db-benchmark/](https://duckdblabs.github.io/db-benchmark/)

[3] [https://docs.pola.rs/user-guide/migration/pandas/](https://docs.pola.rs/user-guide/migration/pandas/)