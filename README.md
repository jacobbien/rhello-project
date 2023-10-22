Creating the `rhello` R package
================
Your Name
The Date

## How I created this repo

1.  I created a new empty repo named `rhello-project` in GitHub and ran
    the following from an R console:

``` r
rmarkdown::draft("README.Rmd",
                 template = "make-an-r-package",
                 package = "litr")
```

2.  At the top of `README.Rmd`, I changed
    `output: litr::litr_html_document` to `output: github_document`.

3.  I pressed “Knit” in RStudio. (Alternatively could have run
    `litr::render("README.Rmd")` from the R console.). This generated
    `rhello` and `README.md`. \[And of course I then added this section
    describing what I did!\]

## Package setup

We start by specifying the information needed in the DESCRIPTION file of
the R package.

``` r
usethis::create_package(
  path = ".",
  fields = list(
    Package = params$package_name,
    Version = "0.0.0.9000",
    Title = "A Package That Says Hello",
    Description = "This package says hello.  But its actual purpose is to show how an R package can be completely coded in a single R markdown file.",
    `Authors@R` = person(
      given = "First",
      family = "Last",
      email = "you@gmail.com",
      role = c("aut", "cre")
      )
  )
)
usethis::use_mit_license(copyright_holder = "F. Last")
```

## Now to the package itself

### Define a function

Let’s define a function for our R package:

``` r
#' Say hello to someone
#' 
#' @param name Name of a person
#' @param exclamation Whether to include an exclamation mark
#' @export 
say_hello <- function(name, exclamation = TRUE) {
  paste0("Hello ", name, ifelse(exclamation, "!", "."))
}
```

Code chunks whose first line starts with `#'` are added to the package.

We can try running it.

``` r
say_hello("Jacob")
```

    ## [1] "Hello Jacob!"

That code chunk does not start with `#'`, so it is not added to the
package.

Let’s write some tests to make sure the function behaves as desired:

``` r
testthat::test_that("say_hello works", {
  testthat::expect_equal(say_hello("Jacob"), "Hello Jacob!")
  testthat::expect_equal(say_hello("Jacob", exclamation = FALSE), "Hello Jacob.")
})
```

    ## Test passed

Code chunks that have one or more lines starting with `test_that(` (or
`testthat::test_that(`) are added to the package as tests.

## Documenting the package and building

We finish by running commands that will document, build, and install the
package. It may also be a good idea to check the package from within
this file.

``` r
litr::document() # <-- use instead of devtools::document()
# devtools::build()
# devtools::install()
# devtools::check(document = FALSE)
```

    ## ℹ Updating rhello documentation
    ## ℹ Loading rhello
    ## Writing 'NAMESPACE'
    ## Writing 'say_hello.Rd'
