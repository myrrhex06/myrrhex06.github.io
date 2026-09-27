---
title: "CI/CD - Gitlab CI/CD Rule 적용"
date: 2025-09-27 13:00:00 +0900
categories: [DevOps, CI/CD]
tags: [devops, gitlab, ci, cd, rule]
---

`.gitlab-ci.yml` 예시
```yaml
workflow:
  rules:
    - if: '$CI_COMMIT_BRANCH == "develop"'
      when: always
    - when: never

...
```

- `workflow`: 모든 `Job`에 적용