1. The Blueprint: Directory Structure
A marketplace is essentially a Git repository that houses one or more plugins. Each plugin can contain multiple skills.

Set up your repository with this structural layout:

Plaintext
your-marketplace-repo/
├── .claude-plugin/          # Or .github/ depending on targeted ecosystem
│   └── marketplace.json     # The main catalog listing all plugins
└── plugins/
    └── your-custom-plugin/
        ├── .claude-plugin/
        │   └── plugin.json  # Manifest describing this specific plugin
        └── skills/
            └── your-skill/
                └── SKILL.md # The core markdown prompt and instructions


2. Defining the Capability: SKILL.md
The SKILL.md file contains frontmatter metadata followed by clear instructions written in Markdown. The AI reads this file on-demand when a task matches the skill's description.  

Create your file at plugins/your-custom-plugin/skills/your-skill/SKILL.md

Markdown
---
name: schema-validator
description: Validates database migration files against internal enterprise schemas.
tools: [fs_read, fs_write]
---

Review the chosen database migration scripts for naming conventions, column restrictions, and potential locking risks. 

Ensure that:
- Tables use lower_snake_case.
- Indexes are added concurrently where appropriate.
- Column drop operations contain a fallback script comment.

Be concise and output an actionable checklist.


3. Registering the Plugin and MarketplaceTo bundle your skills so an agent can parse them, you need to create the manifest and the catalog files.

## Step A: The Plugin Manifest
Create .claude-plugin/plugin.json inside your specific plugin directory to version-control the package:  

JSON
{
  "name": "enterprise-dev-tools",
  "description": "Custom engineering guardrails and automation utilities",
  "version": "1.0.0"
}


## Step B: The Marketplace Catalog
Create marketplace.json in the root of your repository so the agent knows what packages are available to be installed:  JSON{
  "name": "solomon-public-skills",
  "owner": {
    "name": "Solomon Rajkumar"
  },
  "plugins": [
    {
      "name": "enterprise-dev-tools",
      "source": "./plugins/enterprise-dev-tools",
      "description": "Custom engineering guardrails and automation utilities",
      "category": "Development"
    }
  ]
}


## 4. Distributing to the World

Once your local structure is verified, publish it via standard git distribution workflows:

1.Push to a Git Provider:
GitHub or GitLab.
Commit your files and push the repository to a public GitHub repo 
(e.g., [github.com/your-username/my-skills-marketplace](https://github.com/your-username/my-skills-marketplace)).


2.Register the Marketplace:
User action.
Users can now register your entire marketplace directly into their local development CLI tools or IDE plugins using your repository URL:

Bash
/plugin marketplace add https://github.com/your-username/my-skills-marketplace


## Install Individual Plugins
Execution
Once the catalog is linked, users pull down the specific capabilities they need:

Bash
/plugin install enterprise-dev-tools@solomon-public-skills


Design Tip: Agents use On-Demand Loading. They scan the description fields inside your marketplace catalogs to choose when to trigger a skill. Write highly explicit descriptions so the agent knows exactly when your skill is appropriate for a given task.# my-skills-marketplace
# my-skills-marketplace
# my-skills-marketplace
# my-skills-marketplace
