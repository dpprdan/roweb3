---
slug: "post-template"
title: Latest rOpenSci News Digest
author:
  - The rOpenSci Team
date: '2021-03-16'
tags:
  - newsletter
description: keywords from the content
output:
  html_document:
    keep_md: yes
params:
  last_newsletter: "2021-03-16"
---

```{r setup, include=FALSE}
library("magrittr")
library("rlang")
last_newsletter <- anytime::anytime(params$last_newsletter)
knitr::opts_chunk$set(echo = FALSE)
url <- sprintf(
    "/blog/%s/%s/%s/%s",
    lubridate::year(rmarkdown::yaml_front_matter(knitr::current_input())$date),
    stringr::str_pad(lubridate::month(rmarkdown::yaml_front_matter(knitr::current_input())$date), 2, "0", side = "left"),
    stringr::str_pad(lubridate::day(rmarkdown::yaml_front_matter(knitr::current_input())$date), 2, "0", side = "left"),
    rmarkdown::yaml_front_matter(knitr::current_input())$slug
    )
english <- function(x) {
  as.character(english::english(x))
}
```
<!-- Before sending DELETE THE INDEX_CACHE and re-knit! -->

Dear rOpenSci friends, it's time for our monthly news roundup!
<!-- blabla -->
You can read this post [on our blog](`r url`).
Now let's dive into the activity at and around rOpenSci!

## rOpenSci HQ

<!-- to be curated manually -->

Find out about more [events](/events).

## Software :package:

### New packages

```{r new-packages, cache = TRUE}
cran_unquote <- function(string) {
  gsub("\\'(.*?)\\'", "\\1", string)
}
tidy_package <- function(entry) {
  tibble::tibble(
    package = entry$name,
    description = cran_unquote(entry$description),
    details = cran_unquote(entry$details),
    on_cran = entry$on_cran,
    on_bioc = entry$on_bioc,
    onboarding = entry$onboarding,
    url = entry$url,
    maintainer = entry$maintainer # use desc for more info
    
  )
}

registry <- "https://raw.githubusercontent.com/ropensci/roregistry/gh-pages/registry.json" %>%
  jsonlite::read_json() %>%
  purrr::pluck("packages") %>%
  purrr::map_df(tidy_package)
  
since <- lubridate::as_date(last_newsletter) - 1
until <- lubridate::as_date(last_newsletter) + 1
commits <- gh::gh(
  "GET /repos/{owner}/{repo}/commits",
  owner = "ropensci",
  repo = "roregistry",
  since = sprintf(
    "%s-%s-%sT00:00:00Z",
    lubridate::year(since),
    stringr::str_pad(lubridate::month(since), 2, "0", side = "left"),
    stringr::str_pad(lubridate::day(since), 2, "0", side = "left")
  ),
  until = sprintf(
    "%s-%s-%sT00:00:00Z",
    lubridate::year(until),
    stringr::str_pad(lubridate::month(until), 2, "0", side = "left"),
    stringr::str_pad(lubridate::day(until), 2, "0", side = "left")
  )
)

empty <- TRUE
i <- length(commits)
while (empty == TRUE) {
  old <- "https://raw.githubusercontent.com/ropensci/roregistry/%s/packages.json" %>%
    sprintf(commits[[i]]$sha) %>%
    jsonlite::read_json() %>%
    purrr::map_df(function(x) tibble::tibble(package = x$package, url = x$url, branch = x$branch))
  i <- i - 1
  if (nrow(old) > 100) {
    empty <- FALSE
  }
}

old <- dplyr::filter(
  old,
  !grepl("ropenscilabs\\/", url),
  !grepl("ropensci-archive\\/", url)
)

new <- dplyr::filter(
  registry,
  !package %in% old$package,
  !grepl("ropenscilabs\\/", url),
  !grepl("ropensci-archive\\/", url)
)
```


The following `r if(nrow(new)>1) english(nrow(new))` package`r if(nrow(new)>1) "s"` recently became a part of our software suite:

