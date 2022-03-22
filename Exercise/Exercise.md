---
title: "Exercise RMarkdown and reproductibility course"
author: "The EDB RMarkdown superstar learning team"
date: "01/03/2022"
output: 
  pdf_document:
    number_sections: true
    toc: true
    toc_depth: 1
    highlight: zenburn
    keep_md: TRUE
urlcolor: blue
bibliography: bibliography.bib
---



# Introduction

This exercise is the best way to practice `RMarkdown` [@salinas2020r] now that you have had an overview of how it works. The principle is that you want to produce a copy of this `pdf` file. We provide most of the `R code`, because the scope is to work on the `RMarkdown` [@xie2018r] integration, not on the `R code`. 

# Starters

You want to create a new `Rmarkdown` file and save it in the `Exercise` folder.

# Load packages

What packages will I need? Well I can fill that in later anyway.

BTW: To add a new code chunk the keyboard shortcut is `Ctrl` + `Alt` + `i`.
Or use the insert button on the top left side of this window.



# Research compendium

Let's organize our project. We create a folder called `data` and list the files in the current working folder.


```r
system("mkdir -p data")
list.files(path = ".")
```

```
## [1] "bibliography.bib" "data"             "Exercise_files"   "Exercise.pdf"    
## [5] "Exercise.Rmd"
```

```r
list.dirs(path = ".", full.names = TRUE, recursive = TRUE)
```

```
## [1] "."                             "./data"                       
## [3] "./Exercise_files"              "./Exercise_files/figure-latex"
```

We can now copy the `datasets.xlsx` file (from `data/` in the main folder to the `Exercise/data/` folder).


```r
file.copy("../data/datasets.xlsx", "data")
```

If you're running an \textcolor{red}{Unix system}, you can also use a bash command (`mv ../data/datasets.xlsx data/`). This is the occasion to try to include a code chunk with another language (i.e. bash)!

# Load the project data

We'll use the `mtcars` dataset that we'll load from the `datasets.xlsx` file.
Here we do not provide the code because it is part of the exercise. 




# Analyze the data

## Get a look at the data


```r
my_mtcars2 <- within(my_mtcars, {
   vs <- factor(vs, labels = c("V", "S"))
   am <- factor(am, labels = c("automatic", "manual"))
   cyl  <- ordered(cyl)
   gear <- ordered(gear)
   carb <- ordered(carb)
})
summary(my_mtcars2)
```

```
##       mpg        cyl         disp             hp             drat      
##  Min.   :10.40   4:11   Min.   : 71.1   Min.   : 52.0   Min.   :2.760  
##  1st Qu.:15.43   6: 7   1st Qu.:120.8   1st Qu.: 96.5   1st Qu.:3.080  
##  Median :19.20   8:14   Median :196.3   Median :123.0   Median :3.695  
##  Mean   :20.09          Mean   :230.7   Mean   :146.7   Mean   :3.597  
##  3rd Qu.:22.80          3rd Qu.:326.0   3rd Qu.:180.0   3rd Qu.:3.920  
##  Max.   :33.90          Max.   :472.0   Max.   :335.0   Max.   :4.930  
##        wt             qsec       vs             am     gear   carb  
##  Min.   :1.513   Min.   :14.50   V:18   automatic:19   3:15   1: 7  
##  1st Qu.:2.581   1st Qu.:16.89   S:14   manual   :13   4:12   2:10  
##  Median :3.325   Median :17.71                         5: 5   3: 3  
##  Mean   :3.217   Mean   :17.85                                4:10  
##  3rd Qu.:3.610   3rd Qu.:18.90                                6: 1  
##  Max.   :5.424   Max.   :22.90                                8: 1
```

## First analysis, fuel consumption

I want to assess the motors consumption as a function of the displacement.
Engine displacement is the measure of the cylinder volume swept by all of the pistons of a piston engine, excluding the combustion chambers.


```r
plot(mpg ~ disp, 
     data = my_mtcars,
     xlab="Displacement (cu.in.)",
     ylab="Fuel consumption Miles/gallon", 
     ylim=c(10,40))
arrows(x0 = my_mtcars$disp, 
       y0 = my_mtcars$mpg * 0.95,
       x1 = my_mtcars$disp, 
       y1 = my_mtcars$mpg * 1.05, 
       angle = 90, 
       code = 3, 
       length = 0.04, 
       lwd = 0.4, 
       col="red")
```



\begin{center}\includegraphics{Exercise_files/figure-latex/mpg_disp-1} \end{center}

Whoo, that was **interesting**. Can we concldude that biggers motors consume more fuel?

Let's move on to the next analysis.

## Second analysis, is that all?

To assess whether other variables could also explain the consumption I will plot the correlation among each pair of variables in my data.


```r
mcor <- cor(my_mtcars)
corrplot(mcor)
```



\begin{center}\includegraphics{Exercise_files/figure-latex/mtcorplot-1} \end{center}

Whoohoohooo, that plot looks great!

