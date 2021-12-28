---
weight: 7
title: "Discussion"
draft: false
---

## Discussion

### Main Results and Implications

Our project identified a unique metric to address both technical and social influences of packages in the npm system. By measuring Pearson ($R$) and Spearman ($\rho$) correlation coefficient between several metrics, we selected \textit{PageRank} and \textit{GitHub stars} to represent the technical and social impact of npm packages, and ranked 2,000 popular packages accordingly for vulnerability study.

Our research studied top 2,000 npm packages and their vulnerabilities in past releases, from the perspectives of vulnerability discovery time, vulnerability dependency type, vulnerable types and severity. RQ3.1 reveals that most vulnerable were made to public after the packages were released, and a majority (75.4\%) of vulnerable were introduced through \textbf{indirect dependency}. As the result for RQ3.2 highlighted, 5\% of the vulnerabilities are immediately vulnerable, as the vulnerabilities was published mostly \textbf{within 6 months} before the packages were released. Lastly, RQ3.3 pointed out that both past releases and the most recent release \textbf{share the same set of the most frequency vulnerability types}: "Regular Expression Denial of Service (ReDoS)", "Prototype Pollution", and "Arbitrary Code Injection", while the majority of vulnerabilities are in medium severity level.

Also looking at the packages from the perspective of the type of vulnerability (e.g. Cross-site Scripting (XSS), Arbitrary Code Injection, etc.), we found that vulnerability types of HTTP Header Injection takes the longest to fix (2767 days) followed by Arbitrary Code Injection (2554 days), Improper Access Control (2360 days), Remote Code Execution (RCE) (2165 days) and Cross-Site Request Forgery (CSRF) (2062 days). The five most common vulnerability types that have the most options of being fixed by upgrading the vulnerable packages include Regular Expression Denial of Service (ReDoS), Prototype Pollution, Use After Free (UAF), Arbitrary Code Injection and Denial of Service (DoS). For open source packages, we can see that certain packages with vulnerabilities are not prioritized and fixed as efficiently as others. For example, certain outliers aside, NULL Pointer Dereference, Arbitrary File Write via Archive Extraction (Zip Slip), Uncontrolled Recursion, Out-of-bounds Read and Improper Input Validation are the worst maintained packages from the perspective of vulnerabilities being detected and not fixed with 300, 153, 84, 72 and 31 vulnerabilities not fixed for every vulnerability detected and fixed, respectively.

The results of our vulnerability study have implications to both individual developers and npm community. For developers, it is important to notice that most vulnerabilities in project can be introduced through indirect dependency. Dependencies should be selected more carefully after a quick research of package vulnerability through public database, and immediate vulnerable packages should be avoided in the current project. However, security cannot be maintained through only individual effort, as projects cannot avoid dependencies and new vulnerabilities can be detected any time after the package was released. For packages that has vulnerabilities tracing through a long dependency chain, fixing such vulnerabilities can be difficult and costly for individuals. Therefore, we should raise the awareness of vulnerabilities introduced through indirect dependencies, and make collective efforts to address such issue in the npm community.

### Strength and Limitations

Our project presented unique metric to account for technical and social influences of npm packages, and conducted extensive study of the vulnerabilities among all $1.6M$ releases in the popular packages. We extended the prior research on vulnerability dependency by examining vulnerabilities not only due to self and direct dependencies, but also indirect dependency tracing back to root vulnerable packages. In addition to the dependency study, our results identified immediately vulnerable releases, and highlighted the most frequent vulnerability types in the release data, which provides good reference to researchers and developers who are interested in npm package development security and vulnerability trend study.

Despite the great implications from results, our study is subject to limitations. Since we only used npm package data in the process of determining the popularity metrics, our metrics only apply to npm, while effectiveness in the context of other ecosystem is not measured. Additionally, our vulnerability analysis was based on popular npm packages, and the results highlighted are restricted to this regime only. Thus, the trend of vulnerability dependency types and frequent vulnerabilities may not apply to all npm packages and other ecosystems.

### Future Work

#### Popularity metrics

As mentioned by Zerouali et al. \cite{zerouali2019diversity}, popularity of packages can be measured in many different ways and each metric represent different points of view for measuring popularity. In our study, we focused on the vulnerabilities in dependency chains and used PageRank of dependency graph and the number of GitHub stars as popularity metrics. In different contexts, however, other metrics can be used to identify popular packages.

#### Cross-source discrepancies

The full dependency data is from Libraries.io while the vulnerability data is from Snyk DB. The difference in data source resulted in a discrepancy between npm package name and version name. Furthermore, the project and dependency data from Libraries.io needs cleaning up for mismatching attributes. To fix this, we introduced a complex function to link the two dataset, but the effect is not good enough as only 47.8\% of dependencies between projects are matched. The next step is to either find a reliable source to download the full dependency data and vulnerability data from one dataset (e.g. npm APIs), or to polish the link function.

#### Package definitions and vulnerability classification

When accessing GitHub and GitHub-derived data from other sources such as libraries.io, Google BigQuery Open Datasets, etc. an interesting pattern emerges - not all packages are listed atomically in a GitHub repository. For example, while \verb|airbnb| javascript packages \verb|eslint-config-airbnb-base| and \verb|eslint-config-airbnb| are listed as two distinct packages in npm, on GitHub they are listed under the same repository (\verb|github.com/airbnb/javascript/tree/master/packages|) with the two packages under the same directory \verb|packages|. This is probably due to the fact that these two packages share common code-base. However, it becomes difficult to delineate the popularity, vulnerabilities, activities, etc. between them from GitHub alone, and would require more sophisticated data extraction methodologies combining data from multiple sources to get a more reliable and accurate dataset to work with. This is something that can be considered and incorporated in future iterations.

#### Dependency study

Our project only focused on vulnerabilities and their dependency relations back to the vulnerable root packages, while the impact of the vulnerabilities in popular packages to its dependents was not studied due to the cost of building such dependency graph. As mentioned by Zimmermann et al. \cite{zimmermann2019small}, the indirect dependency between projects increases super-linearly, which make the analysis through a complete dependency graph of npm packages hard to proceed. One idea to simplify this time consuming process is pruning. However, the top popular projects are transitively depended on by a fairly large part of all projects, thus the cost of building such big dependency graph is too high for the limited time. We plan to research more optimized graph search algorithms to speed up the graph analysis.

#### Extending study to other ecosystems

Our dataset was restricted to the packages within npm. In practice, the JavaScript ecosystem is much larger and there are many other kinds of programming languages in use having their own ecosystems. Reproducing our research study within different ecosystems will reveal vulnerability patterns for popular packages in individual ecosystems. Extending the study to other ecosystems also makes it possible to investigate vulnerabilities in dependencies to heterogeneous ecosystems, which was not covered in our study. Considering such a broad view of dependency chain will further highlight the importance of collective effort for security.
