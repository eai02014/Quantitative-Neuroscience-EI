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

### 3. Bootstrapped confidence intervals.

### 4. Bayesian credible intervals. 

