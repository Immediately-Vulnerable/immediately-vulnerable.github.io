---
weight: 2
title: "Research Questions"
draft: false
---

## Research Questions

There are a handful of studies focusing on vulnerabilities in OSS projects through dependency analysis. The study from Decan et al. \cite{decan2018impact}, introduced in the previous section, inspired our project. We extended their vulnerable dependency study to include vulnerabilites due to three dependency types: vulnerability arising from the package itself (\textit{self}), vulnerability from one or more direct dependencies (\textit{direct}), and vulnerabilities from one or more indirect dependencies through a dependency chain (\textit{indirect}). The study from Pashchenko et al. \cite{pashchenko2018vulnerable} built up a framework to trace the vulnerabilities through direct and indirect dependency, which we adopted in our study as the ground work. Lastly, no prior study has focused on identifying the immediate vulnerable OSS packages, which upon the release time had known vulnerable dependencies, either direct or indirect. Therefore, in addition to categorizing the vulnerability dependency, we also diagnosed if the package has immediate vulnerabilities.

Our overall goal for this project was to perform empirical research and vulnerable dependency analysis of popular OSS projects and libraries within the npm ecosystem, with the following specific research questions:

1. Which aspects and metrics should be considered when determining the popularity of npm OSS packages?
1. How many vulnerable npm packages should we consider for our research study and why?
1. Analyzing vulnerabilities in popular vulnerable npm packages to find out:
    1. When are vulnerabilities discovered, and what are their distributions with respect to vulnerability dependency types and severity levels?
    1. Which packages are immediately vulnerable, and what are their distributions with respect to vulnerability dependency types and severity levels?
    1. What are the most frequent vulnerability types and severity levels in all package releases and most recent release?