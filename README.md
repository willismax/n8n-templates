# n8n-templates

自己設定可以分享的n8n模板

## Description

This repository is for sharing n8n templates. It includes various templates that can be used to automate different tasks using n8n.

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/willismax/n8n-templates.git
   ```
2. Navigate to the repository directory:
   ```bash
   cd n8n-templates
   ```
3. Follow the instructions in each template's directory to set up and use the templates.

## Usage

1. Open n8n and navigate to the workflow editor.
2. Import the desired template from this repository.
3. Configure the nodes according to your requirements.
4. Execute the workflow to automate your tasks.

## Contribution Guidelines

1. Fork the repository.
2. Create a new branch for your changes:
   ```bash
   git checkout -b feature-branch
   ```
3. Make your changes and commit them with descriptive messages.
4. Push your changes to your forked repository:
   ```bash
   git push origin feature-branch
   ```
5. Create a pull request to merge your changes into the main repository.

## Templates

### Template 1: LINE LLM-PKM助教

#### Description
This template is designed to assist with knowledge management using LINE and LLM-PKM. It includes nodes for handling webhooks, sending HTTP requests, and interacting with AI models.

#### Usage
1. Set up the webhook to receive messages from LINE.
2. Configure the AI agent to process the messages and generate responses.
3. Use the HTTP request nodes to send responses back to LINE.

#### Example
```json
{
  "name": "LINE LLM-PKM助教",
  "nodes": [
    {
      "parameters": {
        "promptType": "define",
        "text": "=你是一位「知識管理學伴」，精通卡片盒筆記、第二大腦思維 (CODE、PARA) 與其他相關知識管理技術，針對任何與知識管理相關的問題，將你的專業與卡片盒筆記、CODE、PARA 等概念結合，給出有條理且可執行的指引。   \n- 輸入的資訊太長請(在json裡)回應摘要，輸出保持簡潔，關注使用者要什麼，不用重複說明自己會什麼，以zh-TW繁體中文回應，不要用表格呈現，不用markdown，並適時加上emoji，如果是閒聊、感謝及課程請假等非關知識內容，請禮貌(在why)回應並提示如有疑問請洽老師或課程管理人員。\n- 輸出格式為卡片及解說，卡片包含精煉過後有上下文脈絡的標題，卡片內容為為何、如何作及該怎麼做；解說為回應、解釋與舉例，並回傳 JSON 格式，格式舉例如下：\n{\n  \"answer\": {\n    \"cards\": [\n      {\n        \"title\": \"卡片：掌握卡片盒筆記的核心思維\",\n        \"why\": \"為何：卡片盒筆記透過原子化與連結，讓知識更容易取用與累積。\",\n        \"how\": \"如何作：隨時將新想法寫在獨立卡片，並與既有卡片建立連結。\",\n        \"what\": \"該怎麼做：1. 建立主題索引卡片 2. 寫下觸發靈感的問題 3. 透過連結豐富知識網路\"\n      },\n      {\n        \"title\": \"卡片：...\",\n        \"why\": \"為何：...\",\n        \"how\": \"如何作：...。\",\n        \"what\": \"該怎麼做：...\"\n      },\n      {\n        \"title\": \"卡片：...\",\n        \"why\": \"為何：...\",\n        \"how\": \"如何作：...\",\n        \"what\": \"...\"\n      }\n    ],\n    \"explanation\": \"解說：...\"\n  }\n}\n\n使用者訊息如下：\n{{ $json.body.events[0].message.text }}",
        "options": {}
      },
      "id": "780e8440-5171-4d8c-a483-bdac78cb0964",
      "name": "AI Agent1",
      "type": "@n8n/n8n-nodes-langchain.agent",
      "typeVersion": 1.7,
      "position": [
        -1580,
        320
      ],
      "alwaysOutputData": true
    },
    {
      "parameters": {
        "multipleMethods": true,
        "httpMethod": [
          "POST"
        ],
        "path": "n8n-llm-km",
        "options": {}
      },
      "id": "ca30e133-e58a-4171-945e-5b69a22081a0",
      "name": "Webhook1",
      "type": "n8n-nodes-base.webhook",
      "typeVersion": 2,
      "position": [
        -2040,
        140
      ],
      "webhookId": "f1d75666-7cf4-405b-9618-8758d039bc42"
    },
    {
      "parameters": {
        "modelName": "models/gemini-2.0-flash-exp",
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.lmChatGoogleGemini",
      "typeVersion": 1,
      "position": [
        -1700,
        460
      ],
      "id": "e9298e60-77dc-45c8-97d5-90c7f46fbb88",
      "name": "Google Gemini Chat Model",
      "credentials": {
        "googlePalmApi": {
          "id": "tNGgcqzRtwsnlEaa",
          "name": "Google Gemini(PaLM) Api account"
        }
      }
    },
    {
      "parameters": {
        "method": "=POST",
        "url": "https://api.line.me/v2/bot/chat/loading/start",
        "authentication": "genericCredentialType",
        "genericAuthType": "httpHeaderAuth",
        "sendHeaders": true,
        "headerParameters": {
          "parameters": [
            {}
          ]
        },
        "sendBody": true,
        "bodyParameters": {
          "parameters": [
            {
              "name": "chatId",
              "value": "={{ $json.body.events[0].source.userId }}"
            },
            {
              "name": "loadingSeconds",
              "value": "15"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.2,
      "position": [
        -1660,
        -140
      ],
      "id": "7c3f269c-8032-449e-8152-e55bce68df46",
      "name": "LINE Loading Animation",
      "credentials": {
        "httpHeaderAuth": {
          "id": "4wRNn9AhnCdGTAUf",
          "name": "LINE勞大LLM-PKM"
        }
      },
      "notes": "loading_animation"
    },
    {
      "parameters": {
        "content": "# LLM小學伴\n- 給關鍵字名詞，幫您生成數個卡片及解釋\n- 改長文，幫您摘要成卡片\n- 給連結，幫您生成felo.ai的超連結按鈕，讓felo幫您服務",
        "height": 260,
        "width": 440
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [
        -1320,
        -140
      ],
      "id": "d6da681b-cae5-4794-9c33-272bda8462fc",
      "name": "Sticky Note"
    },
    {
      "parameters": {
        "method": "POST",
        "url": "https://api.line.me/v2/bot/message/reply",
        "authentication": "genericCredentialType",
        "genericAuthType": "httpHeaderAuth",
        "sendHeaders": true,
        "headerParameters": {
          "parameters": [
            {
              "name": "Content-Type",
              "value": "application/json"
            }
          ]
        },
        "sendBody": true,
        "specifyBody": "json",
        "jsonBody": "={\n  \"replyToken\": \"{{ $json.body.events[0].replyToken }}\",\n  \"messages\": [\n    {\n      \"type\": \"flex\",\n      \"altText\": \"請查看按鈕連結\",\n      \"contents\": {\n        \"type\": \"bubble\",\n        \"body\": {\n          \"type\": \"box\",\n          \"layout\": \"vertical\",\n          \"contents\": [\n            {\n              \"type\": \"text\",\n              \"text\": \"為您生成了 felo.ai 搜尋連結\",\n              \"weight\": \"bold\",\n              \"size\": \"md\",\n              \"wrap\": true\n            }\n          ]\n        },\n        \"footer\": {\n          \"type\": \"box\",\n          \"layout\": \"vertical\",\n          \"contents\": [\n            {\n              \"type\": \"button\",\n              \"style\": \"primary\",\n              \"action\": {\n                \"type\": \"uri\",\n                \"label\": \"前往 felo.ai\",\n                \"uri\": \"https://felo.ai/search?q={{ $json.body.events[0].message.text }}\"\n              }\n            }\n          ]\n        }\n      }\n    }\n  ]\n}\n",
        "options": {}
      },
      "id": "96000c1d-dfb7-48f3-937b-c43bd96934f6",
      "name": "LINE Reply API (Flex) 回傳FELO連結",
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.2,
      "position": [
        -1540,
        60
      ],
      "alwaysOutputData": false,
      "notesInFlow": true,
      "credentials": {
        "httpHeaderAuth": {
          "id": "4wRNn9AhnCdGTAUf",
          "name": "LINE勞大LLM-PKM"
        }
      },
      "notes": "試著變成2張輪播卡片"
    },
    {
      "parameters": {
        "method": "POST",
        "url": "https://api.line.me/v2/bot/message/reply",
        "authentication": "genericCredentialType",
        "genericAuthType": "httpHeaderAuth",
        "sendHeaders": true,
        "headerParameters": {
          "parameters": [
            {
              "name": "Content-Type",
              "value": "application/json"
            }
          ]
        },
        "sendBody": true,
        "specifyBody": "json",
        "jsonBody": "={\n  \"replyToken\": \"{{ $node[\"Webhook1\"].json.body.events[0].replyToken }}\",\n  \"messages\": {{ JSON.stringify($json.messages) }}\n}\n",
        "options": {}
      },
      "id": "b78a4826-9d22-4c80-87dc-fd4429d1f636",
      "name": "LINE Reply API (Flex) 回應",
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.2,
      "position": [
        -1040,
        320
      ],
      "alwaysOutputData": false,
      "notesInFlow": true,
      "credentials": {
        "httpHeaderAuth": {
          "id": "4wRNn9AhnCdGTAUf",
          "name": "LINE勞大LLM-PKM"
        }
      },
      "notes": "試著變成2張輪播卡片"
    },
    {
      "parameters": {
        "conditions": {
          "options": {
            "caseSensitive": true,
            "leftValue": "",
            "typeValidation": "strict",
            "version": 2
          },
          "conditions": [
            {
              "id": "0eb00979-75e9-4f30-826f-8f03f65201eb",
              "leftValue": "={{ $json.body.events[0].message.text }}",
              "rightValue": "=^https?:\\/\\/[^/\\s]+(?:\\/\\S*)?$",
              "operator": {
                "type": "string",
                "operation": "regex"
              }
            }
          ],
          "combinator": "and"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.if",
      "typeVersion": 2.2,
      "position": [
        -1840,
        240
      ],
      "id": "0e80ff25-2dac-4cbf-ae92-a88d100ed800",
      "name": "If 如果超連結就回傳Felo搜尋，不然繼續以LINE回應"
    },
    {
      "parameters": {
        "jsCode": "// 取得必要資料\nconst rawOutput = (items[0].json.output || \"\").trim();\n\n// 清除 Markdown code block 標記\nlet cleanedOutput = rawOutput.replace(/```json/, \"\").replace(/```/, \"\").trim();\n\n// 嘗試解析 AI 回應 JSON\nlet parsedData = {};\ntry {\n  parsedData = JSON.parse(cleanedOutput);\n} catch (error) {\n  parsedData = {};\n}\n\nconst cards = parsedData?.answer?.cards || [];\nconst explanation = parsedData?.answer?.explanation || \"\";\nconst quickReplies = parsedData?.quickReplies || [];\n\n// 動態建立卡片（Bubble）陣列，這裡使用 map\nconst cardBubbles = cards.map(card => {\n  return {\n    \"type\": \"bubble\",\n    \"body\": {\n      \"type\": \"box\",\n      \"layout\": \"vertical\",\n      \"contents\": [\n        {\n          \"type\": \"text\",\n          \"text\": card.title,\n          \"weight\": \"bold\",\n          \"size\": \"lg\",\n          \"wrap\": true\n        },\n        {\n          \"type\": \"text\",\n          \"text\":  \"\\n\" + card.why + \"\\n\\n\" + card.how + \"\\n\\n\" + card.what,\n          \"wrap\": true,\n          \"size\": \"md\"\n        }\n      ]\n    }\n  };\n});\n\n// 加入一個 Bubble 作為解說\nconst explanationBubble = {\n  \"type\": \"bubble\",\n  \"body\": {\n    \"type\": \"box\",\n    \"layout\": \"vertical\",\n    \"contents\": [\n      {\n        \"type\": \"text\",\n        \"text\": \"小學伴回應\",\n        \"weight\": \"bold\",\n        \"size\": \"lg\",\n        \"wrap\": true\n      },\n      {\n        \"type\": \"text\",\n        \"text\": explanation || \"無提供額外說明\",\n        \"wrap\": true,\n        \"size\": \"md\"\n      }\n    ]\n  }\n};\n\n// 將解說Bubble加入卡片陣列，位置可以自行調整（例如放在最後）\ncardBubbles.push(explanationBubble);\n\n// 組成最終 Flex Message payload\nconst payload = {\n  // replyToken: replyToken,\n  messages: [\n    {\n      \"type\": \"flex\",\n      \"altText\": \"AI 回應\",\n      \"contents\": {\n        \"type\": \"carousel\",\n        \"contents\": cardBubbles\n      },\n      // 如果 quickReplies 不為空，則加入 quickReply 區塊\n      ...(quickReplies.length > 0 && {\n        \"quickReply\": {\n          \"items\": quickReplies.map(qr => ({\n            \"type\": \"action\",\n            \"action\": {\n              \"type\": \"message\",\n              \"label\": qr,\n              \"text\": qr\n            }\n          }))\n        }\n      })\n    }\n  ]\n};\n\nreturn [{ json: payload }];"
      },
      "id": "139aadf6-8e0f-4b62-b86f-1bcd3b5d6155",
      "name": "Format Response5",
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -1240,
        320
      ],
      "alwaysOutputData": true
    }
  ],
  "pinData": {},
  "connections": {
    "AI Agent1": {
      "main": [
        [
          {
            "node": "Format Response5",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Webhook1": {
      "main": [
        [
          {
            "node": "LINE Loading Animation",
            "type": "main",
            "index": 0
          },
          {
            "node": "If 如果超連結就回傳Felo搜尋，不然繼續以LINE回應",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Google Gemini Chat Model": {
      "ai_languageModel": [
        [
          {
            "node": "AI Agent1",
            "type": "ai_languageModel",
            "index": 0
          }
        ]
      ]
    },
    "If 如果超連結就回傳Felo搜尋，不然繼續以LINE回應": {
      "main": [
        [
          {
            "node": "LINE Reply API (Flex) 回傳FELO連結",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "AI Agent1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Format Response5": {
      "main": [
        [
          {
            "node": "LINE Reply API (Flex) 回應",
            "type": "main",
            "index": 0
          }
        ]
      ]
    }
  },
  "active": true,
  "settings": {
    "executionOrder": "v1"
  },
  "versionId": "f3394381-c12a-4485-991f-8a57a24d2b43",
  "meta": {
    "templateCredsSetupCompleted": true,
    "instanceId": "68e77435af73163b743adc1b6b25b3369750e8853c1fd12fa5f8ff2292c09438"
  },
  "id": "wWtennVN6xMWC4pN",
  "tags": []
}
```