It looks like the **number of cylinders** (cyl), **Gross horsepower** (hp) and the **Weight** (wt. 1000 lbs), could play a role too.

If you are looking for the the variables meaning, you can find it [here](https://www.rdocumentation.org/packages/datasets/versions/3.6.2/topics/mtcars).


## Third analysis, we like camembert, they like pie!

Let's produce a pie chart showing the proportion of cars that have different carb values.


```r
#concat the names for the legend
names = as.factor(paste(unique(my_mtcars$carb),"carbs"))
#get the percentages
percent=100*table(my_mtcars$carb)/length(my_mtcars$carb)
pie(x=percent, label=paste(percent, "%"),  col=rainbow(length(names)), 
    main="Percentage of cars per number of carburators" ) 
#add legend
legend("right",legend = names, fill = rainbow(length(names)), cex=0.8)
```

\begin{figure}

\hfill{}\includegraphics{Exercise_files/figure-latex/ho_mae-1} 

\caption{have you ever tried the camenbert pie?}\label{fig:ho_mae}
\end{figure}

# Let's make sure that my figures have been saved!


```r
list.dirs(path = ".", full.names = TRUE, recursive = TRUE)
```

```
## [1] "."                             "./data"                       
## [3] "./Exercise_files"              "./Exercise_files/figure-latex"
```

```r
list.files(path = "./Exercise_files/figure-html/")
```

```
## character(0)
```


# Print a nice table of the `mtcars` dataset

Try to reproduce this table with `kable()` and the `kableExtra` functions.

\begin{table}

\caption{\label{tab:a table}a wonderful table}
\centering
\begin{tabular}[t]{>{}rrrr>{}rrrrrrr}
\toprule
mpg & cyl & disp & hp & drat & wt & qsec & vs & am & gear & carb\\
\midrule
\textcolor{pink}{\cellcolor{gray!6}{21.0}} & \cellcolor{gray!6}{6} & \cellcolor{gray!6}{160} & \cellcolor{gray!6}{110} & \sout{\cellcolor{gray!6}{3.90}} & \cellcolor{gray!6}{2.620} & \cellcolor{gray!6}{16.46} & \cellcolor{gray!6}{0} & \cellcolor{gray!6}{1} & \cellcolor{gray!6}{4} & \cellcolor{gray!6}{4}\\
\cellcolor{black}{\textcolor{white}{21.0}} & \cellcolor{black}{\textcolor{white}{6}} & \cellcolor{black}{\textcolor{white}{160}} & \cellcolor{black}{\textcolor{white}{110}} & \cellcolor{black}{\textcolor{white}{\sout{3.90}}} & \cellcolor{black}{\textcolor{white}{2.875}} & \cellcolor{black}{\textcolor{white}{17.02}} & \cellcolor{black}{\textcolor{white}{0}} & \cellcolor{black}{\textcolor{white}{1}} & \cellcolor{black}{\textcolor{white}{4}} & \cellcolor{black}{\textcolor{white}{4}}\\
\textcolor{pink}{\cellcolor{gray!6}{22.8}} & \cellcolor{gray!6}{4} & \cellcolor{gray!6}{108} & \cellcolor{gray!6}{93} & \sout{\cellcolor{gray!6}{3.85}} & \cellcolor{gray!6}{2.320} & \cellcolor{gray!6}{18.61} & \cellcolor{gray!6}{1} & \cellcolor{gray!6}{1} & \cellcolor{gray!6}{4} & \cellcolor{gray!6}{1}\\
\textcolor{pink}{21.4} & 6 & 258 & 110 & \sout{3.08} & 3.215 & 19.44 & 1 & 0 & 3 & 1\\
\textcolor{pink}{\cellcolor{gray!6}{18.7}} & \cellcolor{gray!6}{8} & \cellcolor{gray!6}{360} & \cellcolor{gray!6}{175} & \sout{\cellcolor{gray!6}{3.15}} & \cellcolor{gray!6}{3.440} & \cellcolor{gray!6}{17.02} & \cellcolor{gray!6}{0} & \cellcolor{gray!6}{0} & \cellcolor{gray!6}{3} & \cellcolor{gray!6}{2}\\
\addlinespace
\textcolor{pink}{18.1} & 6 & 225 & 105 & \sout{2.76} & 3.460 & 20.22 & 1 & 0 & 3 & 1\\
\bottomrule
\end{tabular}
\end{table}

# Insert an image

Insert the image of a cute kitten here :)
Limit the width to 200px and center the picture!


```r
knitr::include_graphics("data/kitten-wallpaper-android.jpeg")
```

\begin{figure}

{\centering \includegraphics[width=200px]{data/kitten-wallpaper-android} 

}

\caption{A cute kitten}\label{fig:cute kitten}
\end{figure}

```r
# this is another way to include external images with knitr - it allows you to use 
# the code chunk parameters to place/resize/etc the image
# source of the image: https://fr.phoneky.com/android/?id=d1d50935#gsc.tab=0
```


# Bibliography
