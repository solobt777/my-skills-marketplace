# Security Audit Plugin Installation & Usage Guide

You can install and use the `security-audit` plugin at either a **project level** (specific to your current workspace) or a **user level** (available globally across your machine). 


---

## 1. Installation

Follow these steps to add the marketplace and install the plugin.

### Step 1: Add the Marketplace
Open your project terminal or workspace and run the following command to register the plugin marketplace:

```bash
claude plugin marketplace add https://github.com/solobt777/my-skills-marketplace
```

### Step 2: Install the Plugin
Choose whether you want to install it for your current project only, or globally for your user profile:

**For Project Level:**
```bash
claude plugin install security-audit@security-audit-skills --scope project
```

**For User Level:**
```bash
claude plugin install security-audit@security-audit-skills --scope user
```

---

## 2. Verification

To verify that the plugin has been successfully installed and enabled, run your validation or plugin list command. It should show details matching your configuration:

```text
security-audit@security-audit-skills
Version: 1.0.0
Scope: project
Status: ✔ enabled
```

---

## 3. How to Use

The plugin is language and framework-agnostic—it works with **any tech stack**. You can run it inside an active project or an entirely empty directory.

* **In an Existing Project:** Trigger the plugin within your workspace directory. It will scan your local files and analyze your code.
* **In an Empty Folder:** If you run the plugin inside an empty directory, it will automatically prompt you for your remote Git repository URL. It will clone the repository down, run the analysis, and perform the security audit.

### Output
Once the audit finishes, the plugin automatically creates a new folder structure inside your project root containing your results:
* `audit/security-report.md`

---

## 4. Managing Scopes & Removal

### Changing Scopes
If you want to shift the plugin from a project-specific scope to a global user scope (or vice-versa), disable it in one scope and enable it in the other:

```bash
claude plugin disable security-audit@security-audit-skills --scope project
claude plugin enable security-audit@security-audit-skills --scope user
```

### Uninstalling the Plugin
To completely remove the plugin from your environment, run:

```bash
claude plugin uninstall security-audit@security-audit-skills
```
 