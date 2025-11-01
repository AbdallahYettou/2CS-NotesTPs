# Machine Learning Regression — Markdown Study Notes

***

## 1. **What is Regression?**
- Regression is a type of **supervised learning** used to predict **continuous numerical values** based on one or more input features.
- **Goal:** Learn a function that maps inputs  X  to a target output  Y 
- **Examples:**
  - Predicting house prices given features like surface and number of rooms.
  - Predicting a son's age given the father's age.

***

## 2. **Linear vs. Nonlinear Regression**
- **Linear Regression:** Models the relationship between input(s) and output as a straight line.
  - **Equation:**
    $$
    Y = \beta_0 + \beta_1 X + \varepsilon
    $$
- **Nonlinear Regression:** Used when relationships in the data form curves (not straight lines).
  - **Example:**
    $$
    Y = a e^{bX} + \varepsilon
    $$
    (exponential function)
- **Tip:** Use linear regression for straight-line relationships, nonlinear regression for curves or more complex patterns.

***

## 3. **Simple Linear Regression Model**
- **Model:**
  $$
  Y_t = \beta_0 + \beta_1 X_t + \varepsilon_t
  $$
- $\beta_0$: Intercept (value of Y when X = 0)
- $\beta_1$: Slope (change in Y per unit change in X)
- $\varepsilon_t$: Error term (captures random noise or unexplained variance)


### **Ordinary Least Squares (OLS)**
- OLS finds $ \beta_0 $ and $ \beta_1 $ that **minimize the sum of squared errors**:
  $$
  \sum_{t=1}^{m} (Y_t - \beta_0 - \beta_1 X_t)^2
  $$
- **Purpose:** Get the best-fit line that predicts $ Y $ with the least error.

***

## 4. **Multiple Linear Regression**
- **Model:** Used for multiple features (inputs):
  $$
  Y = \theta_0 + \theta_1 X_1 + \theta_2 X_2 + \dots + \theta_n X_n
  $$
  - Each $ \theta_i $ measures how much output changes per unit change in input $ X_i $

- **Cost Function:** Measures how well parameters fit the data
  $$
  J(\theta) = \frac{1}{2m} \sum_{i=1}^{m} (f_{X_i} - Y_i)^2
  $$

- **Gradient Descent:**
  - **Algorithm to minimize cost function:**
    - Update each parameter based on the negative gradient.
    $$
    \theta_j := \theta_j - \alpha \frac{\partial J}{\partial \theta_j}
    $$
    - $ \alpha $ is the **learning rate**: step size for each update.
    - **If $ \alpha $ too small:** slow convergence. **Too large:** can overshoot the minimum and fail to converge.

***

## 5. **Evaluating Regression Performance**

- **Mean Absolute Error (MAE):**
  $$
  MAE = \frac{1}{n} \sum_{j=1}^n | y_j - \hat{y}_j |
  $$
  - **Pros:** More robust to outliers. Gives a general sense of average error.
  - **Cons:** Does not heavily penalize large errors.

- **Root Mean Squared Error (RMSE):**
  $$
  RMSE = \sqrt{\frac{1}{n} \sum_{j=1}^n (y_j - \hat{y}_j)^2}
  $$
  - **Pros:** Penalizes large errors/outliers more than MAE.
  - **Cons:** More sensitive to outliers.

- **R-squared ($ R^2 $) Score:**
  $$
  R^2 = 1 - \frac{\sum_{j=1}^n (y_j - \hat{y}_j)^2}{\sum_{j=1}^n (y_j - \bar{y})^2}
  $$
  - Measures how well the model explains the variation in the data. Closer to 1 is better.



***

## 7. **Quick Summary Table: MAE vs RMSE**

| Metric | Formula | Sensitive to Outliers? | Strengths |
|--------|---------|------------------------|----------|
| MAE | $ \mathrm{MAE} = \frac{1}{n}\sum_{j=1}^{n} \lvert y_j - \hat{y}_j \rvert $ | Less sensitive | Simple and interpretable; robust to outliers; same units as target |
| RMSE | $ \mathrm{RMSE} = \sqrt{\frac{1}{n}\sum_{j=1}^{n} (y_j - \hat{y}_j)^2} $ | More sensitive | Penalizes large errors more; useful when large deviations are especially undesirable |

## More :
**What is the role of gradient descent in regression?**: Gradient descent is an iterative optimization algorithm used to find regression model parameters that minimize the cost function. It updates parameters by moving in the direction of the negative gradient of the cost.