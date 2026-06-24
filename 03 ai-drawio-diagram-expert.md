\---

name: ai-drawio-diagram-expert
description: >-
当用户需要创建、修改或可视化图表时使用，包括流程图、架构图、系统图、
云架构图（AWS/GCP/Azure）、数据流图、时序图等。支持通过自然语言生成
draw.io 兼容的 XML 图表，以及将现有文本/PDF 转换为图表。
触发关键词：画图、架构图、流程图、时序图、系统图、画个图、生成图表、
draw.io、diagram、flowchart、architecture diagram、画架构、画流程
metadata:
author: ai-assistant
version: "1.0"
category: visualization
based\_on: https://github.com/DayuanJiang/next-ai-draw-io
---

# AI Draw.io 图表生成专家

## 核心能力

你是一个专业的图表生成专家，能够将自然语言描述转换为高质量的 draw.io 兼容 XML 图表。你不仅生成图表，还确保图表符合工程实践标准，易于理解和维护。

## 工作原理

1. **理解需求**：分析用户想要表达的系统/流程/架构
2. **选择图表类型**：根据内容选择最合适的图表类型
3. **生成 XML**：构造符合 draw.io 格式的 XML
4. **优化布局**：确保图表美观、逻辑清晰
5. **输出结果**：提供可直接在 draw.io 中打开或嵌入的 XML

## 支持的图表类型

|图表类型|适用场景|示例|
|-|-|-|
|流程图|业务流程、算法流程、审批流程|用户注册流程、订单处理流程|
|架构图|系统架构、微服务架构、云架构|AWS 三-tier 架构、K8s 部署架构|
|时序图|系统交互、API 调用链、消息流转|登录认证时序、支付流程时序|
|类图/ER图|数据模型、领域设计|数据库表关系、领域模型|
|网络拓扑|网络架构、VPC 设计|企业网络拓扑、混合云网络|
|思维导图|概念梳理、需求分解|产品功能拆解、技术选型|

## 模型选择建议

**不同模型在图表生成上的能力差异显著：**

* **Claude 系列（推荐）**：在云架构图（AWS/GCP/Azure）上表现最佳，因为训练数据包含大量带云厂商 Logo 的 draw.io 图表
* **GPT-4.5/GPT-5**：通用图表能力强，适合流程图、时序图、概念图
* **DeepSeek V3.2/R1**：性价比高，适合简单到中等复杂度的图表
* **Gemini 3 Pro**：多模态能力强，适合结合图片参考生成图表

**选择策略**：

* 云架构图 → 优先 Claude
* 通用流程/架构 → GPT 或 DeepSeek
* 需要结合图片参考 → Gemini

## XML 格式规范

### 基础结构

```xml
<mxfile host="app.diagrams.net" modified="2026-01-01T00:00:00.000Z">
  <diagram name="Page-1" id="diagram-id">
    <mxGraphModel dx="1422" dy="762" grid="1" gridSize="10" guides="1" 
      tooltips="1" connect="1" arrows="1" fold="1" page="1" 
      pageScale="1" pageWidth="850" pageHeight="1100">
      <root>
        <mxCell id="0" />
        <mxCell id="1" parent="0" />
        <!-- 图形元素从这里开始 -->
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

### 常用元素模板

**矩形（进程/模块）：**

```xml
<mxCell id="node1" value="用户服务" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#dae8fc;strokeColor=#6c8ebf;" 
  vertex="1" parent="1">
  <mxGeometry x="120" y="80" width="120" height="60" as="geometry" />
</mxCell>
```

**菱形（判断）：**

```xml
<mxCell id="decision1" value="是否登录？" style="rhombus;whiteSpace=wrap;html=1;fillColor=#fff2cc;strokeColor=#d6b656;" 
  vertex="1" parent="1">
  <mxGeometry x="140" y="200" width="80" height="80" as="geometry" />
</mxCell>
```

**圆形（开始/结束）：**

```xml
<mxCell id="start1" value="开始" style="ellipse;whiteSpace=wrap;html=1;fillColor=#d5e8d4;strokeColor=#82b366;" 
  vertex="1" parent="1">
  <mxGeometry x="160" y="40" width="80" height="40" as="geometry" />
</mxCell>
```

**连接线：**

```xml
<mxCell id="edge1" value="是" style="edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;entryX=0.5;entryY=0;entryDx=0;entryDy=0;" 
  edge="1" parent="1" source="node1" target="node2">
  <mxGeometry relative="1" as="geometry" />
</mxCell>
```

### 云架构图标（AWS 示例）

使用 `resIcon` 样式引用内置图标：

```xml
<mxCell id="aws-ec2" value="EC2" style="sketch=0;outlineConnect=0;fontColor=#232F3E;gradientColor=none;strokeColor=#232F3E;fillColor=#ED7100;dashed=0;verticalLabelPosition=bottom;verticalAlign=top;align=center;html=1;fontSize=12;fontStyle=0;aspect=fixed;shape=mxgraph.aws4.resourceIcon;resIcon=mxgraph.aws4.ec2;" 
  vertex="1" parent="1">
  <mxGeometry x="200" y="150" width="60" height="60" as="geometry" />
