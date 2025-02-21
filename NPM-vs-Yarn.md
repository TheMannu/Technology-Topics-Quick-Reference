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
