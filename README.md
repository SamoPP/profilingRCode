profilingRCode
==============

Some results of profiling R code

I am trying to create a vectorised version of "quantstrat" for very simple path independent strategies.

I have created a backtester function in backtester.R file and there is a line #438

	entry.transactions$Quantity <- sign(signals$entry[format(index(entry.transactions), "%Y-%m-%d %H:%M:%S")])*quantity

And profiler says:
> Rprof("profile_generateTransactions.out", line.profiling=TRUE)
> master.txns.list.exit.1.day.later <- generateTransactions(SPY.daily, signals.exit.1.day.later, TZ=TZ)
> Rprof(NULL)
> proftable("profile_generateTransactions.out", lines=10)

 PctTime Call                                                                                                           
 9.741   generateTransactions > 3#438 > [ > 3#438 > [.xts                                                               
 9.217   generateTransactions > 3#438                                                                                   
 6.967   generateTransactions > 3#438 > [                                                                               
 5.395   generateTransactions                                                                                           
 4.778   generateTransactions > 3#438 > [ > 3#438                                                                       
 2.928   generateTransactions > 3#438 > [ > 3#438 > [.xts > 2#94                                                        
 1.695   generateTransactions > 3#438 > [ > 3#438 > [.xts > 2#94 > .parseISO8601                                        
 1.572   2#94 > .parseISO8601 > 8#111 > as.POSIXlt > do.call > parse.side > 8#96 > as.list > as_numeric > 8#31 > gsub   
 1.387   2#94 > .parseISO8601 > 8#108 > as.POSIXlt > do.call > parse.side > 8#96 > as.list > as_numeric > 8#31 > gsub   
 0.986   3#438 > [.xts > 2#94 > .parseISO8601 > 8#108 > as.POSIXlt > do.call > <Anonymous> > 7#111 > ISOdatetime > paste

> summaryRprof("profile_generateTransactions.out", lines = "show")
$by.self
                  self.time self.pct total.time total.pct
