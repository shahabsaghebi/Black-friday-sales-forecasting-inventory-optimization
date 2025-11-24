# Black-friday-sales-forecasting-inventory-optimization
Black Friday Sales Forecasting &amp; Inventory Optimization Project

import pandas as pd
import numpy as np
from datetime import datetime, timedelta

# Parameters
start_date = datetime(2024, 6, 1)
end_date = datetime(2024, 11, 30)
num_items = 15
categories = ['Electronics', 'Clothing', 'Home']

# Generate date range
dates = pd.date_range(start_date, end_date)

# Create item info
item_ids = [f'Item_{i+1}' for i in range(num_items)]
item_categories = np.random.choice(categories, num_items)
base_prices = np.random.uniform(20, 500, num_items)

rows = []
for d in dates:
    week = d.isocalendar().week
    is_holiday = 1 if (d.month == 11 and 22 <= d.day <= 30) else 0  # Black Friday week
    for i in range(num_items):
        category = item_categories[i]
        base_price = base_prices[i]
        # Discount more aggressive during Black Friday week
        discount = np.random.uniform(0, 0.6) if is_holiday else np.random.uniform(0, 0.2)
        price = base_price * (1 - discount)
        # Seasonality + random noise
        season_factor = 1 + 0.3 * np.sin(2 * np.pi * (week / 52))
        holiday_boost = 2.5 if is_holiday else 1
        units_sold = int(np.random.normal(loc=20 * season_factor * holiday_boost, scale=5))
        revenue = round(units_sold * price, 2)
        rows.append([d, item_ids[i], category, units_sold, round(base_price,2), round(discount*100,2), is_holiday, revenue, week])

# Create df
df = pd.DataFrame(rows, columns=['date','item_id','category','units_sold','price','discount_percent','is_holiday','revenue','week_number'])

# Save CSV
path = '/mnt/data/black_friday_sales.csv'
df.to_csv(path, index=False)
path