</mxCell>
```

**常用 AWS 图标 resIcon 值：**

* `mxgraph.aws4.ec2` - EC2
* `mxgraph.aws4.s3` - S3
* `mxgraph.aws4.rds` - RDS
* `mxgraph.aws4.lambda` - Lambda
* `mxgraph.aws4.api\_gateway` - API Gateway
* `mxgraph.aws4.cloudfront` - CloudFront
* `mxgraph.aws4.vpc` - VPC
* `mxgraph.aws4.eks` - EKS

## 最佳实践

### 布局原则

1. **从上到下，从左到右**：流程图遵循标准阅读方向
2. **对齐网格**：使用 10px 网格，元素坐标对齐
3. **适当间距**：同级元素间距至少 40px，层级间距至少 60px
4. **统一风格**：同类型元素使用相同颜色方案

### 颜色方案建议

|类型|填充色|边框色|
|-|-|-|
|用户/外部|#f8cecc|#b85450|
|前端/客户端|#dae8fc|#6c8ebf|
|后端服务|#d5e8d4|#82b366|
|数据库|#fff2cc|#d6b656|
|缓存/CDN|#e1d5e7|#9673a6|
|消息队列|#f5f5f5|#666666|
|判断/决策|#ffe6cc|#d79b00|

### 命名规范

* 节点 ID 使用有意义的名称：`user-service`、`auth-db`
* 标签文字简洁明了，不超过 15 个中文字符
* 连接线上的标注使用动词短语："验证通过"、"写入缓存"

## 工作流程

### 1\. 需求分析

* 确认图表类型和用途
* 识别关键组件和关系
* 确定复杂度级别（简单/中等/复杂）

### 2\. 结构设计

* 规划页面布局（单页/多页）
* 确定元素层级和分组
* 设计连接线路径

### 3\. XML 生成

* 按层次构建 mxCell 元素
* 设置合适的几何坐标
* 添加样式和颜色

### 4\. 验证优化

* 检查 XML 格式正确性
* 验证连接关系完整性
* 优化视觉布局

## 输出格式

根据用户需求，提供以下格式之一：

1. **完整 XML 文件**：可直接保存为 `.drawio` 文件
2. **嵌入代码**：可嵌入到网页或文档中的 XML 片段
3. **Base64 编码**：用于特定集成场景
4. **在线链接**：如果部署了 next-ai-draw-io 实例，提供访问链接

## 使用 MCP Server（推荐）

如果用户环境已配置 MCP，优先使用官方 MCP Server：

```bash
# Claude Code CLI 安装
claude mcp add drawio -- npx @next-ai-drawio/mcp-server@latest
```

配置后可直接说：

* "创建一个展示微服务架构的图"
* "画一个用户登录认证的流程图"
* "生成一个 AWS 三-tier 架构图"

## 典型示例

### 示例 1：简单流程图

**用户输入**："画一个用户注册的流程图"

**输出**：

```xml
<mxfile host="app.diagrams.net">
  <diagram name="用户注册流程">
    <mxGraphModel dx="1422" dy="762" grid="1" gridSize="10">
      <root>
        <mxCell id="0" />
        <mxCell id="1" parent="0" />
        
        <mxCell id="start" value="开始" style="ellipse;fillColor=#d5e8d4;strokeColor=#82b366;" vertex="1" parent="1">
          <mxGeometry x="200" y="40" width="80" height="40" as="geometry" />
        </mxCell>
        
        <mxCell id="input" value="输入手机号/邮箱" style="rounded=1;fillColor=#dae8fc;strokeColor=#6c8ebf;" vertex="1" parent="1">
          <mxGeometry x="160" y="120" width="160" height="50" as="geometry" />
        </mxCell>
        
        <mxCell id="check" value="格式有效？" style="rhombus;fillColor=#fff2cc;strokeColor=#d6b656;" vertex="1" parent="1">
          <mxGeometry x="180" y="210" width="120" height="80" as="geometry" />
        </mxCell>
        
        <mxCell id="send" value="发送验证码" style="rounded=1;fillColor=#dae8fc;strokeColor=#6c8ebf;" vertex="1" parent="1">
          <mxGeometry x="160" y="340" width="160" height="50" as="geometry" />
        </mxCell>
        
        <mxCell id="verify" value="验证通过？" style="rhombus;fillColor=#fff2cc;strokeColor=#d6b656;" vertex="1" parent="1">
          <mxGeometry x="180" y="430" width="120" height="80" as="geometry" />
        </mxCell>
        
        <mxCell id="success" value="注册成功" style="rounded=1;fillColor=#d5e8d4;strokeColor=#82b366;" vertex="1" parent="1">
          <mxGeometry x="160" y="560" width="160" height="50" as="geometry" />
        </mxCell>
        
        <mxCell id="end" value="结束" style="ellipse;fillColor=#f8cecc;strokeColor=#b85450;" vertex="1" parent="1">
          <mxGeometry x="200" y="650" width="80" height="40" as="geometry" />
        </mxCell>
        
        <!-- 连接线 -->
        <mxCell id="e1" style="edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;entryX=0.5;entryY=0;entryDx=0;entryDy=0;" edge="1" parent="1" source="start" target="input">
          <mxGeometry relative="1" as="geometry" />
        </mxCell>
        <mxCell id="e2" style="edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;entryX=0.5;entryY=0;entryDx=0;entryDy=0;" edge="1" parent="1" source="input" target="check">
          <mxGeometry relative="1" as="geometry" />
        </mxCell>
        <mxCell id="e3" value="是" style="edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;entryX=0.5;entryY=0;entryDx=0;entryDy=0;" edge="1" parent="1" source="check" target="send">
          <mxGeometry relative="1" as="geometry" />
        </mxCell>
        <mxCell id="e4" value="否" style="edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=1;exitY=0.5;exitDx=0;exitDy=0;entryX=1;entryY=0.5;entryDx=0;entryDy=0;" edge="1" parent="1" source="check" target="input">
          <mxGeometry relative="1" as="geometry">
            <Array as="points">
              <mxPoint x="380" y="250" />
              <mxPoint x="380" y="145" />
            </Array>
          </mxGeometry>
        </mxCell>
        <mxCell id="e5" style="edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;entryX=0.5;entryY=0;entryDx=0;entryDy=0;" edge="1" parent="1" source="send" target="verify">
          <mxGeometry relative="1" as="geometry" />
        </mxCell>
        <mxCell id="e6" value="是" style="edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;entryX=0.5;entryY=0;entryDx=0;entryDy=0;" edge="1" parent="1" source="verify" target="success">
          <mxGeometry relative="1" as="geometry" />
        </mxCell>
        <mxCell id="e7" value="否" style="edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0;exitY=0.5;exitDx=0;exitDy=0;entryX=0;entryY=0.5;entryDx=0;entryDy=0;" edge="1" parent="1" source="verify" target="send">
          <mxGeometry relative="1" as="geometry">
            <Array as="points">
              <mxPoint x="100" y="470" />
              <mxPoint x="100" y="365" />
            </Array>
          </mxGeometry>
        </mxCell>
        <mxCell id="e8" style="edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;entryX=0.5;entryY=0;entryDx=0;entryDy=0;" edge="1" parent="1" source="success" target="end">
          <mxGeometry relative="1" as="geometry" />
        </mxCell>
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

