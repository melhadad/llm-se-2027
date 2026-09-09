
OpenSpec is an add-on to Coding agents. It is available from https://github.com/Fission-AI/OpenSpec - read the documentation there. This system supports "spec-driven" development in a structured manner with LLM support.

The OpenSpec system is a **spec-driven development workflow** that uses structured proposals before implementation. Here's how to work with OpenSpec on the ClassLLM project:

### **Three-Stage Workflow**

1. **Stage 1: Creating Changes (Proposal Phase)**
    
    - When you want to add features, make breaking changes, or redesign architecture, we create a formal proposal
    - We scaffold the proposal under `openspec/changes/<change-id>/` with:
        - [proposal.md](vscode-file://vscode-app/Applications/Visual%20Studio%20Code.app/Contents/Resources/app/out/vs/code/electron-browser/workbench/workbench.html) — Why, what, and impact
        - [tasks.md](vscode-file://vscode-app/Applications/Visual%20Studio%20Code.app/Contents/Resources/app/out/vs/code/electron-browser/workbench/workbench.html) — Implementation checklist
        - [design.md](vscode-file://vscode-app/Applications/Visual%20Studio%20Code.app/Contents/Resources/app/out/vs/code/electron-browser/workbench/workbench.html) (optional) — Technical decisions for complex changes
        - Delta specs — Specific requirement changes per affected capability
    - I validate the proposal with `openspec validate <change-id> --strict`
    - **Approval gate**: Don't start coding until the proposal is reviewed and approved
      
2. **Stage 2: Implementing Changes**
    
    - Read and understand the proposal and tasks
    - I implement changes sequentially, checking off tasks as they're completed
    - All work tracks back to the approved proposal
    - Ensures changes align with specs
      
3. **Stage 3: Archiving Changes**
    
    - After deployment, move `changes/<id>/` → `changes/archive/YYYY-MM-DD-<id>/`
    - Update [specs](vscode-file://vscode-app/Applications/Visual%20Studio%20Code.app/Contents/Resources/app/out/vs/code/electron-browser/workbench/workbench.html) if capabilities changed
    - Keep archived changes for audit trail

### **When to Create a Proposal vs. Direct Work**

**Create a proposal for:**

- New features/capabilities
- Breaking changes (API, schema, architecture)
- Performance optimizations that change behavior
- Security pattern updates

**Skip the proposal for:**

- Bug fixes (restoring intended behavior)
- Typos, formatting, comments
- Non-breaking dependency updates
- Configuration changes

### **Key Commands**

```bash
openspec list                       # See active changes
openspec list --specs               # See existing capabilities
openspec show [item]                # View details
openspec validate [id] --strict     # Validate proposals
openspec archive <change-id> --yes  # Archive after deploy
```

### **How to Trigger Proposal Creation**

Ask things like:

- "Help me plan a feature to add..."
- "Create a change proposal for..."
- "I want to add X functionality"

OpenSpec will then:

1. Review existing specs in [specs](vscode-file://vscode-app/Applications/Visual%20Studio%20Code.app/Contents/Resources/app/out/vs/code/electron-browser/workbench/workbench.html)
2. Check for conflicts in [changes](vscode-file://vscode-app/Applications/Visual%20Studio%20Code.app/Contents/Resources/app/out/vs/code/electron-browser/workbench/workbench.html)
3. Create the proposal structure with clear requirements and scenarios
4. Validate before showing it to you for approval

### **Key Files for Context**

- [project.md](vscode-file://vscode-app/Applications/Visual%20Studio%20Code.app/Contents/Resources/app/out/vs/code/electron-browser/workbench/workbench.html) — Project conventions (which we just filled out!)
- [specs](vscode-file://vscode-app/Applications/Visual%20Studio%20Code.app/Contents/Resources/app/out/vs/code/electron-browser/workbench/workbench.html) — Current truth of what IS built
- [changes](vscode-file://vscode-app/Applications/Visual%20Studio%20Code.app/Contents/Resources/app/out/vs/code/electron-browser/workbench/workbench.html) — Proposals for what SHOULD change

This approach keeps your codebase organized, changes well-documented, and prevents scope creep—every change has an approved spec before implementation begins.
