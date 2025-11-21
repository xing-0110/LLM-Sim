# LLM-Sim

```
一個真實的 LLM 生存模擬遊戲，讓不同模型在原始地球環境中自主探索、互動和生存，並整合 Stima API 與 Langfuse 追蹤

- 真實的 2D 遊戲世界：包含陸地（樹木、果實、草地）、海洋（魚類）、沙漠等具象化元素，每個物件都有符號化的視覺表現
- Stima API 整合：支援所有列出的模型（OpenAI、Anthropic、Google、Meta、Microsoft、Alibaba、xAI、DeepSeek、Moonshot AI、Baidu、Z.AI），並可按廠商或價格排序選擇
- 生存值系統：每個 AI 角色有健康值、飢餓值、疲勞值等可配置屬性，會根據行為動態變化（進食回升、毒果實下降、不睡覺下降等）
- 視覺感知系統：AI 透過代碼「看見」周圍物件（如 object_001、fruit_alpha），由 LLM 自主命名和判斷（是否有毒、是否可食用）
- LLM 決策引擎：定期呼叫 Stima API，AI 根據視覺資訊、生存狀態、記憶做出行動決策（移動、進食、休息、對話、分享資訊）
- 完整行為記錄系統：記錄每個模型的所有思考過程、行動決策、互動內容的詳細 log，可單獨查看特定模型的完整思路
- 時間比例控制：可調整遊戲時間流速（如 1 分鐘 = 2 小時），並實作日夜循環影響 AI 行為
- Langfuse 整合：追蹤每個模型的 API 呼叫、token 消耗、成本計算、決策品質評分
- 模型選擇介面：提供兩種模式（按廠商分類、按價格排序）選擇要使用的 LLM 模型
- 即時匯出功能：可隨時匯出遊戲結果、互動記錄、統計數據
```

