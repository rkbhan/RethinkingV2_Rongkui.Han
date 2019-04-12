---
title: "Statistical Rethinking 4.12"
author: "Rongkui Han"
date: "4/12/2019"
output: 
  html_document: 
    keep_md: yes
---

2E1. Which of the expressions below correspond to the statement: *the probability of rain on Monday*?  
(1) Pr(rain)  
(2) Pr(rain | Monday)   
(3) Pr(Monday | rain)   
(4) Pr(rain, Monday) / Pr(Monday)     

> \(2) and (4)

2E2. Which of the following statements corresponds to the expression: Pr(Monday | rain)?  
(1) The probability of rain on Monday.  
(2) The probability of rain, given that it is Monday.  
(3) The probability that it is Monday, given that it is raining.  
(4) The probability that it is Monday and that it is raining.  

> \(3)

2E3. Which of the expressions below correspond to the statement: *the probability that it is Monday, given that it is raining*?   
(1) Pr(Monday | rain)   
(2) Pr(rain | Monday)   
(3) Pr(rain | Monday)Pr(Monday)   
(4) Pr(rain | Monday)Pr(Monday) / Pr(rain)   
(5) Pr(Monday | rain)Pr(rain) / Pr(Monday)   

> \(1) and (4)

2M1. Recall the globe tossing model from the chapter. Compute and plot the grid approximate posterior distribution for each of the following sets of observations. In each case, assume a uniform prior for p.    

(1) W, W, W     

```r
# define grid
p_grid = seq(from=0 , to=1 , length.out=1000)
# define prior
prior = rep(1 , 1000)
# compute likelihood at each value in grid
likelihood1 = dbinom(3 , size = 3, prob=p_grid)
# compute product of likelihood and prior
unstd.posterior1 = likelihood1 * prior
# standardize the posterior, so it sums to 1
posterior1 = unstd.posterior1 / sum(unstd.posterior1)

plot(p_grid, posterior1, type="b", xlab="probability of water" , ylab="posterior probability")
mtext( "2M1(1), 1000 points" )
```

![](Rongkui_Chap2_HW_files/figure-html/unnamed-chunk-1-1.png)<!-- -->


(2) W, W, W, L    

```r
# compute likelihood at each value in grid
likelihood2 = dbinom(3 , size = 4, prob=p_grid)
# compute product of likelihood and prior
unstd.posterior2 = likelihood2 * prior
# standardize the posterior, so it sums to 1
posterior2 = unstd.posterior2 / sum(unstd.posterior2)

plot(p_grid, posterior2, type="b", xlab="probability of water" , ylab="posterior probability")
mtext( "2M1(2), 1000 points" )
```