endpoints.R#111        3.92    10.32       3.92     10.32
parse8601.R#35         3.64     9.58       3.64      9.58
parse8601.R#36         3.64     9.58       3.64      9.58
endpoints.R#138        3.46     9.11       3.46      9.11
parse8601.R#31         2.80     7.37       2.80      7.37
parse8601.R#125        2.38     6.27       2.38      6.27
parse8601.R#119        2.28     6.00       2.28      6.00
parse8601.R#96         1.92     5.06       4.82     12.69
parse8601.R#153        1.88     4.95       1.88      4.95
parse8601.R#108        1.32     3.48       9.26     24.38
parse8601.R#111        1.26     3.32       9.26     24.38
parse8601.R#112        1.26     3.32       1.26      3.32
utils.R#22             1.00     2.63       1.10      2.90
parse8601.R#66         0.84     2.21       0.84      2.21
parse8601.R#41         0.64     1.69       0.64      1.69
parse8601.R#67         0.62     1.63       0.62      1.63
endpoints.R#125        0.48     1.26       0.48      1.26
parse8601.R#38         0.42     1.11       0.42      1.11
parse8601.R#81         0.32     0.84       0.32      0.84
xts.methods.R#96       0.28     0.74       1.38      3.63
parse8601.R#39         0.28     0.74       0.28      0.74
xts.methods.R#99       0.24     0.63       0.42      1.11
parse8601.R#116        0.22     0.58       0.30      0.79
index.R#85             0.22     0.58       0.22      0.58
parse8601.R#48         0.22     0.58       0.22      0.58
parse8601.R#89         0.18     0.47       0.18      0.47
parse8601.R#64         0.16     0.42       0.16      0.42
parse8601.R#82         0.16     0.42       0.16      0.42
endpoints.R#134        0.12     0.32       0.12      0.32
parse8601.R#70         0.12     0.32       0.12      0.32
parse8601.R#83         0.12     0.32       0.12      0.32
isOrdered.R#30         0.10     0.26       0.10      0.26
parse8601.R#29         0.10     0.26       0.10      0.26
parse8601.R#71         0.10     0.26       0.10      0.26
parse8601.R#88         0.10     0.26       0.10      0.26
xts.methods.R#94       0.08     0.21      35.96     94.68
parse8601.R#84         0.08     0.21       0.08      0.21
parse8601.R#90         0.08     0.21       0.08      0.21
parse8601.R#122        0.06     0.16       0.14      0.37
isOrdered.R#27         0.06     0.16       0.06      0.16
parse8601.R#63         0.06     0.16       0.06      0.16
parse8601.R#87         0.06     0.16       0.06      0.16
parse8601.R#91         0.06     0.16       0.06      0.16
parse8601.R#92         0.06     0.16       0.06      0.16
xts.methods.R#93       0.06     0.16       0.06      0.16
xts.methods.R#95       0.06     0.16       0.06      0.16
endpoints.R#123        0.04     0.11       0.04      0.11
index.R#84             0.04     0.11       0.04      0.11
parse8601.R#110        0.04     0.11       0.04      0.11
parse8601.R#127        0.04     0.11       0.04      0.11
parse8601.R#46         0.04     0.11       0.04      0.11
parse8601.R#47         0.04     0.11       0.04      0.11
as.numeric.R#97        0.02     0.05       0.02      0.05
endpoints.R#115        0.02     0.05       0.02      0.05
isOrdered.R#24         0.02     0.05       0.02      0.05
merge.R#113            0.02     0.05       0.02      0.05
Ops.xts.R#32           0.02     0.05       0.02      0.05
parse8601.R#107        0.02     0.05       0.02      0.05
parse8601.R#115        0.02     0.05       0.02      0.05
parse8601.R#121        0.02     0.05       0.02      0.05
parse8601.R#54         0.02     0.05       0.02      0.05
xts.methods.R#177      0.02     0.05       0.02      0.05
zoo.R#202              0.02     0.05       0.02      0.05

$by.total
                  total.time total.pct self.time self.pct
