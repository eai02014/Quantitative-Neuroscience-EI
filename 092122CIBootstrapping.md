## Confidence Intervals and Bootstrapping Exercise

Compute confidence/credible intervals based on the four methods above for simulated data sampled from a population that is Gaussian distributed with mean mu =10 and standard deviation sigma =2, for n=5, 10, 20, 40, 80, 160, 1000 at a 95% confidence level.

Define mean (mu) and SD (sigma): 

```
Mean = 10;
SD = 2;
```

### 1. The simple, analytic approach with large *n* and/or known SD. 

In this case, use SEM as in tutorial and multiply by Z-score of 95% CI ([-1.96, 1.96]). 

Using the code below: 

```
fprintf('In condition of large N and/or known SD:\n')
for n = [5,10,20,40,80,160,1000] %loop through all values of n we are interested in
    SEM = SD/sqrt(n); %compute SEM using SD (sigma), rather than sample SD, given that population SD is known
    CI951Lower = Mean - 1.96*SEM;
    CI951Upper = Mean + 1.96*SEM;
    fprintf('n = %d ',n)
    fprintf('95%% CI [%f, ',CI951Lower)
    fprintf('%f]\n',CI951Upper)
end

```

This yields the following output: 

```
In condition of large N and/or known SD:
n = 5 95% CI [8.246923, 11.753077]
n = 10 95% CI [8.760387, 11.239613]
n = 20 95% CI [9.123461, 10.876539]
n = 40 95% CI [9.380194, 10.619806]
n = 80 95% CI [9.561731, 10.438269]
n = 160 95% CI [9.690097, 10.309903]
n = 1000 95% CI [9.876039, 10.123961]
```


### 2. The simple, analytic approach with small *n* and unknown population standard deviation.

To use student t distribution, t value must be drawn from student t table for given number of df = n - 1 and probabilities p = 0.025 and 0.975. These t values are multiplied by SEM and added to mean to give two-tailed 95% confidence interval. This process is outlined in the code below: 

```
fprintf('Using the student t distribution:\n')
for n = [5,10,20,40,80,160,1000]
    SEM = SD/sqrt(n);
    df = n - 1; %compute df = n-1
    CI951Lower = Mean + tinv(0.025,df)*SEM; %call value in t table for probability 0.025 and given df for a specific n, multiply by SEM and add to table (result at this probability will be negative so equivalent to subtracting)
    CI951Upper = Mean + tinv(0.975,df)*SEM; %call value in t table for probability 0.975 and given df for a specific n
    fprintf('n = %d ',n)
    fprintf('95%% CI [%f, ',CI951Lower)
    fprintf('%f]\n',CI951Upper)
end
```

This yields the following result, which notably closely approximates the values obtained above at higher ns (i.e., 1000), but diverges especially at n = 5: 

```
Using the student t distribution:
n = 5 95% CI [7.516672, 12.483328]
n = 10 95% CI [8.569286, 11.430714]
n = 20 95% CI [9.063971, 10.936029]
n = 40 95% CI [9.360369, 10.639631]
n = 80 95% CI [9.554922, 10.445078]
n = 160 95% CI [9.687726, 10.312274]
n = 1000 95% CI [9.875891, 10.124109]
```


### 3. Bootstrapped confidence intervals.

To compute bootstrapped confidence intervals, I need to be able to resample n samples from a sample of size n. I will generate 7 samples, one for each of the above ns (5, 10, 20, 40, 80, 160, 1000). I will then compute the bootstrapped CI of each sample by using 2000 bootstrap samples. 



### 4. Bayesian credible intervals. 

