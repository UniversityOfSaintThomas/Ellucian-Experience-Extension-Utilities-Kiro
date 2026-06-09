I need a hook that will execute the following:

**IMPORTANT: Before proceeding, review this file (`ellucian-hook-req.md`) in its entirety as the authoritative reference for all steps and requirements below.**

## Interaction Style

- Prompt the user **ONE question at a time** using the `user_input` tool.
- When a question has a recommended/default answer, provide it as a button option marked `recommended: true`.
- Keep variable names generic (e.g., `uploadToken`, `cardName`, `cardPublisher`, `cardType`, `cardTitle`, `cardDisplayType`, `cardDescription`, `cardIcon`, `requiresEthosApiKey`) so they can be reused if requirements change.
- After collecting each answer, proceed to the next question. Do NOT batch questions together.

### Question Sequence

1. **uploadToken** — Ask for their Experience Extension upload token. No default (free text). If they skip or say none, leave the .env token line commented out.
2. **cardName** — Ask for the card name (PascalCase, no spaces). No default.
3. **cardPublisher** — Ask for the publisher. Default/recommended: `StThomas`.
4. **cardType** — Ask for the card type (PascalCase, no spaces). No default.
5. **cardTitle** — Ask for the user-facing card title. No default.
6. **cardDisplayType** — Ask for the display card type (human-readable). No default.
7. **cardDescription** — Ask for a description of the card. No default.
8. **cardIcon** — Ask for the mini card icon (from Ellucian Path Iconography). Provide option to skip (optional field).
9. **requiresEthosApiKey** — Ask if the project requires an Ethos API Key. Default/recommended: `No`. Provide Yes/No buttons.

---

Use Node 24 by running the nvm use 24 command
Run the Ellucian create-experience-extension command to create your extension folder (be sure to replace my-extension with the parent directory name).

## Initialize Structure

### Create the necessary structure and intial settings of the project by running this command:
```bash
npx -p https://cdn.elluciancloud.com/assets/SDK/latest/ellucian-create-experience-extension-latest.tgz create-experience-extension my-extension
```

**Important!** Double check the .nvmrc file created in the extension directory and ensure it is using 24.x.x. If you see a version earlier than 24. Occasionally, even though we specify nvm use 24 and the latest experience extension release, it will use old SDK packages. If it happens again, notify the user to see if someone else can run the command to init the repo. It is unknown why this happens or how to reproduce it, and therefore, we have no fix for it.

