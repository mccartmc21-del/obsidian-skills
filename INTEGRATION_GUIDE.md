# Integration Guide: Obsidian Skills for Vault-Hearst

**For:** Michael McCarthy (michael.mccarthy@hearst.com)
**Vault:** Hearst (Google Drive)
**Fork:** mccartmc21-del/obsidian-skills (upstream: kepano/obsidian-skills)

---

## What This Repo Contains

This repository provides **5 Agent Skills** that teach AI coding agents (Claude Code, Codex CLI, and other Agent Skills-compatible tools) how to properly create, edit, and interact with Obsidian vault files. These are *not* Obsidian plugins — they are reference documents that agents read to understand Obsidian's file formats.

| Skill | What It Teaches Agents | File Types |
|-------|----------------------|------------|
| **obsidian-markdown** | Wikilinks, callouts, embeds, frontmatter, tags, and all Obsidian-flavored Markdown syntax | `.md` |
| **obsidian-bases** | Database-like views with YAML filters, formulas, grouping, and summaries | `.base` |
| **json-canvas** | Visual canvas files with nodes, edges, groups, and connections | `.canvas` |
| **obsidian-cli** | CLI commands to read/create/search notes, manage tasks, develop plugins | N/A (CLI) |
| **defuddle** | Web page content extraction into clean Markdown (saves tokens) | N/A (CLI) |

---

## Step-by-Step Integration with Vault-Hearst

### Option A: Claude Code Integration (Recommended)

Copy the `skills/` directory into a `.claude/` folder at the root of your vault:

```
Google Drive/
└── michael.mccarthy@hearst.com/
    └── Vault-Hearst/              ← Your Obsidian vault root
        ├── .claude/
        │   └── skills/
        │       ├── obsidian-markdown/
        │       │   └── SKILL.md
        │       ├── obsidian-bases/
        │       │   └── SKILL.md
        │       ├── json-canvas/
        │       │   └── SKILL.md
        │       ├── obsidian-cli/
        │       │   └── SKILL.md
        │       └── defuddle/
        │           └── SKILL.md
        ├── .obsidian/
        ├── Daily Notes/
        ├── Projects/
        └── ... (your other vault folders)
```

**Commands to set this up (adjust the vault path to match your actual location):**

```bash
# Set your vault path (adjust as needed)
VAULT_PATH="$HOME/Google Drive/michael.mccarthy@hearst.com/Vault-Hearst"

# Create the .claude/skills directory
mkdir -p "$VAULT_PATH/.claude/skills"

# Copy all skills into the vault
cp -r skills/* "$VAULT_PATH/.claude/skills/"
```

Once in place, when you use Claude Code pointed at your vault directory, the agent will automatically discover these skills and understand how to work with your Obsidian files correctly.

### Option B: Codex CLI Integration

```bash
# Copy skills to Codex's standard path
mkdir -p ~/.codex/skills
cp -r skills/* ~/.codex/skills/
```

### Option C: Claude Plugin Marketplace (Simplest)

If your agent supports the plugin marketplace:

```
/plugin marketplace add kepano/obsidian-skills
/plugin install obsidian@obsidian-skills
```

---

## Recommended Skills by Use Case

### For Day-to-Day Note Taking
- **obsidian-markdown** — Essential. Ensures the agent creates proper wikilinks (`[[Note Name]]`), correct frontmatter/properties, valid callouts, and embeds.

### For Building Dashboards and Databases
- **obsidian-bases** — Use this to have the agent create `.base` files that act as dynamic views over your notes. Great for:
  - Task trackers filtered by tags
  - Project overviews grouped by status
  - Reading lists with computed fields
  - Daily notes indexes

### For Visual Thinking and Mapping
- **json-canvas** — Lets the agent generate `.canvas` files for mind maps, flowcharts, project boards, and research canvases with proper node/edge structure.

### For Vault Management from Terminal
- **obsidian-cli** — Requires Obsidian to be running. Enables the agent to:
  - Read and create notes programmatically
  - Append to daily notes
  - Search across the vault
  - Manage properties/tags
  - Develop and debug plugins

### For Web Research
- **defuddle** — Install with `npm install -g defuddle-cli`. Lets the agent extract clean Markdown from URLs, stripping ads and navigation. Pairs well with obsidian-markdown for saving web content as vault notes.

---

## Google Drive Considerations

Since your vault lives in Google Drive, keep these points in mind:

1. **Sync conflicts** — Google Drive's sync can occasionally create conflict copies. Consider using Obsidian Sync or Git-based sync as a complement for version history.

2. **`.claude/` folder visibility** — The `.claude/` folder is hidden by default (dot-prefix). It won't clutter your vault in Obsidian's file explorer. If you want it visible, you can name it `_claude` instead and adjust the agent's configuration.

3. **Path with spaces** — The Google Drive path likely contains spaces. Always quote paths in shell commands:
   ```bash
   cd "$HOME/Google Drive/michael.mccarthy@hearst.com/Vault-Hearst"
   ```

4. **`.gitignore` for vault** — If you version-control your vault, add `.claude/` to your vault's `.gitignore` to keep skills separate from your notes. Or keep them tracked if you want version history on skill customizations.

---

## Customizing Skills for Hearst Workflows

Since you've forked this repo, you can customize the skills for your specific needs. Here are some ideas:

### 1. Add a Hearst-Specific Markdown Skill

Create `skills/hearst-notes/SKILL.md` with your team's conventions:
- Standard frontmatter properties used across Hearst (e.g., `brand`, `vertical`, `campaign`)
- Common tag taxonomies
- Template structures for recurring note types (meeting notes, content briefs, etc.)

### 2. Create Custom Bases Templates

Add `.base` file examples to the skill that match your workflow:
- Content calendar tracking by brand/publication
- Project status dashboards
- Contact/source databases

### 3. Add a Web Clipping Workflow

Combine defuddle + obsidian-markdown to define a standard flow:
- Clip article → extract with defuddle → format with proper frontmatter → save to vault

---

## Keeping Your Fork in Sync

To pull updates from the upstream `kepano/obsidian-skills` repo:

```bash
# Add upstream remote (one-time setup)
git remote add upstream https://github.com/kepano/obsidian-skills.git

# Fetch and merge upstream changes
git fetch upstream
git merge upstream/main
```

---

## Quick Start Checklist

- [ ] Copy `skills/` into your vault at `Vault-Hearst/.claude/skills/`
- [ ] Install defuddle CLI: `npm install -g defuddle-cli`
- [ ] Install Obsidian CLI (if not already): check `obsidian help`
- [ ] Test with Claude Code by opening your vault directory and asking it to create a note
- [ ] (Optional) Customize skills for Hearst-specific workflows
- [ ] (Optional) Set up upstream remote to stay current with kepano's updates
