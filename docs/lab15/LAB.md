### 💎 Lab 15 - Setting up CI

###### ⏰ Estimated time: 5-10 minutes

#### 📚 Learning outcomes:

- Basics of GitHub actions
- Use Nx to setup scalable checks on your PRs to ensure only passing code goes into master
- Explore other practical uses of `nx affected`

#### 🏋️‍♀️ Steps :

Before starting on this lab, it's important that you have a version of your local workshop
pushed to your GitHub repo. 

1. Let's make sure the master branch is up to date:
    - If you already are on `master` - commit everything:
        ```
        git add . && git commit -m "finished lab 14"
        git push origin master
        ```

    - If you are on a different branch, commit everything, switch to master
and bring it up to date:

        ```
        git add . && git commit "finish lab 14"
        git checkout master
        git merge previous-branch-you-were-on
        git push origin master
        ```

2. Create a new file `.github/workflows/ci.yml`

    ```
    name: Run CI checks <-- The name will show up on the GitHub Actions dashboard
    
    on: [pull_request] <-- This workflow will run only on Pull Requests
    
    jobs:
      test: <-- give our job an ID
        runs-on: ubuntu-latest <-- the image our job will run on
        name: Testing affected apps <-- the name that will appear on the Actions UI
        steps: <-- what steps it will perform
          - uses: actions/checkout@v1 <-- checkout whatever branch the PR is using
          - uses: bahmutov/npm-install@v1.4.5 <-- trigger an `npm install`
          - run: npm run nx test store <-- test the "store" project
          - run: npm run nx test api <-- test the "api" project
    ```
   
   ⚠️ Make sure to remove the "helper" comments above.

3. Create and switch to a new branch:

    ```
    git add . && git commit -m "add ci"
    git checkout -b dynamic-title
    ```

    ⚠️ I know we **just** switched to master above. But it was important we bring it
    up to date. Now we need to switch to a new branch so we can submit our PR.

4. Open `apps/store/src/app/app.component.html`
5. And make the title of the header dynamic:

    ```
    <bg-hoard-header [title]="title"></bg-hoard-header>
    ```

6. Commit all your changes and push your new branch.
7. Go to GitHub and make a Pull Request to `master`
8. After a few moments you'll see something like this:
    ![GitHub Actions example](./github_actions.png)
9. The unit tests will be failing - that's expected.

---

We are starting to set-up our CI, that will verify our Pull Requests to ensure bad code
doesn't go into `master`.

But now we're testing both projects - even though we only changed the store.

---

##### Testing only affected

10. Let's use `nx affected` to only test the changed projects:
    
    Instead of running two `nx` commands in your CI, run a single `nx affected` command
    that tests all affected projects.

    <details>
    <summary>🐳 Hint 1</summary>
    
    Check-out this [handy tutorial](https://nx.dev/latest/angular/tutorial/11-test-affected-projects#step-11-test-affected-projects)
    Refer to the [docs](https://nx.dev/latest/angular/cli/affected#affected) 
    </details>
    
    <details>
    <summary>🐳 Hint 2</summary>
    
    Since it's a Pull Request, your base commit will always be `--base=origin/master` 
    </details>

    ⚠️ It's okay to work on this on your new branch. We'll merge everything to `master`
    eventually.

11. Commit and push. On your Github Actions log you should see only the `store` tests running.
12. Our tests are now being ran sequentially for each project. See if you can run them in parallel (consult the Nx Affected [docs](https://nx.dev/latest/angular/cli/affected#affected) if unsure) 
12. Our CI only does testing now. But we also have targets for `lint`, `e2e` and `build`. 
Would really be handy if CI also told us if any of those failed.

    **Add more jobs under your CI workflow that run affected for each of the above targets**

13. Commit and push your `ci.yml` changes.
14. You'll notice some new steps in the GitHub Actions UI. Some of them are failling. That is okay. We can fix them later.
15. For now, you can merge your PR into `master `
16. Switch to `master` locally and pull latest so all your new CI changes are up to date.

---

🎓If you get stuck, check out [the solution](SOLUTION.md)

---

[➡️ Next lab ➡️](../lab16/LAB.md)
