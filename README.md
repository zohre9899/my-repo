"""
GitHub Repository Manager
-------------------------
A simulation of a GitHub-like repository system.
This version supports:
- repository creation
- adding and removing files
- searching files
- updating description
- creating branche
- making commits
- showing commit history
"""

import datetime


class Repository:
    def __init__(self, name, description, language="Python"):
        self.name = name
        self.description = description
        self.language = language
        self.files = []
        self.branches = {"main": []}
        self.current_branch = "main"
        self.commits = []

    def add_file(self, filename):
        if filename not in self.files:
            self.files.append(filename)
            print(f"✅ File '{filename}' added.")
        else:
            print(f"⚠️ File '{filename}' already exists.")

    def remove_file(self, filename):
        if filename in self.files:
            self.files.remove(filename)
            print(f"🗑 File '{filename}' removed.")
        else:
            print(f"❌ File '{filename}' not found.")

    def search_file(self, filename):
        if filename in self.files:
            print(f"🔍 File '{filename}' exists in repository.")
        else:
            print(f"❌ File '{filename}' not found.")

    def update_description(self, new_desc):
        self.description = new_desc
        print("📝 Repository description updated!")

    def create_branch(self, branch_name):
        if branch_name not in self.branches:
            self.branches[branch_name] = list(self.files)  # copy files from main
            print(f"🌿 Branch '{branch_name}' created.")
        else:
            print(f"⚠️ Branch '{branch_name}' already exists.")

    def switch_branch(self, branch_name):
        if branch_name in self.branches:
            self.current_branch = branch_name
            print(f"🔀 Switched to branch '{branch_name}'.")
        else:
            print(f"❌ Branch '{branch_name}' does not exist.")

    def commit(self, message):
        timestamp = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        commit_entry = {
            "branch": self.current_branch,
            "message": message,
            "time": timestamp,
            "files": list(self.files)
        }
        self.commits.append(commit_entry)
        print(f"📌 Commit created on branch '{self.current_branch}': {message}")

    def show_commits(self):
        if not self.commits:
            print("📭 No commits yet.")
            return
        print("📜 Commit history:")
        for c in self.commits:
            print(f"- [{c['time']}] ({c['branch']}) {c['message']} -> Files: {len(c['files'])}")

    def show_info(self):
        print("=" * 50)
        print(f"📦 Repository: {self.name}")
        print(f"📝 Description: {self.description}")
        print(f"💻 Language: {self.language}")
        print(f"🌿 Branches: {', '.join(self.branches.keys())}")
        print(f"📂 Files: {len(self.files)}")
        print("=" * 50)


def main():
    print("🚀 Welcome to GitHub Repository Manager 🚀")

    # Create repository
    repo = Repository(
        name="advanced-repo",
        description="An advanced GitHub-like repository simulation."
    )

    repo.show_info()

    # Add some files
    repo.add_file("main.py")
    repo.add_file("utils.py")
    repo.add_file("README.md")

    # Commit changes
    repo.commit("Initial commit with basic files")

    # Create new branch
    repo.create_branch("dev")
    repo.switch_branch("dev")

    # Modify files in dev branch
    repo.add_file("dev_feature.py")
    repo.commit("Added new feature in dev branch")

    # Show commits
    repo.show_commits()

    # Back to main
    repo.switch_branch("main")
    repo.commit("Hotfix in main branch")

    repo.show_commits()
    repo.show_info()


if __name__ == "__main__":
    main()
