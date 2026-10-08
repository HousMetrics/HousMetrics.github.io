# HousMetrics
# Causal inference
In machine learning, a linear regression model can be used to predict data points when the data is assumed to have a linear trend. Another use case of the linear regression model is to determine a...

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
The above image is a simple case of causal relationship, *in reality there are more variables or more intricate complexities lurking around the corner*. Take, for instance, the infamous example of...

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
Where the last terms of each formula represent the error term. When we receive the data and check the Pearson correlation between shark attacks and ice cream sales it is equal to 92%. Surely these...

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
This could then be misleadingly interpreted as for every ice cream sale there will be 1.29 shark attacks. However, we have seen that the original data explicitly shows that these two variables are...
```
Intercept:, 0.22
Slope: [0.03 2.94]
```
and without sales we get:
```
Intercept:, 0.223
Slope: 3.01
```
