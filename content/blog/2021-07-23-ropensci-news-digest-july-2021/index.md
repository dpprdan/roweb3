---
title: rOpenSci News Digest, July 2021
author:
  - The rOpenSci Team
date: '2021-07-23'
slug: ropensci-news-digest-july-2021
categories: []
tags:
  - newsletter
description: keywords from the content
output:
  html_document:
    keep_md: yes
params:
  last_newsletter: '2021-06-21'
---


<!-- Before sending DELETE THE INDEX_CACHE and re-knit! -->

Dear rOpenSci friends, it's time for our monthly news roundup!
<!-- blabla -->
You can read this post [on our blog](/blog/2021/07/23/ropensci-news-digest-july-2021).
Now let's dive into the activity at and around rOpenSci!

## rOpenSci HQ

<!-- to be curated manually -->

Find out about more [events](/events).

## Software :package:

### New packages




The following two packages recently became a part of our software suite:

+ [awardFindR](https://docs.ropensci.org/awardFindR), developed by Michael McCall: Queries a number of scientific awards databases. Collects relevant results based on keyword and date parameters, returns list of projects that fit those criteria as a data frame. Sources include: Arnold Ventures, Carnegie Corp, Federal RePORTER, Gates Foundation, MacArthur Foundation, Mellon Foundation, NEH, NIH, NSF, Open Philanthropy, Open Society Foundations, Rockefeller Foundation, Russell Sage Foundation, Robert Wood Johnson Foundation, Sloan Foundation, Social Science Research Council, John Templeton Foundation, and USASpending.gov. It has been [reviewed](https://github.com/ropensci/software-review/issues/432) by João Martins, Kara Woo.

+ [katex](https://docs.ropensci.org/katex), developed by Jeroen Ooms: Convert latex math expressions to HTML and MathML for use in markdown documents or package manual pages. The rendering is done in R using the V8 engine (i.e. server-side), which eliminates the need for embedding the MathJax library into your web pages. In addition a math-to-rd wrapper is provided to automatically render beautiful math in R documentation files.  It is available on [CRAN]( https://CRAN.R-project.org/package=katex). 

Discover [more packages](/packages), read more about [Software Peer Review](/software-review).

### New versions



The following thirteen packages have had an update since the latest newsletter: [gert](https://docs.ropensci.org/gert "Simple Git Client for R") ([`v1.3.1`](https://github.com/r-lib/gert/releases/tag/v1.3.1)), [awardFindR](https://docs.ropensci.org/awardFindR "awardFindR") ([`1.0`](https://github.com/ropensci/awardFindR/releases/tag/1.0)), [BaseSet](https://docs.ropensci.org/BaseSet "Working with Sets the Tidy Way") ([`v0.0.17`](https://github.com/ropensci/BaseSet/releases/tag/v0.0.17)), [beastier](https://docs.ropensci.org/beastier "Call BEAST2") ([`v2.4.3`](https://github.com/ropensci/beastier/releases/tag/v2.4.3)), [fingertipsR](https://docs.ropensci.org/fingertipsR "Fingertips Data for Public Health") ([`v1.0.7`](https://github.com/ropensci/fingertipsR/releases/tag/v1.0.7)), [GSODR](https://docs.ropensci.org/GSODR "Global Surface Summary of the Day (GSOD) Weather Data Client") ([`v3.1.2`](https://github.com/ropensci/GSODR/releases/tag/v3.1.2)), [ijtiff](https://docs.ropensci.org/ijtiff "Comprehensive TIFF I/O with Full Support for ImageJ TIFF Files") ([`v2.2.7`](https://github.com/ropensci/ijtiff/releases/tag/v2.2.7)), [plotly](https://docs.ropensci.org/plotly "Create Interactive Web Graphics via plotly.js") ([`v4.9.4.1`](https://github.com/ropensci/plotly/releases/tag/v4.9.4.1)), [rcol](https://docs.ropensci.org/rcol "Catalogue of Life Client") ([`v0.2.0`](https://github.com/ropensci/rcol/releases/tag/v0.2.0)), [stantargets](https://docs.ropensci.org/stantargets "Targets for Stan Workflows") ([`0.0.3`](https://github.com/ropensci/stantargets/releases/tag/0.0.3)), [tarchetypes](https://docs.ropensci.org/tarchetypes "Archetypes for Targets") ([`0.2.1`](https://github.com/ropensci/tarchetypes/releases/tag/0.2.1)), [taxa](https://docs.ropensci.org/taxa "Classes for Storing and Manipulating Taxonomic Data") ([`v0.4.0`](https://github.com/ropensci/taxa/releases/tag/v0.4.0)), [tic](https://docs.ropensci.org/tic "CI-Agnostic Workflow Definitions") ([`v0.11.1`](https://github.com/ropensci/tic/releases/tag/v0.11.1)).

## Software Peer Review

There are twelve recently closed and active submissions and 5 submissions on hold. Issues are at different stages: 

* One at ['6/approved'](https://github.com/ropensci/software-review/issues?q=is%3Aissue+is%3Aopen+sort%3Aupdated-desc+label%3A6/approved):

     * [stantargets](https://github.com/ropensci/software-review/issues/430), Targets for Stan Workflows. Submitted by [Will Landau](https://wlandau.github.io).

* Six at ['4/review(s)-in-awaiting-changes'](https://github.com/ropensci/software-review/issues?q=is%3Aissue+is%3Aopen+sort%3Aupdated-desc+label%3A4/review(s)-in-awaiting-changes):

     * [rsat](https://github.com/ropensci/software-review/issues/437), Tools for Downloading, Customizing, and Processing Time Series of Satellite Images from Landsat, MODIS, and Sentinel. Submitted by [Unai Pérez-Goya](https://unai-perez.github.io/).

    * [gendercoder](https://github.com/ropensci/software-review/issues/435), Recodes Sex/Gender Descriptions Into A Standard Set. Submitted by [Emily Kothe](http://emilykothe.com).

    * [mctq](https://github.com/ropensci/software-review/issues/434), An R Package for the Munich ChronoType Questionnaire. Submitted by [Daniel Vartanian](https://orcid.org/0000-0001-7782-759X).

    * [slopes](https://github.com/ropensci/software-review/issues/420), Calculate Slopes of Roads, Rivers and Trajectories. Submitted by [RFlx](http://www.rosafelix.bike).

    * [healthdatacsv](https://github.com/ropensci/software-review/issues/358), Access data in the healthdata.gov catalog. Submitted by [iecastro](http://iecastro.netlify.com).

    * [chemspiderapi](https://github.com/ropensci/software-review/issues/329), R Wrapper for ChemSpider's API Services. Submitted by [Raoul Wolf](https://github.com/RaoulWolf).

* Four at ['3/reviewer(s)-assigned'](https://github.com/ropensci/software-review/issues?q=is%3Aissue+is%3Aopen+sort%3Aupdated-desc+label%3A3/reviewer(s)-assigned):

     * [allodb](https://github.com/ropensci/software-review/issues/436), Tree Biomass Estimation at Extratropical Forest Plots. Submitted by [Erika Gonzalez-Akre](https://sites.google.com/site/forestecoclimlab/home).

    * [ROriginStamp](https://github.com/ropensci/software-review/issues/433), Interface to OriginStamp API to Obtain Trusted Time Stamps. Submitted by [Rainer M Krug](https://github.com/rkrug).

    * [jagstargets](https://github.com/ropensci/software-review/issues/425), Targets for JAGS Workflows. Submitted by [Will Landau](https://wlandau.github.io).

    * [occCite](https://github.com/ropensci/software-review/issues/407), Querying and Managing Large Biodiversity Occurrence Datasets. Submitted by [Hannah Owens](http://hannahlowens.weebly.com/).

* One at ['1/editor-checks'](https://github.com/ropensci/software-review/issues?q=is%3Aissue+is%3Aopen+sort%3Aupdated-desc+label%3A1/editor-checks):

     * [rdbhapi](https://github.com/ropensci/software-review/issues/443), Interface to DBH-API. Submitted by [Marija Ninic](https://hkdir.no/).

Find out more about [Software Peer Review](/software-review) and how to get involved.

## On the blog

<!-- Do not forget to rebase your branch! -->



### Other topics

* [rOpenSci at useR!2021 - Presentations from Staff and Community](/blog/2021/07/02/ropensci-user2021) by Stefanie Butland. 4 rOpenSci staff presentations at useR! 2021.



### Tech Notes

* [How to create your personal CRAN-like repository on R-universe](/blog/2021/06/22/setup-runiverse) by Jeroen Ooms.

* [How We Curate Our Monthly Newsletter](/blog/2021/06/24/news-meta) by Maëlle Salmon. Our workflow for preparing and sending our monthly newsletter, partly automatically.

* [New package katex: rendering math to HTML and MathML in R](/blog/2021/07/13/katex-release) by Jeroen Ooms.

## Citations

No new citations added to our database this month ([browse all citations](/citations)).
Thanks for citing our tools!

## Use cases



Four use cases of our packages and resources have been reported since we sent the last newsletter.

* [Mapping Asian elephant observations with rgbif](https://discuss.ropensci.org/t/mapping-asian-elephant-observations-with-rgbif/2524). Reported by Tuija Sonkkila.

* [Environment Canada air temperature using weathercan](https://discuss.ropensci.org/t/environment-canada-air-temperature-using-weathercan/2537). Reported by Alexandre Bevington.

* [Historical dataviz recreations with a sprinkle of magick](https://discuss.ropensci.org/t/historical-dataviz-recreations-with-a-sprinkle-of-magick/2538). Reported by Matt Dray.

* [pdftools + tesseract para extraer texto en español](https://discuss.ropensci.org/t/pdftools-tesseract-para-extraer-texto-en-espanol/2544). Reported by Silvia Gutiérrez.

Explore [other use cases](/usecases) and [report your own](https://discuss.ropensci.org/c/usecases/10)!

## Call for maintainers

<!--IF CALL
* [our guidance on _Changing package maintainers_](https://devguide.ropensci.org/changing-maintainers.html)
* [our _Package Curation Policy_](https://devguide.ropensci.org/curationpolicy.html)

IF NO CALL
There's no open call for new maintainers at this point but you can refer to our [contributing guide](https://contributing.ropensci.org/) for finding ways to get involved!
As the maintainer of an rOpenSci package, feel free to contact us on Slack or email `info@ropensci.org` to get your call for maintainer featured in the next newsletter. -->

## Package development corner

Some useful tips for R package developers. :eyes:

<!-- To be curated by hand -->

## Last words

Thanks for reading! If you want to get involved with rOpenSci, check out our [Contributing Guide](https://contributing.ropensci.org) that can help direct you to the right place, whether you want to make code contributions, non-code contributions, or contribute in other ways like sharing use cases.

If you haven't subscribed to our newsletter yet, you can [do so via a form](/news/). Until it's time for our next newsletter, you can keep in touch with us via our [website](/) and [Twitter account](https://twitter.com/ropensci).