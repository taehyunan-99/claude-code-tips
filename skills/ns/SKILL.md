Open a new Claude Code session in a new terminal at the current project directory.

Steps:
1. Detect OS and get current directory
2. Open new terminal based on platform:

   **For Windows (Git Bash/MINGW):**
   ```bash
   CURRENT_DIR=$(pwd | sed 's|^/c/|C:\\|' | sed 's|/|\\|g')
   powershell.exe -Command "Start-Process cmd -ArgumentList '/k','cd /d $CURRENT_DIR && set CLAUDECODE= && claude'"
   ```

   **For macOS:**
   ```bash
   CURRENT_DIR=$(pwd)
   osascript -e "tell application \"Terminal\" to do script \"cd '$CURRENT_DIR' && unset CLAUDECODE && claude\""
   ```

   **For Linux:**
   ```bash
   CURRENT_DIR=$(pwd)
   # Try common terminal emulators
   if command -v gnome-terminal &> /dev/null; then
       gnome-terminal -- bash -c "cd '$CURRENT_DIR' && unset CLAUDECODE && claude; exec bash"
   elif command -v xterm &> /dev/null; then
       xterm -e "cd '$CURRENT_DIR' && unset CLAUDECODE && claude; exec bash" &
   else
       x-terminal-emulator -e bash -c "cd '$CURRENT_DIR' && unset CLAUDECODE && claude; exec bash" &
   fi
   ```

3. Use this detection logic:
   ```bash
   if [[ "$OSTYPE" == "msys" || "$OSTYPE" == "win32" || "$OSTYPE" == "cygwin" ]]; then
       # Windows
   elif [[ "$OSTYPE" == "darwin"* ]]; then
       # macOS
   else
       # Linux
   fi
   ```

4. Tell user: "New Claude Code session opened in new terminal"
