# Mac setup: VS Code, Git, and Claude Code

This guide sets up a beginner-friendly coding workspace on a Mac. You do **not** need a GitHub account.

Almost everything happens in Apple’s **Terminal** app. Two system interactions are expected:

1. macOS may show an Apple window to approve the Command Line Developer Tools.
2. Claude Code opens a browser once so you can sign in to your Claude subscription.

## Before you begin

You need:

- A Mac with macOS 13 or later
- An internet connection
- The password you use to sign in to your Mac
- A Claude Pro, Max, Team, or Enterprise subscription

> Run **one code block at a time**. Wait for the command to finish and for the Terminal prompt to return before moving to the next block.

## 1. Open Terminal

1. Press **Command + Space**.
2. Type **Terminal**.
3. Press **Return**.

Check your macOS version:

```bash
sw_vers -productVersion
```

The first number should be `13` or higher. If it is lower, update macOS before continuing.

## 2. Install Git and Apple’s command-line tools

Paste this into Terminal:

```bash
xcode-select -p >/dev/null 2>&1 || xcode-select --install
```

If an Apple installer window appears:

1. Select **Install**.
2. Agree to Apple’s license.
3. Wait for the installation to finish.
4. Return to Terminal.

Confirm that Git is available:

```bash
git --version
```

Success looks like `git version` followed by a version number.

## 3. Install VS Code from Terminal

Create an Applications folder for your user account:

```bash
mkdir -p "$HOME/Applications"
```

Download Microsoft’s current universal Mac build of VS Code:

```bash
curl -fL "https://update.code.visualstudio.com/latest/darwin-universal/stable" -o "/tmp/VSCode.zip"
```

Extract it into your Applications folder:

```bash
ditto -x -k "/tmp/VSCode.zip" "$HOME/Applications"
```

Remove the downloaded ZIP after extraction:

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

## 4. Install Claude Code

Run Anthropic’s recommended native installer:

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

Reload your Terminal environment:

```bash
exec zsh -l
```

Confirm that Claude Code is installed:

```bash
claude --version
```

Success shows a Claude Code version number.

## 5. Create your local demo repository

Create the folder:

```bash
mkdir -p "$HOME/Documents/claude-demo"
```

Move into it:

```bash
cd "$HOME/Documents/claude-demo"
```

Turn the folder into a local Git repository:

```bash
git init
```

Success includes `Initialized empty Git repository`.

Open the folder as the workspace root in VS Code:

```bash
code .
```

VS Code should open with `claude-demo` shown as the top-level folder.

## 6. Add your working files

Put **copies** of the files you want to use into `Documents/claude-demo`. Remove confidential, private, or sensitive information first.

To reveal the folder in Finder from Terminal:

```bash
open "$HOME/Documents/claude-demo"
```

## 7. Sign in to Claude Code

Return to Terminal and make sure you are in the demo repository:

```bash
cd "$HOME/Documents/claude-demo"
```

Start Claude Code:

```bash
claude
```

Claude opens a browser for a one-time sign-in. Use the Claude account connected to your subscription. If the browser does not open, press `c` in Terminal to copy the login URL, then open that URL in your browser.

When Claude Code opens successfully, enter:

```text
/status
```

Confirm that it shows the expected subscription login.

## 8. Run Claude inside VS Code

1. Bring VS Code to the front.
2. Press **Control + backtick** to open VS Code’s integrated terminal.
3. Run:

```bash
claude
```

You are ready when VS Code is open at the `claude-demo` repository root and Claude Code is running in the integrated terminal.

## Quick verification

From the demo folder, each of these should succeed:

```bash
cd "$HOME/Documents/claude-demo"
git --version
code --version
claude --version
```

## If a command is not found

Reload the Terminal environment:

```bash
exec zsh -l
```

Then retry the failed verification command. If it still fails, save a screenshot of the complete Terminal error for the meeting.

## Official references

- [Apple Command Line Tools](https://developer.apple.com/documentation/xcode/installing-the-command-line-tools)
- [VS Code downloads](https://code.visualstudio.com/docs/supporting/faq#_previous-release-versions)
- [VS Code command-line interface](https://code.visualstudio.com/docs/configure/command-line)
- [Claude Code quickstart](https://code.claude.com/docs/en/quickstart)
