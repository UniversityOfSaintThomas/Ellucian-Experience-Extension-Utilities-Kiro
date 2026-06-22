# Ellucian Experience Extension — Kiro Hook Setup

## Prerequisites

Follow the **Create Repository** step in the [Ellucian Experience Extension Development: Project Set-Up](https://services.stthomas.edu/TDClient/1898/ClientPortal/KB/Article/168278/Ellucian-Experience-Extension-Development-Project-Set-Up) guide before proceeding.

Additionally, be sure to use the Kiro IDE for this set up.

## Step 1: Configure Trusted Commands

1. Click the **Gear** icon (bottom-left) and search for **"Kiro Agent: Trusted Commands"**.
2. Add the following trusted commands:

    - bash -c *
    - mv *
    - cp *
    - ls *
    - cat *
    - basename *
    - source *
    - curl *
    - rm extension/src/fetchUtilsConfig.js
    - rm -rf /tmp/ellucian-utils
    - rm extension/src/fetchUtilsConfig.js && rm -rf /tmp/ellucian-utils

## Step 2: Download the `.kiro` Directory

Download the `.kiro` directory into your Ellucian project folder by running this command:

```
curl -L https://github.com/UniversityOfSaintThomas/Ellucian-Experience-Extension-Utilities-Kiro/archive/refs/heads/main.zip -o repo.zip && unzip repo.zip && cp -a Ellucian-Experience-Extension-Utilities-Kiro-main/. . && rm -rf Ellucian-Experience-Extension-Utilities-Kiro-main repo.zip
```

## Step 3: Run the Hook

> **Note:** Make sure the `.kiro` directory is inside the Ellucian project folder. `cd` into that folder before running the hook.

Enter the following in the Kiro prompt:

```
Execute hook: Ellucian Initialize and Structure Code.
```

Alternatively you can:
1. Click the **Kiro** icon on the left sidebar.
2. Under **Agent Hooks**, look for the `Ellucian Initialize and Structure Code` hook.
3. Click on the play button next to `Ellucian Initialize and Structure Code` to run the hook

> Follow the execution and provide relevant information when prompted.
