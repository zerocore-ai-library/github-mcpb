# GitHub MCP Server

A remote MCP server for interacting with the GitHub API, providing tools for repositories, issues, pull requests, and more.

## Setup

### Using tool CLI

Install the CLI from https://github.com/zerocore-ai/tool-cli

```bash
# Install from tool.store
tool install library/github
```

```bash
# View available tools
tool info library/github
```

```bash
# List issues
tool call library/github -m list_issues -p owner=github -p repo=github-mcp-server
```

```bash
# Get file contents
tool call library/github -m get_file_contents -p owner=github -p repo=github-mcp-server -p path=README.md
```

### Authentication

GitHub MCP uses a Personal Access Token (PAT) for authentication. Create one at:

https://github.com/settings/personal-access-tokens

**Recommended scopes:**
- `repo` - Full control of private repositories
- `read:org` - Read org membership
- `read:user` - Read user profile data
- `read:packages` - Read packages

**Connection endpoint:**
- HTTP: `https://api.githubcopilot.com/mcp`

## Configuration

| Field | Required | Description |
|-------|----------|-------------|
| `gh_pat` | Yes | GitHub Personal Access Token for authentication |

**Create a PAT:** https://github.com/settings/personal-access-tokens

Recommended scopes: `repo`, `read:org`, `read:user`, `read:packages`

## Tools

### `list_issues`

List issues in a GitHub repository.

**Input:**
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `owner` | string | Yes | Repository owner |
| `repo` | string | Yes | Repository name |
| `state` | string | No | Filter by state: "OPEN" or "CLOSED" |
| `labels` | array | No | Filter by labels |
| `orderBy` | string | No | Order by: "CREATED_AT", "UPDATED_AT", "COMMENTS" |
| `direction` | string | No | Order direction: "ASC" or "DESC" |
| `perPage` | number | No | Results per page (max 100) |
| `after` | string | No | Cursor for pagination |

### `issue_read`

Get information about a specific issue.

**Input:**
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `owner` | string | Yes | Repository owner |
| `repo` | string | Yes | Repository name |
| `issue_number` | number | Yes | Issue number |
| `method` | string | Yes | "get", "get_comments", "get_sub_issues", or "get_labels" |

### `issue_write`

Create or update an issue.

**Input:**
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `owner` | string | Yes | Repository owner |
| `repo` | string | Yes | Repository name |
| `method` | string | Yes | "create" or "update" |
| `issue_number` | number | No | Issue number (required for update) |
| `title` | string | No | Issue title |
| `body` | string | No | Issue body content |
| `state` | string | No | "open" or "closed" |
| `labels` | array | No | Labels to apply |
| `assignees` | array | No | Usernames to assign |

### `add_issue_comment`

Add a comment to an issue or pull request.

**Input:**
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `owner` | string | Yes | Repository owner |
| `repo` | string | Yes | Repository name |
| `issue_number` | number | Yes | Issue number |
| `body` | string | Yes | Comment content |

### `list_pull_requests`

List pull requests in a repository.

**Input:**
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `owner` | string | Yes | Repository owner |
| `repo` | string | Yes | Repository name |
| `state` | string | No | "open", "closed", or "all" |
| `base` | string | No | Filter by base branch |
| `head` | string | No | Filter by head user/org and branch |
| `sort` | string | No | "created", "updated", "popularity", "long-running" |
| `direction` | string | No | "asc" or "desc" |

### `pull_request_read`

Get information about a specific pull request.

**Input:**
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `owner` | string | Yes | Repository owner |
| `repo` | string | Yes | Repository name |
| `pullNumber` | number | Yes | Pull request number |
| `method` | string | Yes | "get", "get_diff", "get_files", "get_commits", etc. |

### `create_pull_request`

Create a new pull request.

**Input:**
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `owner` | string | Yes | Repository owner |
| `repo` | string | Yes | Repository name |
| `title` | string | Yes | PR title |
| `head` | string | Yes | Branch containing changes |
| `base` | string | Yes | Branch to merge into |
| `body` | string | No | PR description |
| `draft` | boolean | No | Create as draft PR |

### `merge_pull_request`

Merge a pull request.

**Input:**
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `owner` | string | Yes | Repository owner |
| `repo` | string | Yes | Repository name |
| `pullNumber` | number | Yes | Pull request number |
| `merge_method` | string | No | "merge", "squash", or "rebase" |
| `commit_title` | string | No | Title for merge commit |

### `get_file_contents`

Get contents of a file or directory.

**Input:**
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `owner` | string | Yes | Repository owner |
| `repo` | string | Yes | Repository name |
| `path` | string | No | Path to file/directory (default: "/") |
| `ref` | string | No | Git ref (branch, tag, or commit) |

### `create_or_update_file`

Create or update a file in a repository.

**Input:**
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `owner` | string | Yes | Repository owner |
| `repo` | string | Yes | Repository name |
| `path` | string | Yes | Path to file |
| `content` | string | Yes | File content |
| `message` | string | Yes | Commit message |
| `branch` | string | Yes | Branch name |
| `sha` | string | No | SHA of file being replaced (for updates) |

### `search_code`

Search for code across repositories.

**Input:**
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `query` | string | Yes | Search query |
| `page` | number | No | Page number |
| `perPage` | number | No | Results per page (max 100) |

### `search_repositories`

Search for GitHub repositories.

**Input:**
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `query` | string | Yes | Search query |
| `page` | number | No | Page number |
| `perPage` | number | No | Results per page (max 100) |

### `list_commits`

Get list of commits in a repository.

**Input:**
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `owner` | string | Yes | Repository owner |
| `repo` | string | Yes | Repository name |
| `sha` | string | No | Branch, tag, or commit SHA |
| `author` | string | No | Filter by author |
| `page` | number | No | Page number |
| `perPage` | number | No | Results per page (max 100) |

### `get_commit`

Get details for a specific commit.

**Input:**
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `owner` | string | Yes | Repository owner |
| `repo` | string | Yes | Repository name |
| `sha` | string | Yes | Commit SHA, branch, or tag |
| `include_diff` | boolean | No | Include file diffs (default: true) |

### `create_branch`

Create a new branch.

**Input:**
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `owner` | string | Yes | Repository owner |
| `repo` | string | Yes | Repository name |
| `branch` | string | Yes | New branch name |
| `from_branch` | string | No | Source branch (defaults to repo default) |

### `create_repository`

Create a new repository.

**Input:**
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | Repository name |
| `description` | string | No | Repository description |
| `private` | boolean | No | Whether repo should be private |
| `autoInit` | boolean | No | Initialize with README |
| `organization` | string | No | Organization to create in |

### `fork_repository`

Fork a repository.

**Input:**
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `owner` | string | Yes | Repository owner |
| `repo` | string | Yes | Repository name |
| `organization` | string | No | Organization to fork to |

### `get_me`

Get details of the authenticated user.

**Input:** None

### `assign_copilot_to_issue`

Assign GitHub Copilot to work on an issue.

**Input:**
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `owner` | string | Yes | Repository owner |
| `repo` | string | Yes | Repository name |
| `issue_number` | number | Yes | Issue number |
| `base_ref` | string | No | Branch to start work from |
| `custom_instructions` | string | No | Additional guidance for Copilot |

## License

MIT

## References

- https://github.com/github/github-mcp-server
- https://docs.github.com/en/rest
