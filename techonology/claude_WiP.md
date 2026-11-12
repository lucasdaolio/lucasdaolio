# Claude

## Developer foundation lesson by certSafari (2026-10)

### Agent arquitecture

Single LLM with retrieval and sample, nobody decides because there is a singlestep;
Fixed workflow is cal by your code in splitted chained task;
Orchestrador-worker a central LLM pick subtasks and your code run around it, this makes unpredrict subtask;
Autonomous agent run in turns, making it flexble and expensive.

benefit and symptons of subagent over single-agent:
- Context isolation looks for tokens before make context;
- Parallel exploration is when a agent search for multiple questions;
- Specialization with focused toolset.

### Build and host a Claude agent

Claude agent run in loop, the amount varies how define your prompt is.\
Client SDK calls Claude API directly; Agent SDK uses Python or TypeScript to call the agent binary; Claude code is a agent with CRUD and run commands.

Use Client SDK when you want to call Claude API on your code.\
Use Agent SDK whem you want to embed Claude Code`s agent to you Python or Typescript.\
Use Manage agents when you want Anthropic host the agent and setup via Claude API.\
Use Claude Code CLI for one-off task at terminal.

Agent is the model, system prompt, tools, MCP and skill; you create a and referece it by your ID.\
Enviroment is where the agent run.\
Sesssion is agent intance un an enviroment, doing tasks.\
Event are message between application and agent like turns and results

| Concern | Cloud enviromnet | Self-hosted sandbox |
|:--|:--|:--|
| Where tools run | Anthropic-managed sandboxes | Your infrastructure |
| Network reach | Anthropic`s egress controls | Yout network policy |
| File and Github repo mounting | Managed by Anthropic | Manage by you |
| Memory store | Mounted by Anthropic at /mnt/memory/ |  Downloaded to /mtn/memor/ and synced by SDK worker |
| Licecycle | Managed by Anthropic | Managed by you |

### Agent SDK query() loop, hooks and subagents

