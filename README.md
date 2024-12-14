# IPL-2025-MEGA-AUCTION-

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

IPL_df = pd.read_csv("C:/Users/vishalappu/Downloads/ipl_2025_auction_players.csv")
IPL_df

IPL_df.info()

IPL_df.head()

IPL_df.isnull().sum().sum()

IPL_df.describe()

IPL_df["Sold"] = IPL_df["Sold"].fillna("TBA")
IPL_df['Sold_numeric'] = pd.to_numeric(IPL_df['Sold'], errors='coerce')
IPL_df['Sold_numeric']

import plotly.express as px
import plotly.graph_objects as go

player_type = IPL_df['Type'].value_counts()
player_type

fig_1=px.pie(
   names = player_type.index,
    values = player_type.values,
    title = "Player Type",
    hole = 0.4
)
fig_1.show(renderer='notebook')

team_contributuon = IPL_df['Team'].value_counts()
team_contributuon

Fig_2 = px.bar(
     x = team_contributuon.index,
     y = team_contributuon.values,
    title = "Team_cont",
    labels={"x":"Teams", "y":"No of Players"},
        text = team_contributuon.values

)
Fig_2.update_traces(textposition='outside')
Fig_2.show(renderer='notebook')

Base_cont = IPL_df['Base'].value_counts().sort_index()
Base_cont

Fig_3 = px.bar(
     x = Base_cont.index,
     y = Base_cont.values,
    title = "Base_price contibutuon",
    labels={"x":"Base price", "y":"No of Players"},
    text = Base_cont.values
)
Fig_3.update_traces(textposition='outside')
Fig_3.show(renderer='notebook')

fig4 = px.scatter(
    IPL_df.dropna(subset=['Sold_numeric']),
    x='Base',
    y='Sold_numeric',
    color='Type',
    title="Base Price vs Sold Price",
    labels={'Base': 'Base Price', 'Sold_numeric': 'Sold Price'}
)


Fig_4 = px.scatter(
    IPL_df.dropna(subset=['Sold_numeric']),
    x ="Base",
    y = "Sold_numeric",
    color="Type",
    title = "Base_price VS Sold_price",
    labels = {"Base" : "Base price", "Sold_numeric":"Sold price"}
)

Fig_4.show(renderer = "notebook")

IPL_df['Base_1'] = pd.to_numeric(IPL_df['Base'], errors='coerce').sort_index()
IPL_df['Base_1']

IPL_df["Sold_numeric"] = pd.to_numeric(IPL_df["Sold_numeric"],errors="coerce").fillna(0).sort_index()
IPL_df["Sold_numeric"]

IPL_df.head()

IPL_df["price_distri"] = IPL_df["Sold_numeric"] - IPL_df['Base_1']
IPL_df["price_distri"]
Value_players = IPL_df[IPL_df["price_distri"].notna()]
Value_players

Fig_5 = px.bar(
Value_players,
x = "Players",
y = "price_distri",
color = "Type",
title = "Value of Money : Sold Vs Base",
labels={'Price_Difference': 'Price Difference'}
)
Fig_5.show(renderer='iframe_connected')



Fig_5 = px.bar(
Value_players,
x = "Players",
y = "price_distri",
color = "Type",
title = "Value of Money : Sold Vs Base",
labels={'Price_Difference': 'Price Difference'},
hover_data = ["Base", "Sold_numeric"]
)
Fig_5.show(renderer='iframe_connected')



tba_count = IPL_df["Sold"].value_counts().get("TBA",0)
tba_count

X = IPL_df.drop(columns = ['Sold'])
y = IPL_df['Sold'].value_counts().get("TBA")
y

tba_perce = (tba_count/len(IPL_df))*100
tba_perce

print(f"Number of players with 'TBA' status: {tba_count} ({tba_perce:.2f}%)")


{f"No of Players with TBA Statues : {tba_count}({tba_perce:.0f}%)"}
