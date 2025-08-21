# Uninstall Cursor-Cli from Mac-Os

This guide provides step-by-step instructions to completely remove cursor-cli and all associated files from your macOS system.

## What cursor-cli installs

When you run `curl https://cursor.com/install -fsS | bash`, it installs:

- **Main installation**: `~/.local/share/cursor-agent/` (at my system its around ~136MB)
- **Symlink**: `~/.local/bin/cursor-agent` 
- **PATH modification**: Adds `~/.local/bin` to your shell's PATH in `~/.zshrc`

## Complete Uninstall Steps

### 1. Remove the cursor-agent binary and installation directory

```bash
# Remove the symlink
rm -f ~/.local/bin/cursor-agent

# Remove the entire cursor-agent installation directory
rm -rf ~/.local/share/cursor-agent
```

### 2. Clean up shell configuration

Remove the PATH modification from your shell configuration file:

```bash
# For zsh users (most common on macOS)
sed -i '' '/export PATH=.*\.local\/bin.*PATH/d' ~/.zshrc

# For bash users
sed -i '' '/export PATH=.*\.local\/bin.*PATH/d' ~/.bashrc
```

Alternatively, you can manually edit the file and remove this line:
```bash
export PATH="$HOME/.local/bin:$PATH"
```

### 3. Clean up ~/.local/bin directory (if empty)

```bash
# Check if ~/.local/bin is empty and remove if so
if [ -d ~/.local/bin ] && [ -z "$(ls -A ~/.local/bin)" ]; then
    rmdir ~/.local/bin
    echo "Removed empty ~/.local/bin directory"
fi

# Check if ~/.local is empty and remove if so
if [ -d ~/.local ] && [ -z "$(ls -A ~/.local)" ]; then
    rmdir ~/.local
    echo "Removed empty ~/.local directory"
fi
```

### 4. Optional: Remove Cursor Editor data (if you also want to remove the Cursor editor)

**⚠️ Warning**: Only do this if you also want to remove all Cursor editor settings and data.

```bash
# Remove Cursor editor application data
rm -rf ~/Library/Application\ Support/Cursor

# Remove Cursor editor caches (if they exist)
rm -rf ~/Library/Caches/com.todesktop.230313mzl4w4u92.ShipIt

```

### 5. Restart your terminal

After making these changes, restart your terminal or run:

```bash
# Reload your shell configuration
source ~/.zshrc  # or source ~/.bashrc for bash users
```

## Verification

Verify that cursor-cli has been completely removed:

```bash
# This should return "command not found"
cursor-agent --version

# Check that the files are gone
ls ~/.local/share/cursor-agent  # Should show "No such file or directory"
ls ~/.local/bin/cursor-agent    # Should show "No such file or directory"

# Verify PATH cleanup
echo $PATH | grep -o '\.local/bin'  # Should return nothing
```

## Summary of Removed Files and Directories

- `~/.local/share/cursor-agent/` - Main installation directory
- `~/.local/bin/cursor-agent` - Symlink to the binary
- PATH modification in `~/.zshrc` - Shell configuration change
- Optional: `~/Library/Application Support/Cursor/`  - Cursor editor data

If you find any problems or have any feedback to the progress, feel free to make a PR or Issue. 