## Multiple Comparisons Exercise

*In this exercise we will run through an example of correcting for multiple comparisons with both the Benjamini-Hochberg procedure and the more conservative Bonferroni correction.*

### First, simulate multiple (say, 1000) t-tests comparing two samples with equal means and standard deviations, and save the p-values. Obviously, at p<0.05 we expect that ~5% of the simulations to yield a "statistically significant" result (of rejecting the NULL hypothesis that the samples come from distributions with equal means).

```
NTests = 1000; %number of t-tests I will conduct
Ps = []; %empty array to store Ps I will generate
for i = 1:NTests
    Sample1 = normrnd(1,1,100,1); %generate Sample 1 by randomly sampling normal distribution with mean 1 and SD 1
    Sample2 = normrnd(1,1,100,1); %generate Sample 2 using same method as sample 1
    [Output,P] = ttest2(Sample1,Sample2); %conduct independent 2-samples t test comparing the two samples
    Ps = [Ps; P]; %add p value from this t test to the Ps array
end

%How many of these P values are significant without correction for multiple
%comparisons
Alpha = 0.05; %set alpha
SigPs = numel(Ps(Ps<Alpha)); %determine how many Ps in the Ps array are less than alpha
fprintf('Number of p-values less than 0.05 = %d\n',SigPs) %print this number

```

On one iteration of this code, I get the following output: 
Number of p-values less than 0.05 = 46

### Second, once you have the simulated p-values, apply both methods to address the multiple comparisons problem.

#### Bonferroni

```
BonferroniCorrThresh = Alpha/NTests; %determine Bonferroni-corrected threshold based on number of tests conducted
SigBonfPs = numel(Ps(Ps<BonferroniCorrThresh)); %determine how many Ps are less than Bonferroni-corrected threshold
fprintf('Number of p-values less than Bonferroni-corrected threshold (%f) = %d\n',BonferroniCorrThresh,SigBonfPs)
```

On the same iteration of the code as the iteration above, this yields: 
Number of p-values less than Bonferroni-corrected threshold (0.000050) = 0

#### Benjamini-Hochberg

```
%How many of these P values are significant with Benjamini-Hochberg
%correction
FDR = 0.20; %normally 0.05, but set more leniently for this iteration
RankPs = floor(tiedrank(Ps)); %rank P values relative to each other
CritVal = (RankPs/NTests) * FDR; %calculate critical values (i/n)*Q
BHPs = [RankPs Ps CritVal]; %generate array with rank, p-values, and critical values
BHPs = sortrows(BHPs); %sort array in ascending order based on rank

%find largest P-value that is smaller than its associated critical value
for i = 1:1000
    %following if loop prints the P-value before the first P that is
    %greater than or equal to its associated critical value
    if BHPs(i,2) >= BHPs(i,3) && i-1 ~= 0 
        fprintf('Benjamini-Hochberg Criterion P = %f\n at FDR = %f\n',BHPs(i-1,2),FDR)
        BHCritP = BHPs(i-1,2);
        BHCritPExists = true;
        break
    end

    %if no P-value is smaller than its associated critical value, forego
    %B-H correction
    if BHPs(i,2) >= BHPs(i,3) && i-1 == 0
            fprintf('No Benjamini-Hochberg Criterion P exists\n')
            BHCritPExists = false;
            break
    end
end

%Determine number of p values less than or equal to the B-H criterion p
%value, only if this value exists
if BHCritPExists == true
SigBHPs = numel(Ps(Ps<=BHCritP));
fprintf('Number of p-values less than Benjamini-Hochberg threshold = %d\n',SigBHPs)
else
    fprintf('Number of p-values significant after B-H correction = 0\n')
end
```
On the same iteration of the code as the iterations above, this yields the following output:
Benjamini-Hochberg Criterion P = 0.000090
 at FDR = 0.200000
Number of p-values less than Benjamini-Hochberg threshold = 1

On a different iteration where no p-value was less than its associated critical value, the following output is generated: 
No Benjamini-Hochberg Criterion P exists
Number of p-values significant after B-H correction = 0

### Third, set the sample 1 and sample 2 means to be 1 and 2 respectively, and re-run the exercise. What do you notice? What if you make the difference between means even greater?

Rerun the code above but with change in the following lines: 

```
for i = 1:NTests
    Sample1 = normrnd(1,1,100,1); %generate Sample 1 by randomly sampling normal distribution with mean 1 and SD 1
    Sample2 = normrnd(2,1,100,1); %generate Sample 2 using same method as sample 1, BUT WITH MEAN OF 2
```
