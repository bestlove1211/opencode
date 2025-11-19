# MCP Server Setup Guide for BTTB Development

## Configuration File Location
`C:\Users\tvi\AppData\Roaming\Claude\claude_desktop_config.json`

## Configured MCP Servers

### 1. Filesystem MCP ✅ (Already Active)
**Purpose:** Enhanced file operations for your project
**Status:** No additional setup needed
**Capabilities:**
- Read/write files with advanced permissions
- Directory traversal
- File search and manipulation

---

### 2. GitHub MCP (Requires API Token)
**Purpose:** Manage GitHub repositories, issues, PRs
**Setup Required:**

1. **Generate GitHub Personal Access Token:**
   - Go to: https://github.com/settings/tokens
   - Click "Generate new token (classic)"
   - Select scopes:
     - `repo` (Full control of private repositories)
     - `workflow` (Update GitHub Action workflows)
     - `read:org` (Read org and team membership)
   - Copy the generated token

2. **Update Configuration:**
   - Open: `C:\Users\tvi\AppData\Roaming\Claude\claude_desktop_config.json`
   - Replace `YOUR_GITHUB_TOKEN_HERE` with your actual token
   - Save the file

**Capabilities:**
- Create/manage repositories
- Create/update issues and PRs
- Search code across GitHub
- Manage branches and commits

---

### 3. PostgreSQL MCP (Requires Database)
**Purpose:** Database operations for BTTB backend
**Setup Required:**

1. **Install PostgreSQL:**
   ```bash
   # Download from: https://www.postgresql.org/download/windows/
   # Or use chocolatey:
   choco install postgresql
   ```

2. **Create BTTB Database:**
   ```bash
   # Connect to PostgreSQL
   psql -U postgres

   # Create database
   CREATE DATABASE bttb_dev;

   # Create user (optional)
   CREATE USER bttb_user WITH PASSWORD 'your_password';
   GRANT ALL PRIVILEGES ON DATABASE bttb_dev TO bttb_user;
   ```

3. **Update Connection String (if needed):**
   ```json
   "postgresql://username:password@localhost:5432/bttb_dev"
   ```

**Capabilities:**
- Execute SQL queries
- Database schema management
- Data import/export
- Performance monitoring

---

### 4. Brave Search MCP (Requires API Key)
**Purpose:** Web search for Solidity docs, security best practices
**Setup Required:**

1. **Get Brave Search API Key:**
   - Go to: https://brave.com/search/api/
   - Sign up for free tier (2,000 queries/month)
   - Get your API key

2. **Update Configuration:**
   - Replace `YOUR_BRAVE_API_KEY_HERE` with your actual key

**Capabilities:**
- Search Solidity documentation
- Find security audit reports
- Research DeFi protocols
- Check latest blockchain news

---

## How to Apply Configuration

### Option 1: Restart Claude Desktop (Recommended)
1. Close Claude Code completely
2. Reopen Claude Code
3. MCP servers will auto-start

### Option 2: Check MCP Status
Run in Claude Code:
```
/mcp
```

---

## Verifying MCP Servers

After restart, you should see these servers:
- ✅ `filesystem` - Always available
- ⏳ `github` - If you added token
- ⏳ `postgres` - If database is running
- ⏳ `brave-search` - If you added API key

---

## Optional: Additional MCP Servers for Blockchain Dev

### Etherscan MCP (Coming Soon)
```json
"etherscan": {
  "command": "npx",
  "args": ["-y", "@modelcontextprotocol/server-etherscan"],
  "env": {
    "ETHERSCAN_API_KEY": "YOUR_KEY"
  }
}
```

### Alchemy/Infura RPC MCP (Custom)
For blockchain RPC calls directly from Claude

---

## Troubleshooting

### MCP Server Not Starting?

1. **Check Node.js:**
   ```bash
   node --version  # Should be v18+ or v20+
   npm --version
   ```

2. **Test MCP Package:**
   ```bash
   npx -y @modelcontextprotocol/server-filesystem --help
   ```

3. **Check Logs:**
   - Windows: `%APPDATA%\Claude\logs\`
   - Look for MCP errors

### GitHub Token Issues?
- Ensure token has correct scopes
- Token should not have expired
- Check for special characters in token

### PostgreSQL Connection Failed?
- Verify PostgreSQL is running:
  ```bash
  pg_isready
  ```
- Check connection string format
- Verify database exists: `psql -l`

---

## Next Steps

1. **Restart Claude Code** to activate MCP servers
2. **Verify with:** `/mcp` command
3. **Optional:** Add GitHub token and Brave API key
4. **Optional:** Set up PostgreSQL for backend development

---

## Quick Setup Commands

```bash
# Install PostgreSQL (Windows)
choco install postgresql

# Create database
psql -U postgres -c "CREATE DATABASE bttb_dev;"

# Test connection
psql -U postgres -d bttb_dev -c "SELECT version();"
```

---

## Support

- MCP Documentation: https://modelcontextprotocol.io/
- Claude Code Docs: https://docs.claude.com/en/docs/claude-code/mcp
- Report Issues: https://github.com/anthropics/claude-code/issues

