# survival-analysis
###construct input for survival analysis, get the number of days from HF index date to death date.

```
import pandas as pd
import numpy as np 
df1=pd.read_csv("/home/ydw529/other/phenotype0720/pipeline_runs/HF-only1215/results/NTF_files/patient_ids/HF-only_alpha_1.0_gamma_0.001_0.08_0.07-4.dat_patient_id.csv",header=None)
df1.columns=['group','mrd_id']
df2=pd.read_csv("/home/ydw529/other/data_preprocessing/data_csv_process/downloaded/limited_HF_Cohort_Demographics_CASE_1215.csv")
merge=pd.merge(df1,df2,how='left',left_on='mrd_pt_id', right_on='mrd_pt_id')
merge['death_dts'].fillna('2016-11-24',inplace=True)
merge['days_diff']=pd.to_datetime(merge['death_dts'])-pd.to_datetime(merge['HF_index_date'])
merge['death_index_flg'].fillna(0,inplace=True)
merge['days_diff']=merge['days_diff'].dt.days
df=merge[['group','days_diff','death_index_flg']]
df.columns=['group','T','E']
df.to_csv('/home/ydw529/other/phenotype0720/survival_analysis/HF-only_DEMO_case_1215.csv')
df1=merge[['group','gender_nm','racee_nm','deid_age_at_index_HF_dx','death_index_flg','days_diff']]
df1.to_csv('/home/ydw529/other/phenotype0720/survival_analysis/HF-only_DEMO_case_1215_with_race_etc.csv')
```
##plot survival curve
```
from lifelines import KaplanMeierFitter
kmf = KaplanMeierFitter()
import pandas as pd
import os
os.getcwd()
#edit the directory according to the directory of your file 
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
#g0 is phenotypes 
g0 = df.group==0
T = df[g0]['T']
C = df[g0]['E']

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
kmf.fit(T, event_observed=C, label=['phenotype1'])
kmf.survival_function_.plot(ax=ax)
kmf.fit(T2, event_observed=C2, label=['phenotype2'])
kmf.survival_function_.plot(ax=ax)
kmf.fit(T3, event_observed=C3, label=['phenotype3'])
kmf.survival_function_.plot(ax=ax)
kmf.fit(T4, event_observed=C4, label=['phenotype4'])
kmf.survival_function_.plot(ax=ax)
kmf.fit(T5, event_observed=C5, label=['phenotype5'])
kmf.survival_function_.plot(ax=ax)
plt.title('Kaplan Meier Survival Curve among Phenotypes')
kmf2 = plt.gcf()
```

###logrank test
```
from lifelines.statistics import logrank_test
summary_= logrank_test(T, T2, C, C2, alpha=0.95)
```
###result
when using 7 phenotypes, phenotype1 and phenotype 5 have significant difference. 

###code for generate plot for abstract
```
df=pd.read_csv("HF-only_7.dphenotypes_DEMO_case_1215_test.csv")
T = df["T"]
C = df["E"]
kmf.fit(T, event_observed=C )
import matplotlib.pyplot as plt 
kmf.survival_function_.plot()
plt.title('Survival function of political regimes')
%matplotlib inline
kmf.plot()
#g0 is phenotypes 
g0 = df.group==0
T1 = df[g0]['T']
C1 = df[g0]['E']

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


g5 = df.group==5
T6 = df[g5]['T']
C6 = df[g5]['E']


g6 = df.group==6
T7 = df[g6]['T']
C7 = df[g6]['E']

ax = plt.subplot(111)
kmf.fit(T1, event_observed=C1, label=['phenotype1'])
kmf.survival_function_.plot(ax=ax)
#kmf.fit(T2, event_observed=C2, label=['phenotype2'])
#kmf.survival_function_.plot(ax=ax)
#kmf.fit(T3, event_observed=C3, label=['phenotype3'])
#kmf.survival_function_.plot(ax=ax)
#kmf.fit(T4, event_observed=C4, label=['phenotype4'])
#kmf.survival_function_.plot(ax=ax)
kmf.fit(T5, event_observed=C5, label=['phenotype5'])
kmf.survival_function_.plot(ax=ax)
#kmf.fit(T6, event_observed=C6, label=['phenotype6'])
#kmf.survival_function_.plot(ax=ax)
#kmf.fit(T7, event_observed=C7, label=['phenotype7'])
#kmf.survival_function_.plot(ax=ax)
plt.title('Kaplan Meier Survival Curve among Phenotypes')
kmf2 = plt.gcf()
```
