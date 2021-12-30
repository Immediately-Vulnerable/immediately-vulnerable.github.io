---
weight: 6
title: "Results"
draft: false
---

## Results

### RQ1: Which aspects and metrics should be considered when determining the popularity of npm OSS packages?

Popularity can be defined and measured in various ways. Thus, each study needs to rely on study-specific popularity metrics considering the context of popularity. In our project, we investigated both *technical* and *social* aspects of popularity and selected one metric from each aspect. Finally, we derived the top $N$ popular packages by combining these two different metrics.

#### Technical aspect

The more dependents packages have, the greater effect their vulnerabilities. This is because a package's vulnerability can be disseminated throughout their dependents. In this context, the number of dependents is one of the important metrics in the technical aspect of popularity in our project. However, simply using this as a metric may fall into a pitfall since it does not take the chain of dependencies into account.

For example, let's consider a 8-node dependency graph. In the figure below, each node represents a package, and an edge from package *u* to package *v* represents that *u* is a dependent of *v*. The example graph shows that the number of dependents only captures the direct neighbors of each node, thus implicitly important packages may have low values. Although the in-degree, which is the number of dependents, of package *A* is only 1, it is clear that executing any of packages *B, D, E, F, G* and *H* results in executing package *A*.

<p align="center">
    <img src="/assets/img/simple_dep_graph.png" alt="tag" width="100%"/>
</p>

<p class="caption">
    <i>8-node dependency graph</i>
</p>

