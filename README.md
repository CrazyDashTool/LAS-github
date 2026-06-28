# GitHub Git Operator

Test MCP plugin for Local Agent Studio.

It can:

- List GitHub repositories
- Clone a repository into the workspace
- Show git status
- Show recent commits
- Create local git commits

It does **not** push commits.

## Requirements

- Git
- GitHub CLI (`gh`)
- Node.js/npm for `npx github:<owner>/<repo>`

## Publish

Create a new GitHub repository and upload this folder with `plugin.json`, `package.json`, `server.cjs`, and `README.md`.

Then update `plugin.json`:

```json
"homepage": "https://github.com/<your-name>/las-github-git-operator",
"repository": "https://github.com/<your-name>/las-github-git-operator",
"args": ["-y", "github:<your-name>/las-github-git-operator"]
```

## Install In Local Agent Studio

After publishing, paste the GitHub repository URL into the Plugins tab:

```text
https://github.com/<your-name>/las-github-git-operator
```

LAS will download `plugin.json` and add this MCP server config:

```text
npx -y github:<your-name>/las-github-git-operator
```

Then open Settings -> MCP, set:

- `LAS_WORKSPACE` is filled automatically by Local Agent Studio
- `GITHUB_TOKEN` is optional if you use the plugin Sign in button
- `LAS_ALLOW_COMMITS=false` if you want to block commit creation

Then:

1. Open Plugins.
2. Click `Sign in with GitHub`.
3. Finish browser login.
4. Enable the MCP server.
5. Refresh tools.

Now the agent can decide to call `github_list_repos`, `git_clone`, `git_status`, `git_log`, or `git_commit`.

## Local Smoke Test

From this folder:

```powershell
$env:LAS_WORKSPACE=(Get-Location).Path
node server.cjs
```

Then send JSON-RPC lines through stdin, for example:

```json
{"jsonrpc":"2.0","id":1,"method":"initialize","params":{}}
{"jsonrpc":"2.0","id":2,"method":"tools/list","params":{}}
```
