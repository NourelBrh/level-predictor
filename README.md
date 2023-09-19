# level-predictor
import pandas as pd
import matplotlib.pyplot as plt
from scipy.stats import linregress

# Read the data
df = pd.read_csv("epa-sea-level.csv")

# Create a scatter plot
plt.figure(figsize=(10, 6))
plt.scatter(df["Year"], df["CSIRO Adjusted Sea Level"], label="Data", c="b")

# Perform linear regression on the entire dataset
slope, intercept, r_value, p_value, std_err = linregress(df["Year"], df["CSIRO Adjusted Sea Level"])

# Calculate the predicted sea level rise in 2050
x_predict = list(range(1880, 2051))
y_predict = [slope * x + intercept for x in x_predict]
plt.plot(x_predict, y_predict, "r", label=f"Line of Best Fit ({round(slope, 2)} inches/year)")

# Filter data from year 2000 onwards
df_recent = df[df["Year"] >= 2000]

# Perform linear regression on recent data
slope_recent, intercept_recent, _, _, _ = linregress(df_recent["Year"], df_recent["CSIRO Adjusted Sea Level"])

# Calculate the predicted sea level rise in 2050 for recent data
y_predict_recent = [slope_recent * x + intercept_recent for x in x_predict]
plt.plot(x_predict, y_predict_recent, "g", label=f"Recent Fit ({round(slope_recent, 2)} inches/year)")

# Set labels and title
plt.xlabel("Year")
plt.ylabel("Sea Level (inches)")
plt.title("Rise in Sea Level")

# Add legend
plt.legend()

# Save the plot and return it
plt.savefig("sea_level_plot.png")
# plt.show()  # Uncomment to display the plot

# Return the plot
plt.close()
