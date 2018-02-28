
### 统计学：检验心理学现象

在下方单元格中回答问题并执行相关代码，你可以 [参考项目指导](https://github.com/udacity/new-dand-advanced-china/blob/master/%E6%A3%80%E9%AA%8C%E5%BF%83%E7%90%86%E5%AD%A6%E7%8E%B0%E8%B1%A1/%E7%BB%9F%E8%AE%A1%E5%AD%A6%EF%BC%9A%E6%A3%80%E9%AA%8C%E5%BF%83%E7%90%86%E5%AD%A6%E7%8E%B0%E8%B1%A1.md) 并在正式提交前查看 [项目要求](https://review.udacity.com/#!/rubrics/305/view)。提交时请将 Jupyter notebook 导出成 HTML 或者 PDF 进行提交（File -> Download As）。

背景信息

在一个Stroop （斯特鲁普）任务中，参与者得到了一列文字，每个文字都用一种油墨颜色展示。参与者的任务是将文字的打印颜色大声说出来。这项任务有两个条件：一致文字条件，和不一致文字条件。在一致文字条件中，显示的文字是与它们的打印颜色匹配的颜色词，如“红色”、“蓝色”。在不一致文字条件中，显示的文字是与它们的打印颜色不匹配的颜色词，如“紫色”、“橙色”。在每个情况中，我们将计量说出同等大小的列表中的墨色名称的时间。每位参与者必须全部完成并记录每种条件下使用的时间。

调查问题

作为一般说明，请确保记录你在创建项目时使用或参考的任何资源。作为项目提交的一部分，你将需要报告信息来源。

(1) 我们的自变量是什么？因变量是什么？

* 自变量是"字义与颜色是否一致"，因变量是"读出墨色的时间"

(2) 此任务的适当假设集是什么？你需要以文字和数学符号方式对假设集中的零假设和对立假设加以说明，并对数学符号进行定义。你想执行什么类型的统计检验？为你的选择提供正当理由（比如，为何该实验满足你所选统计检验的前置条件）。

* 原假设 (H0)：字义与颜色不一致不会导致读出墨色的时间明显变长。μd = μ0
* 备择假设 (H1)：字义与颜色不一致会导致读出墨色的时间明显变长。μd > μ0
* 由于未知样本总体的标准差，且样本量不足30个，故选用采用t检验方法，
* 由于假设主要关注在测试时间是否变长，对于变短的情况不予考虑，且该检验是针对同一组测试者，在不同条件下的测试结果，故应该采用单边配对T检验方法
* 显著性水平选用0.05

现在轮到你自行尝试 Stroop 任务了。前往此链接，其中包含一个基于 Java 的小程序，专门用于执行 Stroop 任务。记录你收到的任务时间（你无需将时间提交到网站）。现在下载此数据集，其中包含一些任务参与者的结果。数据集的每行包含一名参与者的表现，第一个数字代表他们的一致任务结果，第二个数字代表不一致任务结果。

(3) 报告关于此数据集的一些描述性统计。包含至少一个集中趋势测量和至少一个变异测量。


```python
# 在这里执行你的分析
# reference the "python for data analysis - CHAPTER 6"
import pandas as pd
import matplotlib
from scipy import stats
from scipy.stats import ttest_rel
df = pd.read_csv('stroopdata.csv')
d_con = df.Congruent
d_inc = df.Incongruent
df.describe()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Congruent</th>
      <th>Incongruent</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>24.000000</td>
      <td>24.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>14.051125</td>
      <td>22.015917</td>
    </tr>
    <tr>
      <th>std</th>
      <td>3.559358</td>
      <td>4.797057</td>
    </tr>
    <tr>
      <th>min</th>
      <td>8.630000</td>
      <td>15.687000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>11.895250</td>
      <td>18.716750</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>14.356500</td>
      <td>21.017500</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>16.200750</td>
      <td>24.051500</td>
    </tr>
    <tr>
      <th>max</th>
      <td>22.328000</td>
      <td>35.255000</td>
    </tr>
  </tbody>
</table>
</div>



该数据集的描述性统计指标如上图所示

(4) 提供显示样本数据分布的一个或两个可视化。用一两句话说明你从图中观察到的结果。


```python
# 在这里创建可视化图表
df.plot.box()
```




![](https://github.com/erickzy/ND002_DA_PROJECT1_Psychology phenomenon/raw/master/img/output_8_1.png)




箱体图可以很好的描述数据分布的状态，从上图中可以看到：
* 信息一致的条件下所需测试时间的均值低于不一致条件下测试时间
* 条件不一致的情况下所测得的结果有更高的集中度

(5) 现在，执行统计测试并报告你的结果。你的置信水平和关键统计值是多少？你是否成功拒绝零假设？对试验任务得出一个结论。结果是否与你的期望一致？


```python
# 在这里执行统计检验
stats.ttest_rel(d_con, d_inc, axis=0)
```




    Ttest_relResult(statistic=-8.020706944109957, pvalue=4.1030005857111781e-08)



设置置信水平为95%
p-value = 4.1030005857111781e-08 该值远远小于5%，且statistic=-8.020706944109957，说明μd > μ0，故可以拒绝原假设。故备选假设成立。
### 结论：字义与颜色不一致会导致读出墨色的时间明显变长。


