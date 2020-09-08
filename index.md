--- 
title: "Skeleton Tutorial Template"
author: "TJ McKinley"
site: bookdown::bookdown_site
output:
    bookdown::pdf_book:
        includes:
            in_header: header.tex
    bookdown::gitbook:
        config:
            sharing: null
        css: 'style.css'
        includes:
            in_header: _toggle.html
        keep_md: TRUE
linkcolor: blue
documentclass: book
link-citations: yes
description: "Skeleton Tutorial Template"
---

# Opening page




Basically a standard Bookdown template with a few tweaks. New chapters need to be in separate '.Rmd' files, where each file starts with a chapter heading as seen [here](https://bookdown.org/yihui/bookdown/usage.html). In order to use the task and solution blocks in \LaTeX, you must input the order of the files into the `_bookdown.yml` file, and the first file must be called `index.Rmd` e.g.

```
rmd_files:
    html: ['index.Rmd', 'ch1.Rmd']
    latex: ['index.Rmd', 'ch1.Rmd', 'ch_appendix.Rmd']
output_dir: "docs"
```

The `latex:` path above ***must*** have `'ch_appendix.Rmd'` as its last entry. This ensures that the appendix is properly formatted for the solutions to the problems.

You must have the following lines at the start of your `index.Rmd` file:

````
```{r, child = "_setup.Rmd", include = F, purl = F, cache = F}
```
````

There are a couple of useful special blocks. A `task` block, and a `solution` block. These can be used as e.g.

````
```{task}
Here is a task written in **markdown**.
```
````

which renders as:

<div class="panel panel-default"><div class="panel-heading"> Task </div><div class="panel-body"> 
Here is a task written in **markdown**. </div></div>

You can include chunks within the `task` chunk, but you need to use double backticks *within* the chunk, and leave carriage returns around the internal chunk e.g.

````

```{task}

``{r}
x <- 2 + 2
x
``

```

````

which renders as:

<div class="panel panel-default"><div class="panel-heading"> Task </div><div class="panel-body"> 

```r
x <- 2 + 2
x
```

```
## [1] 4
```
 </div></div>

Be careful to have suitable carriage returns around e.g. `enumerate` or `itemize` environments inside the chunk also. For example:

````

```{task}
Here is a list:
1. item 1
2. item 2
```

```` 

will not render nicely. But

````

```{task}
Here is a list:

1. item 1
2. item 2

```

```` 

will:

<div class="panel panel-default"><div class="panel-heading"> Task </div><div class="panel-body"> 
Here is a list:

1. item 1
2. item 2
 </div></div>

The `solution` chunk works in the same way, and the numbers will follow the previous `task` chunk (so you can set tasks without solutions) e.g.

````

```{task}
Add 2 and 2 together
```

```{solution}

``{r}
2 + 2
``

```

````

gives:

<div class="panel panel-default"><div class="panel-heading"> Task </div><div class="panel-body"> 
Add 2 and 2 together </div></div>

<button id="displayTextunnamed-chunk-6" onclick="javascript:toggle('unnamed-chunk-6');">Show: Solution</button>

<div id="toggleTextunnamed-chunk-6" style="display: none"><div class="panel panel-default"><div class="panel-heading panel-heading1"> Solution </div><div class="panel-body">

```r
2 + 2
```

```
## [1] 4
```
</div></div></div>

## Additional extensions

### Different task and solution titles

Task and solution boxes can also be given different names using the `title` option e.g.

````

```{task, title = "Question"}
What is the meaning of life, the universe and everything?
```

```{solution, title = "Answer"}
Why 42 of course!
```

````

gives:

<div class="panel panel-default"><div class="panel-heading"> Question </div><div class="panel-body"> 
What is the meaning of life, the universe and everything? </div></div>

<button id="displayTextunnamed-chunk-8" onclick="javascript:toggle('unnamed-chunk-8');">Show: Answer</button>

<div id="toggleTextunnamed-chunk-8" style="display: none"><div class="panel panel-default"><div class="panel-heading panel-heading1"> Answer </div><div class="panel-body">
Why 42 of course!</div></div></div>

### Turning tasks and solutions on and off

Sometimes you might want to hide task and/or solution boxes. This can be done with the `renderTask` and `renderSol` chunk options, which can be set globally or locally. For example:

````

```{task, title = "Question"}
Can I set a task and not show the answer?
```

```{solution, title = "Answer", renderSol = FALSE}
Indeed, though you won't see this answer unless `renderSol = TRUE`...
```

````

typesets as:

<div class="panel panel-default"><div class="panel-heading"> Question </div><div class="panel-body"> 
Can I set a task and not show the answer? </div></div>



### Generic information environments

You can also set generic boxed environments containing arbitrary information. 

````

```{info, title = "Some interesting titbit"}
This box contains invaluable information!
```

````

typesets as:

<infobutton id="displayTextunnamed-chunk-11" onclick="javascript:toggle('unnamed-chunk-11');">Show: Some interesting titbit</infobutton>

<div id="toggleTextunnamed-chunk-11" style="display: none"><div class="panel panel-default"><div class="panel-body">
This box contains invaluable information!</div></div></div>

Note that it is useful to set the `title` option here, else it defaults to `info`. You can also use this environment to simply display an alert box with information, by setting the `collapsible` argument to `FALSE` in the chunk options e.g.

````

```{info, title = "Some interesting aside", collapsible = FALSE}
Yet more valuable information - this time displayed directly!
```

````

typesets as:

<div class="panel panel-default"><div class="panel-heading"> Some interesting aside </div><div class="panel-body"> 
Yet more valuable information - this time displayed directly! </div></div>

In the PDF output, setting `collapsible = TRUE` will place the information boxes in a separate Appendix, with links in the main document. You can again hide the `info` boxes by setting `renderInfo = FALSE` in the chunk options.

### Tabbed boxed environments

Originally developed to put base R and `tidyverse` solutions side-by-side, using a `multCode = T` option to the solution box. Here the two tabs are separated by four consecutive hashes: `####`, and the `titles` option gives the tab titles (these can be set globally if preferred) e.g.



````

```{task}
Filter the `iris` data by `Species == "setosa"` and find the mean `Petal.Length`.
```

```{solution, multCode = T, titles = c("Base R", "tidyverse")}

``{r}
## base R solution
mean(iris$Petal.Length[
    iris$Species == "setosa"])
``

####

``{r}
## tidyverse solution
iris %>% 
    filter(Species == "setosa") %>%
    select(Petal.Length) %>%
    summarise(mean = mean(Petal.Length))
``
    
```

````

will typeset to:

<div class="panel panel-default"><div class="panel-heading"> Task </div><div class="panel-body"> 
Filter the `iris` data by `Species == "setosa"` and find the mean `Petal.Length`. </div></div>

<button id="displayTextunnamed-chunk-15" onclick="javascript:toggle('unnamed-chunk-15');">Show: Solution</button>

<div id="toggleTextunnamed-chunk-15" style="display: none"><div class="panel panel-default"><div class="panel-heading panel-heading1"> Solution </div><div class="panel-body"><div class="tab"><button class="tablinksunnamed-chunk-15 active" onclick="javascript:openCode(event, 'option1unnamed-chunk-15', 'unnamed-chunk-15');">Base R</button><button class="tablinksunnamed-chunk-15" onclick="javascript:openCode(event, 'option2unnamed-chunk-15', 'unnamed-chunk-15');">tidyverse</button></div><div id="option1unnamed-chunk-15" class="tabcontentunnamed-chunk-15">

```r
## base R solution
mean(iris$Petal.Length[
    iris$Species == "setosa"])
```

```
## [1] 1.462
```
</div><div id="option2unnamed-chunk-15" class="tabcontentunnamed-chunk-15">

```r
## tidyverse solution
iris %>% 
    filter(Species == "setosa") %>%
    select(Petal.Length) %>%
    summarise(mean = mean(Petal.Length))
```

```
##    mean
## 1 1.462
```
    
</div><script> javascript:hide('option2unnamed-chunk-15') </script></div></div></div>

Note that there is also a `multCode` chunk that does not link to task and solution boxes e.g.

````

```{multCode}

Two options: 

* Option 1

####

Two options:
    
* Option 2

```

````

will typeset to:

<div class="tab"><button class="tablinksunnamed-chunk-16 active" onclick="javascript:openCode(event, 'option1unnamed-chunk-16', 'unnamed-chunk-16');">Option 1</button><button class="tablinksunnamed-chunk-16" onclick="javascript:openCode(event, 'option2unnamed-chunk-16', 'unnamed-chunk-16');">Option 2</button></div><div id="option1unnamed-chunk-16" class="tabcontentunnamed-chunk-16">

Two options: 

* Option 1
</div><div id="option2unnamed-chunk-16" class="tabcontentunnamed-chunk-16">

Two options:
    
* Option 2
</div><script> javascript:hide('option2unnamed-chunk-16') </script>

The `titles` option can be set as before.

<!--chapter:end:index.Rmd-->

