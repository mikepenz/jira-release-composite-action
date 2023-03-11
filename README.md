# jira-release-composite-action

This composite action will take release notes generated with a tool as input, parse its content and automatically create a related ticket in jira. 

It can be used to coordinate testing by creating a jira ticket.

This action will try to move all tickets which are:

- Code Review state
  - require QA -> QA state
  - require no QA -> Done state

Release tickets for a specific release will be associated to each other to better keep track of all pre releases a specific release run trough. 

For final releases (not -a, -b, -rc) it will try to find the related version in jira, and mark it as released. 

# Setup

Add it to your github actions workflow:

```hml
- name: Jira-Release
  if: startsWith(github.ref, 'refs/tags/')
  uses: mikepenz/jira-release-composite-action@SHA-1
  with:
    configurationDirectory: ${{ github.workspace }}/.github/config
    projectDirectory: ${{ github.workspace }}
    cachesDirectory: ${{ github.workspace }}/cache
    releaseVersion: ${{ steps.tag_version.outputs.VERSION }}
    changeLog: ${{ steps.github_release.outputs.body }}
    jiraToken: ${{ secrets.JIRA_TOKEN }}
```

Additionally specify the configuration file `${{ github.workspace }}/.github/config/jira_magic_config.json`:
```json
{
  "jiraInstance": "https://yourOrg.atlassian.net/",
  "title": "[A] Test Product",
  "descriptionPlaceholder": "## 🚀 Features\n\n- Equal to previous tag\n\n## 🐛 Fixes\n\n- /",
  "releaseBaseUrl": "https://github.com/yourOrg/yourProject/releases/tag/",
  "releaseAssignee": "123456:5f9e786f-1234-4eed-1234-489681234",
  "releaseProject": "PROJ1",
  "associatedProjects": [
    "PROJ1",
    "PROJ2"
  ],
  "fallbackNextVersion": true,
  "fallbackNextVersionName": "next",
  "versionBase": "android_",
  "prBaseUrl": "https://github.com/yourOrg/yourProject/pull/",
  "pluginDirectory": ".github/config/plugins/",
  "migrateTickets": true,
  "customReleaseFields": {
    "customfield_12345": [ { "value": "Android" } ],
    "customfield_12346": [ "YES" ]
  },
  "customRequireQAField": "customfield_12346"
}
```

# Sample output

```
Project directory: /home/runner/work/yourProject/yourProject
Config directory: /home/runner/work/yourProject/yourProject/.github/config
Loading Config
Config:
-- jiraInstance: https://yourOrg.atlassian.net/
-- title: [A] Test Product
-- releaseBaseUrl: https://github.com/yourOrg/yourProject/releases/tag/
-- releaseAssignee: 123456:5f9e786f-1234-4eed-1234-489681234
-- releaseProject: PROJ1
-- associatedProjects: PROJ1, PROJ2
-- fallbackNextVersion: true
-- fallbackNextVersionName: next
-- versionBase: android_
-- version: 2020.10.15-a01
-- resolvedTargetVersion: android_2020.10.15
-- prBaseUrl: https://github.com/yourOrg/yourProject/pull/
-- pluginDirectory: .github/config/plugins/

Doing jira migration for: 2020.10.15-a01
This release contains:
- Ticket: PROJ1-1, Backlog, null
- Ticket: PROJ1-2, Done, null
Find and execute plugins
Create release ticket
-- Fallback version exists in jira (PROJ1) - android_next
-- Found 2 matching release tickets to associate
--- Matching release ticket: PROJ1-3
--- Matching release ticket: PROJ1-5
- Assigning release ticket to 'qa': https://yourOrg.atlassian.net/browse/PROJ1-4
-- Transitioned ticket into: qa
Associate previous release tickets
-- Associated ticket PROJ1-3 to PROJ1-4
-- Associated ticket PROJ1-5 to PROJ1-4
Update release versions
-- Couldn't find id for release (PROJ1) - android_2020.10.15-a01
-- Couldn't find id for release (PROJ2) - android_2020.10.15-a01
```

## Other actions

- [release-changelog-builder-action](https://github.com/mikepenz/release-changelog-builder-action)
- [xray-action](https://github.com/mikepenz/xray-action/)
- [action-junit-report](https://github.com/mikepenz/action-junit-report)


# License

    Copyright 2022 Mike Penz

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
