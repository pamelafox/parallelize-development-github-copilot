# Parallelize Your Development with GitHub Copilot

## Abstract

We're finally at a point with coding where we can truly work on multiple tasks at
once - and actually have it be an enjoyable and manageable experience! I'll
demonstrate how I split my work across multiple surfaces: the VS Code IDE when I
need to be the most hands-on, the GitHub Copilot desktop app for tasks that
require light supervision, and cloud agents for routine maintenance. I'll share
my tips for enabling truly parallel development of full-stack web applications
thanks to git worktrees and smart environment variable management. Plus, I'll
share my favorite automation workflows that run while I'm asleep, saving me time
so that I can focus on the fun parts of software development.

## Talk thesis

Parallel development is a coordination problem, not simply an agent-count
problem.

GitHub Copilot can help us work across multiple tasks, repositories, and time
horizons, but effective parallel development requires us to:

- Give each task the right amount of human supervision.
- Choose the surface that best fits the work.
- Isolate branches, worktrees, environments, ports, and credentials.
- Give every agent consistent instructions, skills, and tools.
- Notice when work finishes and review it efficiently.
- Integrate and validate changes before deciding what ships.

## Organizing questions

The talk is organized around three questions:

1. **Who is doing the parallel work?**
   - The developer
   - A single agent
   - Multiple independent agents
   - Subagents coordinated by another agent

2. **Where is the parallel work happening?**
   - In an existing VS Code workspace
   - In isolated local sessions and git worktrees
   - Across related repositories
   - In cloud agents
   - In scheduled or event-driven automation

3. **When is the parallel work happening?**
   - While I am actively coding
   - In the background while I supervise lightly
   - During an agent session through subagents
   - Later, on a schedule or in response to repository events
   - While I am away or asleep

## Learning outcomes

By the end of the talk, attendees should be able to:

1. Choose an appropriate Copilot surface based on how much supervision a task
   requires.
2. Prepare a full-stack repository for safe parallel development using git
   worktrees and isolated environment configuration.
3. Coordinate multiple agents across sessions and repositories while keeping
   their instructions and tools consistent.
4. Use notifications, code review, automations, and Agentic Workflows to manage
   work that completes asynchronously.

## Proposed talk flow

### 1. Opening: coding no longer has to be sequential

**Key idea:** Waiting for one task to finish before beginning another is no
longer the only manageable workflow.

- A typical developer has several tasks in progress:
  - A feature that needs hands-on architectural decisions
  - A well-scoped bug fix
  - A change in a related repository
  - Pull requests waiting for review
  - Routine maintenance and issue triage
- These tasks require different levels of attention.
- The opportunity is not just generating code faster; it is reducing waiting
  and context switching.

**Opening question:** Who should do each piece of work - me, an agent, or several
agents?

**Possible visual:** A sequential task timeline transforming into several
parallel lanes.

### 2. Who does the parallel work?

**Key idea:** The amount of supervision a task needs determines how it should be
delegated.

- **Developer-led work**
  - Ambiguous requirements
  - Architectural decisions
  - UX decisions
  - High-risk changes
- **Lightly supervised agent work**
  - Features with clear acceptance criteria
  - Changes that benefit from occasional human decisions
  - Work that can be inspected through a running app or preview
- **Hands-off cloud work**
  - Routine maintenance
  - Small, well-scoped issues
  - Changes with reliable automated validation
- **Subagent work**
  - Independent research or implementation threads within one larger task
  - Parallel review of distinct files or concerns

**Decision framework:**

| Task characteristic | Recommended approach |
| --- | --- |
| Ambiguous or high risk | Work hands-on in VS Code |
| Clear but needs occasional guidance | Copilot App or VS Code agent session |
| Bounded and strongly testable | Cloud agent |
| Several independent parts of one goal | Subagents |
| Repetitive or recurring | Automation or Agentic Workflow |

### 3. Where does the work happen? Choosing a Copilot surface

**Key idea:** GitHub Copilot is available across multiple surfaces, and each
surface makes a different kind of parallelism easier.

- **VS Code**
  - Best when I need to stay hands-on
  - Tight inner loop with code, diagnostics, tests, and the running application
- **VS Code Agents view**
  - Multiple visible sessions
  - Easier to monitor concurrent work without leaving the IDE
- **Copilot CLI**
  - Terminal-native work
  - Useful for scripts, repository operations, tests, and worktree-based tasks
- **GitHub Copilot App**
  - Multiple sessions visible in one UI
  - Light supervision, previews, screenshots, pull requests, and cross-session
    coordination
