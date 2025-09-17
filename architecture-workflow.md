### Architecture and Workflow

This solution is built on a two-repository architecture that is automated using **GitHub Actions**. The main goal is to seamlessly synchronize your technical documentation (in the `technical-docs` repository) with the public website (in the `digital-garden` repository).

The workflow is as follows:

1.  **Document Creation**: You write your documentation in Markdown and store it in your `technical-docs` repository.
2.  **Trigger**: A `git push` to the `main` branch of the `technical-docs` repository triggers a dedicated GitHub Action.
3.  **API Call**: This action uses a secure **Personal Access Token (PAT)** to call the GitHub API and manually trigger a workflow in the other repository.
4.  **Deployment**: The trigger (`repository_dispatch`) in your `digital-garden` repository starts the build and deployment process. This workflow first retrieves the files from your `technical-docs` repository, copies them into the Quartz `content` folder, and then builds the website.
5.  **Publishing**: The generated website is published on **GitHub Pages**, making your documentation publicly accessible.

---

### Key Technical Points

* **GitHub Actions**: This is the automation engine. It allows you to define workflows that run in response to specific events (like a `push` or a `repository_dispatch`).
* **Two Repositories**:
    * **`technical-docs`**: This repository is your source of truth. It contains your raw documentation.
    * **`digital-garden`**: This repository is the engine for your website. It holds the Quartz configuration, the deployment workflows, and the public-facing content.
* **`repository_dispatch`**: This GitHub event is the communication link between your two repositories. It allows an action in one repository to trigger a workflow in another.
* **`Personal Access Token (PAT)`**: The PAT is a personal authentication token that gives an action the permission to interact with your repositories. It's stored as a **Repository secret** for security reasons.
* **`actions/checkout`**: This action is used in the deployment workflow to clone the contents of both repositories into the runner's container, which enables file copying.
* **`cp` and `find`**: Simple shell commands are used to copy the content from your `technical-docs` repository to the Quartz `content` folder and to exclude unwanted `README` files.