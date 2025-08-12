# Apex-CLI

**Apex-CLI** is a Node.js-based command-line tool that helps manage configuration settings for application development and deployment workflows, especially for containerized environments.

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
|env|manage compilation project meta information |
|config|configuration of how to generate the settings for microservices|
|ssl|generate SSL certificate for project|
|docker|register containers required for the services|
|build|build all settings and image files for the services|
|sys|current APEX CLI information|
|compile|compile specific project to docker images, developer need to restart the service manually|

---

## 🔁 Workflow Guide

### 1. Create a Project Scope

Define a new environment scope to manage the root of your solution:

```bash
apex env -f [folder contains all resources for service] [project name]
```

---

### 2. List Existing Scopes

You can view existing scopes with:
```bash
apex env
```

---

### 3. Switch Active Scope

Change the current working scope:
```bash
apex env [project name]
```

---

### 4. Configure Project Parameters

Modify specific scope settings using:

```bash
apex config <property> <value>
```

Available configuration keys:

- `--imageNameCallback`, `-img` : generation of image name based on project name
- `--system_network`,`-sys`: Main dependencies for system container
- `--dockerFile`,`-d`: location of all the dockerfile to be generated
- `--mount`,`-m`: location of all files required for the deployment
- `--sslCert`,`-c`: location for all SSL cert to be generated
- `--imageNamespace`,`-ns`: namespace of the images

---

### 5. Set up docker file dependencies

```bash
apex docker -op [add|rm|list] -img [image name] (...image meta)
```
Developer can use the following function to import and export the container list

Developer can use following to import and export the settings
```bash
# Export current configuration
apex docker -op export > [filename]

# Import configuration
apex docker -op import [filename | URL]

# example to load default apex container setting
apex docker -op import "https://raw.githubusercontent.com/CLML-APEX/apex-cli/refs/heads/main/default_apex_containers.json"

#to verify. run 
apex docker -op list
```

### 6. Generation for all resources for deployment

Generate APEX configuration files:

```bash
apex build [-c] [-d] [-opt] [-u] [-b [image directory]] [-arg]
```
- `-c`: generate docker compose file for all CLML services
- `-d`: generate docker compose file for all 3-rd party services
- `-opt`: generate docker compose file for developer services
- `-u`: update appsettings.json with the configured configurations
- `-img`: back up images to tar files

The setting files will be created based on appsettings.template.json for each project and .env file located at the root of the project

This creates:
- Settings JSON files
- Angular environment UI config
- Migration settings

If a password is provided, a PFX SSL certificate will also be generated for HTTPS use.

#### Example Mount Configuration

The following files will be treated as inputs of the generation


> * ./**/`CSharp project root folder`/appsettings.template.json  
> Configuration that are going to override ./appsettings.json
> * ./.env  
> Environments file that will be used to generate the configuration files



---

### 6. Generate Docker Files

Upon built, docker compose files and container run scripts will be generated.

```bash
docker compose -f docker compose -f docker-compose-system.yaml up -d
docker compose -f docker compose -f docker-compose-service.yaml up -d

# To load optional utility like podtainer, pgadmin and mongo GUI
docker compose -f docker compose -f docker-compose-optional.yaml up -d
```
To run migration project (console app)
```
apex-dbmigrator.cmd
```
For rancher desktop, developer may need to remove -rm tag from the scripts

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
