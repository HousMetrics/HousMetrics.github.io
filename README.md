# HousMetrics
# Causal inference
In machine learning, a linear regression model can be used to predict data points when the data is assumed to have a linear trend. Another use case of the linear regression model is to determine a cause-and-effect relationship between variables. This cause-and-effect relationship is established by estimating a causal quantity known as the average treatment effect (ATE). The ATE is defined as the average causal effect of a treatment or covariate on the outcome variable. More specifically, the ATE represents the cause-and-effect relationship between a covariate and the outcome variable. This is different than a pure correlative relationship between variables. Visualized we want to find the following:

```latex
\documentclass[tikz,border=10pt]{standalone}
\usetikzlibrary{arrows.meta}
\begin{document}
\begin{tikzpicture}[
  node distance=4cm,
  circ/.style={circle, draw, thick, minimum size=2.4cm, font=\large}
]
  \node[circ] (v) at (0,0) {Variable};
  \node[circ] (o) at (6,0) {Outcome};
  \draw[-{Stealth[length=3mm]}, very thick] (v) -- node[above, font=\large] {causal} (o);
\end{tikzpicture}
\end{document}
```
The above image is a simple case of causal relationship, *in reality there are more variables or more intricate complexities lurking around the corner*. Take, for instance, the infamous example of ice cream sales and shark attacks. We could see in the data that as sales go up shark attacks increase as well. This could lead someone to make the wrong conclusion that an incraese in ice cream sales cause shark attacks (or the other way around). The real reason is that during summer more people buy ice cream and more people take a swim increasing the chance of getting attack by a shark. Therefore, the cause-and-effect relationship is not between sales and shark attacks but it actually has to with whether it is summer or not. This issue is called confounding; it basically means that we forgot to add variable in our linear regression model.

```latex
\documentclass[tikz,border=10pt]{standalone}
\usetikzlibrary{arrows.meta}
\begin{document}
\begin{tikzpicture}[
  circ/.style={circle, draw, thick, minimum size=2.4cm, font=\large},
  arr/.style={-{Stealth[length=3mm]}, very thick}
]
  \node[circ] (v) at (0,0) {Variable};
  \node[circ] (o) at (6,0) {Outcome};
  \node[circ] (c) at (3,4) {Confounder};

  \draw[arr] (v) -- (o);
  \draw[arr] (c) -- (v);
  \draw[arr] (c) -- (o);
\end{tikzpicture}
\end{document}
```

Let's try to estimate the causal effect of ice cream sales on shark attacks. We assume the following relationships hold:
```python
shark_attacks = 3*summer_indicator + epsilon_sharks
ice_cream_sales = 2*summer_indicator + epsilon_sales
```
Where the last terms of each formula represent the error term. When we receive the data and check the Pearson correlation between shark attacks and ice cream sales it is equal to 92%. Surely these two quantities must be causally related to each other right? Fitting a linear regression model on this we get the following:

``` python
X = ice_cream_sales.reshape(-1, 1)
y = shark_attacks
model = LinearRegression()
model.fit(X, y)

print("Intercept:", model.intercept_)
print("Slope:", model.coef_[0])
```
```
Intercept: 0.31
Slope: 1.29
```
This could then be misleadingly interpreted as for every ice cream sale there will be 1.29 shark attacks. However, we have seen that the original data explicitly shows that these two variables are not causally related. With this knowledge and knowing that the summer indicator is the confounder we get the following:
```
Intercept:, 0.22
Slope: [0.03 2.94]
```
and without sales we get:
```
Intercept:, 0.223
Slope: 3.01
```
