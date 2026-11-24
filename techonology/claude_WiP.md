# Claude

## Minor QA

Which order claude.md reads:
Organization policy (C:\Program Files\ClaudeCode\CLAUDE.md)
User (~/.claude/CLAUDE.md)
Project memory (./.claude/CLAUDE.mr)
Local memory (./CLAUDE.local.md)

## Developer foundation lesson by certSafari (2026-10)

### Agents and workflow

workflow sends model calls and tools down code paths you wrote in advance. An agent lets the model direct its own process, loop the action untils decides its done. Anthropic tag them as "agentic systems".\
Messages API is for teams that want custom agent loops and fine-grained control. Claude Managed Agents provides a ready-made harness for long-running, asynchronous work.

Multiple agents is better than one when we have context pollutio, parallel exploration or specialization, but end up costly.

Workflows:

Prompt chaining makes previous output e processes with optional programmatic gates, making a task split for accuracy at cost of latency.\
Routing classify the unpit and send to specialized element.\
Parallelization section split the code, run parallely and join later.\
Parallelization voting makes task run in loop for multiple output to get confidence result.\
Orchrestrator worker, break steps in runtime, delegate and synthetizes the result, good for multi-file but unpredictable.\
Evaluator-optimizer, one generate and another evaluate in loop.

Agent runs:

- Client SDK, you write the tool for your code, use for maximum control.\
- Agend SDK, Claude Code binary run using Python or Typescript, use for C.Code agent embedded on your application.\
- C. Code CLI, run on developer terminal, use for interaction or single task.\
- Managed Agents, a hosted agent harness that run the agent loop on sandbox or your own infrastructure, use for ready to use long and asynchronous interaction.

Agent SDK run a stra of typed message. SystemMessage with "init" subtype star the session, Claude response have AssistantMessage, tools returna a UserMessage, ResultMessage is the last loop having final text, token usage, cost and session ID.\
Subtype can be sucess, error_max_turns, error_max_budget_usd, and error_during_execution; only the first have result fields.

Agent can configure how to handle tools using allowedTools, canUseTool, disallowedTools, permissionMode, and bypassPermision; subagents have AgentDefinition tools and promts.

Hooks have event, matcher and callback(s), it trigget by the PreToolUse.\
Every callback have PreToolUseHookInput (with tool_name and tool_input), and every input carries session_id, cwd and hook_event_name.
Then hooks call a subagent it passes agent_id and agent_type; in case of failure  onFailure (on_failure in Python) to "block" on the matcher. A failed callback then blocks the action.\
It can have the outputs permissionDecision for SDK to block or skip tool permission call, permissionDecisionReason (on deny) for Claude to explain why it was blocked. systemMessage for the user, and continue / continue_ for SDK to explain the loop.

Lifecycle hooks have the events, run triggers and usage:

- PreToolUse; before a tool and block it; Deny dangerous Bash command;
- PostToolUse; after tool succeeds; log changes and audit trail;
- UserPromptSubmit; when a prompt is made and before Claude read it; Add context to prompt;
- Stop; when Claude finish reponding; Save session before exit;
- PreCompact; When recieve a compaction request; arqive transcript before sum.arize;
SubagentStart / ..Stop; when agent start / stop; log the summary;
- SessionStart / ..End; Session start / ends; start telemetry and clean up.

Agent can failure, here is what symphom, cause and correction:
- Run stop abrutly after model say "complete", fix by using stop_reson is tool_use and finish on end_turn;
- Tool call repeated until turn call, fix using tool_result with is_error and an instruction;
- Main chat offupy by file, fix by delegating a read subagent to it tht return the final message;
- Agent forget history, fix with memory tool that read before a new task;
- Early message is ignored after summarization, fix by ading a re-injected confi or enfornce with hook.