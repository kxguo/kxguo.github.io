---
layout: page
title: Python画图笔记
description: >
  这里记录了一些画图模块
hide_description: false
sitemap: false
---

1. 列表（占位作用）
{:toc}

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

### 散点等密度图
~~~python
fig,ax=plt.subplots()
ax.scatter(nondata.cospa,nondata.profit,alpha=0.3,s=20)
ax.scatter(data.cospa,data.profit,alpha=0.7,s=20)
ax.legend(['non-data','data'])

#countor plot
nbins=30
k=kde.gaussian_kde((np.log10(data.cospa), np.log10(data.profit)))
xi, yi = np.mgrid[np.log10(data.cospa.min()):np.log10(data[n0].cospa.max()):nbins*1j
                  , np.log10(data.profit.min()):np.log10(data[n0].profit.max()):nbins*1j]
zi=k(np.vstack([xi.flatten(),yi.flatten()]))
ax.contour(10**xi,10**yi,zi.reshape(xi.shape),cmap="Greys")

ax.set_xscale('log')
ax.set_yscale('log')
ax.set_xlabel('x-label')
ax.set_ylabel('y-label')
plt.tight_layout()
plt.draw()
~~~
![Full-width image](/assets/img/blog/loglincont.png)

### 上色散点图两侧叠加密度histogram
~~~python
df=pd.DataFrame({'a':np.log10(data.a),'b':np.log10(data.b),'c':np.log10(data.c)})
sns.set_style("white")
#regression line together with histogram
g=sns.jointplot(ax=ax, data=df,x="a",y="b",kind="reg"
                ,scatter=False
                #change hist style
                ,marginal_kws=dict(bins=25, fill=False, color='grey')
                #change line color and style
                ,joint_kws={'color':'grey'},line_kws={'alpha':0.5,'linestyle':'dashed'}
                #height is the only way to change fig size
                ,height=6, ratio=6)
#calculate linear regression parameters
slope,intercept,r,p,std=stats.linregress(df.a,df.b)
phantom, =g.ax_joint.plot([], [],linestyle="", alpha=0)
#annotate fitting parameters
g.ax_joint.legend([phantom],['log_a={:.2f}, log_b={:.2f}, r={:.2f}, p={:.2f}'.format(slope,intercept,r,p)])

#plot scatter and color bar
im=g.ax_joint.scatter(df.a,df.b,alpha=0.2,c=df.c,cmap='plasma')
cbar_ax=g.fig.add_axes([0.5,.2,.32,.03])
cb=fig.colorbar(im, cax=cbar_ax,orientation="horizontal")
cb.ax.set_xlabel("bar-label",fontsize=12)

g.ax_joint.set_xlabel('x-label',fontsize=12)
g.ax_joint.set_ylabel('y-label',fontsize=12)
xlocs=g.ax_joint.get_xticks()
ylocs=g.ax_joint.get_yticks()
g.ax_joint.set_ylim([-3,8])
g.ax_joint.set_xticklabels(["%.e"%10**i for i in xlocs],fontsize=12)
g.ax_joint.set_yticklabels(["%.e"%10**i for i in ylocs],fontsize=12)
plt.tight_layout() 
plt.draw()
~~~
![Full-width image](/assets/img/blog/hisbar.png)


Continue with [常用工具](tools.md){:.heading.flip-title}
{:.read-more}

Back to [Some Notes](README.md){:.heading.flip-title}


