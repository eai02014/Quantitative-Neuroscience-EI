### Parametric Correlation Coefficient Exercise

#### 1. Plot wing length vs. tail length. Do they look related? 

```
%Add data from PSet
WingLength = [10.4; 10.8; 11.1; 10.2; 10.3; 10.2; 10.7; 10.5; 10.8; 11.2; 10.6; 11.4];
TailLength = [7.4; 7.6; 7.9; 7.2; 7.4; 7.1; 7.4; 7.2; 7.8; 7.7; 7.8; 8.3];

%Plot X vs. Y. Do they look related? 
scatter(WingLength,TailLength);
title('Plot of Wing Length vs. Tail Length');
xlabel('Wing Length (cm)');
ylabel('Tail Length (cm)');
```
Generates the following graph: 

![WingLengthTailLengthPSet7](https://user-images.githubusercontent.com/112706184/192166528-b43a87ba-2b20-4bdb-83fc-67adf3f43e80.jpg)

I think they look related. 

#### 2. Calculate rXY and rYX, first using the equations in the tutorial, and then using Matlab's built-in corrcoef. Did you get the same answers? 

rXY and rYX computation with equations in tutorial: 

```
%Calculate rXY and rYX using the equation. 
n = 12;
x = WingLength;
y = TailLength;
xbar = mean(x);
ybar = mean(y);
Numerator = 0;
Denomx = 0;
Denomy = 0;
for i = 1:n
    Numerator = Numerator + ((x(i,1) - xbar)* (y(i,1) - ybar));
    Denomx = Denomx + ((x(i,1)-xbar)^2); 
    Denomy = Denomy + ((y(i,1)-ybar)^2); 
end

Denominator = sqrt(Denomx*Denomy);
rXY = Numerator/Denominator;
fprintf('rXY using equation = %f\n',rXY)

%for rYX
n = 12;
y = WingLength;
x = TailLength;
xbar = mean(x);
ybar = mean(y);
Numerator = 0;
Denomx = 0;
Denomy = 0;
for i = 1:n
    Numerator = Numerator + ((x(i,1) - xbar)* (y(i,1) - ybar));
    Denomx = Denomx + ((x(i,1)-xbar)^2); 
    Denomy = Denomy + ((y(i,1)-ybar)^2); 
end

Denominator = sqrt(Denomx*Denomy);
rYX = Numerator/Denominator;
fprintf('rYX using equation = %f\n',rYX)
```

rXY and rYX with built-in corrcoef:

```
%Using corrcoef
MLrXY = corrcoef(x,y);
fprintf('rXY using corrcoef = %f\n',MLrXY(1,2))
fprintf('rYX using corrcoef = %f\n',MLrXY(2,1))
```

This code yields the following output: 

```
rXY using equation = 0.870355
rYX using equation = 0.870355
rXY using corrcoef = 0.870355
rYX using corrcoef = 0.870355
```
They are all the same. 

#### 3. What is standard error of rXY? What are the 95% confidence intervals computed from the standard error? 

Standard error of rXY can be computed as follows: 

```
srXY = sqrt((1-(rXY^2))/(n-2)); %from formula in tutorial
fprintf('Standard error of rXY = %f\n',srXY)
```
This yields: Standard error of rXY = 0.155719.

95% confidence intervals can be computed as follows: 

```
%1. Take Fisher transformation of r:
zrXY = 0.5 * log((1+rXY)/(1-rXY)); %note to self: log(X) in matlab returns natural logarithm (i.e., ln) of X

%2. Compute its standard deviation as: 
szrXY = sqrt(1/(n-3));

%3. Compute confidence intervals in this z-space as: 
zcriterion = 1.96; %for 95% confidence interval
UpperzrXY = zrXY + (zcriterion*szrXY);
LowerzrXY = zrXY - (zcriterion*szrXY);

%4. Then translate each z value back to r as: 
CIUpper = (exp(2*UpperzrXY) -1)/(exp(2*UpperzrXY) +1);
CILower = (exp(2*LowerzrXY) -1)/(exp(2*LowerzrXY) +1);
fprintf('rXY 95%% CI [%f, %f]\n',CILower,CIUpper)
```

This yields: rXY 95% CI [0.592303, 0.963161].

#### 4. Should the value of rXY be considered significant at the p < 0.05 level, given a two-tailed test (i.e., we reject if the test statistic is too large on either tail of the null distribution) for H0: rXY = 0?

Yes. Given the code below, it is possible to determine p-value of 0.000231.

```
%Compute t value for rXY:
t = rXY / srXY;

%Use this information to compute two-tailed p-value:
df = n-2;
p = (1 - tcdf(t,df))*2; %compute p-value from t-distribution cdf and multiply by 2 to make 2-tailed
fprintf('p-value relative to H0: r = 0 is as follows: p = %f\n',p)
```

#### 5. Yale does the exact same study and finds that his correlation value is 0.75. Is this the same as yours? That is, evaluate H0: r = 0.75. 

No. Given the code below, it is possible to determine p-value of 0.303489, suggesting our correlation values are not significantly different. 

```
%1. First compute z transformation of r0 and rXY, yielding test statistic
%Lambda. 
r0 = 0.75;
zr0 = 0.5 * log((1+r0)/(1-r0));
zrXY = 0.5 * log((1+rXY)/(1-rXY));
Lambda = (zrXY-zr0)/sqrt(1/(n-3));
df = n-2;

%2. Plug into tcdf to determine p value
p = (1 - tcdf(Lambda,df))*2; %compute p-value from t-distribution cdf and multiply by 2 to make 2-tailed
fprintf('p-value relative to H0: r = 0.75 is as follows: p = %f\n',p)

```

#### 6. Finally, calculate the statistical power and sample size needed to reject H0: r0 = 0 when r1 >= 0.5. 

First, z-transform both r0 and r1. r0 will simply be 0. r1 is calculated below: 

```
r = 0.5
rz = 0.5 * log((1+r)/(1-r));
```

For ns ranging from 4 to 14, compute: 
1. SD, which is given by sqrt(1/(n-3)). 
2. Power, computed using sampsizepwr function in MATLAB. 

```
for n = 4:14
    SD = sqrt(1/(n-3)); %SD for z-transformed r values is given by this formula
    Power = sampsizepwr('t', [0,SD],rz,[],n);
    fprintf('At n = %d, Power = %f, i.e., beta = %f\n', n, Power, 1-Power)
end
```

Doing this, we get the following output: 

```
At n = 4, Power = 0.123936, i.e., beta = 0.876064
At n = 5, Power = 0.268162, i.e., beta = 0.731838
At n = 6, Power = 0.469630, i.e., beta = 0.530370
At n = 7, Power = 0.680023, i.e., beta = 0.319977
At n = 8, Power = 0.844479, i.e., beta = 0.155521
At n = 9, Power = 0.940535, i.e., beta = 0.059465
At n = 10, Power = 0.982407, i.e., beta = 0.017593
At n = 11, Power = 0.996019, i.e., beta = 0.003981
At n = 12, Power = 0.999316, i.e., beta = 0.000684
At n = 13, Power = 0.999911, i.e., beta = 0.000089
At n = 14, Power = 0.999991, i.e., beta = 0.000009
```

Suggesting that at n>8, power will be greater than 0.8, and comparison between two correlation coefficients of 0.5 relative to null will be sufficiently powered. 



