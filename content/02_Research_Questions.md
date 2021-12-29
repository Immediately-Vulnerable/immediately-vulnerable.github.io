---
weight: 2
title: "Research Questions"
draft: false
---

## Research Questions

There are a handful of studies focusing on vulnerabilities in OSS projects through dependency analysis. The study from [Decan et al.](https://dl.acm.org/doi/pdf/10.1145/3196398.3196401), introduced in the previous section, inspired our project. We extended their vulnerable dependency study to include vulnerabilites due to three dependency types: vulnerability arising from the package itself (*self*), vulnerability from one or more direct dependencies (*direct*), and vulnerabilities from one or more indirect dependencies through a dependency chain (*indirect*). The study from [Pashchenko et al.](https://www.researchgate.net/profile/Antonino-Sabetta/publication/328089558_Vulnerable_open_source_dependencies_counting_those_that_matter/links/5bd88e2da6fdcc3a8db16906/Vulnerable-open-source-dependencies-counting-those-that-matter.pdf) built up a framework to trace the vulnerabilities through direct and indirect dependency, which we adopted in our study as the ground work.

{{< notification type="info" title="">}}
No prior study has focused on identifying the immediate vulnerable OSS packages, which upon the release time had known vulnerable dependencies, either direct or indirect. Therefore, in addition to categorizing the vulnerability dependency, we also diagnosed if the package was immediate vulnerable upon release.
{{< /notification >}}

Our overall goal for this project was to perform empirical research and vulnerable dependency analysis of popular OSS projects and libraries within the npm ecosystem, with the following specific research questions:

1. Which aspects and metrics should be considered when determining the popularity of npm OSS packages?
1. How many vulnerable npm packages should we consider for our research study and why?
1. Analyzing vulnerabilities in popular vulnerable npm packages to find out:
    1. When are vulnerabilities discovered, and what are their distributions with respect to vulnerability dependency types and severity levels?
    1. Which packages are immediately vulnerable, and what are their distributions with respect to vulnerability dependency types and severity levels?
    1. What are the most frequent vulnerability types and severity levels in all package releases and most recent release?
