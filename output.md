{
  "type": "object",
  "required": ["reasoning", "cards"],
  "properties": {
    "reasoning": {
      "type": "string",
      "maxLength": 160,
      "description": "选题依据（首页还是详情页，技能卡还是话题卡，cardType的选择依据，referenceSkills的选择依据）与收获总结"
    },
    "cards": {
      "type": "array",
      "description": "一个意图最多输出1张话题card、1张技能card，共至多2张",
      "maxItems": 2,
      "items": {
        "oneOf": [
          {
            "type": "object",
            "description": "首页卡片 trigger:celia_ai_homepage",
            "required": ["cardType", "chips", "greeting"],
            "properties": {
              "cardType": {
                "type": "string",
                "enum": ["0", "1", "2", "3"]
              },
              "chips": {
                "type": "array",
                "items": {
                  "type": "object",
                  "required": ["displayText", "clawAgentPrompt", "referenceSkills"],
                  "properties": {
                    "displayText": {
                      "type": "string"
                    },
                    "clawAgentPrompt": {
                      "type": "string"
                    },
                    "referenceSkills": {
                      "type": ["array", "null"],
                      "items": { "type": "string" },
                      "description": "话题chip为null；技能chip为包含一个技能名的非空数组"
                    }
                  }
                }
              },
              "greeting": {
                "type": "string",
                "minLength": 13,
                "maxLength": 20
              }
            },
            "if": { "properties": { "cardType": { "const": "0" } } },
            "then": {
              "properties": { "chips": { "minItems": 2, "maxItems": 2 } }
            },
            "else": {
              "properties": { "chips": { "minItems": 1, "maxItems": 1 } }
            }
          },
          {
            "type": "object",
            "description": "详情页卡片 trigger:co_space_detail_page",
            "required": ["cardType", "chips"],
            "not": { "required": ["greeting"] },
            "properties": {
              "cardType": {
                "type": "string",
                "enum": ["0", "4"]
              },
              "chips": {
                "type": "array",
                "items": {
                  "type": "object",
                  "required": ["displayText", "clawAgentPrompt", "referenceSkills"],
                  "properties": {
                    "displayText": {
                      "type": "string"
                    },
                    "clawAgentPrompt": {
                      "type": "string"
                    },
                    "referenceSkills": {
                      "type": ["array", "null"],
                      "items": { "type": "string" },
                      "description": "话题chip为null；技能chip为包含一个技能名的非空数组"
                    }
                  }
                }
              }
            },
            "if": { "properties": { "cardType": { "const": "0" } } },
            "then": {
              "properties": { "chips": { "minItems": 2, "maxItems": 2 } }
            },
            "else": {
              "properties": { "chips": { "minItems": 1, "maxItems": 1 } }
            }
          }
        ]
      }
    }
  }
}
