# Hikma

A Zsh-based CLI for organizing knowledge and digital information.

> **Status:** In active development (v0.2.0)

Hikma (meaning "wisdom" in Arabic) helps you structure and manage your digital information using the [PARA method](https://fortelabs.com/blog/para/) with Zettelkasten-style linking between notes.

- Organize content into Projects, Areas, Resources, and Archives
- Create standardized notes, tasks, and documents from templates
- Sync everything with Git

## Core Concepts

- **Workspace** - the root directory containing all your knowledge
- **Categories** - top-level organizational units (projects, areas, resources, archives by default)
- **Concerns** - specific projects, areas, or topics within a category
- **Items** - individual files: notes, tasks, meeting logs, emails, journals

## Prerequisites

- [Zsh](https://www.zsh.org/)
- [Git](https://git-scm.com/)
- Standard Unix utilities (`sed`, `awk`, `date`, `mkdir`, `cp`, `rm`, `mv`, `cat`, `find`, `tr`, `head`, `grep`)

## Installation

### Using Make (Recommended)

```bash
git clone https://github.com/asymmsystems/hikma-cli.git
cd hikma-cli
make install PREFIX=$HOME/.local
```

For a system-wide installation:

```bash
sudo make install
```

### Manual Installation

```bash
git clone https://github.com/asymmsystems/hikma-cli.git
chmod +x hikma-cli/bin/hikma.zsh

# Symlink to a directory in your $PATH
mkdir -p $HOME/.local/bin
ln -s "$(pwd)/hikma-cli/bin/hikma.zsh" $HOME/.local/bin/hikma
```

Verify:

```bash
hikma help
```

## Getting Started

### 1. Initialize Your Workspace

```bash
cd ~/Documents
hikma init
```

This creates a directory structure (`projects/`, `areas/`, `resources/`, `archives/`), a `.hikma_config` file, and initializes a Git repository.

### 2. Create a Project

```bash
hikma concern create projects my-first-project
```

### 3. Create Content

```bash
cd projects/my-first-project

# Create a task
hikma item create task --title "Outline project goals"

# Create a meeting note
hikma item create meeting --title "Initial planning session" --attendees "Team"
```

## Command Reference

| Command | Description |
|---------|-------------|
| `hikma init` | Initialize a new workspace |
| `hikma help` | Display help information |
| `hikma concern create <cat> <name>` | Create a new concern in a category |
| `hikma concern delete <cat> <name>` | Delete a concern (with confirmation) |
| `hikma concern mv <cat1> <name> <cat2>` | Move a concern between categories |
| `hikma concern archive <cat> <name>` | Archive a concern |
| `hikma item path <type>` | Get the resolved path for a new item |
| `hikma item content <type> [--options]` | Get processed template content |
| `hikma item create <type> [--options]` | Create a new item from a template |
| `hikma submodule create <repo_url>` | Add a Git submodule |
| `hikma submodule delete <name>` | Remove a Git submodule |

### Item Types

| Type | Description |
|------|-------------|
| `email` | Email correspondence logs |
| `meeting` | Meeting minutes and agendas |
| `journal` | Daily journal entries |
| `task` | Actionable tasks with priority and deadlines |
| `index-note` | Index/map of content (Zettelkasten-style) |
| `concept-note` | Permanent conceptual notes (Zettelkasten-style) |

## Configuration

Hikma is configured via `.hikma_config` in your workspace root (key=value properties file).

```conf
# Categories
hikma.categories=projects,areas,resources,archives

# Item types
hikma.item_types=email,meeting,journal,index-note,concept-note,task

# Item directories (supports variables like {{current_context}})
hikma.item_types.dir.email={{current_context}}/emails
hikma.item_types.dir.meeting={{current_context}}/meetings

# Templates
hikma.item_types.template.email={{default_template_dir}}/email.org
hikma.item_types.template.meeting={{default_template_dir}}/meeting.org

# Filename generators
hikma.item_types.filename.email=gen_email_name
hikma.item_types.filename.meeting=gen_meeting_name
```

## Design Principles

- **Separation of Concerns** - path determination, content preparation, and file creation are independent operations
- **Context Awareness** - auto-detects category and concern from current directory
- **Configuration-Driven** - behavior is controlled via `.hikma_config`, without changing code
- **Editor Agnostic** - works with Emacs, VS Code, Vim, or any editor
- **Git Integration** - operations are automatically committed

## Editor Integration

Since `item path`, `item content`, and `item create` are separate commands, Hikma is easy to integrate with any editor:

- **Emacs/Org-mode** - call Hikma from Elisp for `org-capture` workflows
- **VS Code** - wrap Hikma commands in an extension or tasks
- **Vim/Neovim** - invoke Hikma from custom commands or plugins

## Development

```bash
make lint      # Run shellcheck and zsh syntax checks
make test      # Run unit and integration tests
make docs      # Generate documentation
make package   # Create distributable tarball
```

## Tech Stack

- Zsh (POSIX-compatible shell scripting)
- Git (version control integration)
- Org-mode templates (plain-text Markdown-compatible)
- Make (build automation)

## License

MIT License - see [LICENSE](LICENSE) for details.

## Acknowledgments

- [PARA Method](https://fortelabs.com/blog/para/) by Tiago Forte

---

Part of [Asymm Systems](https://asymm.systems)
