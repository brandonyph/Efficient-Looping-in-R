# What is a loop?

Doing something over and over again

1.  FOR

for (value in that) { this }

2.  WHILE

    while (condition) { code }

``` r
for(i in 1:5){
  print(i) 
}
```

    ## [1] 1
    ## [1] 2
    ## [1] 3
    ## [1] 4
    ## [1] 5

``` r
j <- 0
while(j<5){
  print(j)
  j <- j +1 #not entering information for j could results in an infinte loop
}
```

    ## [1] 0
    ## [1] 1
    ## [1] 2
    ## [1] 3
    ## [1] 4

``` r
# I like to use FOR more than While
```

2.  Using list in loops

``` r
list1 <- c("Hello","How","Do","You","Do","Today")

for(i in list1){
  print(i)
  
}
```

    ## [1] "Hello"
    ## [1] "How"
    ## [1] "Do"
    ## [1] "You"
    ## [1] "Do"
    ## [1] "Today"

3.  Interrupting Loops and Skipping Elements

``` r
# Break
for(i in 1:5){
  if(i ==2){break}
  print(i)
}
```

    ## [1] 1

``` r
for(i in list1){
  if(i =="Do"){break}
  print(i)
}
```

    ## [1] "Hello"
    ## [1] "How"

``` r
#Next
for(i in 1:5){
  if(i ==2){next}
  print(i)
}
```

    ## [1] 1
    ## [1] 3
    ## [1] 4
    ## [1] 5

``` r
for(i in list1){
  if(i =="Hello"){next}
  print(i)
}
```

    ## [1] "How"
    ## [1] "Do"
    ## [1] "You"
    ## [1] "Do"
    ## [1] "Today"

3.  Efficient Looping?

-   Don’t make objects you dont use
-   Garbage collections
-   Initialize objects before loops
-   Use simple data-types
-   Declare Size of output prior to loops

``` r
#   Don't make objects you dont use

for(i in 1:5){
  P1 <- i
  P2 <- 25
  P3 <- P1 + P2
  print(P3) 
  #P1 and P2 might make the code easier to understand but do no need to exsist in this loops
  
}
```

    ## [1] 26
    ## [1] 27
    ## [1] 28
    ## [1] 29
    ## [1] 30

``` r
#do this instead
for(i in 1:5){
  P3 <-i + 25
  print(P3) 
  #P1 and P2 
  
}
```

    ## [1] 26
    ## [1] 27
    ## [1] 28
    ## [1] 29
    ## [1] 30

``` r
# Garbage collections


for(i in 1:5){
  P1 <- i
  P2 <- 25
  P3 <- P1 + P2
  print(P3) 
  # If P1 and P2 is really needed, you can remove them after the looping calculation to save memory
  # might slow down the overall calculation
  rm(P1)
  rm(P2)
  rm(P3)
}
```

    ## [1] 26
    ## [1] 27
    ## [1] 28
    ## [1] 29
    ## [1] 30

``` r
# -   Initialize objects before loops

P2 <- 25 # Declare the object before the loop to prevent multiple declarations

for(i in 1:5){
  P1 <- i
  P3 <- P1 + P2
  print(P3) 
  rm(P1)
  rm(P3)
}
```

    ## [1] 26
    ## [1] 27
    ## [1] 28
    ## [1] 29
    ## [1] 30

``` r
# -   Use simple data-types
cycle <- 1:10000

# Using a dataframe
P2 <- data.frame(25) 
system.time(
  for(i in cycle){
    P1 <- i
    P3 <- P1 + P2
  }
)
```

    ##    user  system elapsed 
    ##    2.86    0.08    3.36

``` r
# Using a dataframe
P2 <- data.frame(25) # Remove P1 declaration
system.time(
  for(i in cycle){
    P3 <- i + P2
  }
)
```

    ##    user  system elapsed 
    ##    2.72    0.02    3.11

``` r
#Use Numerical 
P2 <- 25 # Declare the object before the loop to prevent multiple declarations
system.time(
  for(i in cycle){
    P3 <- i + P2
  }
)
```

    ##    user  system elapsed 
    ##       0       0       0

``` r
# Declare Size of output prior to loops
cycle <- 1:1000000
P2 <- 25 # Declare the object before the loop to prevent multiple declarations
rm(P3)

P3 <- list()
system.time(
  for(i in cycle){
    P3[i] <- i/ P2
  }
)
```

    ##    user  system elapsed 
    ##    2.71    0.14    3.91

``` r
P3 <- seq(1,length(cycle))
system.time(
  for(i in cycle){
    P3[i] <- i/ P2
  }
)
```

    ##    user  system elapsed 
    ##    0.16    0.03    0.39

# In Summary, do as little as possible inside the loop

-   Calculate results before the loop
-   initialize objects before the loop
-   Iterate on as few numbers as possible
-   Write as little functions inside a loop as possible

4.  Parallel Computing

-   Change the cycle number to understand the effects of multiple
    computing

``` r
cycle <- 1:10000

system.time(
  for(i in cycle)( 
    P1 <- sqrt(i))
)
```

    ##    user  system elapsed 
    ##    0.04    0.00    0.03

``` r
library(doSNOW)
```

    ## Loading required package: foreach

    ## Loading required package: iterators

    ## Loading required package: snow

``` r
cl <- makeCluster(2, type="SOCK") # 4 – number of cores

# Using your custom defined function: "myCustomFunc()" and store in 'output' variable # Example 4
myCustomFunc <- function(i){sqrt(i)}

system.time(
  foreach(i = cycle, .combine = "c") %dopar% {
    myCustomFunc(i)
  }
)
```

    ## Warning: executing %dopar% sequentially: no parallel backend registered

    ##    user  system elapsed 
    ##    2.82    0.04    3.10

``` r
stopCluster(cl)
```