![](Rongkui_Chap2_HW_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

(3) L, W, W, L, W, W, W    

```r
likelihood3 = dbinom(5 , size = 7, prob=p_grid)
# compute product of likelihood and prior
unstd.posterior3 = likelihood3 * prior
# standardize the posterior, so it sums to 1
posterior3 = unstd.posterior3 / sum(unstd.posterior3)

plot(p_grid, posterior3, type="b", xlab="probability of water" , ylab="posterior probability")
mtext( "2M1(3), 1000 points" )
```

![](Rongkui_Chap2_HW_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

2M2. Now assume a prior for p that is equal to zero when p < 0.5 and is a positive constant when p = 0.5. Again compute and plot the grid approximate posterior distribution for each of the sets of observations in the problem just above.   

(1) W, W, W

```r
# define prior
prior2 = c(rep(0 , 500), rep(2, 500))
# compute product of likelihood and prior
unstd.posterior2_1 = likelihood1 * prior2
# standardize the posterior, so it sums to 1
posterior2_1 = unstd.posterior2_1 / sum(unstd.posterior2_1)

plot(p_grid, posterior2_1, type="b", xlab="probability of water" , ylab="posterior probability")
mtext( "2M2(1), 1000 points" )
```

![](Rongkui_Chap2_HW_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

(2) W, W, W, L     

```r
# compute product of likelihood and prior
unstd.posterior2_2 = likelihood2 * prior2
# standardize the posterior, so it sums to 1
posterior2_2 = unstd.posterior2_2 / sum(unstd.posterior2_2)

plot(p_grid, posterior2_2, type="b", xlab="probability of water" , ylab="posterior probability")
mtext( "2M2(2), 1000 points" )
```

![](Rongkui_Chap2_HW_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

(3) L, W, W, L, W, W, W     

```r
# compute product of likelihood and prior
unstd.posterior2_3 = likelihood3 * prior2
# standardize the posterior, so it sums to 1
posterior2_3 = unstd.posterior2_3 / sum(unstd.posterior2_3)

plot(p_grid, posterior2_3, type="b", xlab="probability of water" , ylab="posterior probability")
mtext( "2M2(3), 1000 points" )
```

![](Rongkui_Chap2_HW_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

2M3. Suppose there are two globes, one for Earth and one for Mars. The Earth globe is 70% covered in water. The Mars globe is 100% land. Further suppose that one of these globes—you don’t know which—was tossed in the air and produced a “land” observation. Assume that each globe was equally likely to be tossed. Show that the posterior probability that the globe was the Earth, conditional on seeing “land” (Pr(Earth|land)), is 0.23.   

$Pr(Earth|land)$ = $\frac{Pr(Earth \cap land)}{Pr(land)}$   
= $\frac{Pr(Earth\cap land)}{Pr(Earth\cap land) + Pr(Mars\cap land)}$   
= $\frac{Pr(land|Earth)Pr(Earth)}{Pr(land|Earth)Pr(Earth) + Pr(land|Mars)Pr(Mars)}$   
= $\frac{0.3 \times 0.5}{0.3 \times 0.5 + 1 \times 0.5}$  
= $\frac{0.15}{0.65}$    
= 0.2307692     

2M4. Suppose you have a deck with only three cards. Each card has two sides, and each side is either black or white. One card has two black sides. The second card has one black and one white side. The third card has two white sides. Now suppose all three cards are placed in a bag and shuffled. Someone reaches into the bag and pulls out a card and places it flat on a table. A black side is shown facing up, but you don’t know the color of the side facing down. Show that the probability that the other side is
also black is 2/3. Use the counting method (Section 2 of the chapter) to approach this problem. This means counting up the ways that each card could produce the observed data (a black side facing up on the table).     

> {All possible outcome} = {Card 1 side A (white) up; Card 1 side B (white) up; Card 2 side A (white) up; Card 2 side B (black) up; Card 3 side A (black) up; Card 3 side B (black) up}     
Therefore, ||{All possible outcome}|| = 6;    
     
> {Black side up} = {Card 2 side B (black) up; Card 3 side A (black) up; Card 3 side B (black) up}   
> Therefore, ||{Black side up}|| = 3;    
> Within the outcome space {Black side up}, 2 events, {Card 3 side A (black) up; Card 3 side B (black) up}, fall into set {Black side down}.   
> Therefore, Pr(Black side down|Black side up) = 2/3.    

2M5. Now suppose there are four cards: B/B, B/W, W/W, and another B/B. Again suppose a card is drawn from the bag and a black side appears face up. Again calculate the probability that the other side is black.   

> {Black side up} = {Card 2 side B (black) up; Card 3 side A (black) up; Card 3 side B (black) up; Card 4 side A (black) up; Card 4 side B (black) up}   
> Therefore, ||{Black side up}|| = 5;    
> Within the outcome space {Black side up}, 4 events, {Card 3 side A (black) up; Card 3 side B (black) up; Card 4 side A (black) up; Card 4 side B (black) up}, fall into set {Black side down}.   
> Therefore, Pr(Black side down|Black side up) = 4/5.    
