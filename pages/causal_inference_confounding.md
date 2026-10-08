# HousMetrics
# Causal inference
Imagine you fit a linear regression model to your data. How should you interpret the coefficients? How are your covariates related to your outcome variable? Can you say that the x variable is the cause of the y variable? These questions will be answered here.

Today we will estimate the causal effect of ice cream sales on shark attacks (or the lack thereof). Say we have done an analysis on a dataset and we find that every year for ten years whenever ice cream sales go up shark attacks increase as well. Now we fit a linear regression model (OLS) with shark attacks as the outcome variable and ice cream sales as the regressor. 

```python
X = ice_cream_sales.reshape(-1,1)

model = sm.OLS(shark_attacks, X).fit()
```
After running the regression we get a significant coefficient for ice cream sales of 1.44. Surely this should mean that if ice cream sales increase then shark attacks will increase by a factor of 1.44? WRONG! We have not established causality here. What if I told you there is another variable which is actually the true cause behind the increase of ice cream sales and shark attacks. This variable is a binary variable indicating whether it is summer or not. As in summer more people buy ice cream and more people go for swim increasing their chances of getting attacked by a shark. Now conducting the same regression but adding this binary variable will change the results drastically. 

```python
X = np.column_stack((ice_cream_sales, summer_indicator))

model = sm.OLS(shark_attacks, X).fit()
```
Now the coefficient for ice cream sales is insignificant and equal to 0.011 and the coefficient for the binary summer variable is significant and equal to 3.00 (equal to the true value). Where in the previous regression one could think that shark attacks is causally (and significantly) dependent on ice cream sales. In the second regression it is clear that the relationship between the two variables is more intricate. Visually in the first setting we assume the following:

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

However, in reality there is another variable which is the actual cause of the two variables increasing. Not taking that variable into account yielded a large bias and therefore an incorrect representation of reality. This bias is called confounding bias. It arises when an important variable is not included or accounted for in the analysis. So the actual visualization we should consider is the following:


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

The image above is the one we used in the second regression which led to highly accurate parameter estimates. Not taking the binary variable into account leads to confounding bias as we have seen. 

So how are ice cream sales and shark attacks related? Not causally! They are correlated to each other in that whenever it is summer people start buying more ice cream and people (possible others) go for a swim risking a shark attack. 

# references
