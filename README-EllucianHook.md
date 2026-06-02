# Ellucian Experience Extension — Kiro Hook Setup

## Prerequisites

Follow the **Create Repository** steps in the [Ellucian Experience Extension Development: Project Set-Up](https://services.stthomas.edu/TDClient/1898/ClientPortal/KB/Article/168278/Ellucian-Experience-Extension-Development-Project-Set-Up) guide before proceeding.

## Step 1: Configure Trusted Commands

1. Click the **Gear** icon (bottom-left) and search for **"Kiro Agent: Trusted Commands"**.
2. Add the following trusted commands:

    - bash -n .kiro/scripts/*
    - source .ve/bin/activate && git pull
    - basename "$(pwd)"
    - bash -c *
    - cat */.nvmrc
    - mv *
    - cp *
    - bash *
    - ls *
    - cat *
    - basename *
    - source *
    - curl *
    - rm src/fetchUtilsConfig.js
    - rm extension/src/fetchUtilsConfig.js
    - rm -rf /tmp/ellucian-utils
    - rm extension/src/fetchUtilsConfig.js && rm -rf /tmp/- ellucian-utils

## Step 2: Download the `.kiro` Directory

Download or copy the `.kiro` directory into your Ellucian project folder.

## Step 3: Create the Agent Hook

1. Click the **Kiro** icon on the left sidebar.
2. Under **Agent Hooks**, click "Add a hook" and select **"Manually create a hook"**.
3. Set the **Title** to: `Ellucian Initialize and Structure Code`
4. Paste the contents of `ellucian-hook-req.md` into the **Instructions for Kiro agent** field.
5. Save the hook.

## Step 4: Run the Hook

> **Note:** Make sure the `.kiro` directory is inside the Ellucian project folder. `cd` into that folder before running the hook.

Enter the following in the Kiro prompt:

```
Execute hook: Ellucian Initialize and Structure Code.
Do not make a spec folder, just execute the hook.
```

Follow the execution and provide relevant information when prompted.
