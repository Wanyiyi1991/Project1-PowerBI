# 📊 零售销售仪表盘 — Power BI 项目

[![Power BI](https://img.shields.io/badge/Power_BI-Desktop-yellow)](https://powerbi.microsoft.com/)
[![Data Source](https://img.shields.io/badge/Data-Sample_Superstore-blue)](https://www.kaggle.com/datasets)
[![Status](https://img.shields.io/badge/Status-Complete-green)]()

> 基于 Superstore 数据集构建的 3 页交互式销售仪表盘，涵盖 KPI 概览、地区分析、产品与客户洞察，支持切片器联动和页面钻取。

---

## 📊 项目概览

使用 Power BI Desktop 对某零售企业 2014-2017 年的 9,994 条订单进行可视化分析。通过 Power Query 清洗数据、DAX 构建度量值、设计 3 页交互式仪表盘，最终输出一份可供业务决策的销售诊断报告。

## 🎯 业务问题

| 序号 | 问题 | 分析页面 |
|------|------|---------|
| 1 | 整体销售与利润趋势如何？是否有季节性？ | 概览页 |
| 2 | 哪些品类/子品类在赚钱，哪些在亏损？ | 概览页 |
| 3 | 亏损集中在哪些地区？ | 地区分析页 |
| 4 | 不同客户细分群体的贡献如何？ | 产品与客户页 |
| 5 | 折扣与利润的关系是什么？ | 产品与客户页 |

## 🔍 核心发现

1. **Furniture 品类是利润黑洞**：销售额占比不低但利润极低，其中 Tables 子品类累计亏损最大
2. **高折扣直接导致亏损**：折扣超过 30% 的订单中，68% 为亏损订单
3. **Central 地区问题突出**：德克萨斯州、伊利诺伊州利润为负，是亏损重灾区
4. **9 月旺季利润反常下降**：销售额高峰但利润低谷，促销结构需优化
5. **季节性规律明显**：11-12 月为销售高峰，1-2 月为低谷

## 🛠 技术栈

| 工具 | 用途 |
|------|------|
| **Power Query** | 数据清洗：类型转换、删除空值、筛选列 |
| **DAX** | 度量值：SUM、DIVIDE、CALCULATE、时间智能函数 |
| **数据建模** | 日期表创建、表关系建立（星型模型） |
| **可视化** | KPI卡片、折线图、柱状图、条形图、地图、散点图、饼图、表格 |
| **交互设计** | 切片器联动、页面导航、跨页同步筛选 |

## 📁 项目结构
Project1-PowerBI/
├── README.md ← 本文件
├── 零售销售仪表盘.pbix ← Power BI 源文件
├── 零售销售仪表盘.pdf ← 导出的 PDF 报告
├── data/
│ └── Sample - Superstore.xlsx ← 原始数据
└── screenshots/
├── page1_overview.png ← 概览页截图
├── page2_region.png ← 地区分析页截图
└── page3_product.png ← 产品与客户页截图

## 📈 仪表盘页面

### 第 1 页：KPI 概览
- 4 个 KPI 卡片：总销售额、总利润、订单数量、利润率
- 月度销售与利润双轴趋势图
- 品类利润柱状图
- 子品类利润条形图（正负值对比）

### 第 2 页：地区分析
- 美国各州利润着色地图
- 地区与州级利润明细表格（含条件格式）

### 第 3 页：产品与客户
- 产品散点图（销售额 vs 利润，气泡=订单量）
- 客户细分饼图
- 折扣与利润关系堆积柱状图

## 🧠 学习收获

- **Power BI 完整工作流**：从 Power Query 数据清洗 → 数据建模 → DAX 度量值 → 可视化设计 → 交互发布
- **星型模型建模**：理解事实表与维度表的关系设计，掌握一对多关联
- **DAX 核心函数**：掌握 SUM、DIVIDE、CALCULATE、PREVIOUSMONTH、SAMEPERIODLASTYEAR、DATESYTD 等核心函数
- **时间智能计算**：实现环比增长、同比增长、年初至今累计等常见业务指标
- **仪表盘设计原则**：概览→下钻→明细的信息层级，切片器联动的一致性体验
- **业务思维训练**：从数据可视化中提炼业务洞察，输出可执行的建议

## 🚀 快速复现

1. 下载并安装 [Power BI Desktop](https://www.microsoft.com/store/productId/9NTXR16HNW1T0)
2. 从 Kaggle 下载 `Sample - Superstore.xls` 数据集
3. 打开 Power BI → 获取数据 → Excel → 选择数据文件
4. 在 Power Query 中清洗数据（类型转换、删除无用列）
5. 创建日期表（DAX 公式见下方）
6. 建立表关系（Order Date ↔ Date）
7. 创建度量值（DAX 公式见下方）
8. 设计可视化仪表盘

## 📝 关键 DAX 公式

```dax
-- 日期表
日期表 = 
ADDCOLUMNS(
    CALENDAR(MIN(Orders[Order Date]), MAX(Orders[Order Date])),
    "年份", YEAR([Date]),
    "月份", MONTH([Date]),
    "季度", "Q" & QUARTER([Date]),
    "年-月", FORMAT([Date], "YYYY-MM")
)

-- 基础度量值
总销售额 = SUM(Orders[Sales])
总利润 = SUM(Orders[Profit])
利润率% = DIVIDE([总利润], [总销售额])

-- 时间智能
环比增长% = DIVIDE([总销售额] - [上月销售额], [上月销售额])
同比增长% = DIVIDE([总销售额] - [去年同月销售额], [去年同月销售额])
年初至今销售额 = CALCULATE([总销售额], DATESYTD('日期表'[Date]))

🔗 相关链接
数据来源：Kaggle - Sample Superstore

Power BI 下载：Microsoft Power BI Desktop

⭐ 如果这个项目对你有帮助，欢迎给个 Star！
