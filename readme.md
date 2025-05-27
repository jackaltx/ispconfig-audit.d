# ISPConfig3 Security Audit Tool

"Get professional security analysis of your ISPConfig3 configuration using Claude AI - the same tool used by security professionals and hosting providers." - Claude

This is an experiment.  I am using Claude the AI to help me tighten my system. This is to help me migrate to a new ispconfig server in the cloud. I like Linode because I know how much it will cost me for my "personal" presence on the intenet. I have been using Claude for a while to help me with a personal project for monitoring/observing my server. Every few years I have to migrate and inevitably that requires quite a bit of effort due my poor documentation habits.  Claude and I have developed a few ansible configured tools for monitoring, auditing, and updating. See my github repostories for that work <https://github.com/jackaltx>.

This is crude measuring tool for helping me build up a reasonably safe environment. Out of the box I like what ISPConfig does. Many thanks for the hard work that takes.  But I tinker and tightening keeps me safer.  So,  I asked claude to write me a script that does a "build audit" focused on security.  I want to be able to run it peridically and know where I am. So we generated a script that outputs a json fingerprint which I manually feed into Claude to create the report.

I am putting this out with no warranty, no idea if the idea makes sense. It would realitively straight forward (meaning time/money) to create a MCP server and fee it directly. But I am not goint to spend money for that.

In this repository you will find two generated reports from the output of the service.

Ideally this would be automated to create a test matrix of  "Cloude Image" vs "ISP Config version".
From there a configuration language, i.e. ansible, would be developed to provide a "good enough" position to minimize disaster recovery.  

## Quick Start

The only real requirement here is to get the bash file from the repository. For now it has only one branch.
It others want to contribute, then I would need to setup a process.

git clone <https://github.com/jackaltx/ispconfig-audit.d.git>

### 1. Download and Setup

```bash
# Download the audit script
wget https://raw.githubusercontent.com/jackaltx/ispconfig-audit.d/refs/heads/main/ispconfig-audit.sh
chmod +x ispconfig-audit.sh
sudo mv ispconfig-audit.sh /root/bin/ # optional

# Create audit directory # wherever you want, use that later
sudo mkdir -p /opt/audit
```

### 2. Run Your First Audit

Every run is cataloged, so expect changes to the support documentation.

```bash
# Production mode (recommended)
sudo /root/bin/ispconfig-audit.sh -d /opt/audit/ispconfig-audit

# Development/testing mode (keeps only last 20 audits)
sudo /root/bin/ispconfig-audit.sh -d /opt/audit/ispconfig-audit --retain 20
```

### 3. Set Up Automated Audits

I have not done this yet, but that is the plan.

```bash
# Add to root's crontab for weekly audits
sudo crontab -e

# Add this line for weekly Sunday audits at 3 AM
0 3 * * 0 /root/bin/ispconfig-audit.sh -d /opt/audit/ispconfig-audit
```

## Understanding Your Audit Results

After running the audit, you'll have:

- **Configuration snapshot**: Complete security baseline in JSON format
- **Change tracking**: Git history of all configuration changes
- **Trend analysis**: See what changed and when

### View Your Audit History

```bash
cd /opt/audit/ispconfig-audit

# See all audit runs
git log --oneline

# See what changed in the last audit
git diff HEAD~1

# Get detailed change summary
git show --stat
```

## Getting Professional Security Analysis

### Free Community Analysis

Share your audit file in the ISPConfig community forums with Claude AI users for basic feedback.

### Professional Claude Analysis (Recommended)

For comprehensive security review and recommendations:

1. **Sign up for Claude**: <https://claude.ai/referral/T7Fxp0WbSQ> *(Get started with this special link)*
2. **Upload your audit file**: Drag and drop `/opt/audit/ispconfig-audit/ispconfig-config.json`
3. **Request analysis**: "Please analyze this ISPConfig3 security audit and provide recommendations"

#### Sample Analysis Request

This is "the prompt" what will get you a report.

