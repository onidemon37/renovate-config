# 🤖 Renovate Configuration Presets

A collection of reusable [Renovate](https://docs.renovatebot.com/) configuration presets for automated dependency management across multiple repositories.

## 📋 Table of Contents

- [Overview](#overview)
- [Available Presets](#available-presets)
- [Usage](#usage)
- [Configuration Details](#configuration-details)
- [Examples](#examples)
- [Contributing](#contributing)

## 🎯 Overview

This repository provides standardized Renovate configurations that can be shared across multiple projects. It includes sensible defaults, security-focused settings, and specialized configurations for different project types.

### Key Benefits

- **Consistency**: Same dependency management behavior across all projects
- **Security**: Prioritizes security updates and enables vulnerability scanning
- **Automation**: Reduces manual maintenance overhead
- **Flexibility**: Modular presets that can be mixed and matched
- **Best Practices**: Incorporates Renovate community recommendations

## 📦 Available Presets

### `default.json` - Base Configuration

The main preset that includes comprehensive dependency management for most projects.

**Features:**
- 🔄 Weekly updates (Saturdays)
- 🚨 Security update prioritization
- 📊 Dependency dashboard
- ✍️ Semantic commit messages
- 🔐 GPG signed commits
- 🏷️ GitHub Actions digest pinning
- 🐳 Docker image management
- 📋 Pre-commit hook integration

**Supported Technologies:**
- Docker containers and base images
- GitHub Actions workflows
- Kubernetes manifests (YAML)
- Ansible playbooks and templates
- Flux CD configurations
- Helm charts and values
- Devbox development environments
- Node.js dependencies (future)
- Python dependencies (future)

## 🚀 Usage

### Basic Usage

Add a `renovate.json` file to your repository root:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": [
    "local>onidemon37/renovate-config"
  ]
}
```

### Project-Specific Customization

Override or add project-specific rules:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json", 
  "extends": [
    "local>onidemon37/renovate-config"
  ],
  "packageRules": [
    {
      "matchPackageNames": ["nginx", "redis"],
      "commitMessageSuffix": " for web services"
    }
  ]
}
```

## ⚙️ Configuration Details

### Scheduling

- **Default**: Every Saturday
- **Security updates**: Immediate (any time)
- **Rate limiting**: Disabled for faster processing

### Commit Messages

- **Format**: Semantic conventional commits
- **Signature**: Signed-off-by author
- **Examples**:
  - `feat: update docker base image nginx to v1.25.3`
  - `fix: update devDependencies (non-major)`
  - `ci: update GitHub Actions to latest versions`

### Grouping Strategy

- **Docker images**: Grouped by type (base images, application images)
- **Dev dependencies**: Grouped for cleaner PR management
- **Minor/patch updates**: Bundled together
- **GitHub Actions**: Separate group for CI/CD updates

### Auto-merge Policies

- **Digest updates**: Auto-merge (security patches)
- **Branch updates**: Auto-merge non-breaking changes
- **Major updates**: Manual review required

## 📚 Examples

### Docker Projects

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["local>onidemon37/renovate-config"],
  "packageRules": [
    {
      "matchPackageNames": ["alpine", "ubuntu", "debian"],
      "commitMessageSuffix": " for container builds"
    },
    {
      "matchPackageNames": ["nginx"],
      "matchUpdateTypes": ["major"],
      "prPriority": 15,
      "reviewers": ["team-leads"]
    }
  ]
}
```

### Kubernetes Projects

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["local>onidemon37/renovate-config"],
  "kubernetes": {
    "fileMatch": [
      "k8s/**/*.yaml",
      "kubernetes/**/*.yml",
      "manifests/**/*.yaml"
    ]
  },
  "packageRules": [
    {
      "matchManagers": ["kubernetes"],
      "groupName": "Kubernetes manifests",
      "commitMessageTopic": "K8s {{depName}}"
    }
  ]
}
```

### Development Environment Projects

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["local>onidemon37/renovate-config"],
  "packageRules": [
    {
      "matchFiles": ["devbox.json", ".devcontainer/**"],
      "groupName": "Development environment",
      "prPriority": 5
    }
  ]
}
```

## 🔧 Customization Options

### Common Overrides

**Change schedule:**

```json
{
  "schedule": ["every weekday"]
}
```

**Disable auto-merge:**

```json
{
  "packageRules": [
    {
      "matchUpdateTypes": ["major", "minor", "patch"],
      "automerge": false
    }
  ]
}
```

**Custom reviewers:**

```json
{
  "reviewers": ["@username", "team:team-name"]
}
```

**Platform-specific settings:**

```json
{
  "platform": "github",
  "includeForks": true,
  "forkProcessing": "enabled"
}
```

## 📋 Supported File Types

| Technology | File Patterns | Manager |
|------------|---------------|---------|
| **Docker** | `Dockerfile`, `*.dockerfile` | dockerfile, regex |
| **GitHub Actions** | `.github/workflows/*.yml` | github-actions |
| **Kubernetes** | `*.yaml`, `*.yml` | kubernetes |
| **Ansible** | `*.yml`, `*.yml.j2` | regex |
| **Devbox** | `devbox.json`, `devbox.lock` | npm (planned) |
| **Helm** | `values.yaml`, `Chart.yaml` | helm-values |
| **Flux** | `*.yaml` (in flux dirs) | flux |

## 🛡️ Security Features

- **Vulnerability alerts**: Enabled for all dependencies
- **Security updates**: Highest priority, immediate processing  
- **Digest pinning**: GitHub Actions use SHA digests for security
- **Branch protection**: Compatible with required status checks
- **Signed commits**: All updates include GPG signatures

## 🤝 Contributing

### Adding New Presets

1. Create a new `.json` file for your preset
2. Follow the existing naming conventions
3. Document the preset in this README
4. Test with a sample project

### Modifying Existing Presets

1. Ensure changes are backward compatible
2. Update documentation accordingly
3. Test changes across multiple project types
4. Consider impact on existing projects

### Testing Changes

```bash
# Test configuration locally
npx renovate --dry-run --log-level=debug

# Validate JSON syntax
jq . renovate.json

# Test with specific repository
npx renovate --dry-run onidemon37/test-repo
```

## 📈 Monitoring and Analytics

### Dependency Dashboard

Each repository gets an automated dashboard showing:
- 📊 Open dependency updates
- 🚨 Security vulnerabilities  
- ⏱️ Update frequency statistics
- 📋 Configuration status

### Notifications

- **Slack/Teams**: Integration available for team notifications
- **Email**: Configurable for critical updates
- **GitHub**: PR comments and reviews

## 🔗 Related Resources

- [Renovate Documentation](https://docs.renovatebot.com/)
- [Renovate Configuration Options](https://docs.renovatebot.com/configuration-options/)
- [Preset Configurations](https://docs.renovatebot.com/config-presets/)
- [Best Practices Guide](https://docs.renovatebot.com/best-practices/)

## 📄 License

MIT License - feel free to use and modify for your projects.

---

**Maintained by**: [@onidemon37](https://github.com/onidemon37)  
**Last Updated**: November 2024  
**Renovate Version**: Compatible with v37.x and later