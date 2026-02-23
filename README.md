# Get-Basic-Sales-Summary-from-a-Tiny-SQLite-Database-using-Python

# ===============================
# Basic Sales Summary with SQLite
# ===============================

import sqlite3
import pandas as pd
import matplotlib.pyplot as plt

# -------------------------------
# 1. Connect to SQLite database
# -------------------------------
conn = sqlite3.connect("sales_data.db")
cursor = conn.cursor()

# -------------------------------
# 2. Create sales table
# -------------------------------
cursor.execute("""
CREATE TABLE IF NOT EXISTS sales (
    id INTEGER PRIMARY KEY,
    product TEXT,
    quantity INTEGER,
    price REAL
)
""")

# -------------------------------
# 3. Insert sample data
# -------------------------------
sample_data = [
    ("Laptop", 5, 800),
    ("Laptop", 3, 850),
    ("Phone", 10, 500),
    ("Phone", 6, 520),
    ("Tablet", 7, 300),
    ("Tablet", 4, 320)
]

cursor.executemany(
    "INSERT INTO sales (product, quantity, price) VALUES (?, ?, ?)",
    sample_data
)

conn.commit()

# -------------------------------
# 4. Run SQL Query
# -------------------------------
query = """
SELECT 
    product, 
    SUM(quantity) AS total_quantity,
    SUM(quantity * price) AS total_revenue
FROM sales
GROUP BY product
"""

df = pd.read_sql_query(query, conn)

# -------------------------------
# 5. Print Results
# -------------------------------
print("===== SALES SUMMARY =====")
print(df)

# -------------------------------
# 6. Create Bar Chart
# -------------------------------
df.plot(kind='bar', x='product', y='total_revenue', legend=False)
plt.title("Total Revenue by Product")
plt.ylabel("Revenue")
plt.xlabel("Product")
plt.tight_layout()


# Save chart
plt.savefig("sales_chart.png")

# Show chart
plt.show()

# -------------------------------
# 7. Close Connection
# -------------------------------
conn.close()



![image alt](https://raw.githubusercontent.com/Annu-Saini001/Get-Basic-Sales-Summary-from-a-Tiny-SQLite-Database-using-Python/e23e529c2e40459a6880283dd8f449e28da15178/image.png)
 
