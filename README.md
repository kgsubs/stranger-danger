# stranger-danger

Check a stranger's GitHub repo before you let it onto your computer.

Someone posts a repo and says it will save you hours. Installing it means running a stranger's code, or handing a stranger's instructions to your AI agent, with your files and your logins in reach. This skill reads the repo's source where it sits on GitHub (skipping bundled third-party libraries, binaries and lockfiles), without downloading or running any of it, and tells you whether to adopt it, adopt it with care, or avoid it.

## What it checks

- Truth: does the code do what the README says it does? Each claim is marked backed, partial or unbacked.
- Quality: tests, error handling, structure, upkeep, and whether one person or a team keeps it alive
- Install hooks: anything that runs by itself the moment you install
- Hidden code: encoded blobs, `eval`, code pulled from a URL at run time
- Data leaving your computer: calls to outside hosts, undisclosed tracking
- Reaching for your secrets: SSH keys, cloud logins, browser cookies, env files
- Risky dependencies: look-alike package names, unpinned versions, packages pulled from odd places
- Things that stay behind or destroy: scheduled jobs, shell profile edits, recursive deletes
- Permissions wider than the tool needs
- Agent assets: prompts, skills, hooks, settings and MCP configs that an AI agent would load or obey, including "paste this line and the agent sets it up" installs and steps that switch off approvals or a sandbox
- Where it came from: repo age against its stars, who the author is, and whether the download you would get can be matched to the source

It assumes the author might be hostile, and it treats everything in the repo as data. Text in a repo that tries to give the reviewer orders is itself reported as a finding.

## What you get

- One verdict up front: ADOPT, ADOPT WITH CAVEATS or AVOID
- A three-line scorecard: does what it claims, quality, safety
- The exact version it reviewed, so you can install that version and no other
- Repo facts, not held against the repo: one author, few stars, no tests, ready-made downloads and other things most public repos share, each with a brief line on why it is normal or not
- Each finding with its severity, the file, the quoted lines, and why it matters to you
- "If you adopt anyway": the guardrails to use, such as pinning a reviewed version
- "What I could not assess": the limits of a read-only review, stated plainly
- Only findings that were checked a second time against the file. The rest are dropped.

## Install

1. Open Claude Code on your computer, in the desktop app or a terminal.
2. Send Claude this message:

```
Install the stranger-danger skill: clone https://github.com/kgsubs/stranger-danger into ~/.claude/skills/stranger-danger (on Windows: %USERPROFILE%\.claude\skills\stranger-danger) and check out tag v1.3. If git isn't installed, download https://github.com/kgsubs/stranger-danger/archive/refs/tags/v1.3.zip and put the files from its stranger-danger-1.3 folder there instead.
```

3. If Claude asks for permission to run git or write to the skills folder, say yes.
4. Start a new Claude Code session so it sees the skill.

The install uses a fixed version (v1.3), so you get the version that was published under that name, not whatever changed since.

It works best with the GitHub command line tool (`gh`) installed and signed in. Without it, the skill reads public repos through the web, which is slower and has a lower hourly limit.

## Use it

Type this in Claude Code, with the repo you want checked:

```
/stranger-danger https://github.com/owner/repo
```

It runs only when you type that. Claude never starts it on its own.

Then:

1. Wait while it reads the repo. Nothing is downloaded, installed or run.
2. Read the verdict and the scorecard.
3. Read the findings if you plan to adopt it anyway.

## Good to know

- It reads, it does not run. It cannot tell you whether the tests pass or what the code does when it is running. It says so in every report.
- It reviews the source. If the repo's install gives you a ready-made download, the skill cannot confirm that download was built from the source it read, and it tells you.
- A clean report covers the repo as it stood that day. Code that changes later, and other tools the repo installs, are not covered.
- It uses tokens. One run on a mid-sized repo (guides, prompts, templates and a few scripts) took about 5 minutes and about 200,000 tokens. A large repo costs more.
- It changes nothing in the repo and writes no fixes. It keeps one notes file of its findings in your cache folder.
- If you tell Claude in plain words to download or run the repo, your words win. Without that, it never does.

## Privacy

The skill is one text file of instructions. It has no script. Its instructions send it only to GitHub, to read the repo you named, and to the public package registries when it checks whether a dependency is current.

It never copies a secret it finds into its notes or its report. It names the file instead.

## Update or remove

- To update, look at the tags on the GitHub page, then ask Claude: "Update the stranger-danger skill to tag v1.4" (with the tag you want). Claude fetches that tag and checks it out, or downloads that tag's zip and replaces the files.
- To remove, ask Claude: "Remove the stranger-danger skill."

MIT license. See [LICENSE](LICENSE).