```{r, results='asis', cache = TRUE}
packages <- split(new, seq(nrow(new)))
present_one <- function(package) {
  url_parts <- urltools::url_parse(package$url)
  desc_link <- gh::gh(
    "/repos/{owner}/{repo}/contents/{path}",
    owner = strsplit(url_parts$path, "\\/")[[1]][1],
    repo = strsplit(url_parts$path, "\\/")[[1]][2],
    path = "DESCRIPTION"
  ) %>%
    purrr::pluck("download_url")
  withr::with_tempfile(
    "tf", {
      download.file(desc_link, tf) 
      desc <<- desc::desc(file = tf)
    }
  )
  # as in pkgdown
  authors <- unclass(desc$get_authors())
  aut <- purrr::keep(authors, function(x) {any( x$role %in% "aut") && all(x$role != "cre") })
  aut <- purrr::map_chr(aut, function(x) paste(x$given, x$family))
  rev <- purrr::keep(authors, function(x) {any( x$role %in% "rev") && all(x$role != "cre") })
  rev <- purrr::map_chr(rev, function(x) paste(x$given, x$family))
  maintainer <- purrr::keep(authors, function(x) {any( x$role %in% "cre") })
  maintainer <- paste(c(maintainer[[1]]$given, maintainer[[1]]$family), collapse = " ")
  
  author_string <- sprintf("developed by %s", maintainer)
  
  if (length(aut) > 0) {
    author_string <- paste0(author_string, sprintf(" together with %s", toString(aut)))
  } 
  
  string <- sprintf(
    "[%s](https://docs.ropensci.org/%s), %s: %s. ",
    package$package, 
    package$package, 
    author_string,
    stringr::str_remove(stringr::str_squish(package$details), "\\.$")
  )
  
  if (package$on_cran) {
    string <- paste0(
      string, 
      sprintf(
        " It is available on [CRAN]( https://CRAN.R-project.org/package=%s). ",
        package$package
      )
    )
  }
  if (package$on_bioc) {
    string <- paste0(
      string, sprintf(
        " It is available on [Bioconductor](https://bioconductor.org/packages/%s/). ",
        package$package
      )
    )
  }
  if (nzchar(package$onboarding)) {
    string <- paste0(string, sprintf("It has been [reviewed](%s)", package$onboarding))
    if (length(rev) > 0) {
      string <- paste0(string, sprintf(" by %s.", toString(rev)))
    } else {
      string <- paste0(string, ".")
    }
  }
  
  paste("+", string)

}
text <- purrr::map_chr(
  packages,
  present_one
)
cat(paste0(text, collapse = "\n\n"))
```

Discover [more packages](/packages), read more about [Software Peer Review](/software-review).

### New versions

```{r news, cache=TRUE}
registry <- dplyr::filter(
  registry,
  !grepl("ropenscilabs\\/", url),
  !grepl("ropensci-archive\\/", url)
)

registry <- registry %>%
  dplyr::rowwise() %>%
  dplyr::mutate(
  owner = strsplit(urltools::path(url), "/")[[1]][1],
  repo = strsplit(urltools::path(url), "/")[[1]][2]
) %>%
  dplyr::filter(
    !is.na(owner)
  )
packages <- split(registry, seq(nrow(registry)))
get_release <- function(repo) {
  info <- gh::gh(
    "GET /repos/{owner}/{repo}/releases",
    owner = repo$owner,
    repo = repo$repo,
    per_page = 3,
    description = repo$description
  )
  info <- info[!purrr::map_lgl(info, "draft")]
  if(length(info) == 0 || anytime::anytime(info[[1]]$published_at) < last_newsletter) {
    return(NULL)
  }
  
  tibble::tibble(
    package = repo$package,
    version = info[[1]]$tag_name,
    url = info[[1]]$html_url,
    description = repo$description
  )
}
releases <- purrr::map_df(
  packages,
  get_release
)
releases <- split(releases, seq(nrow(releases)))
format_release <- function(release) {
  sprintf(
    '[%s](https://docs.ropensci.org/%s "%s") ([`%s`](%s))',
    release$package,
    release$package,
    release$description,
    release$version,
    release$url
  )
}
all_releases <- purrr::map_chr(releases, format_release)
text <- toString(all_releases)
```

The following `r if (length(releases) > 1) english(length(releases))` package`r if (length(releases) > 1) "s"` `r if (length(releases) > 1) "have" else "has"` had an update since the latest newsletter: `r text`.

## Software Peer Review