To take transitive dependency into account, we computed [PageRank](http://ilpubs.stanford.edu:8090/422/1/1999-66.pdf) of the dependency graph. The PageRank algorithm gives more weight on the edge of relatively important nodes. Thus, in the above figure, even if package *A* has only one dependent package *B*, package *A* has the highest PageRank since its dependent has many other dependents.

We measured Pearson (*R*) and Spearman (*&rho;*) correlation coefficient between *dependent projects count* and *PageRank* of packages. We observed strong non-linear correlations between them (*&rho; = 0.40, R = 0.87*) and decided it is high enough to select one metric on behalf of the other one. Thus, PageRank is finally selected as a single metric for the technical aspect of popularity metric.

#### Social aspect

[Al-Rubaye et al.](https://arxiv.org/pdf/2011.04865.pdf) surveyed several mechanisms for scoring popularity among GitHub repositories. They mention starring, forking and watching repositories as the most common mechanisms for *liking* content and providing social media style interactions on GitHub. Thus, in context of social aspects, we used these metrics to measure popularity.

We measured the Pearson (*&rho;*) and Spearman (*R*) correlation coefficients for the number of *watchers*, *forks* and *stars*. The table below shows the correlations for these GitHub popularity metrics.

| | #watchers | #forks | #stars |
|:-:|:-------:|:------:|:------:|
| <b>#watchers</b> | *&rho;* = 1.00<br/>*R = 1.00* | *&rho;* = 0.76<br/>*R = 0.92*<br/><b><i>strong</i></b> | *&rho;* = 0.91<br/>*R = 0.91*<br/><b><i>strong</i></b> |
| <b>#forks</b>    | | *&rho;* = 1.00<br/>*R = 1.00* | *&rho;* = 0.79<br/>*R = 0.96*<br/><b><i>strong</i></b> |
| <b>#stars</b>    | | | *&rho;* = 1.00<br/>*R = 1.00* |

<p class="caption">
    <i>Pearson and Spearman correlation coefficients for GitHub popularity metrics</i>
</p>

We observed strong linear correlations between the three popularity metrics. Therefore, we decided to only use the number of stars to determine the top-N popular packages.

### RQ2: How many vulnerable npm packages should we consider for our research study and why?

To determine the count of the top npm packages for our study, we closely examined the distribution of the two popularity metrics, PageRank and Github repository stars.

Starting with PageRank, the left graph in the figure below shows the calculated PageRank value with respect to the corresponding package rank. The PageRank value demonstrates a beyond exponential decrease as the package rank is increasing. Thus, selecting only top 5,000 shown by the vertical red line in the figure, PageRank packages is sufficient for covering the popular package from the technical aspect.

<p align="center">
    <img src="/assets/img/threshold_cutoff.png" alt="tag" width="100%"/>
</p>

<p class="caption">
    <i>PageRank (left) and GitHub stars (right) distribution with respect to Package's Popularity Rank</i>
</p>

We fetched GitHub repositories for the top 5,000 packages ranked by PageRank and sorted the package list again to account for the social influence of the package. Similar to the PageRank, the distribution of packages' GitHub star number in the right of the figure declined exponentially as the package order increases. As highlighted by the red vertical line in the same plot, choosing *N = 2,000* as a threshold can capture the popular npm packages from both technical and social perspective, while keeping the dataset in a reasonable size.

### RQ3: Analyzing vulnerabilities in popular vulnerable npm packages

The top 2,000 npm packages selected yields 231,488 releases in total, with the earliest dated back to 2010. Among these, 60.6% releases do not have known vulnerabilities, 38.8% have one or more vulnerabilities, and 0.695% releases do not have data from Snyk DB. By examining the data that does not have record at Snyk DB, we discovered that these are either old or preview releases and does not concern with our purpose of study, and thus the 0.695% , or 1609 releases, was removed from our data.

Between the packages with vulnerabilities, there are in total 864 packages and 89,658 releases, resulting in 1,654,568 vulnerabilities for further analysis. The sections 4.3.1 through 4.3.3 address the study of scanning vulnerability discovery time, zooming into immediate vulnerable, and ranking the top vulnerable types and severity levels.

#### When are vulnerabilities discovered, and what are their distributions with respect to vulnerability dependency types and severity levels?

We first began by surveying the vulnerability type of all 1,654,568 vulnerabilities to decide if the vulnerability was introduced within the package itself (self), from a directly dependent package (direct), or from depending on a vulnerable root package through a chain (indirect). The statistics revealed that **most vulnerabilities (75.4%) were introduced from indirect dependency**, with the rest 18.5% from direct dependency, and only a small portion (6.19%) was from package itself.

<p align="center">
    <img src="/assets/img/vulnDiscoveryTime_depType.png" alt="tag" width="100%"/>
</p>

<p class="caption">
    <i>Vulnerable discovery time based on dependency type</i>
</p>

To determine when the vulnerabilities were discovered, we calculated the time lapsed between the *release time* of a vulnerable package and the *publication time* of the corresponding vulnerability in days, and plotted it against the vulnerability dependency type of self, direct, or indirectly depending on a vulnerable root package in the figure above. Most vulnerabilities were discovered after the release of the package, as the time lapsed between package release and vulnerable publication is positive. As the figure uncovers, the discovery time of vulnerabilities shows different patterns based on dependency type: **for packages that have vulnerabilities within themselves, it took shorter time to discover those vulnerabilities**, compared to those which have vulnerabilities due direct or indirect dependencies. The time to catch vulnerabilities due to indirect dependencies can range from one to nearly 3,000 days, while the time to detect and publish a vulnerability from direct dependency peaked around 1,000 days. Only a small portion of vulnerabilities are immediately vulnerable with negative time lapsed, and will be discussed in detail in the next section.

<p align="center">
    <img src="/assets/img/vulnDiscoveryTime_severity.png" alt="tag" width="100%"/>
</p>

<p class="caption">
    <i>Vulnerable discovery time based on dependency type</i>
</p>

In addition to examining the vulnerability discovery time with respect to the dependency type, we further studied the discovery time from the perspective of vulnerability severity level, as shown in the above figure with a box plot. Note that there are no significant difference in vulnerability discovery time based on severity, with relatively shorter discovery time in severity vulnerabilities.

#### Which packages are immediately vulnerable, and what are their distributions with respect to vulnerability dependency types and severity levels?

Through this question, we aimed to understand how many packages are immediately vulnerable, i.e. a vulnerability had already been found and published in a direct or indirect dependency at the time of the release. We also analyzed how many of such immediately vulnerable packages have vulnerabilities due to direct and indirect dependencies.

Of all the vulnerabilities that we analyzed, we found that 82,777 (5% of the 1,654,568 vulnerabilities) can be considered immediate vulnerabilities. Of these immediate vulnerabilities, **a large majority (74,258 out of 82,777 or 89.71%) are caused by indirect dependencies** and only a small percentage (7,177 out of 82,777 or 9.32%) due to direct dependencies. The figure below shows the number of days before the release of a package when a vulnerability was published in its dependencies. We can see that most immediately vulnerable releases are using dependencies in which **vulnerabilities have been published within 6 months before the release**. Interestingly, there are some immediately vulnerable releases that have used dependencies where vulnerability was published ~6.5 years ago.

<p align="center">
    <img src="/assets/img/immediately_vuln.png" alt="tag" width="100%"/>
</p>

<p class="caption">
    <i>Immediately vulnerable releases based on dependency type</i>
</p>

The table below shows the severity levels of vulnerabilities found in all immediate vulnerabilities. We observed that majority of the vulnerabilities were of medium severity, around a quarter were of high severity and a very small fraction were of critical severity.

<div class="tableStyle" align="center">

| <b>Severity</b> | <b>Vulnerabilities</b> | <b>Percentage</b> |
|:---------------:|-----------------------:|------------------:|
| Low             | 13,826                 | 16.70%            |
| Medium          | 49,105                 | 59.32%            |
| High            | 19,123                 | 23.10%            |
| Critical        | 723                    | 0.87%             |
| <b>Total</b>    | <b>82,777</b>          | <b>100.00%</b>    |

</div>
<p class="caption">
    <i>Severity levels of vulnerabilities in immediately vulnerable releases</i>
</p>

#### What are the most frequent vulnerable types and severity levels, in all package releases and most recent release?

Based on the 1,654,568 vulnerabilities analyzed, for both all past releases and the most recent release, we summarized the most frequent types of vulnerabilities in the table below. Interestingly, both most recent releases and all past releases **share the same set of the most frequency vulnerability types**:

- Regular Expression Denial of Service (ReDoS)
- Prototype Pollution
- Arbitrary Code Injection

and the majority of vulnerabilities across both datasets are in medium severity level.

| **Most Frequent Vulnerability Types** | **All Releases** | **Most Recent release** |
|----------------------------------------------|---------------------------|-----------------------------------|
| Regular Expression Denial of Service (ReDoS) | 39.8%           | 19.3%                            |
| Prototype Pollution                          | 23.8%                    | 9.30%                            |
| Arbitrary Code Injection                     | 7.34%                    | 56.5%                   |

<p class="caption">
    <i>Most frequent vulnerable types in all releases and most recent release</i>
</p>

<div class="tableStyle" align="center">

| **Severity**                            | **All Releases**     | **Most Recent Release**     |
|-----------------------------------------|----------------------|------------------------------|
| Low                                          | 4.25%                    | 3.52%                            |
| Medium                                       | 50.6%           | 75.1%                   |
| High                                         | 43.0%                    | 20.2%                            |
| Critical                                     | 2.21%                    | 1.15%                            |

</div>

<p class="caption">
    <i>Most frequent vulnerability severity levels in all releases and most recent releases</i>
</p>

In addition to the most frequent types and severity level of the vulnerabilities, we noticed some very interesting trends and outliers worth highlighting. Across all of the severity levels, the percentage of vulnerabilities that can be fixed by upgrading the vulnerable packages is highest for critical vulnerabilities (79%) and low-severity vulnerabilities (77%) while medium and high severity packages can only be fixed by upgrading 61% and 67% of vulnerable packages, respectively. Also we found that for critical severity packages, for every 438 vulnerabilities fixed there is 1 vulnerability that is not fixed. For non-critical packages, that ratio drops to an average of 1 vulnerability not fixed for every 21 vulnerabilities fixed, which is a quantifiably big difference on the attitude and importance given to critically vulnerable packages in the open source community.
