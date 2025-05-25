# DevOps Multi-Repo Merge Queue Challenge

## Objective

Evaluate your ability to design and implement a solution for coordinating merges across multiple repositories, ensuring stability through automated integration testing. You will propose a solution for a multi-repo merge queue system and implement a component of it using Ansible.

## Problem Statement

Your team manages three interdependent repositories: `genlayer-js`, `studio`, and `cli`. Changes to these repositories often have cross-repo dependencies, and merging directly to the `main` branch without validation can break the system. You need to design a system that:

- Coordinates merges across these repositories into their respective `main` branches only after passing end-to-end (E2E) integration tests.
- Uses a centralized mechanism to define which commits or branches from each repository should be tested together.
- Automates the setup and execution of the integration pipeline.

The provided document outlines a solution using a coordinator repository with a manifest file, a CI pipeline, and protected branches. Your task is to propose a similar or improved solution and implement a specific component using Ansible.

## Challenge Requirements

### Part 1: Solution Proposal

Propose a solution for a multi-repo merge queue system that meets the following requirements:

1. **Central Coordination**: A mechanism to specify which commits/branches from each repository (`genlayer-js`, `studio`, `cli`) should be tested together.
2. **Protected Branches**: Ensure the `main` branch of each repository is protected and only mergeable via the automated pipeline after passing E2E tests.
3. **Integration Pipeline**: A pipeline that checks out the specified commits, builds the repositories, runs E2E tests, and merges changes if successful.
4. **Failure Handling**: If tests fail, notify developers with logs and prevent merging.
5. **Scalability**: The system should handle multiple changesets and scale to support additional repositories.

In your proposal, address:

- How you will structure the coordinator mechanism (e.g., a manifest file, database, or other approach).
- The CI/CD tool you would use (e.g., GitHub Actions, GitLab CI, Jenkins) and why.
- How you will handle permissions and security for automated merges.
- Any enhancements or differences from the provided document’s approach (e.g., batching, dynamic queue management).

### Part 2: Ansible Implementation

Implement an Ansible playbook to automate the setup of the coordinator repository and its initial configuration on a Linux server (Ubuntu 22.04). The playbook should:

1. **Create the Coordinator Repository**:
   - Set up a new Git repository named `multi-repo-coordinator` in `/opt/multi-repo-coordinator`.
   - Initialize it with a `main` branch and an empty commit.
2. **Configure the Manifest File**:
   - Create a `manifest.yaml` file in the repository with a sample changeset referencing the three repositories (`genlayer-js`, `studio`, `cli`).
   - Ensure the file has proper permissions (e.g., readable by the CI user).
3. **Set Up CI User**:
   - Create a user `ci-bot` with appropriate permissions to read/write to the repository.
   - Generate an SSH key pair for the `ci-bot` user and configure it for Git access (assume GitHub or a similar service).
4. **Install Dependencies**:
   - Install Git and any necessary tools (e.g., `yq` for parsing YAML) on the server.
5. **Ensure Idempotency**:
   - The playbook should be idempotent, meaning it can be run multiple times without causing errors or duplicate configurations.

### Deliverables

1. **Solution Proposal**:
   - A document (PDF or Markdown) detailing your proposed solution, addressing the points in Part 1.
   - Maximum 2 pages, concise and clear.
2. **Ansible Playbook**:
   - A file named `setup_coordinator.yml` containing the Ansible playbook for Part 2.
   - Include any necessary roles, templates, or variable files.
   - Provide a brief README explaining how to run the playbook and any assumptions made.
3. **Submission**:
   - Submit all files in a ZIP archive or a Git repository link.
   - Include a cover letter (optional) explaining your approach and any trade-offs.

### Assumptions

- The server is a fresh Ubuntu 22.04 instance with root access.
- The repositories are hosted on GitHub, but you can assume a similar Git-based service if preferred.
- You do not need to implement the full CI pipeline or E2E tests, only the coordinator repository setup.
- The E2E tests are assumed to exist and can be run via a script (e.g., `./scripts/run_integration_tests.sh`).

## Evaluation Criteria

Your submission will be evaluated based on the following:

### Solution Proposal (40%)

- **Clarity and Completeness** (15%): Is the proposal clear, concise, and comprehensive? Does it address all requirements?
- **Feasibility and Scalability** (15%): Is the solution practical and scalable for multiple repositories and changesets?
- **Innovation** (10%): Does the proposal improve upon the provided document’s approach or introduce creative solutions?

### Ansible Implementation (50%)

- **Correctness** (20%): Does the playbook correctly set up the coordinator repository, manifest file, and CI user as specified?
- **Idempotency and Robustness** (15%): Is the playbook idempotent and resilient to errors (e.g., handles existing files/users)?
- **Best Practices** (15%): Does the playbook follow Ansible best practices (e.g., modular roles, clear variable names, proper use of handlers)?

### Documentation and Presentation (10%)

- **README Quality** (5%): Is the README clear, with instructions for running the playbook and any assumptions?
- **Code Comments** (5%): Are the Ansible tasks well-commented to explain their purpose?

## Example Manifest File

For reference, the `manifest.yaml` file should look like this:

```yaml
changesets:
  - id: CS-1001
    repositories:
      - name: genlayer-js
        ref: feature/new-config
      - name: studio
        ref: feature/ui-update
      - name: cli
        ref: feature/cli-update
    status: pending
```

## Submission Deadline

Please submit your deliverables within 2 days of receiving this challenge (You don't need full time to complete it). Late submissions will not be accepted unless prior approval is granted.

## Questions?

If you have any questions about the requirements or scope, please reach out to the evaluation team.

Good luck, and we look forward to reviewing your submission!
