---
layout: page
title: Python画图笔记
description: >
hide_description: true
sitemap: false
---

这里记录了一些画图模块

1. 列表（占位作用）
{:toc}

## 直方图

### seaborn画帕累托图
~~~python
import matplotlib.ticker as mtick
import seaborn as sns

sns.set_theme(style="whitegrid")
ax=sns.histplot(data=df[df.profit>0.01], x="profit",log_scale=True,binwidth=1,alpha=0.5,edgecolor="w")
ax.set_xticks([1e-3,1e-2,0.1,1,10,100,1000,1e4,1e5])
ax.set_xlim([1e-3,1e5])
#add percentage labels
label_data=ax.containers[0].datavalues/sum(ax.containers[0].datavalues)*1
labels=[f'{v*100:0.1f}%' for v in label_data]
ax.bar_label(ax.containers[0],labels=labels)
#turn minor tick off on x-axis
ax.tick_params(axis='x', which='minor', bottom=False)
ax.set_ylim([0,1e5])
ax2 = plt.twinx()
ax2=sns.ecdfplot(data=df, x="profit",color="orange")
#format the 2nd y-axis in percentage
vals = ax2.get_yticks()
ax2.set_yticklabels(['{:,.0%}'.format(i) for i in vals])
~~~
![Full-width image](/assets/img/blog/cumhist.png)

<!--- Continue with [Scripts](scripts.md){:.heading.flip-title}-->
{:.read-more}


