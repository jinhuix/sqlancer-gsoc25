# GSoC 2025: Enhancing SQLancer

- Repository: [jinhuix/sqlancer-gsoc25](https://github.com/jinhuix/sqlancer-gsoc25)
- Project Page: [https://summerofcode.withgoogle.com/programs/2025/projects/LNNO9Bcf](https://summerofcode.withgoogle.com/programs/2025/projects/LNNO9Bcf)

## Project Overview

This repository documents the work I completed during **Google Summer of Code 2025**, under the project **SQLancer: Enhancing SQLancer with External Reducer Support and Hierarchical Delta Debugging**.

The main goals of this project were:

- Improve the **serialization and reproducibility** of SQLancer’s internal state (`StateToReproduce`).
- Implement a **standalone reducer** independent of SQLancer’s core, allowing flexible query reduction from serialized states.
- Clean up and refactor existing code contributions into maintainable commits.

For details, see the [official project description](https://summerofcode.withgoogle.com/programs/2025/projects/LNNO9Bcf).



## Main Deliverables

#### **1. Serialization Improvements**

- Implemented serialization of `StateToReproduce` into `.ser` files when a bug is detected, encapsulating all necessary information to reproduce the issue.
- Added unit tests for the serialization functionality and ensured all tests passed.

🔗 Related PRs:

- [Serialize StateToReproduce to JSON #1261](https://github.com/sqlancer/sqlancer/pull/1261)
- [Add StateToReproduce serialization tests #1268](https://github.com/sqlancer/sqlancer/pull/1268)

#### **2. Standalone Reducer**

- Implemented `StandaloneReducer`, an independent tool that:
  - Reads serialized `.ser` files containing `StateToReproduce`.
  - Applies partition-based delta debugging to iteratively minimize failing query sets.
  - Outputs the minimized queries into `.sql` files.

🔗 Related PRs:

- [Add a standalone reducer and serialization support #1252](https://github.com/sqlancer/sqlancer/pull/1252)
- [Add a standalone reducer #1270](https://github.com/sqlancer/sqlancer/pull/1270)

#### 3. **Additional Contributions**

Other merged PRs during GSoC 2025:

- [Resolves #1115 Add unit tests for schema classes #1178](https://github.com/sqlancer/sqlancer/pull/1178)
- [Bump SQLite from 3.47.2.0 to 3.49.1.0 #1245](https://github.com/sqlancer/sqlancer/pull/1245)
- [Bump DuckDB from 1.2.0 to 1.3.0.0 #1247](https://github.com/sqlancer/sqlancer/pull/1247)



## Demo

#### Usage

As an example, using SQLite 3.28.0. First, run SQLancer with the `--serialize-reproduce-state` option to serialize bug information into a `.ser` file:

```bash
mvn package -DskipTests
java -jar target/sqlancer-2.0.0.jar --random-seed 1697613568309 --serialize-reproduce-state sqlite3 --oracle NoREC
```

Then, run the `StandaloneReducer` to reduce the query set and write the minimized queries to a `.sql` file:

```bash
java -cp target/sqlancer-2.0.0.jar sqlancer.StandaloneReducer <path-to-ser-file>
```

#### Example Output

Console output:

```bash
Reduction completed successfully!
Final size: 1 statement (98.7% reduction)
Final queries written to: reduced.sql
```

Contents of `.sql`:

```sql
INSERT INTO vt0(vt0) VALUES('integrity-check');
```

More information can be found in the [demo](demo/) directory.



## References

- SQLancer: https://github.com/sqlancer/sqlancer
- C-Reduce: https://github.com/csmith-project/creduce
- DD: https://www.debuggingbook.org/html/DeltaDebugger.html



## Acknowledgements

I would like to thank my mentors [Manuel Rigger](https://github.com/mrigger) and [Kabilan Mahathevan](https://github.com/KabilanMA) for their invaluable guidance and feedback throughout this project. It was a great experience working with SQLancer community—I not only deepened my technical knowledge but also learned how to collaborate effectively in an open-source environment. I would also like to thank Google for providing this opportunity.
