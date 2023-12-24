# Logit_model_5

This script conducts a thorough logistic regression analysis, including fitting multiple models, selecting the best model based on stepwise procedures, making predictions, evaluating model fit with various diagnostics, and analyzing the model's predictive performance using an ROC curve.

### Displaying Contrasts for Factor Variables:

- `contrasts(Chapman.Cuali$Presion)`: Displays the contrast matrix for the `Presion` factor variable in the `Chapman.Cuali` DataFrame.
- `contrasts(Chapman.Cuali$IMC)`: Displays the contrast matrix for the `IMC` factor variable in the `Chapman.Cuali` DataFrame.
- These contrast matrices show how R internally represents categorical variables for use in models.

### Fitting Logistic Regression Models:

- `Ajuste.0 <- glm(Coronarios ~ 1, data = Chapman.Cuali, family = binomial)`: Fits a null logistic regression model with only an intercept. This model predicts `Coronarios` without any predictors.
- `Ajuste.All <- glm(Coronarios ~ Edad + Colesterol + Presion + IMC, data = Chapman.Cuali, family = binomial)`: Fits a logistic regression model with `Coronarios` as the response variable and `Edad`, `Colesterol`, `Presion`, and `IMC` as predictors.

### Stepwise Model Selection:

- `Ajuste.Step <- step(Ajuste.0, scope = list(lower = Coronarios ~ 1, upper = Coronarios ~ Edad + Colesterol + Presion + IMC), direction = "both")`: Performs stepwise model selection starting from the null model (`Ajuste.0`). The function iteratively adds or removes predictors within the specified scope to find an optimal model.

### Model Summary and Fitted Values:

- `summary(Ajuste.Step)`: Displays a summary of the stepwise-selected logistic regression model, including coefficients and their significance.
- `fitted.values(Ajuste.Step)[1:10]`: Retrieves the first 10 fitted values (predicted probabilities) from the stepwise-selected model.

### Prediction for New Data:

- `Nuevo.Chapman <- data.frame(c(66, 66, 66), c(200, 220, 300), c("Normal", "Sobrepeso", "Normal"))`
- `names(Nuevo.Chapman) <- c("Edad", "Colesterol", "IMC")`
- These two commands create a new DataFrame `Nuevo.Chapman` with specified values for prediction.
- `predict(Ajuste.Step, type = "response", newdata = Nuevo.Chapman)`: Predicts `Coronarios` for the new data using the stepwise-selected model.

### Goodness-of-Fit Test:

- `library(ResourceSelection)`
- `hoslem.test(Chapman.Cuali$Coronarios, fitted.values(Ajuste.Step))`: Performs the Hosmer-Lemeshow goodness-of-fit test for the stepwise-selected model.

### Confidence Intervals and Odds Ratios:

- `confint.default(Ajuste.Step)`: Calculates the confidence intervals for the model parameters.
- `exp(confint.default(Ajuste.Step))`: Exponentiates the confidence intervals to interpret them as odds ratios.

### Residuals and Diagnostic Measures:

- Various commands calculate and display different types of residuals (`pearson` and `deviance`), standardized residuals, hat values (leverage), and Cook's distance for the first 10 observations. These are used for diagnosing the model fit and identifying influential observations.

### ROC Curve Analysis:

- `library(pROC)`
- `CurvaROC.Step <- roc(Chapman.Cuali$Coronarios, fitted.values(Ajuste.Step))`: Calculates the Receiver Operating Characteristic (ROC) curve for the model.
- `plot(CurvaROC.Step)`: Plots the ROC curve to assess the model's diagnostic ability.

