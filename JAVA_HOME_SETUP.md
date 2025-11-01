# How to Find and Set JAVA_HOME on Windows

## Quick Method - Find Your Java Installation

1. **Open PowerShell or Command Prompt** on your Jenkins server
2. **Run this command** to find Java:
   ```powershell
   where java
   ```
   Or:
   ```powershell
   java -version
   ```
   This shows the Java version and sometimes the path.

3. **Common Java Installation Paths on Windows:**
   - `C:\Program Files\Java\jdk-21`
   - `C:\Program Files\Java\jdk-17`
   - `C:\Program Files\Eclipse Adoptium\jdk-21.0.1+12`
   - `C:\Program Files\Microsoft\jdk-17.0.13.11-hotspot`
   - `C:\Program Files (x86)\Java\jdk1.8.0_XXX`

## Update Jenkinsfile

1. **Find your Java installation path** using the steps above
2. **Edit the Jenkinsfile** in your repository
3. **Update line 17** with your actual Java path:
   ```groovy
   JAVA_HOME = 'C:\\Program Files\\Java\\jdk-21'  // Change this path!
   ```

   **Important:** Use double backslashes (`\\`) in the path!

## Verify Java Path

To verify the path is correct:
```powershell
# In PowerShell, check if the path exists:
Test-Path "C:\Program Files\Java\jdk-21"

# Check if bin\java.exe exists:
Test-Path "C:\Program Files\Java\jdk-21\bin\java.exe"
```

## Alternative: Use Jenkins JDK Tool Configuration

If you prefer to use Jenkins JDK tool configuration instead:

1. Go to **Manage Jenkins** → **Tools** → **JDK**
2. **Add JDK** with name `JDK`
3. Set **JAVA_HOME** to your Java path
4. In Jenkinsfile, **uncomment line 6**:
   ```groovy
   jdk 'JDK'  // Uncomment this line
   ```
5. **Remove or comment out** the environment JAVA_HOME (line 17)

---

## Current Jenkinsfile Configuration

The Jenkinsfile currently:
- Uses Maven wrapper (`mvnw.cmd`) - no need to configure Maven separately
- Sets JAVA_HOME manually to `C:\Program Files\Java\jdk-21` (you may need to change this)
- Uses Maven tool `M3` from Jenkins (if configured)

**Next Steps:**
1. Find your Java installation path
2. Update JAVA_HOME in Jenkinsfile line 17
3. Push the changes
4. Run the pipeline again

