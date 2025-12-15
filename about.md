---
layout: page
title: Results
---

## Matching vs Filling-In

Comparing the Matching and Filling-In for both distributions, it is clear that the Matching process is more quicker and efficient. As the number of parties increases, the average wait time remains lower for the Matching process compared to the Filling-In process.

```
Matching Queue with Uniform Party Sizes         Filling-In Queue with Uniform Party Sizes
 Number of Parties  Average Wait Time              Number of Parties  Average Wait Time
                10               1.55                             10               1.51
                20               4.66                             20               5.24
                50               5.51                             50               6.08
               100              12.71                            100              12.83
               200              11.51                            200              22.81
               500              14.51                            500              23.07
              1000              28.67                           1000              53.48

Matching Queue with Geometric Party Sizes        Filling-In Queue with Geometric Party Sizes
 Number of Parties  Average Wait Time              Number of Parties  Average Wait Time
                10               4.22                             10               4.60
                20               3.13                             20               4.33
                50               7.46                             50              12.12
               100               6.16                            100               9.60
               200               5.14                            200               8.29
               500               5.04                            500               9.17
              1000               4.96                           1000               8.45
```

## Uniform vs Geometric

Before comparing the two distributions, it is imporant to note key differences that affect their outcomes. The Uniform Distribution gives the same probability of choosing any party size. Therefore, the amounts of the different party sizes are close to each other in value. The Geometric Distribution uses a fixed probability of success, resulting in values that are more skewed toward the smaller party sizes. 

When comparing the Uniform arrivals to the Geometric arrivals, the arrivals using the Geometric Distribution produce the most efficient and effective process. I compared the Uniform Matching Queue to the Geometric Matching Queue and the Uniform Filling-In Queue to the Geometric Filling-In Queue, and noticed that the Geometric distribution produced significantly lower waiting times. I believe this occurs because of the idea that I have previously dicussed about the Geometric distribution being skewed toward the smaller party sizes. 

```
Matching Queue with Uniform Party Sizes         Matching Queue with Geometric Party Sizes
 Number of Parties  Average Wait Time              Number of Parties  Average Wait Time
                10               1.55                             10               4.22
                20               4.66                             20               3.13
                50               5.51                             50               7.46
               100              12.71                            100               6.16
               200              11.51                            200               5.14
               500              14.51                            500               5.04
              1000              28.67                           1000               4.96

Filling-In Queue with Uniform Party Sizes        Filling-In Queue with Geometric Party Sizes
 Number of Parties  Average Wait Time              Number of Parties  Average Wait Time
                10               1.51                             10               4.60
                20               5.24                             20               4.33
                50               6.08                             50              12.12
               100              12.83                            100               9.60
               200              22.81                            200               8.29
               500              23.07                            500               9.17
              1000              53.48                           1000               8.45
```

Furthermore, I took into consideration the remaning amount of unmatched parties. I realized that the Uniform distribution had a larger number for this parameter in comparison to the Geometric distribution. Therefore, the Geometric arrivals work better in producing more groups efficiently.

```
Matching Queue with Uniform Party Sizes                     Matching Queue with Geometric Party Sizes
 Number of Parties   Remaining Unmatched Parties              Number of Parties    Remaining Unmatched Parties
                10                       2                             10                              2
                20                       5                             20                              3
                50                       9                             50                              0
               100                      11                            100                              3                                                       200                       4                            200                              2
               500                       5                            500                              0
              1000                      14                           1000                              3

Filling-In Queue with Uniform Party Sizes                     Filling-In Queue with Geometric Party Sizes
 Number of Parties   Remaining Unmatched Parties              Number of Parties    Remaining Unmatched Parties
                10                       2                                   10                        1
                20                       5                                   20                        4
                50                       8                                   50                        4
               100                      13                                  100                        1
               200                      14                                  200                        2
               500                      20                                  500                        0
              1000                      28                                 1000                        2
```

Additionally, I realized that the Uniform arrivals was able to form more groups than the Geometric arrivals. However, the difference in groups formed is not as significant. Therefore, the Geometric Distribution works better for the arrivals to produce the most efficient and effective process. 
```
Matching Queue with Uniform Party Sizes         Matching Queue with Geometric Party Sizes
 Number of Parties      Groups Formed              Number of Parties      Groups Formed
                10                  4                             10                  4
                20                  8                             20                  9
                50                 22                             50                 27
               100                 49                            100                 53
               200                112                            200                107
               500                286                            500                270
              1000                581                           1000                543

Filling-In Queue with Uniform Party Sizes        Filling-In Queue with Geometric Party Sizes
 Number of Parties      Groups Formed              Number of Parties      Groups Formed
                10                  4                             10                  4
                20                  8                             20                  8
                50                 22                             50                 24
               100                 46                            100                 53
               200                105                            200                107
               500                276                            500                270
              1000                569                           1000                543
```

## Conclusion

After evaluating both Matching vs Filling-In and Uniform vs Geometric, the Matching Queue with Geometric Party Sizes produces the best group combining process with low waiting times. 