backtester.R#438       37.90     99.79      0.00     0.00
xts.methods.R#94       35.96     94.68      0.08     0.21
parse8601.R#108         9.26     24.38      1.32     3.48
parse8601.R#111         9.26     24.38      1.26     3.32
parse8601.R#96          4.82     12.69      1.92     5.06
endpoints.R#111         3.92     10.32      3.92    10.32
parse8601.R#35          3.64      9.58      3.64     9.58
parse8601.R#36          3.64      9.58      3.64     9.58
endpoints.R#138         3.46      9.11      3.46     9.11
parse8601.R#31          2.80      7.37      2.80     7.37
parse8601.R#125         2.38      6.27      2.38     6.27
parse8601.R#119         2.28      6.00      2.28     6.00
parse8601.R#153         1.88      4.95      1.88     4.95
xts.methods.R#96        1.38      3.63      0.28     0.74
parse8601.R#112         1.26      3.32      1.26     3.32
utils.R#22              1.10      2.90      1.00     2.63
parse8601.R#66          0.84      2.21      0.84     2.21
parse8601.R#41          0.64      1.69      0.64     1.69
parse8601.R#67          0.62      1.63      0.62     1.63
endpoints.R#125         0.48      1.26      0.48     1.26
parse8601.R#38          0.42      1.11      0.42     1.11
xts.methods.R#99        0.42      1.11      0.24     0.63
parse8601.R#81          0.32      0.84      0.32     0.84
parse8601.R#116         0.30      0.79      0.22     0.58
parse8601.R#39          0.28      0.74      0.28     0.74
index.R#85              0.22      0.58      0.22     0.58
parse8601.R#48          0.22      0.58      0.22     0.58
parse8601.R#89          0.18      0.47      0.18     0.47
parse8601.R#64          0.16      0.42      0.16     0.42
parse8601.R#82          0.16      0.42      0.16     0.42
parse8601.R#122         0.14      0.37      0.06     0.16
endpoints.R#134         0.12      0.32      0.12     0.32
parse8601.R#70          0.12      0.32      0.12     0.32
parse8601.R#83          0.12      0.32      0.12     0.32
isOrdered.R#30          0.10      0.26      0.10     0.26
parse8601.R#29          0.10      0.26      0.10     0.26
parse8601.R#71          0.10      0.26      0.10     0.26
parse8601.R#88          0.10      0.26      0.10     0.26
parse8601.R#84          0.08      0.21      0.08     0.21
parse8601.R#90          0.08      0.21      0.08     0.21
isOrdered.R#27          0.06      0.16      0.06     0.16
parse8601.R#63          0.06      0.16      0.06     0.16
parse8601.R#87          0.06      0.16      0.06     0.16
parse8601.R#91          0.06      0.16      0.06     0.16
parse8601.R#92          0.06      0.16      0.06     0.16
xts.methods.R#93        0.06      0.16      0.06     0.16
xts.methods.R#95        0.06      0.16      0.06     0.16
xts.methods.R#47        0.06      0.16      0.00     0.00
endpoints.R#123         0.04      0.11      0.04     0.11
index.R#84              0.04      0.11      0.04     0.11
parse8601.R#110         0.04      0.11      0.04     0.11
parse8601.R#127         0.04      0.11      0.04     0.11
parse8601.R#46          0.04      0.11      0.04     0.11
parse8601.R#47          0.04      0.11      0.04     0.11
bind.R#26               0.04      0.11      0.00     0.00
zoo.R#220               0.04      0.11      0.00     0.00
as.numeric.R#97         0.02      0.05      0.02     0.05
endpoints.R#115         0.02      0.05      0.02     0.05
isOrdered.R#24          0.02      0.05      0.02     0.05
merge.R#113             0.02      0.05      0.02     0.05
Ops.xts.R#32            0.02      0.05      0.02     0.05
parse8601.R#107         0.02      0.05      0.02     0.05
parse8601.R#115         0.02      0.05      0.02     0.05
parse8601.R#121         0.02      0.05      0.02     0.05
parse8601.R#54          0.02      0.05      0.02     0.05
xts.methods.R#177       0.02      0.05      0.02     0.05
zoo.R#202               0.02      0.05      0.02     0.05
backtester.R#105        0.02      0.05      0.00     0.00
backtester.R#179        0.02      0.05      0.00     0.00
backtester.R#434        0.02      0.05      0.00     0.00
backtester.R#453        0.02      0.05      0.00     0.00
index.R#3               0.02      0.05      0.00     0.00
last.R#25               0.02      0.05      0.00     0.00
merge.R#103             0.02      0.05      0.00     0.00
xtsible.R#36            0.02      0.05      0.00     0.00
xts.R#203               0.02      0.05      0.00     0.00
zoo.R#205               0.02      0.05      0.00     0.00

$by.line
                  self.time self.pct total.time total.pct
