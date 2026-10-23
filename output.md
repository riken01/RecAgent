# 输出格式

最终结果须符合以下工具调用参数 JSON Schema。实际输出只包含本次判断结果，不输出 Schema 本身；可选字段无依据或不适用时省略。

```json
{
  "type": "object",
  "required": ["add"],
  "additionalProperties": false,
  "properties": {
    "add": {
      "type": "array",
      "description": "经筛选的新子意图，最多3条，每条来源意图最多对应1条。没有合适候选时为空数组。",
      "maxItems": 3,
      "items": {
        "type": "object",
        "required": ["reasoning", "intentDesc", "intentType", "intentL1Tag", "intentL2Tag", "parent"],
        "additionalProperties": false,
        "properties": {
          "reasoning": {
            "type": "string",
            "maxLength": 30,
            "description": "简述输入依据及新增价值，含标点最多30字符，不展开推理过程。"
          },
          "intentDesc": {
            "type": "string",
            "description": "用户视角的具体需求或兴趣切口，保留关键对象、偏好和约束，不只复述来源或画像。"
          },
          "intentType": {
            "type": "string",
            "enum": ["ShortTermIntent", "LongTermIntent"],
            "description": "按「类型与时间规则」根据子意图的需求性质判断。"
          },
          "startTime": {
            "type": "string",
            "pattern": "^[0-9]{4}-[0-9]{2}-[0-9]{2}T[0-9]{2}:[0-9]{2}:[0-9]{2}$",
            "description": "可选。需求开始值得关注的时间，可根据上下文推测，格式YYYY-MM-DDThh:mm:ss；无合理依据时省略。"
          },
          "expireTime": {
            "type": "string",
            "pattern": "^[0-9]{4}-[0-9]{2}-[0-9]{2}T[0-9]{2}:[0-9]{2}:[0-9]{2}$",
            "description": "可选。需求预计结束或失效的时间，可根据上下文推测，格式YYYY-MM-DDThh:mm:ss；无合理依据时省略。与startTime均填写时须满足startTime不晚于expireTime。"
          },
          "intentL1Tag": {
            "type": "string",
            "description": "一级标签，取标签分类体系中的键名。"
          },
          "intentL2Tag": {
            "type": "string",
            "description": "二级标签，取intentL1Tag对应数组中的值，不得跨一级组合或自造标签。"
          },
          "parent": {
            "type": "string",
            "description": "按来源的原始意图分类和意图描述拼接为一级标签&二级标签&意图描述；不使用新子意图的标签或描述，不改写来源原值。"
          },
          "searchQueries": {
            "type": "array",
            "maxItems": 2,
            "items": { "type": "string" },
            "description": "可选。需要外部信息支持时，围绕意图中的关键问题生成具体搜索词，带上影响答案的已知条件，按真实用户的搜索习惯书写；模型已有知识足以展开时省略。查询词不得补造用户条件，也不代表已经搜索或核实答案。"
          }
        }
      }
    }
  }
}
```

严禁包含任何 JSON 结构以外的文字或 Markdown 标记（如 \```json）。
