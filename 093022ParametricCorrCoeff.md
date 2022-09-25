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