- **Cloud agents**
  - Work is distributed to remote environments
  - Can be assigned from GitHub.com and other supported entry points such as
    Teams or Slack
  - Good for asynchronous, well-scoped tasks

**Possible visual:** A comparison matrix with columns for supervision,
environment, session visibility, and best-fit work.

**Demo opportunity:** Launch or show multiple Copilot App sessions associated
with the same project.

### 4. Is the repository ready for parallel development?

**Key idea:** Agents cannot work independently if their environments are not
independent.

#### Git worktrees

- A worktree gives each task an isolated checkout and branch.
- Multiple sessions can work in the same repository without changing the files
  underneath each other.
- Worktrees reduce branch switching and protect the developer's active
  workspace.

#### Common worktree problems

- Untracked and ignored files are not copied automatically.
- `.env` and other local configuration may be missing.
- Dependencies may need to be installed independently.
- Applications may compete for the same ports.
- Local databases, caches, generated files, and credentials may be shared
  accidentally.

#### Worktree instructions

- Share the Copilot App instructions used to initialize a new worktree.
- Copy or link the required `.env` files deliberately.
- Install or restore dependencies.
- Allocate unique ports.
- Run a small readiness check before implementation starts.

**Asset needed:** A sanitized excerpt of the real worktree setup instructions.

#### Smart environment-variable management

- Use environment variables rather than hard-coded ports and resource names.
- Give each local frontend and backend instance a unique port.
- Give each feature branch a distinct cloud environment when backend resources
  must be deployed.
- Include a stable naming convention derived from the session, branch, or
  developer.

**Possible visual:** Two worktrees running the same full-stack application on
different frontend and backend ports.

### 5. Is the cloud environment ready for parallel development?

**Key idea:** Source isolation is only half the problem; cloud configuration and
authentication also need isolation.

- Use separate staging environments for features that need backend deployment.
- Avoid agents accidentally sharing mutable deployment state.
- Scope credentials and configuration to the task.
- Clean up temporary environments after integration.

#### `az-azd` skill

- Show the `az-azd` skill by Matt Gotteiner:
  <https://github.com/mattgotteiner/skills>
- Explain how it isolates Azure CLI and Azure Developer CLI configuration and
  authentication state.
- Show selected snippets that demonstrate the safety and isolation approach.

**Assets needed:**

- Screenshot or snippet of the skill instructions
- Example of two isolated feature environments
- A simple lifecycle diagram: create, deploy, validate, review, clean up

### 6. Parallel development across related repositories

**Key idea:** Product work often spans more than one repository.

- A feature may require updates to:
  - The main application
  - A shared library or SDK
  - Infrastructure
  - Samples
  - Documentation
- Separate Copilot App sessions can own each repository.
- Independent work can proceed concurrently.
- Dependent work needs an explicit order and contract.

**Questions to cover:**

- Which repository defines the interface or contract?
- Can downstream work use a temporary branch, commit, package, or mock?
- Which changes can be reviewed independently?
- In what order should the pull requests merge?

**Demo opportunity:** Prompt a session to start or coordinate a task in a
different repository.

**Possible visual:** A feature divided into application, library, infrastructure,
and documentation lanes.

### 7. Keep agents consistent

**Key idea:** Parallel agents amplify inconsistent instructions just as easily
as they amplify productivity.

- Use repository instructions for project-wide expectations.
- Configure the same relevant MCP servers.
- Use shared skills for repeatable workflows.
- Give every task:
  - Clear scope
  - Acceptance criteria
  - Validation commands
  - Constraints and non-goals
- Decide what belongs in:
  - Repository instructions
  - Personal instructions
  - Skills
  - The individual task prompt

**Possible visual:** A shared context layer feeding several agent sessions.

### 8. Parallel work within one session: subagents

**Key idea:** Separate sessions are not the only unit of parallelism.

- A primary agent can delegate bounded work to subagents.
- Good subagent tasks:
  - Independent research questions
  - Separate implementation components
  - Targeted test or build execution
  - Review of independent changes
- Poor subagent tasks:
  - Several agents editing the same tightly coupled code
  - A single continuous debugging trace split without a clear boundary

#### Examples to collect

- Copilot App commands that reliably invoke parallel review or subagents
- A prompt that asks the agent to divide a task into independent workstreams
- A prompt that starts work in a different repository

**Example prompt shape:**

> Split this feature into independent frontend, backend, and test workstreams.
> Run the independent investigations in parallel, identify dependencies before
> editing, and integrate the results only after each workstream has validated
> its changes.

### 9. Agents finish at different times: notifications and inboxes

**Key idea:** Parallel work is only useful if I can notice when it needs my
attention.

