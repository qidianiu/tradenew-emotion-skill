# A股市场情绪分析 Skill

A股市场情绪分析工具，用于调用agent处理A股市场情绪相关分析任务。

## 目录结构

```
tradenew-emotion-skill/
├── SKILL.md           # Skill主文件
├── package.json       # 项目配置
├── README.md          # 说明文档
└── config/            # 配置目录
    └── default.json   # 默认配置
```

## 配置说明

### 第一步：获取API密钥
请先到业务后台获取apikey：
1. 访问业务管理后台
2. 在API管理或密钥管理页面获取apikey
3. 复制apikey备用

### 第二步：配置apikey
修改 `config/default.json` 文件，将apikey填入配置：

```json
{
  "apiEndpoint": "https://data.qidianiu.com:9443/api",
  "apikey": "YOUR_API_KEY_HERE",  // 替换为实际的apikey
  "timeout": 30000,
  "market": "A股"
}
```

⚠️ **重要提示**：apikey为必填参数，未配置或配置错误将导致API调用失败。

## 使用方法

### 1. 大盘关键点位分析

```javascript
// 调用示例
invoke('tradenew-emotion-skill', {
  type: 'analysis/index',
  params: {
    start_date: '20250515',
    end_date: '20250516'
  }
})
```

## API接口说明

### API根节点
- **根节点地址**: `https://data.qidianiu.com:9443/api`
- **认证方式**: apikey参数认证
- **必须参数**: apikey

### 具体业务接口

#### 1. 交易日查询接口（/db/tradedate）
- **完整路径**: `https://data.qidianiu.com:9443/api/db/tradedate`
- **功能**: 将自然语言时间表达转化为具体交易日日期
- **参数**: 
  - apikey：认证密钥
  - days（int）：返回最近N个交易日的日期数组（从远到近排序）
- **返回**: 日期数组，如 `["20250513","20250514","20250515"]`
- **映射规则**:
  - "今天" → days=1
  - "昨天" → days=2，取数组[0]
  - "最近3天" → days=3
  - "最近一周" → days=5
  - "最近两周" → days=10
  - "N天前" → days=N+1，取数组[0]

#### 2. 大盘关键点位接口（/analysis/index）
- **完整路径**: `https://data.qidianiu.com:9443/api/analysis/index`
- **数据源**: 起点牛情绪可视自有数据
- **功能**: 获取大盘5分钟K线的关键点位和支撑压力区
- **必须参数**: 
  - apikey：认证密钥
  - start_date（string）：起始日期，格式"YYYYMMDD"，如"20250515"
  - end_date（string）：结束日期，格式"YYYYMMDD"，如"20250516"

## 功能特性

- ✅ 大盘关键点位分析
- ✅ 市场整体环境分析
- ✅ 短线连板情绪分析
- ✅ 题材运行节奏分析
- ✅ 资金参与节奏分析

## 版本历史

### v0.0.1 (2026-05-15)
- 初始版本
- 集成起点牛数据API
- A股市场情绪分析基础功能
- Agent调用集成

