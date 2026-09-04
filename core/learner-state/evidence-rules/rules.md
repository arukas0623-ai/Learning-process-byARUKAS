# Evidence Rules

1. `observed_facts` 只收录可追溯行为，不收录“聪明、懒惰、视觉型”等人格或学习风格标签。
2. `model_hypotheses` 必须链接一个或多个事实，并写明 falsifier / verification。
3. `evidence` 至少包含来源、日期、任务标识和结果；路径失效时标记为不可定位，不得假装可审计。
4. `VERIFIED` 必须有至少一条 evidence、至少一项已完成 verification，且 `confidence >= 0.6`。
5. `transfer_score` 只能来自未见情境；原题正确不等于迁移通过。
6. `constraints` 只有在相同错误或教学现象重复出现并有 `evidence_refs` 时才可长期化。
