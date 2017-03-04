# survival-analysis
```
from lifelines import KaplanMeierFitter
kmf = KaplanMeierFitter()
import pandas as pd
import os
os.getcwd()
os.chdir("/Users/yudeng/Dropbox/")
df=pd.read_csv("survival_curve.csv")
df.head()
T = df["T"]
C = df["E"]
kmf.fit(T, event_observed=C )
import matplotlib.pyplot as plt 
kmf.survival_function_.plot()
plt.title('Survival function of political regimes')
%matplotlib inline
kmf.plot()
#male
g0 = df.group==0
T = df[g0]['T']
C = df[g0]['E']
#female
g1 = df.group==1
T2 = df[g1]['T']
C2 = df[g1]['E']

g2 = df.group==2
T3 = df[g2]['T']
C3 = df[g2]['E']

g3 = df.group==3
T4 = df[g3]['T']
C4 = df[g3]['E']

g4 = df.group==4
T5 = df[g4]['T']
C5 = df[g4]['E']

ax = plt.subplot(111)

kmf.fit(T, event_observed=C, label=['g0'])
kmf.survival_function_.plot(ax=ax)
kmf.fit(T2, event_observed=C2, label=['g1'])
kmf.survival_function_.plot(ax=ax)
kmf.fit(T3, event_observed=C3, label=['g2'])
kmf.survival_function_.plot(ax=ax)
kmf.fit(T4, event_observed=C4, label=['g3'])
kmf.survival_function_.plot(ax=ax)
kmf.fit(T5, event_observed=C5, label=['g4'])
kmf.survival_function_.plot(ax=ax)
plt.title('group difference KM')
kmf2 = plt.gcf()
```
