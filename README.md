Secure Local AI Coding Environment 🔒A guide for Computer Science students and developers to set up a private, free, and completely local AI coding assistant. By routing the Claude Code CLI through a local Ollama server, you can leverage agentic coding capabilities without API costs or data privacy concerns.⚠️ Important: This setup keeps your codebase entirely off public networks. No data is sent to Anthropic or third-party servers.PrerequisitesOS: Windows 10/11 (x64)Shell: PowerShell 7 (Run as Administrator)Runtime: Node.js (v18 or newer)Storage: ~20GB free space (for model weights)Step 1: Install Ollama & Load ModelFirst, install the Ollama engine to serve the AI model locally.PowerShell# 1. Install Ollama via Windows Package Manager
winget install --id=Ollama.Ollama -e

# 2. Pull and serve the model
# Note: 'gpt-oss:20b' requires ~14GB VRAM/RAM. 
# For lower-spec machines, substitute with 'llama3' or 'mistral'.
ollama run gpt-oss:20b
🛡️ Privacy ConfigurationTo ensure maximum security:Network Isolation: Ensure Ollama is bound to localhost (default).Offline Mode: You may enable Airplane Mode; this setup requires no active internet connection once the model is downloaded.Step 2: Install & Configure Claude CodeNext, install the Claude CLI and configure the environment variables to "trick" it into communicating with your local machine instead of the cloud.1. Install the CLIPowerShell# Execute the official install script
irm https://claude.ai/install.ps1 | iex
2. Configure Local RoutingSet the base URL to your local Ollama port and provide a dummy token to satisfy the authentication requirement.PowerShell# Route traffic to local Ollama instance
$env:ANTHROPIC_BASE_URL = "http://localhost:11434/v1"

# Set mock API token (value does not matter, but must be present)
$env:ANTHROPIC_AUTH_TOKEN = "ollama"

# (Optional) Make these persistent across sessions
[System.Environment]::SetEnvironmentVariable('ANTHROPIC_BASE_URL', 'http://localhost:11434/v1', 'User')
[System.Environment]::SetEnvironmentVariable('ANTHROPIC_AUTH_TOKEN', 'ollama', 'User')
Step 3: Launch the AgentRun the tool using npx to ensure you are using the installed package, specifying your local model.PowerShell# Launch Claude Code
# Ensure the model name matches what you pulled in Step 1
npx @anthropic-ai/claude-code --model gpt-oss:20b
TroubleshootingIssueSolutionModel not foundRun ollama list to verify the exact model name tag.Connection RefusedEnsure Ollama is running (Get-Process ollama).Slow PerformanceThe 20b model is heavy. Try a smaller model: ollama run llama3.