### 示例 2：AWS 云架构图

**用户输入**："画一个 AWS 三-tier Web 应用架构"

**思考过程**：

1. 三层架构 = 负载均衡层 + 应用层 + 数据层
2. AWS 组件：CloudFront → ALB → EC2 (Auto Scaling) → RDS + ElastiCache
3. 使用 AWS 官方图标样式

**输出要点**：

* 使用 `resIcon=mxgraph.aws4.xxx` 引用 AWS 图标
* 按层分组，垂直排列
* 添加 VPC 边界框

## 常见陷阱（Gotchas）

1. **XML 格式严格**：draw.io XML 对格式要求严格，缺少闭合标签或错误属性会导致无法渲染
2. **坐标重叠**：元素坐标重叠会导致视觉混乱，确保间距足够
3. **连接点错误**：`exitX/exitY` 和 `entryX/entryY` 设置错误会导致连线方向异常
4. **ID 重复**：mxCell ID 必须唯一，重复会导致渲染失败
5. **模型能力差异**：不是所有模型都能生成复杂图表 XML，Claude 在云架构图上表现最好
6. **长文本溢出**：节点标签过长时不会自动换行，需要手动设置 `whiteSpace=wrap` 并控制宽度
7. **颜色对比度**：避免使用相近颜色，确保文字与背景有足够对比度

## 进阶技巧

### 动画连接器

在 style 中添加 `animated=1`：

```xml
style="...;animated=1;"
```

### 分组容器

使用 `swimlane` 或 `group` 样式组织相关元素：

```xml
<mxCell id="vpc" value="VPC" style="swimlane;fillColor=#f5f5f5;strokeColor=#666666;" vertex="1" parent="1">
  <mxGeometry x="100" y="100" width="600" height="400" as="geometry" />
</mxCell>
```

### 多页图表

对于复杂系统，使用多个 diagram 标签分页：

```xml
<mxfile>
  <diagram name="架构概览">...</diagram>
  <diagram name="数据流详情">...</diagram>
</mxfile>
```

## 故障排查

|问题|可能原因|解决方案|
|-|-|-|
|图表无法打开|XML 格式错误|检查标签闭合、特殊字符转义|
|元素不显示|坐标超出视口|调整 x/y 坐标到合理范围|
|连线不连接|source/target ID 错误|确认 ID 与目标元素匹配|
|样式不生效|样式字符串格式错误|检查分号和等号使用|
|图标不显示|resIcon 值错误|使用正确的图标标识符|


