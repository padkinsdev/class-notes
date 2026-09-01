# Insecurity of AI coding assistants

## Introduction
- AI coding assistants are LLMs trained to summarize code, generate code from description, translate code between programming languages, etc.
- Previously developers would write code themselves, but nowadays developers write code in conjunction with AI assistants
- Human developers write vulnerable code, which AI models replicate via being trained on such code

## Measuring code security
- Security rate: The percentage of secure programs within unique compilable/parseable generated programs
    - Problem: Every generation counts
    - Problem: Correctness of generated code
    - The SOTA method uses prefix tuning to increase the security rate from 59% to 92% but it often generates incorrect code (what's the point of code that's secure but doesn't perform the intended function?)
- *Both* security *and* functionality are important
- Alternative metric: pass@k (given k generations, the expected likelihood of generating correct code)
    - secure@k_pass: Given k generations, the likelihood of the code being secure
- SecRepoBench: Utilize known security vulnerabilities to construct a benchmark
- Model ranking on single-file code completion is not generalizable to repository-level code completion!
- Prompting with a security policy reminder is less effective on SecRepoBench than SecCodePLT, and SecRepoBench generally provides more difficult problems for AI models
- About 30% of vulnerabilities take more than 8 days for humans to notice and fix
- About 10% of vulnerabilities take more than 100 days for humans to notice and fix

## Future research directions
- Reinforcement learning
- Generating code and unit tests together
- LLMs can explore code much faster than humans can, and are good at finding code that is similar to existing vulnerabilities
    - LLM patches are fairly close to "good patches" and the models almost always identify the root causes
- It generally takes vendors at least a month to fix reported vulnerabilities
- LLMs can generate code *and* code diffs

## What happens when you give agents access to sensitive information?
- Permissions?
- Prompt injections?
- Leaked user data?
- Malicious skills and plugins?