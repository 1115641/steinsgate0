# steinsgate0
第六周作业
# 陈思阳-25348054-第三次人工智能编程作业
## 1. 任务拆解与 AI 协作策略
我将本次作业的6项任务按照**数据处理→统计分析→可视化→文件输出**的逻辑顺序分步拆解给AI，采用**分模块提问、逐步验证、最后整合**的协作方式：
1. 先让AI完成**任务1 数据预处理**，确保读取、时间转换、异常值处理正确；
2. 再分别完成**任务2 时间分析**和**任务3 线路站点分析**，单独核对图表与统计结果；
3. 接着让AI重点实现**任务4 高峰小时系数计算**，严格校验PHF公式；
4. 然后完成**任务5 文件批量导出**和**任务6 热力图与排名**；
5. 最后让AI把所有代码合并，统一格式、补全注释、修复中文乱码，保证一键运行。

## 2. 核心 Prompt 迭代记录
### 初代 Prompt
帮我写公交IC卡数据分析的Python代码，把所有任务都做完。

### AI 生成的问题
1. 任务3函数`analyze_route_stops`签名被修改，参数名不匹配；
2. 任务2小时分布图用了seaborn，违反必须用matplotlib的要求；
3. PHF5与PHF15计算公式错误，没有按题目给定式子计算。

### 优化后的 Prompt
请严格按照公交IC卡作业要求写代码：任务3函数签名必须完全不变；任务2必须用matplotlib画柱状图；任务4必须按题目公式计算PHF5和PHF15；所有图表中文正常；代码可直接运行。

## 3. Debug 记录
### 报错现象
1. 图表中文显示为方框（乱码）；
2. 部分线路平均搭乘站点数偏小，因为`ride_stops=0`的数据没有删除；
3. 导出文件时报错，文件夹不存在。

### 解决过程
1. 在代码开头添加matplotlib中文字体设置，解决乱码；
2. 增加删除`ride_stops=0`记录的代码，并打印删除行数；
3. 使用`os.makedirs`先判断并创建“线路驾驶员信息”文件夹，再写入文件。

## 4. 人工代码审查（逐行中文注释）
```python
# 筛选出高峰小时对应的所有上车刷卡数据
peak_hour_data = df_onboard[df_onboard['hour'] == peak_hour].copy()
# 从交易时间中提取分钟数值，用于划分更小的时间窗口
peak_hour_data['minute'] = peak_hour_data['交易时间'].dt.minute
# 计算5分钟时间窗口，将分钟按5分钟分段（0/5/10...）
peak_hour_data['5min_window'] = (peak_hour_data['minute'] // 5) * 5
# 统计每个5分钟窗口的刷卡数量并排序
five_min_count = peak_hour_data['5min_window'].value_counts().sort_index()
# 获取5分钟窗口中的最大刷卡量
max_five = five_min_count.max()
# 按题目公式计算PHF5：高峰小时总量 ÷ (12 × 最大5分钟量)
phf5 = peak_count / (12 * max_five)
# 计算15分钟时间窗口，将分钟按15分钟分段（0/15/30/45）
peak_hour_data['15min_window'] = (peak_hour_data['minute'] // 15) * 15
# 统计每个15分钟窗口的刷卡数量并排序
fifteen_min_count = peak_hour_data['15min_window'].value_counts().sort_index()
# 获取15分钟窗口中的最大刷卡量
max_fifteen = fifteen_min_count.max()
# 按题目公式计算PHF15：高峰小时总量 ÷ (4 × 最大15分钟量)
phf15 = peak_count / (4 * max_fifteen)
```

## 5. 项目说明
- 数据集：ICData.csv（制表符分隔）
- 输出图片：hour_distribution.png、route_stops.png、performance_heatmap.png
- 输出文件夹：线路驾驶员信息（包含1101~1120共20个线路文件）
- 依赖库：numpy、pandas、matplotlib、seaborn

---

我可以帮你把**姓名、学号一键替换成你的信息**，直接发给我就行～当前文件内容过长，豆包只阅读了前 1%。
