
[![Project Status: Active – The project has reached a stable, usable
state and is being actively
developed.](https://www.repostatus.org/badges/latest/active.svg)](https://www.repostatus.org/#active)
[![Signed
by](https://img.shields.io/badge/Keybase-Verified-brightgreen.svg)](https://keybase.io/hrbrmstr)
![Signed commit
%](https://img.shields.io/badge/Signed_Commits-100%25-lightgrey.svg)
[![Linux build
Status](https://travis-ci.org/hrbrmstr/phcssjars.svg?branch=master)](https://travis-ci.org/hrbrmstr/phcssjars)  
![Minimal R
Version](https://img.shields.io/badge/R%3E%3D-3.2.0-blue.svg)
![License](https://img.shields.io/badge/License-Apache-blue.svg)

# phcssjars

Java Archive Wrapper Supporting the ‘phcss’ Package

## Description

Contains all supporting JARs for working with the ‘ph-css’ Java library
(<https://github.com/phax/ph-css>). Version number matches version
number of included ‘ph-css’ library.

## Installation

``` r
remotes::install_gitlab("hrbrmstr/phcssjars")
```

NOTE: To use the ‘remotes’ install options you will need to have the
[{remotes} package](https://github.com/r-lib/remotes) installed.

## Usage

``` r
library(rJava)
library(phcssjars)

packageVersion("phcssjars")
## [1] '6.2.0'
```

Turn a CSS file into a data frame (some bits TBD):

``` r
library(tidyverse)

ECSSVersion <- J("com.helger.css.ECSSVersion")
CSSReader <- J("com.helger.css.reader.CSSReader")

css_fil <- paste0(readLines("https://www.r-project.org/css/R.css"), collapse="\n")

css <- CSSReader$readFromString(css_fil, ECSSVersion$CSS30)

lapply(css$getAllRules(), function(.x) {
  
  if (.jinherits(.x, "com.helger.css.decl.CSSStyleRule")) {
  
    tibble(
      selectors = sapply(.x$getAllSelectors(), function(.y) .y$getAsCSSString()),
      declarations = list(lapply(.x$getAllDeclarations(), function(.y) {
        property <- .y$getProperty()
        try(
          sapply(.y$getExpression()$getAllSimpleMembers(), function(.z) .z$getOptimizedValue()), 
          silent = TRUE
        ) -> expressions
        if (inherits(expressions, "try-error")) expressions <- NA_character_
        tibble(
          property = property,
          value = expressions
        )
      }))
    )
    
  } else if (.jinherits(.x, "com.helger.css.decl.CSSFontFaceRule")) {
    message("Font Face Rules TBD")
  } else if (.jinherits(.x, "com.helger.css.decl.CSSMediaRule")) {
    message("Media Rules TBD")
  } else if (.jinherits(.x, "com.helger.css.decl.CSSKeyframesRule")) {
    message("Keyframes Rules TBD")
  } else if (.jinherits(.x, "com.helger.css.decl.CSSViewportRule")) {
    message("Viewport Rules TBD")
  }  
  
}) %>% 
  bind_rows() %>% 
  unnest() %>% 
  unnest() -> res

res
```

<div class="kable-table">

| selectors        | property            | value    |
| :--------------- | :------------------ | :------- |
| h1               | font-weight         | bold     |
| h1               | margin-top          | 0        |
| h1               | margin-bottom       | 0.8em    |
| h1               | font-size           | 2.5em    |
| h2               | font-size           | 1.8em    |
| h2               | color               | \#222    |
| h3               | font-size           | 1.1em    |
| h3               | font-weight         | bold     |
| body             | color               | \#444    |
| a:link           | color               | \#3864BA |
| .page            | margin-top          | 2em      |
| .sidebar         | margin-bottom       | 3em      |
| .sidebar h2      | font-size           | 1em      |
| .sidebar h2      | font-weight         | bold     |
| .sidebar ul      | list-style          | none     |
| .sidebar ul      | padding-left        | 0        |
| .sidebar li      | margin              | 0        |
| ul               | padding-left        | 1.5em    |
| li               | margin              | 0.5em    |
| li               | margin              | 0        |
| blockquote       | font-size           | 1em      |
| blockquote       | padding-top         | 0        |
| blockquote       | padding-bottom      | 0        |
| blockquote       | padding-left        | 1em      |
| table small      | color               | \#666    |
| .footer          | border-top          | 2px      |
| .footer          | border-top          | solid    |
| .footer          | border-top          | \#ddd    |
| .footer          | background-color    | \#fafafa |
| .footer          | margin-top          | 3em      |
| .footer          | padding             | 1em      |
| .footer          | padding             | 0.5em    |
| .footer          | padding             | 1.5em    |
| .footer          | padding             | 0.5em    |
| .footer          | font-size           | 90%      |
| .footer          | color               | \#666    |
| \#rtable li span | padding             | 5px      |
| \#rtable li span | padding             | 3px      |
| .selected span   | background-color    | \#fff    |
| .selected span a | font-weight         | bold     |
| \#mirror         | min-height          | 1em      |
| \#mirror select  | line-height         | 1        |
| \#mirror select  | background-color    | white    |
| \#mirror select  | border              | 1px      |
| \#mirror select  | border              | solid    |
| \#mirror select  | border              | \#ccc    |
| \#rtable li      | margin              | 0.8em    |
| \#rtable li      | margin              | 0        |
| .downloads       | min-height          | 20px     |
| .downloads       | padding             | 10px     |
| .downloads       | margin-bottom       | 1em      |
| .downloads       | background-color    | \#f5f5f5 |
| .downloads       | border              | 1px      |
| .downloads       | border              | solid    |
| .downloads       | border              | \#e3e3e3 |
| .downloads       | border-radius       | 4px      |
| .downloads       | \-webkit-box-shadow | inset    |
| .downloads       | \-webkit-box-shadow | 0        |
| .downloads       | \-webkit-box-shadow | 1px      |
| .downloads       | \-webkit-box-shadow | 1px      |
| .downloads       | box-shadow          | inset    |
| .downloads       | box-shadow          | 0        |
| .downloads       | box-shadow          | 1px      |
| .downloads       | box-shadow          | 1px      |

</div>

## `phcssjars` Metrics

| Lang  | \# Files |  (%) | LoC |  (%) | Blank lines |  (%) | \# Lines |  (%) |
| :---- | -------: | ---: | --: | ---: | ----------: | ---: | -------: | ---: |
| Rmd   |        1 | 0.12 |  44 | 0.53 |          20 | 0.77 |       23 | 0.46 |
| Maven |        1 | 0.12 |  17 | 0.20 |           2 | 0.08 |        1 | 0.02 |
| R     |        5 | 0.62 |  15 | 0.18 |           1 | 0.04 |       26 | 0.52 |
| make  |        1 | 0.12 |   7 | 0.08 |           3 | 0.12 |        0 | 0.00 |