```
我想要寫一個小遊戲，可以串接入 LLM 模型

整個遊戲的畫面會像是一個可以延展的大空間，有點像是原始的地球，其中可能會有草地、沙漠、海洋、天空等場景，而 LLM 就扮演其中的一個個小人物，在這個空間中摸索，進行探尋。

而使用者在遊戲一開始串接 API，指定不同的模型，看他們在這樣的環境中會做什麼事、會講什麼話，比如可能很迷茫、看見一些東西之後，可能會進食、打架有紛爭、繁衍等等

請幫我設計這樣的一個遊戲

其中，要加入日夜與時間機制，現實中的1分鐘就是他們的24小時，這樣運行。

1. 不需要 Demo Mode
2. 每個模型都有對應的生存值，讓他們去探索世界的時候有所依據，當沒有吃東西生存執會下降，吃到讀果實會下降，吃了食物會回升，等等。如果超過一定時間不睡覺也會下降，需要進行休息，這些都可以進行設定，讓他們自行探索
3. 遊戲與現實生活中流速的比例可以調整
4. 請再每次生成新遊戲的時候，再最後產出這個遊戲的功能介紹
5. 然後模型的每個活動要記錄 log，在之後看的時候，可以選擇看單個模型的思路



然後，我要串接的 API 是 Stima API，串接的端點等等如下:


TIMEOUT = 60
## 1-1. STIMA_env
STIMA_KEY = os.getenv("STIMA_API_KEY")
STIMA_URL = "https://api.stima.tech/v1"

# 2️⃣ SET
## 2-1. Client
def get_client():
    if not STIMA_KEY:
        raise ValueError("STIMA_API_KEY 未設置")
    return AsyncOpenAI(
        api_key=STIMA_KEY,
        base_url=STIMA_URL,
        timeout=TIMEOUT
    )


可用的模型號與端點如下:
Open AI / GPT-5                           ； gpt-5-2025-08-07               ； $0.0072
Open AI / GPT-5 Nano                      ； gpt-5-nano-2025-08-07          ； $0.00028
Anthropic / Claude Sonnet 4               ； claude-sonnet-4-20250514       ； $0.01
Google / Gemini 3 Pro                     ； gemini-3-pro-preview-11-2025   ； $0.01
Google / Gemini 2.5 Pro                   ； gemini-2.5-pro                 ； $0.005
xAI / Grok 3                              ； grok-3                         ； $0.028
xAI / Grok 4                              ； grok-4                         ； $0.015
Microsoft / Phi 4 Reasoning Plus          ； phi-4-reasoning-plus           ； $0.00035
Perplexity / Sonar Pro                    ； sonar-pro                      ； $0.015
Moonshot AI / Kimi K2 0905                ；                                ； $0.00237
Z.AI / GLM 4.6                            ； glm-4.6                        ； $0.003
Baidu / ERNIE X1 32K                      ； ernie-x1-32k                   ； $0.012
DeepSeek / DeepSeek V3.2 Exp              ； deepseek-v3.2-exp              ； $0.0006
Mistral AI / Mistral Large 2411           ； mistral-large-2411             ； $0.018

然後用戶可以隨時匯出結果

📋 完整模型列表
OpenAI 系列

Open AI / GPT OSS Safeguard 20B          ； gpt-oss-safeguard-20b           ； $0.00072
Open AI / GPT-5                          ； gpt-5-2025-08-07                ； $0.0072
Open AI / GPT-5 Mini                     ； gpt-5-mini-2025-08-07           ； $0.00144
Open AI / GPT-5 Nano                     ； gpt-5-nano-2025-08-07           ； $0.00028
Open AI / GPT OSS 120B                   ； gpt-oss-120b                    ； $0.0015
Open AI / GPT OSS 20B                    ； gpt-oss-20b                     ； $0.006
Open AI / GPT-4.1 Nano                   ； gpt-4.1-nano                    ； $0.00056
Open AI / GPT-4o Mini Search Preview     ； gpt-4o-mini-search-preview      ； $0.0006
Open AI / GPT-4o mini 2024-07-18         ； gpt-4o-mini-2024-07-18          ； $0.0009

Anthropic 系列

Anthropic / Claude 3 Haiku 20240307      ； claude-3-haiku-20240307         ； $0.00125
Anthropic / Claude Haiku 4.5             ； claude-haiku-4.5                ； $0.004
Anthropic / Claude Haiku 4.5 (Thinking)  ； claude-haiku-4.5-thinking       ； $0.004

Google 系列

Google / Gemini 2.5 Flash Preview 09-2025        ； gemini-2.5-flash-preview-09-2025      ； $0.0025
Google / Gemini 2.5 Flash Lite Preview 09-2025   ； gemini-2.5-flash-lite-preview-09-2025 ； $0.0004
Google / Gemini 2.5 Flash Lite Preview 06-17     ； gemini-2.5-flash-lite-preview-06-17   ； $0.0002
Google / Gemini 2.5 Flash Preview 05-20 (thinking) ； gemini-2.5-flash-preview-05-20:thinking ； $0.0035
Google / Gemma 3 12B                     ； gemma-3-12b-it                  ； $0.0001
Google / Gemma 3 4B                      ； gemma-3-4b-it                   ； $0.00004
Google / Gemma 3 27B                     ； gemma-3-27b-it                  ； $0.0005
Google / Gemini 2.0 Flash Lite           ； gemini-2.0-flash-lite-001       ； $0.00042
Google / Gemini Flash 2.0                ； gemini-2.0-flash-001            ； $0.0004

Meta 系列

Meta / Llama Guard 4 12B                 ； llama-guard-4-12b               ； $0.00005
Meta / Llama 4 Scout                     ； llama-4-scout                   ； $0.0003
Meta / Llama 4 Maverick                  ； llama-4-maverick                ； $0.0006
Meta / Llama Guard 3 8b                  ； llama-guard-3-8b                ； $0.0003
Meta / Llama 3.3 70B Instruct            ； llama-3.3-70b-instruct          ； $0.00025
Meta / Llama 3.2 3B Instruct             ； llama-3.2-3b-instruct           ； $0.000324
Meta / Llama 3.2 90B Vision Instruct     ； llama-3.2-90b-vision-instruct   ； $0.0012
Meta / Llama 3.2 11B Vision Instruct     ； llama-3.2-11b-vision-instruct   ； $0.000049
Meta / Llama 3.1 8B Instruct             ； llama-3.1-8b-instruct           ； $0.00003
Meta / Llama 3 70B                       ； llama-3-70b                     ； $0.00079

Microsoft 系列

Microsoft / Phi 4 Reasoning Plus         ； phi-4-reasoning-plus            ； $0.00035
Microsoft / Phi 4 Multimodal Instruct    ； phi-4-multimodal-instruct       ； $0.00014
Microsoft / Phi-4                        ； phi-4                           ； $0.00042
Microsoft / Phi-3.5 Mini 128K Instruct   ； phi-3.5-mini-128k-instruct      ； $0.00009

Mistral AI 系列

Mistral AI / Mistral Small 3.2 24B       ； mistral-small-3.2-24b-instruct  ； $0.0003
Mistral AI / Mistral Small 3             ； mistral-small-24b-instruct-2501 ； $0.00014
Mistral AI / Ministral 8B                ； ministral-8b                    ； $0.0003

Alibaba 系列

Alibaba / Qwen3 VL 8B Instruct           ； qwen3-vl-8b-instruct            ； $0.00069
Alibaba / Qwen3 Next 80B A3B Thinking    ； qwen3-next-80b-a3b-thinking     ； $0.0008
Alibaba / Qwen3 30B A3B                  ； qwen3-30b-a3b                   ； $0.0003
Alibaba / Qwen3 14B                      ； qwen3-14b                       ； $0.00024
Alibaba / Qwen3 32B                      ； qwen3-32b                       ； $0.0003
Alibaba / Qwen3 235B A22B                ； qwen3-235b-a22b                 ； $0.0006
Alibaba / QwQ 32B Preview                ； qwq-32b-preview                 ； $0.00018
Alibaba / Qwen2.5 72B Instruct           ； qwen-2.5-72b-instruct           ； $0.0004

NVIDIA 系列

NVIDIA / Llama 3.3 Nemotron Super 49B v1 ； llama-3.3-nemotron-super-49b-v1 ； $0.0004
NVIDIA / Llama 3.1 Nemotron 70B Instruct ； llama-3.1-nemotron-70b-instruct ； $0.0003

xAI 系列

xAI / Grok 4 Fast                        ； grok-4-fast                     ； $0.00035
xAI / Grok 3 Mini Beta                   ； grok-3-mini-beta                ； $0.0005

Amazon 系列

Amazon / Nova Lite 1.0                   ； nova-lite-v1                    ； $0.00072
Amazon / Nova Micro 1.0                  ； nova-micro-v1                   ； $0.00042

DeepSeek 系列

DeepSeek / DeepSeek V3.2 Exp             ； deepseek-v3.2-exp               ； $0.0006
DeepSeek / Deepseek R1 Distill Qwen 7B   ； deepseek-r1-distill-qwen-7b     ； $0.0002
DeepSeek / Deepseek R1 0528 Qwen3 8B     ； deepseek-r1-0528-qwen3-8b       ； $0.00009
DeepSeek / DeepSeek V3 0324              ； deepseek-chat-v3-0324           ； $0.00088
DeepSeek / DeepSeek R1 Distill Llama 8B  ； deepseek-r1-distill-llama-8b    ； $0.00004
DeepSeek / Deepseek R1 Distill Qwen 1.5B ； deepseek-r1-distill-qwen-1.5b   ； $0.00018
DeepSeek / DeepSeek R1 Distill Qwen 32B  ； deepseek-r1-distill-qwen-32b    ； $0.00018
DeepSeek / DeepSeek R1 Distill Llama 70B ； deepseek-r1-distill-llama-70b   ； $0.00069

Moonshot AI 系列

Moonshot AI / Kimi K2 0711 Preview Search ； kimi-k2-0711-preview-search    ； $0.00098

Baidu 系列

Baidu / ERNIE 4.5 21B A3B Thinking       ； ernie-4.5-21b-a3b-thinking      ； $0.00028
Baidu / ERNIE 4.5 0.3B A3B               ； ernie-4.5-0.3b-a3b              ； $0.0006

Z.AI 系列

Z.AI / GLM-4.5 Flash                     ； glm-4.5-flash                   ； $0.000112
Z.AI / GLM-4                             ； glm-4                           ； $0.00042


然後，模型有兩個模式可以選，第一個是依照廠商分，第二個是由價格分（由低到高）
```
