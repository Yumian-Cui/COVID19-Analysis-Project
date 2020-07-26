import pandas as pd
import numpy as np
from datetime import datetime
import matplotlib.pyplot as plt

%matplotlib inline

df = pd.read_csv("us-counties.csv",parse_dates=True,index_col=0)

pd.options.display.max_rows = 10

df = df['20200304':'20200611']

df.reset_index(inplace=True)

df

t10 = df.groupby('state').sum().sort_values('cases', ascending=False).head(10)

t10 = t10.drop(columns='fips')

print (t10.index)
t10_list = ['New York', 'New Jersey', 'Illinois', 'California', 'Massachusetts',
       'Pennsylvania', 'Michigan', 'Texas', 'Florida', 'Louisiana']
t10

t_c = t10['cases'].to_list()
t_d = t10['deaths'].to_list()


x = np.arange(len(t10.index))  # the label locations
width = 0.35  # the width of the bars

fig, ax = plt.subplots(figsize=(12,5))
rects1 = ax.bar(x - width/2, t_c, width, label='cases')
rects2 = ax.bar(x + width/2, t_d, width, label='deaths')

# Add some text for labels, title and custom x-axis tick labels, etc.
ax.set_ylabel('number')
ax.set_title('Cases and deaths comparison across top 10 states')

ax.set_xticks(x)
ax.set_xticklabels(t10.index)

#ax.set_yticks(y)
#ax.set_yticklabels(y)
ax.legend()

fig.tight_layout()

plt.show()

# now I have obtained the top 10 states with cases confirmed. Next I want to plot the contrast between
# cases and deaths over time for each state. 

cov19_counties_bystate_cases = pd.Series(df.groupby(['state','date']).cases.sum())
cov19_counties_bystate_deaths = pd.Series(df.groupby(['state','date']).deaths.sum())
cov19_counties_bystate = pd.DataFrame(cov19_counties_bystate_cases)
cov19_counties_bystate['deaths'] = cov19_counties_bystate_deaths

cov19_counties_bystate_deaths = pd.Series(cov19_counties.groupby(['state','date']).deaths.sum())

cov19_counties_bystate = pd.DataFrame(cov19_counties_bystate_cases)

cov19_counties_bystate['deaths'] = cov19_counties_bystate_deaths


cov19_counties_bystate

from matplotlib.dates import DateFormatter
import matplotlib.dates as mdates

cov19_counties.date = pd.to_datetime(cov19_counties.date)

pp.figure(figsize=(12,12))

for i, state in enumerate(t10.index):
    pp.subplot(5,2,i+1) #5 rows, 2 columns
    
    pp.plot(cov19_counties_bystate.loc[state].cases,label='cases')
    pp.plot(cov19_counties_bystate.loc[state].deaths,label='deaths')

    pp.legend()
    pp.title(state)
    #pp.xticks(arange(4), calendar.month_name[3:6+1], rotation=45)

pp.tight_layout()

# drop rows
df = cov19_counties_bystate

ax = cov19_counties_bystate.loc['New York'].cases.plot.line(figsize=(12,5))
ax = cov19_counties_bystate.loc['New Jersey'].cases.plot.line(figsize=(12,5))
ax = cov19_counties_bystate.loc['Illinois'].cases.plot.line(figsize=(12,5))
#ax = cov19_counties_bystate.loc['California'].cases.plot.line(figsize=(12,5))'''


for state in t10.index[:4+1]:
  ax = cov19_counties_bystate.loc[state].cases.plot.line(figsize=(12,5))

ax.legend(t10.index[:4+1])
pp.title('states cases')

for state in t10.index[:4+1]:
  ax = cov19_counties_bystate.loc[state].deaths.plot.line(figsize=(12,5))

ax.legend(t10.index[:4+1])
pp.title('states deaths')

df[df.state == 'New York']

dfc = df[df.state == 'New York'].pivot_table(index = df[df.state == 'New York'].date, values='cases',aggfunc='sum')
dfd = df[df.state == 'New York'].pivot_table(index = df[df.state == 'New York'].date, values='deaths',aggfunc='sum')
ax = dfc.plot.line(figsize=(12,5),grid=True,rot=True)
dfd.plot.line(figsize=(12,5),grid=True,rot=True,ax=ax)
ax.set_xlabel("New York")
ax.set_ylabel("number")

def plotstate(state_name):
    dfc = df[df.state == state_name].pivot_table(index = df[df.state == state_name].date, values='cases',aggfunc='sum')
    dfd = df[df.state == state_name].pivot_table(index = df[df.state == state_name].date, values='deaths',aggfunc='sum')
    ax = dfc.plot.line(figsize=(12,5),grid=True,rot=True)
    dfd.plot.line(figsize=(12,5),grid=True,rot=True,ax=ax)
    ax.set_xlabel(state_name)
    ax.set_ylabel("number")

plotstate('New York')

for i, state in enumerate(t10.index):
    plotstate(state)

# the cases in every county of New York
df_county = df.set_index(['state','county']).sort_index()

df_co = df_county.loc['New York']
df_co

df_co.index.unique()

# get the top ten counties with cases confirmed
def getcountyc(state):
    return (df_county.loc[state].groupby(['county']).sum().sort_values('cases',ascending=False).head(10))
  

getcountyc('New York').index

df_county.loc['New York','New York City'].drop(columns=['fips','deaths'])

df4 = df[df.state == 'New York'].pivot_table(index = 'county', values='cases',aggfunc='sum')
df5 = df[df.state == 'New York'].pivot_table(index = 'county', values='deaths',aggfunc='sum')
df6 = df4.sort_values('cases',ascending=False).head(10)
df6.plot.bar(figsize=(12,5),grid=True,rot=True)

df.columns('New York')

df_county.loc['New York','New York City']

df_county.date

'''def plotcounty(county):
    ax = df_county.loc['New York',county].plot.line(x= 'date',y= 'cases',figsize=(12,5),grid=True,rot=True)
    df_county.loc['New York',county].plot.line(x= 'date',y= 'deaths',figsize=(12,5),grid=True,rot=True,ax=ax)
    ax.set_xlabel(county) 
    ax.set_ylabel("cases")'''  

def plotcounty():
    for i,county in enumerate(getcountyc('New York').index):
        ax = df_county.loc['New York',county].plot.line(x= 'date',y= 'cases',figsize=(12,5),grid=True,rot=True)
        df_county.loc['New York',county].plot.line(x= 'date',y= 'deaths',figsize=(12,5),grid=True,rot=True,ax=ax)
        ax.set_xlabel(county) 
        ax.set_ylabel("cases") 

plotcounty()

for i,county in enumerate(getcountyc('New York').index):
    plotcounty(county)


df3 = df.pivot_table(index='date',values='cases',aggfunc='sum')

ax = df3.plot.line(figsize=(12,5),grid=True,rot=True)

df4 = df.pivot_table(index='date',values='deaths',aggfunc='sum')

ax = df3.plot.line(figsize=(12,5),grid=True,rot=True)
df4.plot.line(figsize=(12,5),grid=True,rot=True,ax=ax)

