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

```
Mean = 10;
SD = 2;
fprintf('Using bootstrapping:\n')
for n = [5,10,20,40,80,160,1000]
    Sample = normrnd(Mean,SD,n,1); %compute "random" sample of size n x 1 from normal distribution based on population mean and SD
    CI95 = bootci(2000,{@mean,Sample}); %2000 bootstrap samples, based on sample and sample mean
    fprintf('n=%d ',n)
    fprintf('Sample Mean = %f ',mean(Sample))
    fprintf('95%%-CI [%f,%f]',CI95)
    fprintf(' with width %f.\n',(CI95(2,1) - CI95(1,1)))
end
```

Given that each sample MATLAB generates will have a different mean, I am printing the resulting confidence intervals with the sample mean, as well as confidence interval width to illustrate narrowing of confidence intervals with increasing sample size:

```
Using bootstrapping:
n=5 Sample Mean = 10.551776 95%-CI [8.840440,11.607473] with width 2.767033.
n=10 Sample Mean = 10.346831 95%-CI [9.109878,11.515853] with width 2.405974.
n=20 Sample Mean = 9.816334 95%-CI [8.814535,10.618014] with width 1.803479.
n=40 Sample Mean = 10.157212 95%-CI [9.309765,10.993551] with width 1.683786.
n=80 Sample Mean = 10.030831 95%-CI [9.626251,10.439931] with width 0.813680.
n=160 Sample Mean = 10.201785 95%-CI [9.917860,10.469017] with width 0.551157.
n=1000 Sample Mean = 9.980164 95%-CI [9.854922,10.114698] with width 0.259777.
```


### 4. Bayesian credible intervals. 

As outlined in the tutorial, "posterior distribution for given X is itself a Gaussian with a mean value of sample mean and a standard deviation of SEM; in other words, a distribution defined by the sample mean and the standard error of the mean! In this case, the credible interval is computed in exactly the same way as the confidence interval computed analytically from the sample mean and standard error of the mean described above."

Confidence intervals based on this philosophy are computed below:

```
Mean = 10;
SD = 2;
fprintf('Bayesian Credible Intervals:\n')
for n = [5,10,20,40,80,160,1000]
    Sample = normrnd(Mean,SD,n,1); %compute "random" sample of size n from normal distribution based on population mean and SD
    SampleMean = mean(Sample);
    SEM = SD/sqrt(n); %compute SEM based on population SD
    CI951Lower = SampleMean - 1.96*SEM; %compute 95% credible interval bounds based on sample mean and SEM
    CI951Upper = SampleMean + 1.96*SEM;
    fprintf('n = %d ',n)
    fprintf('Sample Mean = %f ',SampleMean)
    fprintf('95%% Credible Interval [%f, ',CI951Lower)
    fprintf('%f]\n',CI951Upper)
end
```

The output of this code is given below, with sample mean to frame the credible intervals. 

```
Bayesian Credible Intervals:
n = 5 Sample Mean = 10.662684 95% Credible Interval [8.909606, 12.415761]
n = 10 Sample Mean = 9.876193 95% Credible Interval [8.636580, 11.115806]
n = 20 Sample Mean = 9.938896 95% Credible Interval [9.062357, 10.815434]
n = 40 Sample Mean = 10.391748 95% Credible Interval [9.771942, 11.011555]
n = 80 Sample Mean = 10.144926 95% Credible Interval [9.706656, 10.583195]
n = 160 Sample Mean = 10.189630 95% Credible Interval [9.879727, 10.499533]
n = 1000 Sample Mean = 9.992429 95% Credible Interval [9.868468, 10.116391]
```
