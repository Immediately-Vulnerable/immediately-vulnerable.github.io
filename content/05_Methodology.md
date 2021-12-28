---
weight: 5
title: "Methodology"
draft: false
---

## Methodology

### Study Design

We designed the experiment by first finding the top $N$ most important packages within the npm ecosystem, and then investigating the vulnerabilities in the selected packages.

#### Identifying important packages

There are various popularity metrics for packages, making it challenging to decide what metrics should be used to select popular packages. Although those metrics show moderate correlation ($p = 0.57$) \cite{zerouali2019diversity}, each can be interpreted from a different point of view, and can also be measured in a different way. Therefore, we combined both \textit{social} aspect (e.g. the number of stars, forks and watchers on GitHub) and \textit{technical} aspect of popularity (e.g. download count and number of depending packages) to avoid any possible bias.

#### Identifying Vulnerabilities and Dependency Analysis

Given a list of important packages, we searched for vulnerabilities by the name of each package on Snyk Advisor \cite{snykadvisor}. For npm, Snyk reports vulnerabilities not only in that very package and direct dependencies, but also transitive dependencies over all the versions published within npm \cite{Snyk20}. Vulnerabilities found are then categorized by the vulnerability types and severity levels for further analysis.

### Data Collection

For this project, we explored various sources of data, including \textit{Libraries.io}, \textit{npm}, \textit{GitHub} and \textit{Synk Vulnerability Database}, to investigate packages dependencies and vulnerabilities in the published packages within npm.

#### Libraries.io and npm

Open Source Repository and Dependency Metadata dataset, or \textit{Libraries.io} \cite{jeremy_katz_2020_3626071}, includes metadata of packages (package name, version, list of dependencies, etc.) from 34 different package managers. We filtered data for packages published in npm to extract the required dependencies for them. This allowed us to investigate the relationships among packages without manually extracting information from \textit{.json} manifest file that stores the metadata of each npm package. In addition to \textit{Libraries.io}, we utilized npm as the source by querying all release metadata, including release name and timestamp, for each package of interest.

#### GitHub

Based on the data filtered for npm packages, we extracted GitHub stars, forks and watchers using GitHub APIs \cite{githubapi}. We combined these metrics to account for the \textit{social} influences in selecting popular npm packages for vulnerability analysis.

#### Snyk Vulnerability Database

We explored Snyk Vulnerability Database (Snyk DB) for identifying vulnerabilities in npm packages \cite{snykvdb}. The database allows users to use package name and optionally with package release name to search for known vulnerabilities in that package. To pipeline the querying process from Snyk, we developed an extractor in Python script to search a given npm package's vulnerability with package name, version, and GitHub URL through Snyk Command Line Interface (CLI) tool.

### Variables Considered

The two main tasks of the project, i.e. identifying popular npm packages and investigating their vulnerabilities, are concerned with different sets of variables and attributes from the data extracted. In order to extract popular npm packages from the \textit{Libraries.io} dataset, we considered their direct dependent package list, to examine the technical influence of each package. We also developed a tool to extract each package's  stars, forks and watchers from their GitHub repositories to examine their social influence.

To analyze the vulnerability of each popular package, specifically the vulnerability dependency types and discovery time, we divided the vulnerability information extraction into two phases. First, for all popular packages and their past releases, we scanned their basic vulnerable data by querying from Snyk whether a package release was vulnerable, their number of vulnerabilities, and the count of critical, high, medium, low severity vulnerabilities, if present. Then, for the releases that has vulnerabilities recorded, we further extracted detailed vulnerable attributes, including the disclosure and publication time of the vulnerabilities, the dependency path tracing back to the problematic source package, and the release name when the vulnerability was fixed.

### Analysis Methods

To compare the different metrics in searching for the ones that can best represent the social and technical aspect of the npm packages, we adopted the methods in the research of Zerouali et al. which computed Pearson ($R$) and Spearman ($\rho$) correlation factors across different popularity metrics \cite{zerouali2019diversity}. Using the threshold cutoff relation of $0 < \textit{very weak} \leq 0.2 < \textit{weak} \leq 0.4 < \textit{moderate} \leq 0.6 < \textit{moderately strong} \leq 0.8 < \textit{strong} \leq 1$, we compared the similarities between the candidate metrics to decide if the correlation between any metric is strong enough to demonstrate the influence of the packages.

Our analysis of vulnerabilities in a package can be divided into several steps: if the package has any vulnerabilities, then finding the discovery time of such vulnerabilities, the type of vulnerable dependency, and the severity level. To check if the package and its release have any vulnerabilities, we first queried the vulnerability in a form of json file from Snyk using the package name and its release we obtained from npm, and extracted the node \verb|ok| in the json to determine if any vulnerability was recorded.

For every vulnerability found, the discovery time of each can be associated by either \verb|disclosureTime|, the time the vulnerable was first disclosed, or \verb |publicationTime|, the time the vulnerable was publicly announced. In our research, we adopted the \verb|publicationTime| instead of \verb|disclosureTime| since the former is a more accurate measure of when a release is publicly know to be vulnerable than the latter. The vulnerability dependency type can be analyzed by the node \verb|from| which listed the source of vulnerability through the dependency. If \verb|from| has only one package, then the vulnerable dependency comes from the package itself. Similarly, if \verb|from| has two packages, then vulnerability arises from direct dependencies too, while more than two packages indicates the vulnerability in a release arises from indirect dependencies as well. Lastly, the severity of the vulnerability is highlighted by the node \verb|severity| in Snyk DB, ranging from critical, high, medium, to low. Once all these attributes are tabulated for each vulnerability in all packages, we aggregated the statistics using histograms and box plots to answer RQ3.
