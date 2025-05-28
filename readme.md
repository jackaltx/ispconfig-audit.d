# ISPConfig3 Security Audit Tool

"Get professional security analysis of your ISPConfig3 configuration using Claude AI - the same tool used by security professionals and hosting providers." - Claude

This is an experiment.  I am using Claude the AI to help me tighten my system. This is to help me migrate to a new ISPconfig server in the cloud.

I use Linode for that purpose.  Linode makes it easy to know how much it will cost me for my "personal" presence on the internet. However, I am not going to pay for a "baseline image", so I use their images. It is up to me to secure it.  This "process" is how I hope to do a better job.

This is a simple, crude measuring tool for helping me build up a reasonably safe environment using their images as I migrate to a newer version of the OS.

ISPConfig provides a solid baseline for a self-hosted service with web, email, and database support. I appreciate the way it integrates these
open source product into a system.

I tinker and tightening keeps me safer from those tinkerings.  So, I asked "Claude the AI" to write me a script that does a "build audit" focused on security.  The goal is to run it periodically and know where I am in the process.  So we developed a fast-running script that outputs a JSON fingerprint which I manually feed into Claude to create a status report.

I am putting this out with no warranty and do not know if this idea will be useful to anyone else.  The idea of having an AI in the loop to help tighten security will inevitably happen if it has not already.  

This is the first step in a larger process.  This will be used to develop a set of Ansible scripts to harden a baseline system.  Then use security assessment tools to evaluate and refine.  In the end, I hope to have a good enough set of assessment tools.

It is relatively straightforward (meaning time/money) to create an MCP server to feed the AI directly. But I am not going to spend money on that. So this process has a manual step. Ideally, this could be automated to create a test matrix of  "Cloud Image version" vs "ISPConfig version". But that costs money.  

## Quick Start

The only real requirement here is to get the bash file from the repository. For now it has only one branch.
It others want to contribute, then I would need to setup a process.

git clone <https://github.com/jackaltx/ispconfig-audit.d.git>

### 1. Download and Setup

Note:  this needs to be run as root and the output will be owned by root.

```bash
# Download the audit script
git clone <https://github.com/jackaltx/ispconfig-audit.d.git>
cd ispconfig.audit.d
chmod +x ispconfig-audit.sh
sudo mv ispconfig-audit.sh /root/bin/ # optional

# Create audit output directory
sudo mkdir -p /opt/audit
```

### 2. Run Your First Audit

Every run is cataloged, so expect changes to the support documentation on each run. The important file is the json output.

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

This next section is all written by Claude.  

### Professional Claude Analysis (Recommended)

For comprehensive security review and recommendations:

1. **Sign up for Claude**: <https://claude.ai/referral/T7Fxp0WbSQ> *(Get started with this special link)*
2. **Upload your audit file**: Drag and drop `/opt/audit/ispconfig-audit/ispconfig-config.json`
3. **Request analysis**: "Use the prompt in the next section"

### Claude Prompt to perform analysis

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

*This tool is developed by Jackaltx and powered by Anthropic's Claude AI for professional security analysis.*
