---
weight: 3
title: "Contributions"
draft: false
---

## Contributions

Our project adopts an empirical approach in identifying and analyzing the vulnerabilities occurring in JavaScript npm packages through both direct and indirect dependencies, with the following contributions:

- We identified unique metrics to account for both technical and social influence in determining the popularity of npm packages.
- We determined the top 2,000 packages to represent the most-influential npm packages since vulnerabilities found in these packages will have a widespread impact.
- We highlighted the vulnerabilities among top 2,000 npm packages and their past releases based on
    - Vulnerability discovery time: Whether the vulnerability in a dependency of the package was published before or after the release of that package. If the vulnerability in the dependency was published before the release of the package, then the package is said to be \textit{immediately vulnerable}.
    - Vulnerability dependency: if the vulnerability is within the package itself (\textit{self}), directly depending on a vulnerable package (\textit{direct}), or depending on a vulnerable package through a dependency chain (\textit{indirect}).
    - Vulnerability types and severity levels (\textit{critical}, \textit{high}, \textit{medium}, or \textit{low}).
