---
weight: 1
title: "Introduction"
draft: false
---

## Introduction

One of the major security concerns with modern-day software applications is their strong dependency on Open Source Software (OSS) libraries. The vulnerabilities in the OSS libraries can be exploited by malicious cyber attacks, and threaten the security of dependent software. With strong dependency on OSS libraries, software applications continue to face malicious attacks due to the vulnerabilities discovered in OSS libraries.

According to [Synopsys Cybersecurity Research Center's (CyRC)](https://www.synopsys.com/) [Open Source Security and Risk Analysis (OSSRA) report in 2021](https://www.synopsys.com/software-integrity/resources/analyst-reports/open-source-security-risk-analysis.html), 98% of 1546 industry codebases audited rely on OSS libraries and 84% of them are subject to at least one public open source vulnerability, which is a 9% increase compared to 2020. Furthermore, according to Snyk's [The State of Open Source Security 2020](https://snyk.io/open-source-security/) report, vulnerabilities are found in both direct and indirect dependencies, where the majority of vulnerabilities found in Node.js, Java, and Ruby projects are introduced by indirect dependencies. Therefore, **detecting vulnerabilities in OSS libraries through dependency analysis is crucial in preventing security attacks**.

{{< notification type="info" title="">}}
<b>98%</b> of industry codebases audited rely on open-source libraries and <b>84%</b> of them are subject to at least one public open source vulnerability.
{{< /notification >}}

Previous research has focused on studying different scopes to identify vulnerabilities introduced by OSS library dependency. For example, Decan et al. in their 2018 paper ["On the impact of security vulnerabilities in the npm package dependency network"](https://dl.acm.org/doi/pdf/10.1145/3196398.3196401) used the metadata from 610K JavaScript packages to analyze the time and duration of vulnerabilities in those packages introduced by direct dependencies.

In parallel to Decan's work, Pashchenko et al. in their paper ["Vulnerable Open Source Dependencies: Counting Those That Matter"](https://www.researchgate.net/profile/Antonino-Sabetta/publication/328089558_Vulnerable_open_source_dependencies_counting_those_that_matter/links/5bd88e2da6fdcc3a8db16906/Vulnerable-open-source-dependencies-counting-those-that-matter.pdf) also used metadata to identify  vulnerabilities due to dependencies. They published a framework on how to report vulnerable dependencies in OSS with a focus on the 200 most popular Java libraries.

Taking a different approach than identifying vulnerabilities from metadata, in 2020, [Ponta et al.](https://link.springer.com/content/pdf/10.1007/s10664-020-09830-x.pdf) developed a code-centric, usage-based vulnerable detection tool, [Eclipse Steady](https://projects.eclipse.org/proposals/eclipse-steady), to overcome the inaccurate release versions and unavailable vulnerability information from metadata.

However, no studies have been done to count for vulnerabilities induced by both direct and indirect dependencies in JavaScript npm packages, especially as 86% of the npm projects are reported to be affected by vulnerabilities from indirect dependencies according to Snyk's [The State of Open Source Security 2020](https://snyk.io/open-source-security/) report.

{{< notification type="info" title="">}}
Our project is the first of its kind to analyze vulnerabilities caused by both direct and indirect dependencies in npm packages.
{{< /notification >}}