```{r software-review, results='asis'}
# from pkgdown https://github.com/r-lib/pkgdown/blob/1ca166905f1b019ed4af9642617ea09fa2b8fc17/R/utils.r#L176

get_description <- function(body) {
  lines <- strsplit(body, "\n")[[1]]
  name <- stringr::str_squish(sub("Package:", "", lines[grepl("^Package", lines)][1]))
  description <- stringr::str_squish(sub("Title:", "", lines[grepl("^Title", lines)][1]))
  description <- cran_unquote(sub("\\.$", "", description))
  list(name = name, description = description)
}

get_user_text <- function(issue) {
  info <- gh::gh("GET /users/{username}", username = issue$user$login)
  name <- info$name %||% issue$user$login
  url <- if (nzchar(info$blog)) info$blog else info$html_url
  if (!grepl("^https?:", url)) url <- paste0("http://", url)
  sprintf("[%s](%s)", name, url)
  
}

tidy_issue <- function(issue) {
  labels <- purrr::map_chr(issue$labels, "name")
  label <- labels[grepl("[0-9]\\/.*", labels)][1]
  df <- tibble::tibble(
    label = label,
    name = get_description(issue$body)$name,
    description = get_description(issue$body)$description,
    title = issue$title,
    holding = "holding" %in% purrr::map_chr(issue$labels, "name"),
    others = toString(purrr::map_chr(issue$labels, "name")),
    closed_at = issue$closed_at %||% NA,
    url = issue$html_url,
    user = get_user_text(issue)
  )
  
  dplyr::rowwise(df) %>%
    dplyr::mutate(text = sprintf("    * [%s](%s), %s. Submitted by %s.", name, url, description, user))
}

get_issues <- function(label, state) {
  issues <- gh::gh(
    "GET /repos/{owner}/{repo}/issues",
    owner = "ropensci",
    repo = "software-review",
    state = state, 
    labels = label
  )
  
  purrr::map_df(issues, tidy_issue)
}
  
active_issues <- purrr::map_df(
  c("1/editor-checks","2/seeking-reviewer(s)","3/reviewer(s)-assigned","4/review(s)-in-awaiting-changes","5/awaiting-reviewer(s)-response","6/approved"),
  get_issues,
  state = "open"
)

closed_issues <- get_issues(state = "closed", label  ="6/approved")

ok_date <- function(date) {
  if (is.na(date)) {
    return(TRUE)
  } 
  
  anytime::anytime(date) >= last_newsletter
}

closed_issues <- dplyr::rowwise(closed_issues) %>%
  dplyr::filter(ok_date(closed_at))

issues <- dplyr::bind_rows(active_issues, closed_issues)


no_holding <- sum(issues$holding)
issues <- dplyr::filter(issues, !holding)
text <- sprintf("There are %s recently closed and active submissions", english(nrow(issues)))
if (no_holding > 0) {
  text <- paste0(
    text,
    sprintf(
      " and %s submission%s on hold.",
      no_holding,
      if (no_holding > 1) "s" else ""
    )
  )
} else {
  text <- paste0(text, ".")
}

count_label <- function(label) {
  no <- snakecase::to_sentence_case(english(sum(issues$label == label, na.rm = TRUE)))
  url <- paste0("https://github.com/ropensci/software-review/issues?q=is%3Aissue+is%3Aopen+sort%3Aupdated-desc+label%3A", label)
  sprintf("* %s at ['%s'](%s):\n\n %s", no, label, url, paste0(issues$text[!is.na(issues$label)][ issues$label == label], collapse = "\n\n"))
}

cat(text)
cat(
  paste0(
    " Issues are at different stages: \n\n",
    paste0(
      purrr::map_chr(sort(unique(issues$label[!is.na(issues$label)]), decreasing = TRUE), count_label),
      collapse = "\n\n"
    )
  )
)
```

Find out more about [Software Peer Review](/software-review) and how to get involved.

## On the blog

<!-- Do not forget to rebase your branch! -->

```{r blog}

parse_one_post <- function(path){
  lines <- suppressWarnings(readLines(path, encoding = "UTF-8"))
  yaml <- blogdown:::split_yaml_body(lines)$yaml
  yaml <- glue::glue_collapse(yaml, sep = "\n")
  yaml <- yaml::yaml.load(yaml)
  
  meta <- tibble::tibble(
    date = anytime::anydate(yaml$date),
    author = toString(yaml$author),
    title = yaml$title,
    software_peer_review = "Software Peer Review" %in% yaml$tags,
    tech_note = "tech notes" %in% yaml$tags && !"Software Peer Review" %in% yaml$tags,
    other = !"tech notes" %in% yaml$tags && !"Software Peer Review" %in% yaml$tags,
    twitterImg = yaml$twitterImg %||% "",
    twitterAlt = yaml$twitterAlt %||% "",
    description = yaml$description %||% "",
    newsletter = "newsletter" %in% yaml$tags,
    slug = yaml$slug
    )

  meta
}
paths <- fs::dir_ls("..", recurse = TRUE, glob = "*.md")
paths <- paths[!paths %in% c("../_index.md", "../2021-02-03-targets/raw_data_source.md",
  "../2021-02-03-targets/README.md")]
posts <- purrr::map_df(paths, parse_one_post)
posts <- dplyr::filter(posts, date >= as.Date(last_newsletter), !newsletter)
posts <- split(posts, seq(nrow(posts)))
format_post <- function(post) {
  url <- sprintf(
    "/blog/%s/%s/%s/%s",
    lubridate::year(post$date),
    stringr::str_pad(lubridate::month(post$date), 2, "0", side = "left"),
    stringr::str_pad(lubridate::day(post$date), 2, "0", side = "left"),
    post$slug
    )
  string <- sprintf("* [%s](%s) by %s", post$title, url, post$author)
  if (post$description != "") {
    string <- paste0(string, ". ", sub("\\?$", "", sub("\\!$", "", sub("\\.$", "", post$description), ".")), ".")
  } else {
    string <- paste0(string, ".")  
  }
  
  if (post$twitterImg != "") {
    img_file <- fs::path_file(post$twitterImg)
    download.file(sprintf("https://ropensci.org/%s", post$twitterImg), img_file)
    img_file %>% magick::image_read() %>% magick::image_scale("400x") %>% magick::image_write(img_file)
    string <- paste0(
      string,
      sprintf('{{< figure src="%s" alt="%s" width="400" >}}\n\n', img_file, post$twitterAlt)
    )
  }
  
  string
}
```

