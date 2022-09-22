---
layout: post
title: Google Cloud 部署和data studio可视化相关
tags: [BI,Spanner,Data Analysis]
image:
  path: /assets/img/blog/cat_0827.jpeg
description: >
  使用谷歌全家桶部署spanner sql查询以及后续BI搭建时遇到的坑
sitemap: false
---

快速定位：[Cloud Function Trouble Shouting](#cloud function trouble shouting)
, [数据洞察BI看板搭建](#数据洞察BI看板搭建)

- Table of Contents
{:toc .large-only}

## Cloud Function Trouble Shouting

### requirements.txt
为了部署Python查询spanner sql时的样例

~~~shell
functions-framework==3.*
requests==2.28.*
google-cloud-spanner==3.19.0
~~~

### main() takes 0 positional arguments but 1 was given
- 利用如下语句查询时遇到的错误

~~~python
def main():
	spanner_client = spanner.Client()
	...
~~~

- 问题在于实际上执行函数main传入参数不为空，因此加入一个参数占位即可

### The view function for 'run' did not return a valid response. The function either returned None or ended without a return statement.
- Cloud Function 需要返回值，最简单的做法是在主函数最后添加一条return

### Cloud Spanner不支持Insert Conflict语法
- 在插入表之前删除表数据（保留元数据）

- 两个问题最终样例sql如:

~~~python
from google.cloud import spanner

instance_id = "[instance name]"
database_id = "[database name]"

#无传入参数时，在main后加入参数占位
def main(data):

    spanner_client = spanner.Client()
    instance = spanner_client.instance(instance_id)
    database = instance.database(database_id)
	
	#删除表数据
    remaining_none = spanner.KeySet(all_=True)
    with database.batch() as batch:
        batch.delete("[table_name]",remaining_none)
	
	#插入新数据
    database.run_in_transaction(insert)
	#给出返回值
    return "OK"

def insert(transaction):
	
	row_ct=transaction.execute_update(
	"[SQL LINES1]"+\
	"[SQL LINES2]")
~~~

## 数据洞察BI看板搭建

### trouble shooting
- 连接数据源-> 报告，或实验阶段连接数据源->探索，但探索内容不会保存为报告
- 连接spanner的sql语法与spanner相同，但查询中整形数据列必须强制转换为float
，否则看板搭建时会无法显示并且提示错误
- sql查询的时间，timestamp可以被识别为datetime格式，但无法画出时间序列
- 下图中的时间序列展示方式为：连接数据源时，将sql查询的时间以string形式导入
，并画折线图
- 连接后修改字段，也可以在实际使用时增加字段，没有区别

### 展示某产品研发阶段各版本迭代效果的示例图
![Full-width image](/assets/img/blog/bi_datastudio.png)
