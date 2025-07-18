# Apex-CLI

**Apex-CLI** is a Node.js-based command-line tool that helps manage configuration settings for application development and deployment workflows, especially for containerized environments.

The Readme.md is valid for up to v2.0 and RunID 47

---

## 🚀 Requirements

- **Node.js v16 or higher**  
  Check your Node.js version:
  ```bash
  node -v
  ```

---

## 📦 Installation

### Option 1: Install directly from release

```bash
npm install -g https://github.com/CLML-APEX/apex-cli/releases/download/main-Latest/apex-cli.tar.gz
```

### Option 2: Manual download

1. Download the latest `.tar.gz` release from [GitHub Releases](https://github.com/CLML-APEX/apex-cli/releases).
2. Install it manually:
   ```bash
   npm install -g ./apex-cli.tar.gz
   ```

> ℹ️ **Note:** Installed CLI scripts are placed in your global Node.js root. You can find the location by running:
> ```bash
> npm -g root
> ```

---

## ✅ Verify Installation

After installation, run: 
```bash
apex
```

This will list all available CLI commands.

---

## 🛠️ Available Commands

| Command         | Description                                                   |
|------------------|---------------------------------------------------------------|
| `apex`       | Show all available commands                                   |
| `apex clean`      | Clean up all compiled code                                   |
| `apex environ`            | Manage and switch project scopes                             |
| `apex config`     | Update configuration parameters                              |
| `apex prepare`    | Generate all necessary configuration and setting files       |
| `apex docker`     | Generate Dockerfile and `docker-compose.yml` for the project |

---

## 🔁 Workflow Guide

### 1. Create a Project Scope

Define a new environment scope to manage the root of your solution:

```bash
apex -folder <project_root_path> <project_name>
```

> ⚠️ This will **overwrite existing configuration** without confirmation.

---

### 2. List Existing Scopes

You can view existing scopes with:
```bash
apex environ -list
# Or for a specific project:
apex environ -list <project_name>
# or to view the current project
apex config
```

---
### 3. Switch Active Scope

Change the current project:
```bash
apex environ <new project name>
```

---

### 4. Configure Project Parameters

Modify specific scope settings using:

```bash
apex config <property> <value>
```

Available configuration keys:

- `mount`: Folder mappings to be mounted into containers
- `container`: Path where Dockerfiles are generated
- `network`: Docker network name
- `env`: Source file for environment variables (default is `.env` in project root)

---

### 5. Prepare Configuration Files

Generate APEX configuration files:

```bash
apex prepare  [-pwd <password>]
```

This creates:

- Settings JSON files
- Angular environment UI config
- Migration settings

If a password is provided, a PFX SSL certificate will also be generated for HTTPS use.

#### Example Docker build

The following is auto-configured if you use `apex docker` to generate Docker:

```json
{
  "CSharp": {
    "Volume": {
      "./mount/settings": "/app/deployment",
      "./mount/Logs": "/app/Logs"
    }
  },
  "Client": {
    "Volume": {
      "./mount/webapp/scripts": "/usr/share/nginx/html/assets/env",
      "./mount/webapp/ssl": "/etc/nginx/ssl"
    }
  }
}
```

---

### 6. Generate Docker Files

Generate Dockerfiles and `docker-compose.yml` for your services:

```bash
apex docker -- [-sys] [-util] [-image (image location)]
```

- Automatically detects and generates Dockerfiles and docker compose files for C# web apps.
- Prepares the `docker-compose` configuration with mounts and environment settings.

You can then start your application with:

```bash
docker compose -f <Mount folder> up -d
```



---

## 📝 Implementation Notes

- Container builds now support:
  1. Optional `appsettings` from `app/development`
  2. Angular UI Docker env integration using generated settings
  3. Database migration settings
  4. OIDC app now supports insert **and** update
  5. Protocol forwarding support for auth services
  6. JWT certificate issuer validation

---

## 📫 Feedback & Contributions

For issues or feature requests, please open a [GitHub issue](https://github.com/CLML-APEX/apex-cli/issues).
