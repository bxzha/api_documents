# 模型推理 API 文档

## 服务信息

- **API 端点**：`http://36.140.161.122:6005/v1/chat/completions`
- **模型 ID**：`AIReady-sft-0625`
- **认证方式**：`AIReady-API-Key-Test`
- **最大上下文长度**：32768 tokens
- **请求方法**：POST
- **Content-Type**：`application/json`

## 请求参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `model` | string | 是 | 固定为上方模型 ID |
| `messages` | array | 是 | 对话消息，每个元素含 `role`（`system`/`user`/`assistant`）和 `content` |
| `max_tokens` | integer | 否 | 最大输出长度，建议 512 |
| `temperature` | float | 否 | 采样温度 0-2，默认 0.7 |
| `stream` | boolean | 否 | 是否流式输出，默认 false |

## 调用示例（Python）

```python
# openai==2.7.2
import json
from openai import OpenAI

client = OpenAI(
    api_key="EMPTY",
    base_url="http://36.140.161.122:6005/v1/",
)

completion = client.chat.completions.create(
    model="AIReady-sft-0625",
    messages=[{"role": "user", "content": "你好"}],
    max_tokens=512,
    temperature=0.7,
)
response = json.loads(completion.model_dump_json())
print(response["choices"][0]["message"]["content"])
```

## 调用示例（curl）

```bash
curl http://36.140.161.122:6005/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "AIReady-sft-0625",
    "messages": [{"role": "user", "content": "你好"}],
    "max_tokens": 512,
    "temperature": 0.7
  }'
```

## 响应格式

```json
{
  "id": "chatcmpl-xxx",
  "object": "chat.completion",
  "created": 1700000000,
  "model": "/root/zhabx/...",
  "choices": [
    {
      "index": 0,
      "message": {
        "role": "assistant",
        "content": "你好！我是..."
      },
      "finish_reason": "stop"
    }
  ],
  "usage": {
    "prompt_tokens": 10,
    "completion_tokens": 20,
    "total_tokens": 30
  }
}
```
