# Using AI to Audit ISPConfig3 Security: A Practical Guide

Security auditing is one of those tasks we all know we should do more often, but somehow never find time for. Manually checking SSH configs, MySQL settings, Apache security, and PHP configurations gets tedious fast—especially when you're managing multiple servers on a tight budget.

I've been running ISPConfig for years, and like many of us, I learn through tinkering. But tinkering can introduce security gaps, and I wanted a systematic way to catch configuration drift before it becomes a problem.

This tool started as a personal experiment: create a structured snapshot of server configurations, then use AI to analyze what I might have missed. The free tier of Claude AI works fine for basic analysis, and for $20/month you get detailed security reports plus the ability to analyze multiple servers or track changes over time.

The goal isn't to replace good sysadmin practices—it's to catch the stuff we inevitably miss when we're focused on getting things working rather than getting them perfectly secure.

## What This Tool Actually Does

The ISPConfig3 Security Audit Tool creates a complete security baseline of your server configuration in structured JSON format. Rather than asking an AI to "look at my server" (which provides vague results), this approach captures specific configuration details that enable precise security analysis.

Here's what happens in practice:

1. **Configuration Snapshot**: The bash script analyzes Apache, MySQL/MariaDB, PHP, SSH, Postfix, and ISPConfig3 configurations
2. **Change Tracking**: Git automatically tracks every configuration change over time
3. **AI Analysis**: Upload the structured JSON to Claude AI for professional security review
4. **Actionable Results**: Get specific commands to fix identified issues

The difference is remarkable. Instead of generic advice like "harden your SSH," you get specific findings like:

**Before**: "Your server might have security issues"
**After**: "SSH PermitRootLogin is set to 'yes' (HIGH RISK) - change to 'prohibit-password' in /etc/ssh/sshd_config line 32"

## Quick Implementation Guide

### Step 1: Download and Setup

```bash
# Clone the repository
git clone https://github.com/jackaltx/ispconfig-audit.d.git
cd ispconfig-audit.d
chmod +x ispconfig-audit.sh

# Move to a permanent location
sudo mv ispconfig-audit.sh /root/bin/

# Create audit directory
sudo mkdir -p /opt/audit
```

### Step 2: Run Your First Audit

```bash
# Production mode (keeps complete history)
sudo /root/bin/ispconfig-audit.sh -d /opt/audit/ispconfig-audit

# Development mode (keeps last 20 audits only)
sudo /root/bin/ispconfig-audit.sh -d /opt/audit/ispconfig-audit --retain 20
```

The script runs quickly (typically under 30 seconds) and creates `/opt/audit/ispconfig-audit/ispconfig-config.json` containing your complete security baseline.

### Step 3: Get AI Security Analysis

1. **Sign up for Claude AI**: Visit [claude.ai](https://claude.ai) (free tier available, Pro recommended for detailed analysis)

2. **Upload your audit file**: Drag and drop `ispconfig-config.json` into Claude

3. **Request analysis** using this prompt:

```
I've run a security audit on my ISPConfig3 server. Please analyze this configuration and provide specific security recommendations.

Key areas to analyze:
1. SSH configuration security (root access, authentication methods)
2. MySQL/MariaDB security (user accounts, anonymous access, SSL)
3. Apache web server hardening (information disclosure, headers)
4. PHP security settings (version exposure, dangerous functions)
5. Network service binding (localhost vs external exposure)
6. ISPConfig3 file permissions and directory security

Focus on:
- Critical vulnerabilities requiring immediate action
- Configuration changes with specific commands
- Security best practices for ISPConfig3 setups
- Any misconfigurations exposing sensitive information

[Attach: ispconfig-config.json]
```

### Step 4: Track Changes Over Time

```bash
cd /opt/audit/ispconfig-audit

# View audit history
git log --oneline

# See what changed since last audit
git diff HEAD~1

# Compare with specific timeframe
git diff HEAD~7 HEAD  # Changes from last week
```

## Real Security Results

To demonstrate the tool's effectiveness, here's an actual analysis from a production server. The AI identified both excellent security practices and specific areas needing attention:

### Excellent Security Found

- **SSH Hardening**: Root login restricted to SSH keys only (`PermitRootLogin prohibit-password`)
- **Database Security**: Anonymous users removed, test database deleted, root access localhost-only
- **Mail Security**: DANE (DNS-based Authentication) enabled, comprehensive anti-spam configuration
- **Access Controls**: All tunneling and forwarding disabled in SSH

### Critical Issues Identified

- **Information Disclosure**: Apache ServerTokens not configured, PHP expose_php enabled
- **Missing Configuration**: PHP Apache config file not found in expected location

### Specific Fix Commands Provided

```bash
# Fix Apache information disclosure
echo "ServerTokens Prod" >> /etc/apache2/conf-available/security.conf
echo "ServerSignature Off" >> /etc/apache2/conf-available/security.conf
a2enconf security

# Hide PHP version information
sed -i 's/expose_php = On/expose_php = Off/' /etc/php/8.2/cli/php.ini

# Restart services
systemctl restart apache2
```

The analysis included a **Security Score: 9.2/10** with detailed breakdowns by category and compliance framework alignment (CIS Controls, NIST, OWASP).

## Why This Approach Works

### Structured Data = Better Analysis

Generic questions like "is my server secure?" produce generic answers. Structured configuration data enables AI to provide specific, actionable recommendations. The tool captures exact settings, file paths, and service states that AI can analyze against security best practices.

### Change Detection

The Git-based tracking automatically identifies configuration drift. You'll know immediately when someone modifies SSH settings, adds MySQL users, or changes Apache configurations. This visibility is crucial for maintaining security baselines.

### Accessible Security Analysis

Most of us can't justify hiring security consultants at $150-300/hour for configuration reviews. Claude AI's free tier handles basic analysis, and the $20/month Pro tier gives you detailed reports with specific fix commands. For small ISPs or consultants managing multiple servers, it's a reasonable way to add systematic security review to your workflow.

### Immediate Availability

Security issues don't wait for business hours. Whether you're troubleshooting at 2 AM or need to verify configurations before going live, AI analysis is instantly available.

### Specific, Implementable Results

Rather than general security advice, you get exact commands to run, specific line numbers to change, and verification steps to confirm fixes were applied correctly. This eliminates guesswork and reduces implementation time.

## Beyond Basic Security

This tool represents a first step toward AI-assisted infrastructure management. The structured approach can be extended to:

- **Compliance Reporting**: Automated PCI DSS, HIPAA, SOC 2 compliance checks
- **Multi-Server Analysis**: Compare security across multiple ISPConfig installations
- **Custom Baselines**: Define organization-specific security standards
- **Integration Opportunities**: Feed results into monitoring systems or ticketing platforms

For hosting providers, this approach scales security analysis across customer servers without proportional increases in security staff.

## Getting Started

The ISPConfig3 Security Audit Tool is open source and ready to use:

1. **Clone the repository**: `git clone https://github.com/jackaltx/ispconfig-audit.d.git`
2. **Run your first audit**: Takes less than 5 minutes to get complete security baseline
3. **Get AI analysis**: Upload JSON to Claude for professional security review
4. **Implement improvements**: Apply specific recommendations with provided commands

Security shouldn't be guesswork. With structured analysis and AI expertise, you can maintain professional-grade security practices without the traditional overhead.

The complete documentation, script, and examples are available at: <https://github.com/jackaltx/ispconfig-audit.d>

---

*The ISPConfig3 Security Audit Tool is developed for the ISPConfig community and powered by Anthropic's Claude AI for professional security analysis.*
