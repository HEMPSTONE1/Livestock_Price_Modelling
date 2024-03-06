# Import necessary libraries
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.linear_model import LinearRegression
from datetime import datetime

# Loading the data from Excel
data = pd.read_excel('livestock_prices.xlsx')

# Display the first few rows of the data
print(data.head())

# Performing exploratory analysis
plt.figure(figsize=(10, 6))
plt.plot(data['Dates'], data['Bull'], label='Bull', marker='o')
plt.plot(data['Dates'], data['Cow'], label='Cow', marker='o')
plt.plot(data['Dates'], data['Heifer'], label='Heifer', marker='o')
plt.plot(data['Dates'], data['Steer'], label='Steer', marker='o')
plt.xlabel('Date')
plt.ylabel('Price')
plt.title('Livestock Prices Over Time')
plt.legend()
plt.grid(True)
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()

# Convert Dates column to datetime format
data['Dates'] = pd.to_datetime(data['Dates'])

# Extract year and month from Dates column
data['Year'] = data['Dates'].dt.year
data['Month'] = data['Dates'].dt.month

# Fit a linear regression model to predict Bull prices
model = LinearRegression()
model.fit(data[['Year', 'Month']], data['Bull'])

# Predict Bull prices using the model
data['Bull_predicted'] = model.predict(data[['Year', 'Month']])

# Plot actual vs. predicted Bull prices
plt.figure(figsize=(10, 6))
plt.plot(data['Dates'], data['Bull'], label='Actual', marker='o')
plt.plot(data['Dates'], data['Bull_predicted'], label='Predicted', linestyle='--')
plt.xlabel('Date')
plt.ylabel('Bull Price')
plt.title('Actual vs. Predicted Bull Prices')
plt.legend()
plt.grid(True)
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
