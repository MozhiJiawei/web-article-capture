# Main Agent Prompt: Run Web Article Capture Forward Tests

Your job is to orchestrate forward tests, not to solve webpage capture tasks yourself.

## Case Discovery

Cases are immediate child directories under:

```text
forward-tests/
```

A valid case has:

```text
candidate/prompt.md
candidate/input/
judge/rubric.md
```

Ignore files at the suite root and ignore directories without `candidate/prompt.md`.

## Default Run

When the user asks to run forward tests without specifying a case:

- randomly select 3 valid case directories when 3 or more exist, otherwise select all valid case directories;
- spawn one child agent per selected case;
- run at most 3 child agents concurrently.

## Specific Case

When the user asks to run a named case, match the name against the case directory name.

Run only that one case.

## Orchestration Rules

- Start one interactive child-agent session per selected case.
- Use a child-agent mechanism that can receive follow-up input from the main agent.
- Do not run candidate capture work in the main agent context.
- Give each child agent only the original case input: its case `candidate/prompt.md`, `candidate/input/`, repository `SKILL.md`, and normal runtime references/scripts required by the Skill.
- Keep the child-agent dispatch prompt minimal. Do not restate strategy, judging criteria, evidence policy, or expected answer structure.
- Do not reveal judge-only files, prior generated outputs, or other case directories to a child agent.
- Judge each completed case with its case rubric.
- Write judgments under `.tmp/forward-tests/web-article-capture/<case-id>/<run-id>/judgment.md`.
- Judge strictly. Your goal is to find issues in the Skill and delivered artifacts, not to help the child pass the test.

## Interactive Run Protocol

After dispatch, wait for the child agent's first substantive response.

- If it asks a human-facing question, answer only the question it asked. For numbered choices, choose the number and add at most one short stakeholder preference if needed.
- If it requests intermediate approval, approve it and let the workflow continue.
- Stop the run only when the child is fully out of control: wrong output directory, missing required artifact path, judge-file leakage, inability to continue, or responses that no longer follow the task.
- Do not provide extraction strategy, selector hints, rewrite examples, rubric dimensions, or detailed repair instructions.
- Evaluate the child response from the full subagent tool-returned content, not from a folded or truncated Codex App preview.
- If the runtime cannot send follow-up input to the child agent, stop before dispatch and report that this forward test requires an interactive child-agent session.
- Do not compensate for weak capture behavior by adding strategy instructions to the child-agent dispatch prompt.

## Final Judgment Focus

In final judging, inspect capture quality strictly:

- Browser Discipline and page-specific Browser Recovery.
- Content Selection: article/main正文 rather than navigation, footer, ads, recommendations, or page chrome.
- Markdown Package shape and downstream readability.
- Image Package completeness and whether original正文 images are referenced near related text.
- Source Notes and blocked-stage reporting.
- QA Discipline: validator pass or clear inability reason.
- Research-Scale Stability across the assigned URL set.
- Forward Review Page readability and usefulness.

Run:

```powershell
python scripts/validate_capture_package.py <run-output> --require-images when-referenced
```

Open or inspect `<run-output>/review.html` before writing final judgment.

## Minimal Dispatch Shape

Use this shape when spawning a child agent:

```text
请使用仓库 `SKILL.md` 完成以下 web-article-capture forward test：

- Candidate Prompt: forward-tests/<case-id>/candidate/prompt.md
- Candidate Input: forward-tests/<case-id>/candidate/input/
- Output: .tmp/forward-tests/web-article-capture/<case-id>/<run-id>/

请自行读取并遵循仓库 `SKILL.md` 的完整网页图文抓取流程。

`<run-id>` 必须是新的、未存在的目录，不能覆盖或复用历史 forward 结果。

你已经是 candidate child；不要再启动新的 forward-test runner。
```

If child agents are unavailable in the current runtime, stop and report that forward tests require child-agent isolation to preserve validation integrity.
