# SwarmLabs · 多学科科学计算实验自动化 Agent Infra

> **2026 GOAI 世界人工智能开源大赛 · Agent Infra 新智基座赛道** 参赛项目

SwarmLabs 是一个面向科研场景的**多学科科学计算实验自动化 Agent Infra**。它把「目标输入 → 实验设计 → 模型执行 → 结果验证 → 知识沉淀」做成一个**多 Agent 协同、可审计、可复现**的端到端闭环。

- **真实模型**：底层调用 163 个解析物理/数学模型（QAOA MaxCut、VQE H₂ 解离、Naka-Rushton 发放率等），非示意性占位值
- **可信执行层（VeritasGuard）**：每个输出附带文献证据链、置信度评分、反事实分析、可复现证书
- **Skill Registry**：4 个核心 Skill（物理预测 / 实验设计 / 主动学习 / 报告生成）+ JSON Schema 机读注册表，支持外部贡献
- **数据飞轮**：47,566+ 条结构化科研实体覆盖 10 个学科领域，持续以数据密度构建壁垒
- **多 Agent 闭环**：Planner → Executor → Verifier 三 Agent 协同 + 自主学习回路

---

## 🚀 快速开始

```bash
# 1. 环境准备
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt

# 2. 设置环境变量
export OPENAI_API_KEY="your_key"          # 或 OLLAMA_HOST=127.0.0.1:11434
export PYTHONPATH="$(pwd)"

# 3. 运行飞轮（核心循环）
python -m engine.flywheel --domain physics --runs 3

# 4. 生成可复现证书
python -m core.trust_layer
```

完整 Demo 脚本见 [`docs/goai_2026_demo_script.md`](docs/goai_2026_demo_script.md)。

---

## 📁 项目结构

```
swarmlabs/
├── core/                          # 核心模块
│   ├── autonomous_loop.py         # 自主循环（Planner/Executor/Verifier 编排）
│   ├── task_contract.py           # 任务契约（证据收集与验证）
│   ├── trust_layer.py             # ★ 可信执行层（可复现证书生成，新增）
│   ├── tech_radar.py              # 技术雷达（趋势/风险评估）
│   ├── experience_store.py        # 经验存储
│   ├── smart_scheduler.py         # 智能调度器
│   ├── policy_gateway.py          # 策略网关（安全/权限）
│   ├── robin_architecture.py      # Robin 架构
│   ├── agent_relay.py             # Agent 中继
│   ├── multi_agent_debate.py      # 多 Agent 辩论
│   └── ...                        # 共 20+ 核心模块
├── engine/                        # 引擎层
│   ├── flywheel.py                # 飞轮主循环（调用 discovery/autonomous_loop/world_model）
│   ├── discovery.py               # 发现层
│   ├── world_model.py             # 世界模型
│   ├── traceability.py            # 可追溯层（Finding/Provenance/审计）
│   ├── validation.py              # 验证层
│   ├── neural_operator.py         # 神经算子
│   ├── pinn.py                    # PINN（物理信息神经网络）
│   ├── surrogate.py               # 代理模型
│   ├── tree_search.py             # 树搜索
│   ├── emukit_adapter.py          # EMUKIT 适配器（贝叶斯优化）
│   └── demo_full_loop.py          # 端到端演示
├── skills/                        # Skill 注册表
│   └── registry/                  # ★ 机读 Skill Registry（新增）
│       ├── schema.json            #   Skill JSON Schema
│       ├── physics_predict.json   #   物理预测 Skill
│       ├── experiment_design.json #   实验设计 Skill
│       ├── active_learning.json   #   主动学习 Skill
│       ├── report_generation.json #   报告生成 Skill
│       └── CONTRIBUTING.md        #   外部贡献指南
├── agentteams/                    # 多 Agent 团队编排
├── scenarios/                     # 学科场景库
├── data/                          # 数据目录
│   └── trust_surface.json         # 可信面数据（10 域，含文献溯源/置信度/rubric）
├── config/                        # 配置
├── docs/                          # 文档
│   ├── trust_layer.md             # ★ 可信执行层设计文档（新增）
│   ├── agent_output_contract.md   # ★ Agent 输出契约（新增）
│   ├── goai_2026_demo_script.md   # ★ 3 分钟 Demo 脚本（新增）
│   ├── goai_2026_submission_v2.md # ★ 整合申报材料 v2（新增）
│   ├── goai_2026_submission_README.md
│   ├── goai_2026_skill_catalog.md
│   ├── goai_2026_agent_identity.md
│   └── goai_2026_mcp_contract.md
├── scripts/                       # 脚本（100+ 工具脚本）
│   ├── verify_all.py              # 全量验证
│   ├── gh_push.py                 # GitHub Contents API 推送
│   └── ...
├── tests/                         # 测试
├── Dockerfile                     # Docker 构建
├── docker-compose.yml             # Docker Compose（仅主服务）
├── requirements.txt               # 依赖
└── LICENSE                        # MIT License
```

