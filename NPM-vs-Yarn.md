# NPM vs Yarn

Both **NPM (Node Package Manager)** and **Yarn** are package managers used for managing dependencies in JavaScript projects, primarily those built on Node.js. While they serve the same purpose, they have differences in performance, features, and approach.

## Installation

- **NPM**: Comes pre-installed with Node.js, so no extra installation is required.  
  Example:  
  ```bash
  npm -v
  node -v
  ```

- **Yarn**: Requires a separate installation via NPM or directly through the Yarn website.  
  Example:  
  ```bash
  npm install -g yarn
  yarn --version
  ```

## Speed

### NPM
- NPM v5+ introduced a lock file and some optimizations, but traditionally it was slower due to less aggressive caching and network requests.
- With parallelization and improvements in NPM v7+, the speed is much better but still slightly behind Yarn in some cases.

### Yarn
- Yarn is known for being faster, especially with its deterministic dependency resolution, aggressive caching, and parallelized operations.
- Yarn can install packages faster, especially on repeated installs due to better offline cache handling.

## Lock File

- **NPM**: Uses `package-lock.json` to lock dependencies, ensuring the same versions are installed across different environments.

- **Yarn**: Uses `yarn.lock`, which performs a similar function. Some argue Yarn's lock file is more readable and resolves dependencies more deterministically.

## Dependency Resolution

### NPM
- NPM v6 and below had a nested dependency structure that could lead to issues like "dependency hell."
- NPM v7+ now uses a "flat" `node_modules` structure similar to Yarn's approach, which helps avoid these issues.

### Yarn
- Yarn has always had a deterministic dependency resolution system, meaning it ensures the same dependency tree across installations, reducing issues with module conflicts.

## Offline Mode

- **NPM**:
  - NPM does have caching, but offline support is not as robust.
  - Starting from NPM v7+, offline support has improved, but it's not the default behavior.

- **Yarn**:
  - Yarn allows you to install packages offline if they are already cached, making it a more robust solution when working in environments with limited or no internet access.  
    Example:  
    ```bash
    yarn cache list
    ```

## Workspaces Support

- **NPM**: Introduced workspaces support in NPM v7, allowing users to manage monorepos (multiple projects in one repository) with shared dependencies more efficiently.  
  Example:  
  ```bash
  npm install --workspace=<workspace-name>
  ```

- **Yarn**: Yarn has supported workspaces since its early versions, and its implementation is more mature and widely used for managing monorepos.  
  Example:  
  ```bash
  yarn workspaces list
  ```

## Security

- **NPM**: NPM audits your packages for known vulnerabilities and displays security warnings. Use `npm audit` for a detailed report.  
  Example:  
  ```bash
  npm audit
  npm audit fix
  ```

- **Yarn**: Yarn v2+ also provides a built-in security audit tool similar to NPM's.  
  Example:  
  ```bash
  yarn audit
  ```

## CLI Commands

### NPM
- Install a package:  
  ```bash
  npm install <package-name>
  ```
- Install globally:  
  ```bash
  npm install -g <package-name>
  ```

### Yarn
- Install a package:  
  ```bash
  yarn add <package-name>
  ```
- Install globally:  
  ```bash
  yarn global add <package-name>
  ```

## Scalability and Monorepo Handling

- **NPM**: Monorepo support (with workspaces) was added in NPM v7, but it is still not as feature-rich or performant as Yarn for large monorepos.
- **Yarn**: Yarn's workspaces feature has long been a favorite for managing monorepos and large-scale projects, offering more control and scalability.


## Plug'n'Play (PnP)

- **NPM**: Does not support Plug'n'Play natively.
- **Yarn**: Yarn 2 introduced Plug'n'Play (PnP), which eliminates the need for `node_modules`.  

## Community and Ecosystem

- **NPM**:
  - Being the default package manager for Node.js, NPM has the largest ecosystem of packages.
  - Most tutorials, documentation, and third-party integrations are focused on NPM.

- **Yarn**:
  - Yarn was developed by Facebook and has a large community.
  - Yarn's popularity has grown in specific communities due to its performance and feature advantages.

## Versioning

- **NPM**:  
  Example:  
  ```bash
  npm version <new-version>
  ```
  - **Yarn**:  
  Example:  
  ```bash
  yarn version --new-version <new-version>
  ```
## Global Binary Handling

- **NPM**: Installs global packages in a central directory. Ensure the directory is added to your `PATH`.
- **Yarn**: Installs global packages in a separate directory and automatically links them.

## Migration

- **NPM to Yarn**: Install Yarn and run:  
  ```bash
  yarn install
  ```
- **Yarn to NPM**: Delete `yarn.lock` and run:  
  ```bash
  npm install
  ```

## Other Notable Features

- **NPM**:
  - Integrates closely with the npm registry.
  - Has built-in auditing and security features.

- **Yarn**:
  - Offline installation.
  - Zero Installs with Yarn 2 (your codebase contains all dependencies).
  - Better deterministic builds due to the `yarn.lock` file.