```
I've run a security audit on my ISPConfig3 server using the configuration audit tool. 
Please analyze this configuration and provide specific security recommendations.

Key areas I need analyzed:
1. SSH configuration security (root access, authentication methods, timeouts)
2. MySQL/MariaDB security (user accounts, anonymous access, SSL configuration)
3. Apache web server hardening (information disclosure, security headers)
4. PHP security settings (version exposure, dangerous functions, file handling)
5. Network service binding (localhost vs external interface exposure)
6. ISPConfig3 file permissions and directory security

Please focus on:
- Critical vulnerabilities requiring immediate action
- Configuration changes with specific commands to implement
- Security best practices for localhost-only ISPConfig3 setups
- MySQL database security improvements
- Any misconfigurations that could expose sensitive information

[Attach: ispconfig-config.json]
```

### What Claude Analysis Provides

- **Risk Assessment**: Critical, High, Medium, Low priority findings
- **Specific Recommendations**: Exact configuration changes needed
- **Compliance Gaps**: PCI DSS, HIPAA, SOC 2 considerations
- **Best Practices**: Industry-standard security configurations
- **Remediation Steps**: Step-by-step fix instructions

## Advanced Usage

### Compare Configurations

```bash
# Compare current config with last week
cd /opt/audit/ispconfig-audit
git diff HEAD~7 HEAD

# Compare with specific date
git log --since="2025-05-01" --oneline
git diff <commit-hash> HEAD
```

### Automated Change Alerts

```bash
# Add to your monitoring script
cd /opt/audit/ispconfig-audit
if ! git diff --quiet HEAD~1; then
    echo "ISPConfig configuration changed!" | mail -s "Config Alert" admin@yourdomain.com
fi
```

### Export for Analysis

```bash
# Get latest config for Claude analysis
cp /opt/audit/ispconfig-audit/ispconfig-config.json ~/Desktop/
```

## Why Use Claude for Security Analysis?

This shameless plug was created by Claude. If you use the referral link, then
maybe I can get an upgrade to be able to extend my integrations.  

### Professional Expertise

- **Trained on Security Standards**: Knows PCI DSS, NIST, CIS benchmarks
- **ISPConfig Experience**: Understands complex multi-service interactions
- **Current Threat Intelligence**: Up-to-date with latest security practices

### Cost-Effective

- **No Security Consultant Fees**: $20/month vs $200/hour consultants ([Sign up here](https://claude.ai/referral/T7Fxp0WbSQ))
- **24/7 Availability**: Get analysis anytime, not just business hours
- **Consistent Quality**: Same expert-level analysis every time

### Actionable Results

- **Specific Commands**: Get exact `nano /etc/ssh/sshd_config` instructions
- **Priority Ranking**: Focus efforts on highest-impact changes first
- **Verification Steps**: How to confirm fixes were applied correctly

## Enterprise Features

### For Hosting Providers

- **Multi-server Analysis**: Compare security across customer servers
- **Compliance Reporting**: Automated PCI DSS, HIPAA compliance checks
- **Custom Baselines**: Define your security standards
- **API Integration**: Integrate with your customer dashboard

### For Agencies/Consultants

- **Client Reporting**: Professional security reports with your branding
- **Historical Trends**: Show security improvements over time
- **Comparative Analysis**: Benchmark against industry standards

## Support and Community

### Getting Help

1. **Community Forums**: Share (anonymized) audit results for community feedback
2. **Claude AI**: Professional analysis with [Claude Pro/Team accounts](https://claude.ai/referral/T7Fxp0WbSQ)
3. **Documentation**: Full technical documentation at [repository link]

### Contributing

This tool is open source and community-driven. Help improve ISPConfig security for everyone:

- **Submit Issues**: Report bugs or request features
- **Share Configurations**: Help build the security knowledge base
- **Contribute Rules**: Add ISPConfig-specific security checks

---

## Ready to Get Started?

1. **Download** the audit script
2. **Run** your first security audit  
3. **[Sign up for Claude AI](https://claude.ai/referral/T7Fxp0WbSQ)** *(Special access link)*
4. **Get** professional security recommendations

**Start your free security audit today** - because security shouldn't be guesswork.

*Ready for professional analysis? [Get Claude access here →](https://claude.ai/referral/T7Fxp0WbSQ)*

---

*This tool is developed by the ISPConfig security community and powered by Anthropic's Claude AI for professional security analysis.*