---

## 🔑 核心创新

### 1. VeritasGuard 可信执行层（新增命名）
> 所有预测结果附带**可复现证书**（SHA256 数据指纹 + 模型版本 + 文献引用 + rubric 评分），由 `core/trust_layer.py` 自动生成。

### 2. Skill Registry 机读注册表（新增）
> 4 个核心 Skill 以 JSON Schema 机读化（`skills/registry/`），支持自动编排、外部贡献、复用追踪。

### 3. Agent 输出契约（新增）
> 定义 Agent 输出的**标准结构**：evidence（证据链）+ confidence（置信度）+ counterfactual（反事实），确保所有输出可检查、可延续。

### 4. 多 Agent 协同闭环
> PlannerAgent（实验设计）→ ExecutorAgent（模型执行）→ VerifierAgent（主动学习验证），每一步都有明确输入/输出/校验规则。

### 5. 数据飞轮 + 跨域桥接
> 47,566+ 结构化科研实体覆盖 10 域（物理/化学/生物/材料/神经/光子/统计/能源/机器人/信息论），支持跨学科经验迁移。

---

## 📊 关键指标

| 指标 | 数值 |
|------|------|
| 结构化科研实体 | 47,566+ |
| 覆盖学科领域 | 10 |
| 底层物理/数学模型 | 163 |
| 核心 Skill 数 | 4（+ 外部可贡献） |
| Agent 角色 | 3（Planner / Executor / Verifier） |
| Trust Surface 域通过 | 8/10 |
| 累积贡献度 | 60 pts |
| Demo 时长 | 3 分钟闭环 |

---

## 🧪 测试与验证

```bash
# 运行核心引擎验证
python scripts/verify_all.py

# 验证 Skill 注册表 JSON 合法性
for f in skills/registry/*.json; do python -c "import json; json.load(open('$f'))"; done

# 验证可信执行层
python -m core.trust_layer

# Docker 验证
docker build -t swarmlabs .
docker run --rm -e OPENAI_API_KEY swarmlabs python -m engine.flywheel --domain physics --runs 1
```

---

## 📚 参赛文档

| 文档 | 路径 | 说明 |
|------|------|------|
| 整合申报材料 v2 | `docs/goai_2026_submission_v2.md` | 面向评委的整合文档 + 评审维度映射 |
| 3 分钟 Demo 脚本 | `docs/goai_2026_demo_script.md` | 可念台词 + 画面描述 |
| 可信执行层设计 | `docs/trust_layer.md` | VeritasGuard 架构文档 |
| Agent 输出契约 | `docs/agent_output_contract.md` | 输出标准与校验规则 |
| Skill 目录 | `docs/goai_2026_skill_catalog.md` | 4 Skill 详细描述 |
| Agent 身份 | `docs/goai_2026_agent_identity.md` | 3 Agent 角色定义 |
| MCP 契约 | `docs/goai_2026_mcp_contract.md` | 等价 MCP 集成契约 |
| 方案 PPT | `docs/goai_2026_方案.pptx` | 17 页 |

---

## 🐳 Docker 部署

```bash
# 构建
docker build -t swarmlabs .

# 启动（环境变量注入 API Key）
docker-compose up -d

# 运行飞轮
docker exec swarmlabs-main python -m engine.flywheel --domain physics --runs 3

# 查看日志
docker-compose logs -f swarmlabs-main
```

---

## 🔐 安全与合规

- **不依赖外部 LLM 作为可信计算**：核心数值预测来自解析物理模型，LLM 仅用于文档与推理辅助
- **证据链溯源**：每项输出可追溯至具体文献 DOI 和模型版本
- **反事实保护**：所有关键结论附带敏感度分析，防止单点依赖
- **MIT 协议开源**：核心引擎、Skill 注册表、Demo 脚本均开源

---

## 🤝 贡献

详见 `skills/registry/CONTRIBUTING.md`（Skill 贡献）和项目根目录 `CONTRIBUTING.md`。

## 📄 许可证

MIT License
