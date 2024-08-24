# pata_nahi_mereko


import pandas as pd
import matplotlib.pyplot as plt

a = pd.read_csv("C:/Users/Sonu/OneDrive/Desktop/googleplaystore.csv")

df = pd.DataFrame(a)
print(df)

print(a.info())
a.describe()

print(a.isnull().sum())

print(a["App"].duplicated().sum())

print(a.drop_duplicates("App"))

df[["Type", "Content Rating","Current Ver","Android Ver"]] = df[["Type", "Content Rating","Current Ver","Android Ver"]].fillna("Earlier Empty")

mean_value = df["Rating"].mean()

print(mean_value)

df.fillna({"Rating":mean_value},inplace=True)
print(df)

# Remove commas and plus signs
df['Installs'] = pd.to_numeric(df['Installs'].str.replace(',', '').str.replace('+', ''), errors='coerce')

# Fill NaN values with 0 or another placeholder if needed
df['Installs'] = df['Installs'].fillna(0)

print(df)



gb = df.groupby("Category").agg({"Installs":["count","sum"]})
print(gb)


# #adding new columns
gb.columns = ["Number of apps for this category", "Total installs for this category of apps"]
gb = gb.reset_index()
df = df.merge(gb, on = "Category", how = "left")
print(df)

#visualisation
plt.bar(df["Category"], df["Number of apps for this category"])
plt.xticks(rotation = 90)
plt.xlabel("Category")
plt.ylabel("Number Of Apps")
plt.title("Number Of Apps For Each Category")
plt.show()




plt.bar(df["Category"],df["Total installs for this category of apps"])
plt.xticks(rotation = 90)
plt.xlabel("Category Of Apps")
plt.ylabel("Number Of Downloads")
plt.title("Total Number Of Downloads For Each Category Of Apps")
plt.show()


#now calculating total installs based on content rating - everyone, teen, mature(17+)

gb1 = df.groupby("Content Rating").agg({"Installs":"sum"})
print(gb1)


#visualising
plt.bar(gb1.index, gb1["Installs"])
plt.xticks(rotation = 90)
plt.xlabel("Content Rating Type")
plt.ylabel("Total Downloads")
plt.show()
