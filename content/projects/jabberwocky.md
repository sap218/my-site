+++
title = "Jabberwocky: an NLP toolkit for those nonsensical ontologies"
date = "2021-05-11"
categories = ['python','ontology','nlp','tf-idf','tool']
+++

## Jabberwocky

**Jabberwocky** is a Natural Language Processing (NLP) toolkit for those nonsensical ontologies[^jabberwocky].
Available open-source on [GitHub](https://github.com/sap218/jabberwocky "github").



{{< alert type="info" >}}
To avoid duplicating information, all information is provided in the links:
{{< /alert >}}

+ [GitHub](https://github.com/sap218/jabberwocky "github").
+ [Documentation](https://sap218.github.io/jabberwocky/ "documentation").
+ [Changelog](https://github.com/sap218/jabberwocky/blob/master/Changelog.md "change log").

I started development in 2019 and Jabberwocky **v1.0** was published in 2020[^jabberwocky].

After a few years of a PhD and starting a job, in 2024 I came back to Jabberwocky and Version **3.0** was a complete revamp of the repository.
Version **2.0** (2021) improved the annotation script with a Phrase Matcher[^spacy] so both key terms and phrases work.
Futhermore, high-level functions for stop word removal & text cleaning and a new plotting feature was introduced: a pretty word cloud!

Shortly after version **3.1** was released! This included a new plotting feature.
And the TF-IDF[^tfidf] (statistical method) was improved with the to ability to use n-grams so rankings can expand to uni-grams, bi-grams, tri-grams, and more.

In 2025, **v3.1.1** updates were inspired by an old project - [cyannotator](https://github.com/sap218/cyannotator "cyannotator") - users can now request an `HTML` output of the corpus with key terms highlighted.

As of 2026, **v4.0** has been released, with a complete change of log scripts, error handling, convering `excel` to `owl`, and more.

### References

[^jabberwocky]: Pendleton, Samantha C., and Georgios V. Gkoutos. "[**Jabberwocky**](https://joss.theoj.org/papers/10.21105/joss.02168 "jabberwocky manuscript"): an ontology-aware toolkit for manipulating text." Journal of Open Source Software 5.51 (2020): 2168.
[^spacy]: Honnibal, Matthew, et al. "[spaCy](https://spacy.io/api/phrasematcher "spacy phrase matcher function"): Industrial-strength natural language processing in python." (2020).
[^tfidf]: [Term frequency inverse document frequency](https://en.wikipedia.org/wiki/Tf%E2%80%93idf "Wikipedia link to TF-IDF").
