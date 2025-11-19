# After Restart Checklist for MCP Testing

## 🔄 You're About to Restart Claude Code

### What to Expect After Restart:

1. **MCP Servers Will Auto-Start**
   - Claude Code will attempt to launch all configured MCP servers
   - You may see loading indicators briefly

2. **Check MCP Status First:**
   ```
   /mcp
   ```

   Expected output:
   - ✅ `filesystem` - Should be running
   - ⚠️ `github` - May show error (needs token)
   - ⚠️ `postgres` - May show error (needs database)
   - ⚠️ `brave-search` - May show error (needs API key)

---

## ✅ Quick Test Commands After Restart

### Test 1: Verify MCP Status
```
/mcp
```

### Test 2: Check if filesystem MCP is working
Ask Claude: "Can you list all .sol files in my directory using MCP?"

### Test 3: Verify configuration loaded
Ask Claude: "What MCP servers are configured?"

---

## 📋 Next Steps After Testing MCP

Once MCP is working, we'll create these contracts:

1. **EscrowSettlement.sol** - Escrow & dispute resolution
2. **CollateralManager.sol** - Collateral tracking & liquidation
3. **ComplianceEngine.sol** - Advanced compliance checks
4. **BondFactory.sol** - Standardized bond deployment

---

## 🐛 If MCP Servers Show Errors (Normal!)

### `github` - Error Expected
**Why:** You haven't added GitHub token yet
**Fix:** Follow steps in `MCP_SETUP_GUIDE.md` section 2

### `postgres` - Error Expected
**Why:** PostgreSQL not installed/running
**Fix:** Optional - only needed for backend development

### `brave-search` - Error Expected
**Why:** No API key configured
**Fix:** Optional - useful for documentation search

### `filesystem` - Should Work!
**Why:** No setup required, uses local filesystem
**If not working:** Check Node.js installation

---

## 🎯 Priority After Restart

**Minimum Requirement:**
- ✅ At least `filesystem` MCP working

**Optimal Setup:**
- ✅ `filesystem` working
- ✅ `github` working (if you plan to push to GitHub)
- ⚠️ `postgres` optional (for backend later)
- ⚠️ `brave-search` optional (nice to have)

---

## 📝 Quick Command Reference

```bash
# After restart, test these:
/mcp                          # Check MCP status
/doctor                       # Full system diagnostic
/help                         # View all commands

# Ask Claude to test:
"Test the filesystem MCP by listing all files"
"What MCP servers are currently active?"
"Read the MCP_SETUP_GUIDE.md file using MCP"
```

---

## 🚀 Ready to Create Contracts?

After confirming MCP is working (at minimum `filesystem`), we'll:

1. Create 4 critical smart contracts
2. Add them to the architecture document
3. Create integration examples
4. Set up testing framework

---

**👉 GO AHEAD AND RESTART CLAUDE CODE NOW**

**After restart, type:** `/mcp` to verify configuration loaded successfully!

