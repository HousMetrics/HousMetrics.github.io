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
# Confounding
Take, for instance, the infamous example of ice cream sales and shark attacks. We could see in the data that as sales go up shark attacks increase as well. This could lead someone to make the wrong conclusion that an incraese in ice cream sales cause shark attacks (or the other way around). The real reason is that during summer more people buy ice cream and more people take a swim increasing the chance of getting attack by a shark. Therefore, the cause-and-effect relationship is not between sales and shark attacks but it actually has to with whether it is summer or not. This issue is called confounding; it basically means that we forgot to add variable in our linear regression model.

```latex
% Replace this example with your equations.
3*5
```
## Question

What do you want to find out?

## Background

Add context, definitions, or notes for the reader. Markdown lets you use **bold text**,
lists, links, and headings alongside your code.

## Data

Describe where the data comes from, what each column means, and any limitations.

## Analysis

Explain what the next code block does, then add Python between fenced code markers:

```python
# Replace this example with your analysis.
prices = [425_000, 510_000, 390_000]
average_price = sum(prices) / len(prices)

print(f"Average home price: ${average_price:,.0f}")
```

Add another paragraph here to explain the result or introduce the next step.

```python
# Add another Python code block when you need one.
```

## Findings

- **Finding 1:** Summarize an important result.
- **Finding 2:** Add another result or observation.

## Conclusion

Summarize what you learned and what someone should take away.

## Next steps

List follow-up questions or improvements:

- Check another neighborhood or time period.
- Add more data or test a different approach.

> **Tip:** These fenced `python` blocks are for display and syntax highlighting on GitHub.
> They do not execute like cells in a Jupyter notebook.
