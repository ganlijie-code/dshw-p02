# P02：金融数据获取、管理与初步分析

## 股票列表

| 代码 | 名称 | 行业 | 选股理由 |
|------|------|------|---------|
| 601398 | 工商银行 | 银行 | 国有大行龙头，代表金融板块β较低、分红稳定的防御特征 |
| 600036 | 招商银行 | 银行 | 零售银行标杆，与工行形成国有/股份制对照 |
| 002594 | 比亚迪 | 汽车 | 新能源车全球龙头，高成长、高波动，适合观察周期与创新溢价 |
| 601633 | 长城汽车 | 汽车 | 传统车企转型代表，与比亚迪形成新能源产业链对照 |
| 000002 | 万科A | 房地产 | 地产行业标杆，便于分析政策周期与系统性风险 |
| 600519 | 贵州茅台 | 白酒 | A股“股王”，消费龙头，长期超额收益典型 |
| 000858 | 五粮液 | 白酒 | 高端白酒第二梯队，与茅台构成行业内对比 |
| 601857 | 中国石油 | 能源 | 能源央企代表，油价与通胀敏感，周期性显著 |
| 600941 | 中国移动 | 通讯 | 运营商龙头，现金流稳定、防御属性强 |
| 002352 | 顺丰控股 | 物流 | 快递物流龙头，反映消费与电商景气度 |

覆盖行业：**银行、汽车、房地产、白酒、能源、通讯、物流**（7 个，满足至少 5 个要求）。

## 数据来源

- **股票行情**：[akshare](https://github.com/akfamily/akshare) `stock_zh_a_hist()`，**后复权**，日度，2020-01-01 至今
- **市场指数**：akshare `stock_zh_index_daily_em()`；沪深300（000300，CAPM 基准）+ 创业板指（399006，代表成长风格，与大盘对照）
- **宏观指标**：
  - CPI 同比增速（必选）：`macro_china_cpi_monthly()`
  - M2 同比增速（自选）：`macro_china_money_supply()` — 货币供应影响流动性与股市估值
- **财务数据**：`stock_financial_analysis_indicator()`，指标：净资产收益率、销售净利率，长格式存储

## 存储方式

- **基础（方式 A）**：CSV — 可读性强、跨平台、便于 Git 与 Excel 查看；劣势是体积大、无 Schema、全表读取慢
- **进阶（方式 B）**：Parquet — 列式压缩、类型契约、按需读列；本作业清洗后数据同时保存 `stock_clean.csv` 与 `stock_clean.parquet`
- **进阶（方式 C 演示）**：SQLite `fin_data.db` 在 `02_clean.ipynb` 中演示 JOIN 查询；**不提交至 Git**（见 `.gitignore`），他人运行 `02_clean.ipynb` 可重建

选择 Parquet 的理由：金融时间序列列多、重复读取分析场景多，Parquet 在体积与读取速度上优于 CSV，且与 pandas/pyarrow 生态集成好。

## GitHub 仓库

https://github.com/ganlijie-code/dshw-p02.git

## 如何运行

1. 安装依赖：`pip install -r requirements.txt`
2. 在 Jupyter 中打开项目根目录 `dshw-p02`，**从上到下依次运行**三个 Notebook 的全部单元格
3. 每个 Notebook 的代码均为**完整内联实现**（不依赖 `from src import`），可直接执行
4. 运行 `01_download.ipynb` 需联网下载数据（约 3–5 分钟）
5. 导出报告：`jupyter nbconvert --to html 03_analysis.ipynb --output report.html`

验证脚本（已有 `data/` 时可快速测试 02、03）：

```bash
python scripts/verify_notebooks.py
```

命令行等价流程：

```bash
python scripts/run_download.py
python scripts/run_clean.py
python scripts/generate_figures.py
```

## 数据提交说明

`data/stock/`、`data/index/`、`data/macro/`、`data/finance/` 已在 `.gitignore` 中忽略；`data/clean/`、`data/combined/` 可选择性提交。克隆仓库后请运行 `01_download.ipynb` 重新获取原始数据。

## 项目结构

```
dshw-p01/
├── README.md
├── report.html
├── requirements.txt
├── .gitignore
├── 01_download.ipynb
├── 02_clean.ipynb
├── 03_analysis.ipynb
├── src/              # 核心 Python 模块
├── scripts/          # 可执行脚本
├── data/
├── output/
└── download_log.txt
```
