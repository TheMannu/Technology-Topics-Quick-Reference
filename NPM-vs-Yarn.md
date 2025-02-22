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
