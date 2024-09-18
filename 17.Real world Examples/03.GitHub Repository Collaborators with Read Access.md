## GitHub Repository Collaborators with Read Access Script
This Bash script retrieves a list of users with **read access** to a specified GitHub repository using the GitHub API. 
It authenticates via a GitHub username and personal access token, then fetches the list of collaborators for the repository.
The script filters users with **read (pull) permissions** and displays their usernames. If no users are found, it outputs
a relevant message.

## Features:
- Fetches collaborators of a specified GitHub repository.
- Filters and lists users with **read access**.
- Provides clear output for missing collaborators.

```
#!/bin/bash

# GitHub API URL
API_URL="https://api.github.com"

# GitHub username and personal access token
USERNAME=$username
TOKEN=$token

# User and Repository information
REPO_OWNER=$1
REPO_NAME=$2

# Function to make a GET request to the GitHub API
function github_api_get {
    local endpoint="$1"
    local url="${API_URL}/${endpoint}"

    # Send a GET request to the GitHub API with authentication
    curl -s -u "${USERNAME}:${TOKEN}" "$url"
}

# Function to list users with read access to the repository
function list_users_with_read_access {
    local endpoint="repos/${REPO_OWNER}/${REPO_NAME}/collaborators"

    # Fetch the list of collaborators on the repository
    collaborators="$(github_api_get "$endpoint" | jq -r '.[] | select(.permissions.pull == true) | .login')"

    # Display the list of collaborators with read access
    if [[ -z "$collaborators" ]]; then
        echo "No users with read access found for ${REPO_OWNER}/${REPO_NAME}."
    else
        echo "Users with read access to ${REPO_OWNER}/${REPO_NAME}:"
        echo "$collaborators"
    fi
}

# Main script

echo "Listing users with read access to ${REPO_OWNER}/${REPO_NAME}..."
list_users_with_read_access
```

## Conclusion
This Bash script efficiently identifies and lists users with read access to a GitHub repository, making it easier to manage
collaborator permissions using GitHub's API for streamlined access control.