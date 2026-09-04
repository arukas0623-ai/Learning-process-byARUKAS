# Recall Evaluation

Recall 输出只包含 Learner View。Agent 必须先得到用户回答和自报置信度，再请求 Evaluator View。评分可以使用 0–5，但评分规则由 scheduler adapter 承担；不得用答案泄漏后的识别反应代替主动回忆。
