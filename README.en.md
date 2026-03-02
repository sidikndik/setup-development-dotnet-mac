# 💻 .NET Development Setup for macOS
This guide is dedicated to developers who are switching to Mac or performing a fresh install.

## 1. Initial Preparation (Essential Tools)
Before diving into .NET, we need to set up the "foundation" to make installing other tools easier.

- Xcode Command Line Tools: Required for development tools.
```bash
xcode-select --install
```

- Homebrew: The best package manager for macOS.
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

## 2. Installing asdf & .NET Plugin
Instead of installing .NET globally, we use asdf for more flexible version management.

### A. Install asdf
```bash
brew install asdf
```

Make sure to add asdf to your shell (add to ~/.zshrc):
```bash
echo ". $(brew --prefix asdf)/libexec/asdf.sh" >> ~/.zshrc
source ~/.zshrc
```

### B. Install .NET Plugin
```bash
asdf plugin add dotnet
```

### C. Install a Specific .NET SDK
Now you can choose the version you want (e.g., .NET 10):
```bash
# Check available versions
asdf list-all dotnet

# Install the latest version (example: 10.0.103)
asdf install dotnet 10.0.103

# Set that version as the global default
asdf set -u dotnet 10.0.103
```

Tip: The advantage of asdf is that you can run `asdf set dotnet <version>` inside a specific project folder so that older projects continue to use older SDKs.

## 3. VS Code Setup (The Lean Machine)
Since we will be using VS Code exclusively, we need to ensure that C# integration works perfectly.

### A. Install VS Code
```bash
brew install --cask visual-studio-code
```

### B. Required Extensions
Open VS Code (Command + Shift + X) and install the following extensions:
1. **C# Dev Kit**: The official Microsoft extension that provides Visual Studio-like features (Solution Explorer, Test Explorer).
2. **C#**: Language support (Omnisharp/Roslyn).
3. **IntelliCode for C# Dev Kit**: AI-powered code auto-completion.
4. **.NET Install Tool**: Helps VS Code extensions find the SDK path from asdf.

## 4. Database & Containerization
Since SQL Server doesn't run natively on Mac, we use Docker.
- Docker Desktop: `brew install --cask docker`
- Azure Data Studio: (Best paired with VS Code) `brew install --cask azure-data-studio`

Run SQL Server (Azure SQL Edge) in Docker:
```bash
docker run --cap-add SYS_PTRACE -e "ACCEPT_EULA=1" -e "MSSQL_SA_PASSWORD=YourStrongPassword123!" \
-p 1433:1433 --name sql_server -d mcr.microsoft.com/azure-sql-edge
```

## 5. Final Verification
Open the terminal and make sure everything is connected:
```bash
# Check .NET version
dotnet --version

# Check binary location (should point to the .asdf folder)
which dotnet
```

## 🚀 Creating Your First Project (Step-by-Step)
Once asdf and VS Code are ready, follow these steps to create your first Web API:

### Step 1: Create a Project Folder
Open the terminal and navigate to where you store your projects:
```bash
mkdir MyDotNetProjects
cd MyDotNetProjects
```

### Step 2: Generate Project via Terminal
Use the Web API template (the most commonly used one):
```bash
# Create a new project in the 'MyAwesomeApi' folder
dotnet new webapi -n MyAwesomeApi

# Navigate to the project folder
cd MyAwesomeApi
```

### Step 3: Open in VS Code
Type this command in the terminal:
```bash
code .
```

Note: If you see a message saying "Do you trust the authors of the files in this folder?", select Yes.

### Step 4: Run the Project
1. Open the integrated terminal in VS Code (Ctrl + ~).
2. Type:
    ```bash
    dotnet run
    ```
3. Check the terminal output — you should see a URL like `http://localhost:5xxx`.
4. Open a browser and append /swagger or /scalar to the end of the URL (depending on the .NET version) to view your API documentation.