### Rename Project to "Extension"
To standardize, rename the "my-extension" directory to `extension`. (We don't use "extension" while running the npx command since it fills in a lot of information in other files.)
```bash
mv your-extension-name extension
```

## Set up the extension (including the .env file, extension.js, package.json, and .nvmrc)

### Make a copy of sample.env and name it .env:
```
cd extension
cp sample.env .env
```

### Install Config and UtilsE
Go to the following repository and run the `fetchUtilsConfig.js` file from within the `extension/src/` directory. The `config` and `utils` folders should end up under `extension/src/`:
https://github.com/UniversityOfSaintThomas/Ellucian-Experience-Extension-Utilities-JS-React

To do this:
1. Clone the utilities repo to a temp directory:
```bash
source ~/.nvm/nvm.sh && nvm use 24 && git clone https://github.com/UniversityOfSaintThomas/Ellucian-Experience-Extension-Utilities-JS-React.git /tmp/ellucian-utils 2>&1
```
2. Copy `fetchUtilsConfig.js` from the cloned repo into the `extension/src/` directory:
```bash
cp /tmp/ellucian-utils/fetchUtilsConfig.js extension/src/
```
3. Run `node fetchUtilsConfig.js` from within `extension/src/`:
```bash
cd extension/src && source ~/.nvm/nvm.sh && nvm use 24 && node fetchUtilsConfig.js
```
4. Delete `fetchUtilsConfig.js` when done:
```bash
rm extension/src/fetchUtilsConfig.js
```
5. Clean up the cloned repo:
```bash
rm -rf /tmp/ellucian-utils
```
6. Verify that `extension/src/config/` and `extension/src/utils/` exist


Prompt the user for their `upload-token`.


### Modify Files
If they have provide it, in `.env` - Uncomment and fill in:

```env
EXPERIENCE_EXTENSION_UPLOAD_TOKEN={upload-token}
EXPERIENCE_EXTENSION_ENABLED=true
EXPERIENCE_EXTENSION_ENVIRONMENTS=Devl
```
name: No spaces, use Pascal Case (StudentVerifTask)
publisher: StThomas
type: No spaces, use Pascal Case (Student)
title: (This will be displayed to users, so obtain from the project owner)
displayCardType: Look at other similar card properties. Human-readable.
description: Give it a good description.
miniCardIcon: (Optional) Choose an icon from Ellucian Path Iconography. This will be shown to the user in the card's upper-left corner.

In `extension.js`, under `cards`, update:
```
name: No spaces, use Pascal Case (StudentVerifTask)
publisher: StThomas
type: No spaces, use Pascal Case (Student)
title: (This will be displayed to users, so obtain from the project owner)
displayCardType: Look at other similar card properties. Human-readable.
description: Give it a good description.
miniCardIcon: (Optional) Choose an icon from Ellucian Path Iconography. This will be shown to the user in the card's upper-left corner.
```
If your project requires an API Key for ethos, add the configuration code into the cards list. This will add the API Key field to the online Configuration panel when enabling the extension. 
cards: [{
   ...
   configuration: {
      server: [{
      key: 'ethosKey',
      label: 'Ethos API Key',
      type: 'password',
      require: true
      }]
   },
}],

In `package.json`, add --env forceUpload to scripts.deploy-dev property

- Note: Ensure the script property has BOTH --env upload AND --env forceUpload.
- Ellucian dependencies can be unpredictable. For SDK v8.1.2 be sure the following dependencies are present:
```json
"dependencies": {
    "@ellucian/ds-icons": "https://cdn.elluciancloud.com/assets/EDS2/8.4.0/umd/path_design_system_icons.tgz",
    "@ellucian/experience-extension-utils": "https://cdn.elluciancloud.com/assets/SDK/utils/1.1.0/ellucian-experience-extension-utils-1.1.0.tgz",
    "@ellucian/react-design-system": "https://cdn.elluciancloud.com/assets/EDS2/8.4.0/umd/path_design_system.tgz",
    "@ellucian/experience-extension-extras": "github:ellucian-developer/experience-extension-extras#8.1.0",
    "react": "19.0.3",
    "react-dom": "19.0.3",
    "react-intl": "7.1.11",
    "react-router-dom": "5.2.0"
```

.nvmrc
We don't care about the specific minor and patch version, so just give it a nice Major version by removing the dotted numbers after the first number.
For example, `24.18.1` becomes `24`

### Fix .babelrc
After the project is initialized, there will be a `.babelrc` file in the `extension` directory. Add `@babel/plugin-transform-class-properties` to the `plugins` array so it looks like:

```json
"plugins": [
  "@babel/plugin-transform-class-properties",
  "@babel/plugin-transform-runtime"
],
```

### Fix ESLint Config
After the project is initialized, there will be an `eslint.config.mjs` file in the `extension` directory. Update the `globals` section so it uses the spread of `globals.browser` instead of individual browser globals. 

Replace:
```js
globals: {
  window: 'readonly',
  document: 'readonly',
  navigator: 'readonly',
  process: 'readonly',
  console: 'readonly',
}
```

With:
```js
globals: {
  ...globals.browser,
  process: 'readonly',
}
```

Make sure `globals` is imported from the `globals` package at the top of the file (e.g., `import globals from 'globals'`). This package should already be a dev dependency.

### Install Packages
cd into the extension directory and run the install command.
```bash
cd {your_ext_dir}
nvm use
npm install
```