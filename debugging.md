Title: Debbuging Application
Desc: This page documents details debugging tools and its details for aah application.
Keywords: debugging, delve, aah, aah application, vscode, code
---
# Debbuging Application

### Table of Contents

  * [Debugging with VS Code & Delve](#debugging-with-vs-code-delve)


## Debugging with VS Code & Delve

**Steps to Accomplish:**

1. Create `launch.json` (happens when clicking `Start Debugging` from Debug menu OR pressing `F5`)
2. Update `launch.json` with
    - Update program path - `"program": "${workspaceRoot}/app",`
    - Add arguments - `"args": ["run", "--envprofile", "dev"],`
3. Put breakpoints wherever needed
4. Run `aah build`
5. Press `F5` or click `Start Debugging` from Debug menu
6. That's it 😃