as.numeric.R#97        0.02     0.05       0.02      0.05
backtester.R#105       0.00     0.00       0.02      0.05
backtester.R#179       0.00     0.00       0.02      0.05
backtester.R#434       0.00     0.00       0.02      0.05
backtester.R#438       0.00     0.00      37.90     99.79
backtester.R#453       0.00     0.00       0.02      0.05
bind.R#26              0.00     0.00       0.04      0.11
endpoints.R#111        3.92    10.32       3.92     10.32
endpoints.R#115        0.02     0.05       0.02      0.05
endpoints.R#123        0.04     0.11       0.04      0.11
endpoints.R#125        0.48     1.26       0.48      1.26
endpoints.R#134        0.12     0.32       0.12      0.32
endpoints.R#138        3.46     9.11       3.46      9.11
index.R#3              0.00     0.00       0.02      0.05
index.R#84             0.04     0.11       0.04      0.11
index.R#85             0.22     0.58       0.22      0.58
isOrdered.R#24         0.02     0.05       0.02      0.05
isOrdered.R#27         0.06     0.16       0.06      0.16
isOrdered.R#30         0.10     0.26       0.10      0.26
last.R#25              0.00     0.00       0.02      0.05
merge.R#103            0.00     0.00       0.02      0.05
merge.R#113            0.02     0.05       0.02      0.05
Ops.xts.R#32           0.02     0.05       0.02      0.05
parse8601.R#29         0.10     0.26       0.10      0.26
parse8601.R#31         2.80     7.37       2.80      7.37
parse8601.R#35         3.64     9.58       3.64      9.58
parse8601.R#36         3.64     9.58       3.64      9.58
parse8601.R#38         0.42     1.11       0.42      1.11
parse8601.R#39         0.28     0.74       0.28      0.74
parse8601.R#41         0.64     1.69       0.64      1.69
parse8601.R#46         0.04     0.11       0.04      0.11
parse8601.R#47         0.04     0.11       0.04      0.11
parse8601.R#48         0.22     0.58       0.22      0.58
parse8601.R#54         0.02     0.05       0.02      0.05
parse8601.R#63         0.06     0.16       0.06      0.16
parse8601.R#64         0.16     0.42       0.16      0.42
parse8601.R#66         0.84     2.21       0.84      2.21
parse8601.R#67         0.62     1.63       0.62      1.63
parse8601.R#70         0.12     0.32       0.12      0.32
parse8601.R#71         0.10     0.26       0.10      0.26
parse8601.R#81         0.32     0.84       0.32      0.84
parse8601.R#82         0.16     0.42       0.16      0.42
parse8601.R#83         0.12     0.32       0.12      0.32
parse8601.R#84         0.08     0.21       0.08      0.21
parse8601.R#87         0.06     0.16       0.06      0.16
parse8601.R#88         0.10     0.26       0.10      0.26
parse8601.R#89         0.18     0.47       0.18      0.47
parse8601.R#90         0.08     0.21       0.08      0.21
parse8601.R#91         0.06     0.16       0.06      0.16
parse8601.R#92         0.06     0.16       0.06      0.16
parse8601.R#96         1.92     5.06       4.82     12.69
parse8601.R#107        0.02     0.05       0.02      0.05
parse8601.R#108        1.32     3.48       9.26     24.38
parse8601.R#110        0.04     0.11       0.04      0.11
parse8601.R#111        1.26     3.32       9.26     24.38
parse8601.R#112        1.26     3.32       1.26      3.32
parse8601.R#115        0.02     0.05       0.02      0.05
parse8601.R#116        0.22     0.58       0.30      0.79
parse8601.R#119        2.28     6.00       2.28      6.00
parse8601.R#121        0.02     0.05       0.02      0.05
parse8601.R#122        0.06     0.16       0.14      0.37
parse8601.R#125        2.38     6.27       2.38      6.27
parse8601.R#127        0.04     0.11       0.04      0.11
parse8601.R#153        1.88     4.95       1.88      4.95
utils.R#22             1.00     2.63       1.10      2.90
xtsible.R#36           0.00     0.00       0.02      0.05
xts.methods.R#47       0.00     0.00       0.06      0.16
xts.methods.R#93       0.06     0.16       0.06      0.16
xts.methods.R#94       0.08     0.21      35.96     94.68
xts.methods.R#95       0.06     0.16       0.06      0.16
xts.methods.R#96       0.28     0.74       1.38      3.63
xts.methods.R#99       0.24     0.63       0.42      1.11
xts.methods.R#177      0.02     0.05       0.02      0.05
xts.R#203              0.00     0.00       0.02      0.05
zoo.R#202              0.02     0.05       0.02      0.05
zoo.R#205              0.00     0.00       0.02      0.05
zoo.R#220              0.00     0.00       0.04      0.11

$sample.interval
[1] 0.02

$sampling.time
[1] 37.98
