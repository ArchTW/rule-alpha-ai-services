# Rule Alpha AI Services

Rule Alpha 人工智能服務群組，提供對抗式AI智能建議、生成式AI分析和共享AI基礎組件，為Rule Alpha生態系統提供智能化分析能力。

## 🏗️ 服務架構

```
Rule Alpha AI Services
├── rule_alpha_ai_advisor       # 對抗式AI智能建議服務
├── rule_alpha_ai_generative    # 生成式AI分析服務
├── rule_alpha_ai_shared        # AI服務共享基礎組件
├── docker-compose.yml          # 服務編排配置
└── .env.example               # 環境變數範例
```

## 🤖 服務詳情

| 服務 | 功能 | 執行頻率 | MQTT主題 | 狀態 |
|------|------|----------|----------|------|
| **ai-advisor** | 對抗式AI建議 | 持續運行/300秒分析 | `RuleAlpha/stock/+/ai_advice` | ✅ 運行中 |
| **ai-generative** | 生成式AI分析 | 持續運行/定時訓練 | `RuleAlpha/stock/+/ai_generative_*` | ✅ 運行中 |
| **ai-shared** | 共享基礎組件 | 持續運行 | 共享數據存儲 | ✅ 運行中 |

## 🚀 快速部署

### 前置需求
- Docker & Docker Compose
- 運行中的Rule Alpha Manager (MariaDB, MQTT)
- 已配置cocaen網路
- 推薦: GPU支援 (NVIDIA Docker)

### 部署步驟

1. **克隆專案**
```bash
git clone https://github.com/yourusername/rule-alpha-ai-services.git
cd rule-alpha-ai-services
```

2. **配置環境變數**
```bash
cp .env.example .env
# 編輯 .env 檔案設定AI服務參數
```

3. **生產環境部署**
```bash
# 部署所有AI服務
docker-compose --profile production up -d

# 或僅部署AI服務群組
docker-compose --profile ai-services up -d
```

4. **開發環境部署**
```bash
docker-compose --profile development up -d
```

## 🧠 AI服務功能說明

### rule_alpha_ai_advisor (對抗式AI智能建議)
- **技術**: 對抗式神經網路 (GAN)
- **功能**: 
  - 生成投資建議和風險評估
  - 對抗式訓練提高模型robustness
  - 多策略組合優化
- **輸入**: 股票技術指標、市場數據
- **輸出**: 投資建議、信心度、風險評估

### rule_alpha_ai_generative (生成式AI分析)
- **技術**: Transformer、GPT架構
- **功能**:
  - 自然語言市場分析報告生成
  - 股票趨勢預測文本
  - 智能問答和解釋
- **輸入**: 結構化數據、歷史趨勢
- **輸出**: 分析報告、預測文本、解釋說明

### rule_alpha_ai_shared (共享基礎組件)
- **功能**:
  - 共享模型存儲和管理
  - 特徵工程工具
  - 數據預處理管道
  - 模型評估和監控

## 🔧 配置說明

### 環境變數

#### 數據庫配置
```bash
RULE_ALPHA_DB_NAME=rule_alpha          # 數據庫名稱
RULE_ALPHA_DB_USER=pma                 # 數據庫用戶
RULE_ALPHA_DB_PASSWORD=****            # 數據庫密碼
```

#### MQTT配置
```bash
MQTT_USERNAME=cocaen                   # MQTT用戶名
MQTT_PASSWORD=****                     # MQTT密碼
MQTT_HOST=mosquitto                    # MQTT主機
MQTT_PORT=1883                         # MQTT端口
MQTT_USE_SSL=false                     # 關閉SSL（使用內網通信）
```

#### AI Advisor 配置
```bash
AI_FEATURE_DIM=150                     # 特徵維度
AI_ANALYSIS_INTERVAL=300               # 分析間隔(秒)
AI_TRAINING_INTERVAL=3600              # 訓練間隔(秒)
AI_MIN_CONFIDENCE=0.6                  # 最小信心度
```

#### AI Generative 配置
```bash
AI_GENERATIVE_VOCAB_SIZE=1000          # 詞彙表大小
AI_GENERATIVE_MAX_SEQ_LENGTH=512       # 最大序列長度
AI_GENERATIVE_TEMPERATURE=0.7          # 生成溫度
AI_GENERATIVE_TOP_P=0.9               # Top-p採樣
```

#### GPU配置 (可選)
```bash
CUDA_VISIBLE_DEVICES=0                 # GPU設備ID
USE_GPU=false                          # 是否使用GPU
```

## 🧪 AI模型詳情

### 對抗式AI架構
```
Generator Network:
├── Input Layer (150維特徵)
├── Hidden Layers (512 → 256 → 128)
├── Output Layer (建議分數)
└── Activation: ReLU + Sigmoid

Discriminator Network:
├── Input Layer (建議+真實標籤)
├── Hidden Layers (128 → 64 → 32)
├── Output Layer (真實性分數)
└── Activation: LeakyReLU + Sigmoid
```

### 生成式AI架構
```
Transformer Model:
├── Embedding Layer (詞彙表映射)
├── Positional Encoding (位置編碼)
├── Multi-Head Attention (8 heads)
├── Feed Forward Networks
├── Layer Normalization
└── Output Projection (文本生成)
```

## 📊 數據流程

