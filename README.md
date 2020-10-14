# jira-release-composite-action

This composite action will take release notes generated with a tool as input, parse its content and automatically create a related ticket in jira. 

It can be used to coordinate testing by creating a jira ticket.

This action will try to move all tickets which are:

- Code Review state
  - require QA -> QA state
  - require no QA -> Done state

Release tickets for a specific release will be associated to each other to better keep track of all pre releases a specific release run trough. 

For final releases (not -a, -b, -rc) it will try to find the related version in jira, and mark it as released. 


# Developed By

* Mike Penz
 * [mikepenz.com](http://mikepenz.com) - <mikepenz@gmail.com>


# License

    Copyright 2020 Mike Penz

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
