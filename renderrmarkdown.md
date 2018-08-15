---
title: How to automatically generate multiple R markdown reports
---

Just discovered a super simple way to automatically render multiple R markdown reports. A lot of times in my work, I do need to generate reports which differ very little, except for a few key paramaters. Instead of manually filtering and rendering reports, I've started runnning this code instead.

The set up is really simple. First, write the R markdown file. At the head of the document, ensure that you have a filtering variable set up. In the example below, I've written a short R Markdown file to output the mean of `Sepal.Length` and its corresponding `Species`, from the `iris` dataset. You can see I've read in the data as 'iris', and set it to filter according to Speices.

```

```{r, echo=FALSE} 
library(knitr)
library(markdown)
library(rmarkdown)

iris <- iris %>%
filter(Speices==i)

The mean of the Sepal.Length for `r unique(Species)` is `r mean(Sepal.Length)`.
```

Save this as a R markdown file. In this case I've just saved it as `Iris.Rmd`.

Next, you write a separate script to call the file, filter it, and save them uniquely, like so:

```
library(knitr)
library(markdown)
library(rmarkdown)

for (i in unique(iris$Species)){
  rmarkdown::render('/YourFilePath/Iris.Rmd', 
                    output_format = "word_document",
                    output_file =  paste("report_", i, ".docx", sep=''), 
                    output_dir = '/Youroutputdrive')
}
```

That's it! Super simple, no? 

