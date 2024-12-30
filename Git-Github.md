The issue local branch is behind the remote branch and contains some divergent changes. You need to resolve this by pulling the latest changes from the remote repository and merging or rebasing your local changes.

To fix the issue, follow these steps:

1.**Fetch the latest changes from the remote:**

   ```bash
   git fetch origin
   ```

2. **Rebase or merge the changes:**

- To **rebase** (apply your changes on top of the remote branch):

  ```bash
  git pull --rebase origin main
  ```

   - Alternatively, to **merge** the remote changes into your branch:
    ```bash
    git pull origin main
    ```

      You will need to resolve any merge conflicts manually if they arise.

3. **Push your changes after resolving conflicts (if any):**
  ```bash
  git push origin main

  ```

This should resolve the issue and allow you to push your changes to the remote repository. If you always want to pull with a specific method (rebase or merge), you can set it as a default using:

- For rebase:
  ```bash
  git config pull.rebase true
  ```

- For merge:
  ```bash
  git config pull.rebase false
  ```
  -------------------------------------------------------------------------

practical-12 

$ git config --global user.name "Admin"

$ git config --global user.email snsb.mahi@gmail.com

------------

echo "# cloudweb for my first application" >> README.md
git init
git status
git add README.md  100 
git status
git add *
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/vikas4cloud/cloudweb.git
git push -u origin main


------next time 

git add *
git commit -m "2nd commit"
git push
