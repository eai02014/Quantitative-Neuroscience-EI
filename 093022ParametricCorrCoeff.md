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

