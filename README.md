# Mac workspace setup: VS Code, Git, and Claude Code

This guide creates a beginner-friendly coding workspace on a Mac. You do **not** need a GitHub account.

## What success looks like

At the end:

- VS Code is open with one folder as the workspace root.
- That folder contains copies of **all files you want to work with**.
- The same folder is a local Git repository.
- VS Code’s integrated terminal is open at the repository root.
- Claude Code is signed in and running inside that VS Code terminal.

Almost everything begins in Apple’s **Terminal** app. Three interactions outside Terminal are expected: Apple may show a developer-tools approval window, Finder is used once to collect your files, and Claude opens a browser once for sign-in.

> Run **one code block at a time**. Wait for the command to finish and for the Terminal prompt to return before moving on.

## Before you begin

You need:

- A Mac with macOS 13 or later
- An internet connection
- The password used to sign in to the Mac
- A Claude Pro, Max, Team, or Enterprise subscription

## 1. Open Apple Terminal

1. Press **Command + Space**.
2. Type **Terminal**.
3. Press **Return**.

Check the macOS version:

```bash
sw_vers -productVersion
```

The first number should be `13` or higher. If it is lower, update macOS before continuing.

## 2. Create the workspace and collect your files

Create one workspace folder:

```bash
mkdir -p "$HOME/Documents/claude-demo"
```

Reveal that folder in Finder:

```bash
open "$HOME/Documents/claude-demo"
```

Copy every file and subfolder you want to use during the demo into the Finder window that opens.

- Use **copies**, not your only originals.
- Preserve the existing folders and filenames.
- Remove passwords, API keys, private records, and other sensitive information.

Return to Terminal and confirm that the workspace is not empty:

```bash
find "$HOME/Documents/claude-demo" -type f | head -20
```

You should see some of your filenames. **Do not continue with an empty workspace.**

## 3. Install Git and Apple’s command-line tools

Run:

```bash
xcode-select -p >/dev/null 2>&1 || xcode-select --install
```

If an Apple installer window appears:

1. Select **Install**.
2. Agree to Apple’s license.
3. Wait until installation finishes.
4. Return to Terminal.

Confirm that Git is available:

```bash
git --version
```

Success looks like `git version` followed by a version number.

## 4. Install VS Code entirely from Terminal

Create a user Applications folder:

```bash
mkdir -p "$HOME/Applications"
```

Download Microsoft’s current universal Mac build of VS Code:

```bash
curl -fL "https://update.code.visualstudio.com/latest/darwin-universal/stable" -o "/tmp/VSCode.zip"
```

Extract VS Code:

```bash
ditto -x -k "/tmp/VSCode.zip" "$HOME/Applications"
```

Remove the downloaded ZIP:

```bash
rm -f "/tmp/VSCode.zip"
```

Make the `code` command available in current and future Terminal sessions:

```bash
mkdir -p "$HOME/.local/bin"
ln -sf "$HOME/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code" "$HOME/.local/bin/code"
grep -qxF 'export PATH="$HOME/.local/bin:$PATH"' "$HOME/.zprofile" 2>/dev/null || echo 'export PATH="$HOME/.local/bin:$PATH"' >> "$HOME/.zprofile"
export PATH="$HOME/.local/bin:$PATH"
```

Confirm that VS Code is installed:

```bash
code --version
```

Success shows a VS Code version number and build information.

## 5. Install Claude Code

Run Anthropic’s recommended native installer:

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

Reload the Terminal environment:

```bash
exec zsh -l
```

Confirm that Claude Code is installed:

```bash
claude --version
```

Success shows a Claude Code version number.

## 6. Turn the workspace into a repository and open it in VS Code

Move into the workspace:

```bash
cd "$HOME/Documents/claude-demo"
```

Initialize the local Git repository:

```bash
git init
```

Success includes `Initialized empty Git repository`.

Open this repository as the VS Code workspace root:

```bash
code .
```

VS Code should open with `claude-demo` at the top of the Explorer sidebar and your working files underneath it.

## 7. Run Claude Code inside VS Code

From this point forward, work inside VS Code:

1. Bring VS Code to the front.
2. Press **Control + backtick** to open VS Code’s integrated terminal.
3. Confirm that the terminal is at the workspace root:

```bash
pwd
```

The path should end with `/Documents/claude-demo`.

Confirm that the terminal can see your files:

```bash
find . -type f -not -path './.git/*' | head -20
```

Start Claude Code inside the VS Code terminal:

```bash
claude
```

Claude opens a browser for a one-time sign-in. Use the Claude account connected to your subscription. If the browser does not open, press `c` in the terminal to copy the login URL, then open that URL in your browser.

When Claude Code starts, enter:

```text
/status
```

Confirm that it shows the expected subscription login.

## Final check

You are ready only when all of these are true:

- [ ] VS Code is open with `claude-demo` as the top-level workspace folder.
- [ ] All intended working files appear in the VS Code Explorer.
- [ ] VS Code’s integrated terminal is open.
- [ ] The terminal path ends with `/Documents/claude-demo`.
- [ ] Git is initialized in that folder.
- [ ] Claude Code is running in that integrated terminal.

If Claude is not currently running, open a new VS Code terminal with **Control + backtick** and run:

```bash
cd "$HOME/Documents/claude-demo"
claude
```

## Quick diagnostic

Run this from VS Code’s integrated terminal:

```bash
pwd
git rev-parse --show-toplevel
git --version
code --version
claude --version
find . -type f -not -path './.git/*' | head -20
```

The first two paths should both end with `/Documents/claude-demo`, every version command should succeed, and the final lines should list your working files.

## If a command is not found

Reload the shell environment:

```bash
exec zsh -l
```

Then retry the failed verification command. If it still fails, save a screenshot of the complete terminal error for the meeting.

## Official references

- [Apple Command Line Tools](https://developer.apple.com/documentation/xcode/installing-the-command-line-tools)
- [VS Code downloads](https://code.visualstudio.com/docs/supporting/faq#_previous-release-versions)
- [VS Code command-line interface](https://code.visualstudio.com/docs/configure/command-line)
- [Claude Code quickstart](https://code.claude.com/docs/en/quickstart)