```{r, results='asis'}
software_review <- posts[purrr::map_lgl(posts, "software_peer_review")]
if (length(software_review) > 0) {
  cat("### Software Review\n\n")
  cat(
    paste0(
      purrr::map_chr(software_review, format_post),
      collapse = "\n\n"
    )
  )
  cat("\n\n")
}

others <- posts[purrr::map_lgl(posts, "other")]
if (length(others) > 0) {
  if (length(others) != length(posts)) cat("### Other topics\n\n")
  cat(
    paste0(
      purrr::map_chr(others, format_post),
      collapse = "\n\n"
    )
  )
  cat("\n\n")
}


tech_notes <- posts[purrr::map_lgl(posts, "tech_note")]
if (length(tech_notes) > 0) {
  cat("\n\n")
  cat("### Tech Notes\n\n")
  cat(
    paste0(
      purrr::map_chr(tech_notes, format_post),
      collapse = "\n\n"
    )
  )
  cat("\n\n")
}
```

## Citations

```{r cit}
citations <- jsonlite::read_json("https://ropensci-org.github.io/ropensci_citations/citations_all_parts_clean.json")
```

Below are the citations recently added to our database of `r length(citations)` articles, that you can explore on our [citations page](/citations).
We found use of...

```{r citations, results = "asis"}
since <- lubridate::as_date(last_newsletter) - 1
commits <- gh::gh(
  "GET /repos/{owner}/{repo}/commits",
  owner = "ropensci-org",
  repo = "ropensci_citations",
  since = sprintf(
    "%s-%s-%sT00:00:00Z",
    lubridate::year(since),
    stringr::str_pad(lubridate::month(since), 2, "0", side = "left"),
    stringr::str_pad(lubridate::day(since), 2, "0", side = "left")
  )
)
old_commit <- gh::gh("/repos/{owner}/{repo}/commits/{ref}",
  owner = "ropensci-org",
  repo = "ropensci_citations",
  ref = commits[[length(commits)]]$parents[[1]]$sha
  )
old <- "https://raw.githubusercontent.com/ropensci-org/ropensci_citations/%s/citations_all_parts_clean.json" %>%
    sprintf(old_commit$sha) %>%
    jsonlite::read_json() 

new <- citations[length(old):length(citations)]

format_package <- function(package, packages = packages) {
  if (package %in% packages) {
    package <- sprintf("[**%s**](https://docs.ropensci.org/%s)", package, package)
  } else {
    package <- sprintf("**%s**", package)
  }
  package
}

format_one <- function(citation, packages) {
  packages <- toString(purrr::map_chr(citation$name, format_package, packages = packages))
  sprintf("* %s in %s\n\n", packages, citation$citation)
}

cat(
  paste0(
    sort(purrr::map_chr(new, format_one, packages = registry$package)),
    collapse = ""
  )
)

```

Thank you for citing our tools!

## Use cases

```{r usecases}
# rerun get_use_cases.R at the same time
usecases <- jsonlite::read_json("../../../data/usecases/usecases.json")
get_one_case <- function(usecase) {
  tibble::tibble(
    title = usecase$title,
    reporter = usecase$reporter,
    url = usecase$url,
    image = usecase$image,
    date = anytime::anydate(usecase$date)
  )
}
usecases <- purrr::map_df(usecases, get_one_case)
usecases <- dplyr::filter(usecases, date >= as.Date(last_newsletter))
usecases <- split(usecases, seq(nrow(usecases)))
```

`r snakecase::to_sentence_case(english(length(usecases)))` use cases of our packages and resources have been reported since we sent the last newsletter.

```{r usecases2, results='asis'}
format_case <- function(usecase) {
  string <- sprintf("* [%s](%s). Reported by %s.", usecase$title, usecase$url, usecase$reporter)
  string
}
cat(
  paste0(
    purrr::map_chr(usecases, format_case),
    collapse = "\n\n"
  )
)
```

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
