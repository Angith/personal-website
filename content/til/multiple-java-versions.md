---
title: 'Switching Java Versions on macOS: Manual vs. jenv'
date: 2026-04-27
---

## 1. Manual Switching (Native & Quick)

macOS includes `/usr/libexec/java_home` to manage Java installations. You can view installed versions using `/usr/libexec/java_home -V`.

To quickly switch versions without installing extra tools, add this helper function to your shell profile (`~/.zshrc`):

```bash
use-java() {
  export JAVA_HOME=$(/usr/libexec/java_home -v "$1")
  export PATH="$JAVA_HOME/bin:$(echo $PATH | sed -E 's|/Library/Java/JavaVirtualMachines/[^:]+/Contents/Home/bin:||g')"
  java -version
}
```

- **Usage:** Run `use-java 17` or `use-java 1.8` to switch globally in your current terminal.
- **Pros/Cons:** It's simple and native, but you must remember to switch manually for different projects.

## 2. Automated Switching with jenv (Recommended)

If you regularly work on multiple projects requiring different Java versions, **jenv** provides a much cleaner, automated workflow.

### Setup & Configuration

Add the following to your `~/.zshrc` to initialize `jenv`:

```bash
export PATH="$HOME/.jenv/bin:$PATH"
eval "$(jenv init -)"
jenv enable-plugin export
```

Then, register your installed JDKs with `jenv` (one-time setup per version):

```bash
jenv add /Library/Java/JavaVirtualMachines/<your-jdk>/Contents/Home

# Example:
# jenv add /Library/Java/JavaVirtualMachines/zulu-17.jdk/Contents/Home
```

### Common Commands & Project-Based Switching

`jenv` shines with its per-project configuration:

- **`jenv global 17`**: Sets your default system-wide Java version.
- **`jenv local 21`**: Sets the Java version for the current directory (creates a `.java-version` file). When you `cd` into this project, the Java version switches automatically.
- **`jenv versions`**: Lists all registered Java versions.
- **`jenv remove <version>`**: Unregisters a specific version.

### Quick Global Switching with Aliases

If you switch global versions frequently, you can add these aliases to your `~/.zshrc` so you can switch instantly using commands like `j17` or `j21`:

```bash
alias j17="jenv global 17"
alias j21="jenv global 21"
alias j8="jenv global 1.8"
```

## 3. Adding a New Java Version

When you install a new JDK (e.g., via Homebrew or a manual download), here is how to integrate it into your workflow:

**If you use the Manual approach:**

1. Verify the new version is detected by macOS: run `/usr/libexec/java_home -V`.
2. That's it! You can immediately switch to it using your function (e.g., `use-java 21`).

**If you use jenv:**

1. Verify the new version path: run `/usr/libexec/java_home -V`.
2. Register the new path with `jenv`:

   ```bash
   jenv add /Library/Java/JavaVirtualMachines/<new-jdk-folder>/Contents/Home
   ```

3. Run `jenv versions` to verify it was added successfully.

## Key Takeaways

- **Don't mix methods:** Avoid manually setting `JAVA_HOME` if you are using `jenv`.
- **Verify shims:** `jenv` works via executable shims. Running `which java` should always point to `~/.jenv/shims/java`.
- **Verdict:** The manual alias is great for quick overrides, but `jenv local` and `jenv global` offer a superior, error-free development experience.
