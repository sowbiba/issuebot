### Rules

Here are the scenarii implemented in issuebot in a gherkin like syntax

You can find these rules described in src/rule.js and defined in src/rules/


**Scenario C1**: place an Issue in the “To do” column according to its label

GIVEN an Issue in the kanban

WHEN it is labeled “To Do”

THEN it is placed in the “To do” column


**Scenario C2**: place an Issue in the “Done” column when it is closed

GIVEN an Issue in the kanban

WHEN it is closed

THEN it is placed in the “Done” column


**Scenario D1**: make automatic status labels mutually exclusive

GIVEN an Issue

WHEN it is labeled using one of the automatic status labels

THEN any other automatic status label is removed


**Scenario D2**: label an Issue when it is reopened

GIVEN a closed Issue

WHEN it is reopened

THEN remove the FIXED label

AND label it as “TBS”

AND move the issue to the TBS column



**Scenario D3**: remove automatic status labels when closing an Issue in the kanban

GIVEN an open Issue

WHEN it is closed

THEN any automatic status label is removed

AND add the FIxed label



**Scenario D4**: reopen an Issue when its status label is changed

GIVEN a closed Issue

WHEN it is labeled using one of the automatic status labels EXCEPT FIXED

THEN it is reopened

Move to To-Do column if new label is To-Do, else TBS



**Scenario E1**: reflect the Pull Request milestone in the linked Issue

GIVEN an open Issue

AND a Pull Request linked to it

WHEN the PR is set to a milestone

THEN apply that the correct project to the Issue



**Scenario E3**: move Issue in kanban to the “To be tested” column when the linked PR is ready for test

GIVEN an open Issue

AND a Pull Request linked to it

WHEN the PR is labeled “Waiting for QA”

THEN move the linked issue to the “To be tested” column

AND remove all automatic labels

AND remove issue assignee if there is one

IF the issue has WIP label

THEN remove WIP label



**Scenario E4**: move Issue in kanban to the “To be merged” column when the linked PR is ready for merge

GIVEN an open Issue

AND a Pull Request linked to it

WHEN the PR is labeled “QA ✅”

IF the PR is approved => 2 approvals

THEN move the linked issue to the “To be merged” column

AND remove all automatic labels

AND remove assignee if there is oneScenario E5: close the Issue in kanban when the linked PR is merged

GIVEN an open Issue

AND a Pull Request linked to it

WHEN the PR is merged

THEN close the Issue

AND label the issue “Fixed”

AND remove assignee if there is one



**Scenario E6**: linking a PR to a closed issue re-opens it

GIVEN a closed Issue

WHEN a Pull Request is linked to it via “Fixes #123”

THEN re-open the Issue

AND apply the PR automation rules to update it



**Scenario F1**: requesting changes in a PR moves the linked issue to the “In progress” column

GIVEN an open Issue

AND a Pull Request linked to it

WHEN changes are requested in the PR

THEN move the linked Issue to the “In progress” column

AND define the PR’s author as a assignee on the issue



**Scenario G2**: label new Issue moved in “ToDo” column

GIVEN an Issue in any state

WHEN it is created into the column “ToDo”

IF the Issue has not the label “ToDo”

THEN add the label “ToDo” to the Issue

AND remove all other status labels

AND IF the Issue is closed

THEN re-open it



**Scenario H1**: label Issue as in progress when moved to “In progress” column

GIVEN an Issue in any state

WHEN it is moved into the column “In progress”

THEN add the label “WIP” to the Issue

AND remove all other status labels

AND IF the Issue is closed

THEN re-open it



**Scenario H2**: mark Issue as in-progress if WIP PR is linked

WHEN a PR is linked to an Issue

IF the Issue is in the column “ToDo” in the Kanban

AND IF the PR has the label “wip” OR if the PR is in Draft

THEN move the Issue into “In progress” column

AND remove “ToDo” label if it exists



**Scenario I1**: mark Issue as To Be Reviewed if PR is linked

WHEN a PR is linked to an Issue

IF the PR does not have the label “WIP” OR the PR is not in draft

AND IF the Issue is in the column “ToDo” or “In progress” in the Kanban

THEN move the Issue into “To be reviewed” column

AND remove “ToDo” label if it exists

AND remove WIP label



**Scenario J1**: mark Issue as TBT if PR is approved

WHEN a PR is approved

IF the linked Issue is in “In To be reviewed” column

THEN move the Issue into “To be tested” column

AND add the label Waiting for QA to PR

AND remove assignee if there is one



**Scenario J3**: label Issue when moved to “To be tested” column

WHEN an Issue is moved into the column “To be tested”

IF the Issue has not the label “waiting for QA”

THEN add the label “waiting for QA” to the Issue

AND remove assignee if there is one



**Scenario J4**: mark Issue as in-progress if QA disapproves

WHEN a PR is labeled “waiting for author”

IF the linked Issue is in “In To be tested” column

THEN move the Issue into “In progress” column



**Scenario K1**: label/update Issue moved in “Done” column

WHEN an Issue is moved into the column “Done”

AND add the label “Fixed” to the Issue

AND close the Issue if it is open

AND remove assignee if there is one



**Scenario L1**: label/update Issue moved in “TBS” column

WHEN an Issue is moved into the column “TBS”

AND add the label “TBS” to the Issue

AND remove assignee if there is one



**Scenario L2**: Move card in ToBeMerged column if issue is labelled QA OK

WHEN an PR is labeled QA OK

THEN remove all the automatic labels for linked issues

IF the linked card is in the ToBeTested column

THEN move the card in the ToBeMerged column

AND remove assignee if there is one