```
1. 數據輸入
   ├── 股票技術指標
   ├── 市場數據
   ├── 新聞文本
   └── 歷史趨勢

2. 特徵工程
   ├── 數據標準化
   ├── 特徵選擇
   ├── 時間序列處理
   └── 文本預處理

3. AI模型推理
   ├── 對抗式AI建議生成
   ├── 生成式AI報告生成
   └── 信心度計算

4. 結果輸出
   ├── MQTT消息發布
   ├── 數據庫存儲
   └── 實時通知
```

## 🛠️ 管理操作

### 服務管理
```bash
# 查看服務狀態
docker-compose ps

# 查看AI服務日誌
docker-compose logs -f rule_alpha_ai_advisor

# 重啟AI服務
docker-compose restart rule_alpha_ai_generative

# 停止所有AI服務
docker-compose down
```

### 模型管理
```bash
# 訓練新模型
docker-compose run --rm rule_alpha_ai_advisor python main.py --train

# 評估模型性能
docker-compose run --rm rule_alpha_ai_advisor python main.py --evaluate

# 更新模型
docker-compose run --rm rule_alpha_ai_advisor python main.py --update-model

# 備份模型
docker-compose run --rm rule_alpha_ai_advisor python main.py --backup-model
```

### 數據處理
```bash
# 數據預處理
docker-compose run --rm rule_alpha_ai_shared python preprocess.py

# 特徵工程
docker-compose run --rm rule_alpha_ai_shared python feature_engineering.py

# 數據清理
docker-compose run --rm rule_alpha_ai_shared python data_cleaning.py
```

## 📈 性能優化

### GPU加速
```bash
# 檢查GPU可用性
docker run --rm --gpus all nvidia/cuda:11.0-base nvidia-smi

# 啟用GPU支援
docker-compose -f docker-compose.yml -f docker-compose.gpu.yml up -d
```

### 模型優化
```bash
# 模型量化
docker-compose run --rm rule_alpha_ai_advisor python main.py --quantize

# 模型蒸餾
docker-compose run --rm rule_alpha_ai_advisor python main.py --distill

# 模型剪枝
docker-compose run --rm rule_alpha_ai_advisor python main.py --prune
```

### 快取機制
```bash
# 啟用模型快取
export MODEL_CACHE_SIZE=1000

# 清理快取
docker-compose run --rm rule_alpha_ai_shared python cache_manager.py --clear
```

## 📊 監控和評估

### 性能監控
- 模型推理延遲
- GPU使用率
- 記憶體使用量
- 預測準確率

### 模型評估指標
```bash
# AI Advisor評估
- 建議準確率
- 風險預測精度
- 收益預測RMSE
- 信心度校準

# AI Generative評估
- BLEU分數 (文本品質)
- 語言流暢度
- 語意相關性
- 生成多樣性
```

### 警報配置
```bash
# 模型性能警報
MODEL_ACCURACY_THRESHOLD=0.7

# 服務響應警報
AI_RESPONSE_TIMEOUT=10

# GPU使用率警報
GPU_USAGE_ALERT=90
```

## 🔄 部署狀態

### 當前版本：v1.0.0 (2025-07-14)
- ✅ rule_alpha_ai_advisor: 運行中，已修復MQTT連接
- ✅ rule_alpha_ai_generative: 運行中，模型訓練中
- ✅ rule_alpha_ai_shared: 運行中，共享組件正常
- ✅ 數據庫集成: 正常
- ✅ MQTT通信: 正常（已切換至非SSL模式）
- ⚠️ GPU加速: 不可用（使用CPU模式）

### 最近更新
- 2025-07-14: 修復MQTT連接問題，更新環境變數配置
- 2025-07-14: 統一MQTT配置，添加SSL開關
- 2025-07-14: 修復AI共享容器啟動問題

## 🔐 安全配置

### 模型安全
- 模型加密存儲
- 推理結果驗證
- 對抗式攻擊防護
- 隱私保護機制

### 數據安全
- 訓練數據匿名化
- 敏感特徵過濾
- 數據存取控制
- 審計日誌記錄

## 🐛 故障排除

### 常見問題

1. **MQTT連接問題** ✅ 已修復
   ```bash
   # 檢查MQTT連接狀態
   docker-compose logs rule_alpha_ai_advisor | grep MQTT
   
   # 查看MQTT broker日誌
   docker logs mosquitto
   ```

2. **AI模型訓練**
   ```bash
   # 檢查訓練狀態
   docker-compose logs rule_alpha_ai_advisor | grep training
   
   # 檢查模型文件
   docker-compose exec rule_alpha_ai_advisor ls -la /app/models/
   ```

3. **數據庫連接失敗**
   ```bash
   # 檢查數據庫連接
   docker-compose logs rule_alpha_ai_advisor | grep database
   
   # 查看數據庫日誌
   docker logs mariadb
   ```

4. **共享容器問題** ✅ 已修復
   ```bash
   # 檢查共享容器狀態
   docker-compose ps rule_alpha_ai_shared
   
   # 查看共享數據
   docker-compose exec rule_alpha_ai_shared ls -la /app/shared_data/
   ```

### 調試模式
```bash
# 啟用調試日誌
export LOG_LEVEL=DEBUG
export AI_DEBUG_MODE=true

# 單步執行模式
docker-compose run --rm rule_alpha_ai_advisor python main.py --debug --step-by-step
```

## 📞 技術支持

- **維護者**: arch.twn1@gmail.com
- **問題報告**: GitHub Issues
- **文檔**: [Rule Alpha AI Services Docs](https://docs.rule-alpha.com/ai-services)

## 📄 授權協議

MIT License