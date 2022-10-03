### Linear Regression Exercise

#### 1. Plot the relationship between age and wing length. 

```
Age = [3;4;5;6;7;8;9;11;12;14;15;16;17];
WingLength = [1.4;1.5;2.2;2.4;3.1;3.2;3.2;3.9;4.1;4.7;4.5;5.2;5];

scatter(Age,WingLength);
title('Plot of Age vs. Wing Length');
xlabel('Age (years)');
ylabel('Wing Length (cm)');
```


![PSet8](https://user-images.githubusercontent.com/112706184/193486721-14af907b-bd9c-4f05-9fc3-45b6297ca0f3.jpg)

#### 2. Calculate and plot the regression line. 

##### Calculate slope:

```
Term1 = 0;
for i = 1:13
    Term1 = Term1 + (Age(i)*WingLength(i));
end


SumAge = 0;
SumWingLength = 0;
for i = 1:13
    SumAge = SumAge + Age(i);
    SumWingLength = SumWingLength + WingLength(i);
end

n = 13;
Term2 = (SumAge * SumWingLength)/n;

Numerator = Term1-Term2;

Term3 = 0;
for i = 1:13
    Term3 = Term3 + ((Age(i))^2);
end

Term4 = ((SumAge)^2)/n;

Denominator = Term3 - Term4;

b = Numerator/Denominator;

fprintf('This yields a slope of b = %f.\n',b)
```
This yields a slope of b = 0.264684.

##### Calculate intercept:

```
WingLengthAvg = mean(WingLength);
AgeAvg = mean(Age);
a = WingLengthAvg - b*AgeAvg;
fprintf('This yields an intercept of a = %f.\n',a)
```
This yields an intercept of a = 0.829624.

I checked these results using the function fitlm and it yielded the same numbers. 

```
fitlm(Age,WingLength,"linear");
```


##### Plot line: 

```
scatter(Age,WingLength);
title('Plot of Age vs. Wing Length');
xlabel('Age (years)');
ylabel('Wing Length (cm)');
hold on
refline(b,a);
```

Yields the following graph: 

![PSet8](https://user-images.githubusercontent.com/112706184/193488198-d64045cc-d959-4be6-9000-2389678c13a8.jpg)


