[MetaCheck](http://www.scienceverse.org/metacheck) version 0.0.1.9001  
Report Created: 2026-06-10  
DOI:
[10.32614/10.5281/zenodo.2669586](https://doi.org/10.32614/10.5281/zenodo.2669586)

[Metacheck](https://www.scienceverse.org/metacheck/) is a tool that
screens scientific manuscripts and aims to identify potential issues for
improvement. Our goal is to guide researchers towards best practices,
especially with respect to practices that researchers easily forget, or
might not have learned about yet. Metacheck is developed to help
researchers correctly and completely report statistical results, will
point to possibly relevant information about citations, and provides
feedback about data and code sharing.

Metacheck combines existing and new checks in a module-based tool. It is
open source, and anyone can contribute modules. It mainly relies on text
search or retrieving information from external sources through API’s or
web-scraping, but the power modules can optionally use large language
models. The use of LLM’s is always optional and opt-in. The development
of Metacheck is guided by our [values
statement](https://www.scienceverse.org/metacheck/#our-values).

In light with our values, our modules are validated on sets of
open-access papers. In each validated module, there will be a sentence
explaining the prevalence of false positives (incorrect
detection/classification) and false negatives (incorrect omission). For
example, if a hypothetical module detects the inappropriate practice of
“woozling”, the sentence might read:

> In a sample of 250 papers from the *Journal of X*, there were 350
> instances of woozling. This module correctly detected 340 of them, and
> incorrectly identified 7. Therefore 3% of true instances are missed,
> and 2% of detections are false positives.

There is an inherent tradeoff between false positives and false
negatives. Many of our modules are designed like “smoke detectors”,
where they are more likely to detect a practice that needs attention,
but also more likely to incorrectly flag something. Therefore, you need
to check the output of each module, keeping the validated error rates in
mind.

Metacheck is under continuous development. It can be surprisingly
difficult to automatically retrieve information from papers, and there
is a large amount of edge-cases where our tool might not yet work
accurately. By using our tool, and providing us with feedback about what
we can’t get right yet, you will help to improve the tool for all other
users. Issues can be submitted [on
Github](https://github.com/scienceverse/metacheck/issues), and
suggestions for improvement or feedback can be sent to
<metacheck@scienceverse.org>.

## Summary

- ✅️ [Open Practices Check](#open-practices-check): Shared data and code
  detected.  
- ✅️ [COI Check](#coi-check): A conflict of interest statement was
  detected.  
- ⚠️ [Funding Check](#funding-check): No funding statement was
  detected.  
- ℹ️ [Preregistration Check](#preregistration-check): We found 2
  preregistrations.  
- ⚠️ [Power Analysis Check](#power-analysis-check): We detected 3
  potential power analyses.  
- ⚠️ [Exact P-Values](#exact-p-values): We found 1 imprecise *p* value
  out of 3 detected *p* values.  
- 🔍 [Non-Significant P Value Check](#non-significant-p-value-check): We
  found 2 non-significant p values that should be checked for
  appropriate interpretation.  
- ⚠️ [Marginal Significance](#marginal-significance): You described 2
  effects with terms related to ‘marginally significant’.  
- 🔍 [Effect Sizes in t-tests and
  F-tests](#effect-sizes-in-t-tests-and-f-tests): We found 1 t-test
  and/or F-test where effect sizes are not reported. Check these tests
  in the table below, and consider adding effect sizes  
- ⚠️ [StatCheck](#statcheck): 1 possible error in t-tests or F-tests  
- 🔍 [Repository Check](#repository-check):
  - We found 14 files in 3 repositories.
  - We found 1 README file and 2 repositories without READMEs.
  - We found 1 archive file.  
- 🔍 [Code Check](#code-check):
  - We found 4 R, 0 SAS, 0 SPSS, and 0 Stata code files.
  - All your code files had comments.
  - 4 files loaded in the code were missing in the repository.
  - Absolute file paths were found.
  - Libraries/imports were loaded in multiple places.
  - No parsing issues of R-type files were found.  
- 🔍 [Reference Accuracy](#reference-accuracy): We checked 5 references
  in CrossRef and found entries for 3.  
- ℹ️ [Replication Check](#replication-check): We found 1 replication for
  1 original you cited.  
- ℹ️ [RetractionWatch](#retractionwatch): You cited 1 article in the
  RetractionWatch database.  
- ℹ️ [PubPeer Comments](#pubpeer-comments): You cited 1 reference with
  comments in PubPeer.  
- ℹ️ [Summarise References](#summarise-references): Summary information
  provided for 5 references

## General Modules

### ✅️ Open Practices Check

Shared data and code detected.

View detailed feedback

Data was openly shared for this article, based on the following text:

> The paper shows examples of (1) open and closed OSF links; (2a)
> citation of retracted papers, (2b) citations without a doi, (2c)
> citations with Pubpeer comments, (2d) citations in the FLoRA
> replication database, and (2e) missing/mismatched/incorrect citations
> and references; (3a) R files with code on GitHub that do not load
> libraries in one location, (3b) load files that are not shared in the
> repository, (3c) lack comments, and (3d) have absolute file paths; (4)
> imprecise reporting of non-significant p-values; (5) tests with and
> without effect sizes; (6) use of “marginally significant” to describe
> non-significant findings; (7) a power analysis reporting some of the
> essential attributes; and (8) retrieving information from
> preregistrations.

> Data and analysis code is available on GitHub from
> https://github.com/Lak-ens/to_err_is_human and from
> https://researchbox.org/4377.

> Data is also available from https://osf.io/5tbm9 and code is also
> available from https://osf.io/629bx.

Code was openly shared for this article, based on the following text:

> The paper shows examples of (1) open and closed OSF links; (2a)
> citation of retracted papers, (2b) citations without a doi, (2c)
> citations with Pubpeer comments, (2d) citations in the FLoRA
> replication database, and (2e) missing/mismatched/incorrect citations
> and references; (3a) R files with code on GitHub that do not load
> libraries in one location, (3b) load files that are not shared in the
> repository, (3c) lack comments, and (3d) have absolute file paths; (4)
> imprecise reporting of non-significant p-values; (5) tests with and
> without effect sizes; (6) use of “marginally significant” to describe
> non-significant findings; (7) a power analysis reporting some of the
> essential attributes; and (8) retrieving information from
> preregistrations.

> Data and analysis code is available on GitHub from
> https://github.com/Lak-ens/to_err_is_human and from
> https://researchbox.org/4377.

> Data is also available from https://osf.io/5tbm9 and code is also
> available from https://osf.io/629bx.

NoteHow It Works

This module searches for open data, code, materials, and registration
statements.

It is much faster than the previous ODDPub version of this module, and
has a lower false negative rate, but also a higher false positive rate.

This module was developed by Lisa DeBruine

### ✅️ COI Check

A conflict of interest statement was detected.

View detailed feedback

The following conflict of interest statement was detected.

NoteHow It Works

Identify and extract Conflicts of Interest (COI) statements.

The COI Check module uses regular expressions to check sentences for
words related to conflict of interest statements. It will return the
sentences in which the conflict of interest statement was found.

The function incorporates code from
[rtransparent](https://github.com/serghiou/rtransparent), which is no
longer maintained. For their validation, see [the
paper](https://doi.org/10.1371/journal.pbio.3001107).

This module was developed by Daniel Lakens

### ⚠️ Funding Check

No funding statement was detected.

No funding statement was detected. Consider adding one.

NoteHow It Works

Identify and extract funding statements.

The Funding Check module uses regular expressions to check sentences for
words related to funding statements. It will return the sentences in
which the conflict of interest statement was found.

The function incorporates code from
[rtransparent](https://github.com/serghiou/rtransparent), which is no
longer maintained. For their validation, see [the
paper](https://doi.org/10.1371/journal.pbio.3001107).

This module was developed by Daniel Lakens

## Method Modules

### ℹ️ Preregistration Check

We found 2 preregistrations.

View detailed feedback

We found 2 preregistrations.

Meta-scientific research has shown that deviations from preregistrations
are often not reported or checked, and that the most common deviations
concern the sample size. We recommend manually checking the full
preregistration at the links above, and have provided the preregistered
sample size.

TipFull Preregistration

TipLearn More

For metascientific articles demonstrating the rate of deviations from
preregistrations, see:

van den Akker O, Bakker M, van Assen M, Pennington C, Verweij L,
Elsherif M, Claesen A, Gaillard S, Yeung S, Frankenberger J, Krautter K,
Cockcroft J, Kreuer K, Evans T, Heppel F, Schoch S, Korbmacher M, Yamada
Y, Albayrak-Aydemir N, Wicherts J (2024). “The potential of
preregistration in psychology: Assessing preregistration producibility
and preregistration-study consistency.” *Psychological Methods*.
[doi:10.1037/met0000687](https://doi.org/10.1037/met0000687).

For educational material on how to report deviations from
preregistrations, see:

Lakens, Daniël (2024). “When and How to Deviate From a Preregistration.”
*Collabra: Psychology*, **10**(1), 117094.
[doi:10.1525/collabra.117094](https://doi.org/10.1525/collabra.117094).

NoteHow It Works

Retrieve information from preregistrations in a standardised way, and
make them easier to check.

The Preregistration Check module identifies preregistrations on the OSF
and AsPredicted based on links in the manuscript, retrieves the
preregistration text, and organizes the information into a template. The
module then uses regular expressions to identify text from AsPredicted,
and the API to retrieve text from the OSF. The information in the
preregistration is returned.

The module can’t extract information from non-structured preregistration
templates (i.e., where the preregistration is uploaded in a single text
field) and it can’t retrieve information in preregistrations that are
stored as text documents on the OSF.

If you want to extend the package to be able to download information
from other preregistration sites, reach out to the Metacheck development
team.

This module was developed by Daniel Lakens and Lisa DeBruine

### ⚠️ Power Analysis Check

We detected 3 potential power analyses.

View detailed feedback

We used the LLM model ‘ollama/qwen2.5:3b’ to check the contents of 3
paragraphs that contained words suggesting they might contain power
analyses.

Some essential information could not be detected: alpha_level,
effect_size, effect_size_metric, software

TipLearn More

Power analyses need to contain the following information to be
interpretable: the type of power analysis, the statistical test, the
software used, sample size, critical alpha criterion, power level,
effect size, and an effect size metric. In addition, it is recommended
to make sure the power analysis is reproducible (by sharing the code, or
a screenshot, of the power analysis), and to provide good arguments for
why the study was designed to detect an effect of this size.

For an a-priori power analysis, where the sample size is determined,
reporting all information would look like:

> An a priori power analysis for an independent samples t-test,
> conducted using the pwr.t.test function from pwr (Champely, 2020),
> indicated that for a Cohen’s d = 0.5, an alpha level of 0.05, and a
> desired power level of 80% required at least 64 participants in each
> group.

For a sensitivity power analysis, this sentence would look like:

> A sensitivity power analysis for an independent samples t-test,
> conducted using the pwr.t.test function from pwr (Champely, 2020),
> indicated that with 64 participants in each group, and an alpha level
> of 0.05, a desired power level of 80% was reached for an effect size
> of d = 0.5.

NoteHow It Works

This module uses uses regular expressions to identify sentences that
contain a statistical power analysis. If specified by the user, it also
uses a large language module (LLM) to extract information reported in
power analyses, including the statistical test, sample size, alpha
level, desired level of power, and magnitude and type of effect size.

The Power Analysis Check module uses regular expressions to identify
sentences that contain a statistical power analysis. Without the use of
an LMM, the module uses regular expressions to classify the power
analysis as a-priori, sensitivity or post-hoc. With the use of an LMM,
it checks if the power analysis is reported with all required
information.

The regular expressions can miss power analyses, or fail to classify
them correctly. The type of power analysis is often difficult to
classify, which can easily be solved by explicitly specifying the type
of power analysis as ‘a-priori’, ‘sensitivity’, or ‘post-hoc’. Note that
‘post-hoc’ or ‘observed’ power is rarely useful. The LMM can fail to
identify information in the paper, and will not have access to
information in paragraphs in the paper other than those that contain the
word ‘power’. This package was validated by the Metacheck team on
articles in Psychological Science.

This module was developed by Lisa DeBruine, Daniel Lakens and Cristian
Mesquida

**Validation**: In a sample of 128 papers with 246 instances of power
statements, 203 were correctly detected (true positives), 22 were missed
(false negatives) and 21 were incorrectly detected (false positives).
Overall, among all instances flagged as power statements, 90.6% were
correct (positive prediction value).

## Results Modules

### ⚠️ Exact P-Values

We found 1 imprecise *p* value out of 3 detected *p* values.

View detailed feedback

Reporting *p* values imprecisely (e.g., *p* \< .05) reduces
transparency, reproducibility, and re-use (e.g., in *p* value
meta-analyses). Best practice is to report exact p-values with three
decimal places (e.g., *p* = .032) unless *p* values are smaller than
0.001, in which case you can use *p* \< .001.

TipLearn More

The APA manual states: Report exact *p* values (e.g., *p* = .031) to two
or three decimal places. However, report *p* values less than .001 as
*p* \< .001. However, 2 decimals is too imprecise for many use-cases
(e.g., a *p* value meta-analysis), so report *p* values with three
digits.

American Psychological Association (2020). *Publication manual of the
American Psychological Association*, 7 edition. American Psychological
Association.

NoteHow It Works

List any p-values reported with insufficient precision (e.g., p \< .05
or p = n.s.) or reported as exactly zero (e.g., p = .000).

This module uses regular expressions to identify p-values. It will flag
any values reported as p \> ? or p \< numbers greater than .001. It will
also flag p-values reported as exactly zero (e.g., p = .000, p = 0.00),
which are mathematically impossible — p-values are never exactly zero
and should instead be reported as p \< .001.

We try to exclude figure and table notes like “\* p \< .05”, but may not
succeed at excluding all false positives.

This module was developed by Lisa DeBruine

**Validation**: In a sample of 225 papers containing 405 instances of
non-exact p-values, th module correctly detected 269 cases (true
positives) and incorrectly identified 78 (false positives). It missed
136 instances of imprecisely reported p-values (false negatives) and
correctly identified 4557 cases of precisely reported p-values (true
negative). Additionally, 78% of positive detections were correct
(positive predictive value).

### 🔍 Non-Significant P Value Check

We found 2 non-significant p values that should be checked for
appropriate interpretation.

View detailed feedback

Meta-scientific research has shown nonsignificant p values are commonly
misinterpreted. It is incorrect to infer that there is ‘no effect’, ‘no
difference’, or that groups are ‘the same’ after p \> 0.05.

It is possible that there is a true non-zero effect, but that the study
did not detect it. Make sure your inference acknowledges that it is
possible that there is a non-zero effect. It is correct to include the
effect is ‘not significantly’ different, although this just restates
that p \> 0.05.

Metacheck does not yet analyze automatically whether sentences which
include non-significant p-values are correct, but we recommend manually
checking the sentences below for possible misinterpreted non-significant
p values.

TipLearn More

For metascientific articles demonstrating the rate of misinterpretations
of non-significant results is high, see:

Aczel B, Palfi B, Szollosi A, Kovacs M, Szaszi B, Szecsi P, Zrubka M,
Gronau Q, van den Bergh D, Wagenmakers E (2018). “Quantifying Support
for the Null Hypothesis in Psychology: An Empirical Investigation.”
*Advances in Methods and Practices in Psychological Science*, **1**(3),
357–366.
[doi:10.1177/2515245918773742](https://doi.org/10.1177/2515245918773742).

Murphy S, Merz R, Reimann L, Fernández A (2025). “Nonsignificance
misinterpreted as an effect’s absence in psychology: Prevalence and
temporal analyses.” *Royal Society Open Science*, **12**(3), 242167.
[doi:10.1098/rsos.242167](https://doi.org/10.1098/rsos.242167).

For educational material on preventing the misinterpretation of p
values, see [Improving Your Statistical
Inferences](https://lakens.github.io/statistical_inferences/01-pvalue.html#sec-misconception1).

NoteHow It Works

This module checks for imprecisely reported p values. If p \> .05 is
detected, it warns for misinterpretations.

The nonsignificant p-value check searches for regular expressions that
match a predefined pattern. The module identifies all p-values in a
manuscript and selects those that are not reported to be smaller than or
equal to 0.05. It returns all sentences containing non-significant
p-values.

In the future, the Metacheck team aims to incorporate a machine learning
classifier to only return sentences likely to contain
misinterpretations. If you want to help to improve the module, reach out
to the Metacheck development team.

This module was developed by Daniel Lakens

**Validation**: In a sample of 194 papers with 1602 instances of
non-significant p-values, this module correctly detected 1486 of them,
and incorrectly identified 153. Additionally, 91% of detections were
true instances (positive predictive value). That is, when this module
flags non-significant p-values in a paper, it correctly identifies an
issue 91% of the time.

### ⚠️ Marginal Significance

You described 2 effects with terms related to ‘marginally significant’.

View detailed feedback

You described effects with terms related to ‘marginally significant’. If
*p* values above 0.05 are interpreted as an effect, you inflate the
alpha level, and increase the Type 1 error rate. If a *p* value is
higher than the prespecified alpha level, it should be interpreted as a
non-significant result.

TipLearn More

For metascientific articles demonstrating the rate at which
non-significant p-values are interpreted as marginally significant, see:

Olsson-Collentine, A., van Assen, M. MAL, Hartgerink &, J. CH (2019).
“The Prevalence of Marginally Significant Results in Psychology Over
Time.” *Psychological Science*, **30**, 576–586.
[doi:10.1177/0956797619830326](https://doi.org/10.1177/0956797619830326).

For the list of terms used to identifify marginally significant results,
see this [blog post by Matthew
Hankins](https://web.archive.org/web/20251001114321/https://mchankins.wordpress.com/2013/04/21/still-not-significant-2/).

NoteHow It Works

List all sentences that describe an effect as ‘marginally significant’.

The marginal module searches for regular expressions that match a
predefined pattern. The list of terms is a subset of those listed in a
[blog post by Matthew
Hankins](https://web.archive.org/web/20251001114321/https://mchankins.wordpress.com/2013/04/21/still-not-significant-2/).
The module returns all sentences that match terms describing ‘marginally
significant’ results.

Some of the terms identified might not be problematic in some contexts,
and there are ways to describe ‘marginal significance’ that are not
detected by the module.

This module was developed by Daniel Lakens

**Validation**: In a sample of 51 papers with 87 statements, this module
correctly identified 38 statements (true positives) and incorrectly
flagged 22 statements (false positives). It failed to detect 27
statements. Thus, among all statements flagged by the module, 63% were
genuine cases (positive predictive value). However, the module missed
42% of all true statements (false negative rate).

### 🔍 Effect Sizes in t-tests and F-tests

We found 1 t-test and/or F-test where effect sizes are not reported.
Check these tests in the table below, and consider adding effect sizes

View detailed feedback

We recommend checking the sentences below, and add any missing effect
sizes.

For t-tests with a reported d, coherence checks yielded 1 indeterminate
case.

TipLearn More

For metascientific articles demonstrating that effect sizes are often
not reported:

- Peng, C.-Y. J., Chen, L.-T., Chiang, H.-M., & Chiang, Y.-C. (2013).
  The Impact of APA and AERA Guidelines on Effect Size Reporting.
  Educational Psychology Review, 25(2), 157–209.
  doi:[10.1007/s10648-013-9218-2](https://doi.org/10.1007/s10648-013-9218-2).

For educational material on reporting effect sizes:

- [Guide to Effect Sizes and Confidence
  Intervals](https://matthewbjane.quarto.pub/guide-to-effect-sizes-and-confidence-intervals/)

TipAll detected and assessed stats

NoteHow It Works

The Effect Size module checks if effect sizes are correctly reported in
t-tests and F-tests.

The Effect Size check searches for regular expressions that match
typical ways in which effect sizes are reported. It subsequently checks
different ways in which Cohen’s d, g, ηp2, and ωp2 can be computed
against the reported value. If effects are missing, or might be
incorrect, you the module provides a warning. The module was validated
on APA reported statistical tests, and might miss effect sizes that were
reported in other reporting styles. It was validated by the Metacheck
team on papers published in Psychological Science.

This module was developed by Daniel Lakens and Lisa DeBruine

**Validation**: In a sample of 161 papers with 1469 tests, this module
correctly detected 1106 reported effect sizes (true positives) and
correctly identified 295 cases where no effect size was present (true
negatives). However, it missed 23 that were reported (false negatives),
and incorrectly identified 45 effect sizes when none were reported
(false positives). Among all instances detected by the module, 96% were
true cases (positive predictive value). In a validation against 221
reported Cohen’s d effect sizes, it correctly indicated coherence in 218
cases (99%). In a validation against 485 partial eta-squared effect
sizes, it correctly indicated coherence in 480 (99%)

### ⚠️ StatCheck

1 possible error in t-tests or F-tests

View detailed feedback

We detected possible errors in test statistics. Note that as the
accuracy of statcheck has only been validated for *t*-tests and
*F*-tests. As Metacheck only uses validated modules, we only provide
statcheck results for *t* tests and *F*-tests.

TipLearn More

For metascientific research on the validity of statcheck, and it’s
usefulness to prevent statistical reporting errors, see:

Nuijten M, van Assen M, Hartgerink C, Epskamp S, Wicherts J (2017). “The
validity of the tool "statcheck" in discovering statistical reporting
inconsistencies.”
[doi:10.31234/osf.io/tcxaja](https://doi.org/10.31234/osf.io/tcxaja).
Preprint.

Nuijten M, Wicherts J (2023). “The effectiveness of implementing
statcheck in the peer review process to avoid statistical reporting
errors.”
[doi:10.31234/osf.io/bxau9](https://doi.org/10.31234/osf.io/bxau9).
Preprint.

NoteHow It Works

Check consistency of p-values and test statistics

The Statcheck module runs Statcheck. Statcheck searches for regular
expressions that match a predefined pattern, and identifies APA reported
statistical tests. More information on the package can be found at
<https://github.com/cran/statcheck>. The module only returns Statcheck
results for t-tests and F-tests, as these are the only tests which have
been validated, see <https://osf.io/preprints/psyarxiv/tcxaj_v1/>.

Statcheck was developed by Michèle Nuijten and Sascha Epskamp.

Statcheck considers p = 0.000 an error, as you should report p \< 0.001.
Furthermore, p \< 0.03 is an error if the p-value was 0.031, and one
should simply report exact p-values (p = 0.031). Statcheck might miss
one-sided tests, and falsely assume the p-value is incorrect. For more
information, see [StatCheck](https://statcheck.io/).

This module was developed by Daniel Lakens and Lisa DeBruine

**Validation**: In a sample of 685 tests with 34 instances of
inconsistent reporting, Statcheck correctly detected 34 of them, and
incorrectly identified 26. Therefore 0% of true instances were missed,
and 43% of detections were false positives. See
<https://osf.io/preprints/psyarxiv/tcxaj_v1/> for more details of the
validation.

### 🔍 Repository Check

- We found 14 files in 3 repositories.
- We found 1 README file and 2 repositories without READMEs.
- We found 1 archive file.

View detailed feedback

#### Repositories

#### Files

#### README Files

README files are a way to document the contents and structure of a
folder, helping users locate the information they need. You can use a
README to document changes to a repository, and explain how files are
named. Please consider adding a README to each repository or including
‘README’ in the name of your overview document.

#### Archive Files

The following files are archives: Archive.zip. We did not examine their
content. Consider uploading these individually to improve
discoverability and re-use.

NoteHow It Works

This module retrieves information from repositories.

The Repository Check module lists files on the OSF, GitHub, ResearchBox,
and Zenodo based on links in the manuscript.

If you want to extend the package to be able to download files from
additional data repositories reach out to the Metacheck development
team.

This module was developed by Daniel Lakens and Lisa DeBruine

### 🔍 Code Check

- We found 4 R, 0 SAS, 0 SPSS, and 0 Stata code files.
- All your code files had comments.
- 4 files loaded in the code were missing in the repository.
- Absolute file paths were found.
- Libraries/imports were loaded in multiple places.
- No parsing issues of R-type files were found.

View detailed feedback

Below, we describe some best coding practices and give the results of
automatic evaluation of these practices in the code files below. This
check may miss things or produce false positives if your scripts are
less typical.

#### Code Comments

Best programming practice is to add comments to code, to explain what
the code does (to yourself in the future, or peers who want to re-use
your code). All your code files had comments.

#### Missing Files

The scripts load files, but 4 scripts loaded 4 files that could not be
automatically identified in the repository. Check if the following files
are made available, so that others can reproduce your code, or that the
files are missing:

#### Absolute Paths

Best programming practice is to use relative file paths (e.g.,
‘./files’) instead of absolute file paths (e.g.,
‘C://Lakens/project_dir/files’) as these folder names do not exist on
other computers. The following absolute file paths were found in 4 code
files. However, these may be false positives in code like
`paste0(dir, '/file.csv')`.

#### Libraries / Imports

Best programming practice is to load all required libraries/imports in
one block near the top of the code. In 2 code files, libraries/imports
were at multiple places (i.e., with more than 3 non-comment lines in
between).

#### Parsable code

All R-type code files (.R, .Rmd, .qmd) could be read in. There were no
parsing issues.

NoteHow It Works

This module retrieves information from repositories checked by
repo_check about code files (R, SAS, SPSS, Stata).

The Code Check module checks R, Rmd, Qmd, SAS, SPSS, and Stata files,
using regular expressions to check the code. The regular expression
search will detect the number of comments, the lines at which
libraries/imports are loaded, attempts to detect absolute paths to
files, and lists files that are loaded, and checks if these files are in
the repository. The module will return suggestions to improve the code
if there are no comments, if libraries/imports are loaded in lines
further than 4 lines apart, if files that are loaded are not in the
repository, and if absolute file paths are found.

The regular expressions can miss information in code files, or falsely
detect parts of the code as a fixed file path. Libraries/imports might
be loaded in one block, even if there are more than 3 intermittent
lines. The package was validated internally on papers published in
Psychological Science. There might be valid reasons why some loaded
files can’t be shared, but the module can’t evaluate these reasons, and
always gives a warning.

If you want to extend the package to perform additional checks on code
files, or make the checks work on other types of code files, reach out
to the Metacheck development team.

This module was developed by Daniel Lakens and Raphael Merz

## Reference Modules

### 🔍 Reference Accuracy

We checked 5 references in CrossRef and found entries for 3.

View detailed feedback

Double check any references listed in the tables below. This module has
a high false positive rate.

Title mismatches often happen because of errors reading text from PDFs.
Author mismatches often happen because of errors in parsing author
lists. Year mismatches often happen because of differences between date
of first publication and date of print publication.

NoteHow It Works

This module checks references for mismatches with CrossRef.

This module uses the bib_match table from each paper (this can be added
or refreshed using `add_bib_match()`) to detect possible problems in the
reference section.

We check that the title from your reference section is the same as the
retrieved title (ignoring differences in capitalisation) and that all
author last names in your reference section are also in the retrieved
author list (we do not check first names or order yet). This check is
done for all references with crossref entries in the bib_match table.

Mismatches may be because of problems with our parsing of references
from your PDF (we’re working on improving this), incorrect formatting in
CrossRef, or minor differences in punctuation.

This module was developed by Daniel Lakens and Lisa DeBruine

### ℹ️ Replication Check

We found 1 replication for 1 original you cited.

View detailed feedback

We checked 5 references with DOIs. We found 1 replication for 1 original
you cited.

Check if you are aware of the replication studies, and cite them where
appropriate.

NoteHow It Works

This module checks references and warns for citations of original
studies for which replication or reproduction studies exist in the FLoRA
database.

The Replication Check module compares the reference list against studies
in the FLoRA (FORRT Library of Replication Attempts) database based on
the DOI. If a study in the database is found, a reminder is provided
that a replication or reproduction of the original study exists, and
should be cited (currently, a warning is provided regardless of whether
the replication/reproduction study is already cited).

The module requires that the reference has a DOI. If you run the
ref_doi_check module in a pipeline before this, it will use the enhanced
DOI list from that module, otherwise it will only run on references with
existing DOIs.

It is possible the original study was cited for other reasons than the
empirical claim tested, or that the replication/reproduction in the
FLoRA database is for only one of the studies in the paper, and not the
study the authors discuss.

The database can be manually updated with the `FLoRA_update()` function.
For more information, see <https://forrt.org/FLoRA/>.

This module was developed by Daniel Lakens, Lisa DeBruine and Lukas
Wallrich

### ℹ️ RetractionWatch

You cited 1 article in the RetractionWatch database.

View detailed feedback

We checked 5 references with DOIs. You cited 1 article in the
RetractionWatch database.

Check if you are aware of the replication studies, and cite them where
appropriate.

NoteHow It Works

This module checks references and warns for citations in the
RetractionWatch Database.

The RetractionWatch Check module compares the reference list against
studies in the RetractionWatch database based on the DOI. If a study in
the database is found, a reminder is provided that the study was
retracted, has an expression of concern, or a correction.

The module requires that the reference has a DOI. If you run the
ref_doi_check module in a pipeline before this, it will use the enhanced
DOI list from that module, otherwise it will only run on references with
existing DOIs.

It is possible the authors are already aware that a study was retracted,
but the module can’t evaluate this.

The database can be manually updated with the rw_update function. For
more information, see https://gitlab.com/crossref/retraction-watch-data.

This module was developed by Daniel Lakens and Lisa DeBruine

### ℹ️ PubPeer Comments

You cited 1 reference with comments in PubPeer.

View detailed feedback

We checked 5 references with DOIs. You cited 1 reference with comments
in PubPeer.

Pubpeer is a platform for post-publication peer review. We have filtered
out Pubpeer comments by ‘Statcheck’. You can check out the comments by
visiting the URLs below:

NoteHow It Works

This module checks references and warns for citations that have comments
on pubpeer (excluding Statcheck comments).

The PubPeer module uses the PubPeer API to check for each reference that
has a DOI whether there are comments on the post-publication peer review
platform. If comments are found, a link to the comments is provided.
Comments by ‘Statcheck’ on PubPeer are ignored, see
https://retractionwatch.com/2016/09/02/heres-why-more-than-50000-psychology-studies-are-about-to-have-pubpeer-entries/.

The module requires that the reference has a DOI. If you run the
doi_check module in a pipeline before this, it will use the enhanced DOI
list from that module, otherwise it will only run on references with
existing DOIs.

For more information, see
[PubPeer](https://www.pubpeer.com/static/about).

This module was developed by Daniel Lakens and Lisa DeBruine

### ℹ️ Summarise References

Summary information provided for 5 references

View detailed feedback

See the specific reports above for details.

NoteHow It Works

Summarise information about each reference in a paper.

This module summarises previously-run reference section modules:
ref_accuracy, ref_pubpeer, ref_replication, and ref_retraction.

This module was developed by Lisa DeBruine