- Compare notification styles across:
  - VS Code
  - GitHub Copilot App
  - GitHub.com cloud agents
  - Other supported cloud-agent entry points
- Show the Copilot App inbox.
- Show where cloud-agent status and completed work appear on GitHub.com.
- Show the sound hook used when an agent completes while away from the desk.
- Distinguish:
  - Task completed
  - Agent waiting for input
  - Plan waiting for approval
  - Validation failed
  - Pull request ready for review

**Possible visual:** A notification and attention-routing diagram.

### 10. Work that happens while I do something else

**Key idea:** Some parallel work should not need an active development session.

#### Copilot App automations

- Save recurring agent tasks.
- Run them manually, on a schedule, or in response to supported events.
- Run locally or in the cloud, depending on the task.
- Examples:
  - Daily issue triage
  - Morning pull request review-status check
  - Repository health report

#### GitHub Agentic Workflows

- Define repository automation in Markdown.
- Run agents inside GitHub Actions.
- Use scheduled and event-driven triggers.
- Combine agent reasoning with guarded, reviewable repository outputs.
- Examples:
  - Documentation freshness checks
  - CI failure analysis
  - Test improvement suggestions
  - Issue and discussion triage
  - Ready-to-review maintenance pull requests

**Key line:** This is where development continues while I am focused elsewhere -
or while I am asleep.

### 11. Review and integration are also parallel work

**Key idea:** Finishing implementation does not mean the work is ready to ship.

- Use Copilot code review while implementation continues elsewhere.
- Review work as agents finish rather than waiting for every task.
- Triage review comments:
  - Accept and implement
  - Ask the agent to iterate
  - Reject with an explanation
- Watch for:
  - Conflicting changes
  - Cross-repository version dependencies
  - Mismatched assumptions
  - Tests that only pass in isolation
- Merge in dependency order.
- Keep the human responsible for architectural decisions and release approval.

**Possible visual:** Parallel implementation lanes converging into review,
validation, and ordered merges.

### 12. Closing framework

Use five verbs to summarize the workflow:

1. **Delegate** work that is bounded and verifiable.
2. **Isolate** repositories, branches, environments, ports, and credentials.
3. **Standardize** instructions, skills, tools, and acceptance criteria.
4. **Observe** progress through notifications, hooks, and inboxes.
5. **Integrate** through tests, code review, and human judgment.

**Closing idea:** The goal is not to supervise the largest possible number of
agents. The goal is to keep useful work moving without losing control of quality
or architecture.

## Potential demo storyline

The exact demos should be selected after the talk duration is confirmed. A
possible continuous storyline:

1. Begin hands-on work on a feature in VS Code.
2. Delegate a well-scoped issue to a Copilot App session in another worktree.
3. Start a related documentation or library change in another repository.
4. Assign a routine maintenance issue to a cloud agent.
5. Show the environment and worktree isolation that allows all four to run.
6. Use a subagent-enabled operation while the primary sessions continue.
7. Show notifications as the shorter tasks finish.
8. Review one completed change with Copilot code review.
9. Show a scheduled automation or Agentic Workflow that ran while away.
10. Bring the workstreams together in dependency order.

## Slide and demo planning checklist

- [ ] Confirm the talk duration and expected Q&A time.
- [ ] Decide the number of live demos versus screenshots or recordings.
- [ ] Select one primary application and the related repositories.
- [ ] Choose tasks with predictable completion times.
- [ ] Record the intended supervision level for each task.
- [ ] Capture the Copilot surface comparison.
- [ ] Sanitize and capture the worktree initialization instructions.
- [ ] Prepare the multi-port full-stack application example.
- [ ] Capture selected `az-azd` skill snippets.
- [ ] Prepare the cross-repository task example.
- [ ] Collect reliable subagent command and prompt examples.
- [ ] Capture notification and inbox screenshots.
- [ ] Prepare the sound-hook example.
- [ ] Select Copilot App automations to demonstrate.
- [ ] Select GitHub Agentic Workflows to demonstrate.
- [ ] Prepare a Copilot code-review example.
- [ ] Create fallback screenshots or recordings for every cloud-dependent demo.
- [ ] Write a timed run-of-show after the demos are rehearsed.

## Open questions

- How long is the talk?
- How much of the session should be live demonstration?
- Which application and related repositories provide the clearest story?
- Should the main demo emphasize multiple sessions in one repository,
  cross-repository coordination, or both equally?
- Which Copilot surfaces will be generally available to the audience at the
  time of the conference?
- Which commands consistently trigger subagents and are appropriate to show?
- Which task should demonstrate Copilot code review?
- Which automation or Agentic Workflow creates the strongest "work completed
  while I was away" result?
