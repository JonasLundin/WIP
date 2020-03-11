# Pull-Request based Change Management Process POC

## Problem Statement

With more regulation and more focus on compliance the task of Change Management becomes more complex and blocking. 
While compliance is important it is crucial that it doesn't become to much of a hurdle in order to not slow down developement too much.

In the software delivery lifecycle (SDLC) there are several steps that require review and approval. 
Close to developers one common review point is code review, often done as a [pull request](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests). 

Another step of review and approval is the [change management process](https://confluence.leovegas.com/display/PT/Change+Management) which is conducted in Jira with references to the PR that is created above and to a manually created component registry (Confluence). 

This RFC is about if we could adapt the PR review process to cater for the requirements of both code review and change management. 
The main reasons for this are: 
- Limit the number of review and approval points in the software delivery lifecycle.
- Limit the number of systems/tools involved (Jira, Github and Confluence).
- Limit the number of errors (ie wrong information) in current change process as the "truth" about what is both developed and configured is our source-code repository and thus could be defined as our CMDB.


### Details

There are a lot of different ways of defining change management, ITIL does it as: 

_The goal of the change management process is to ensure that standardized methods and procedures are used for efficient and prompt handling of all changes, in order to minimize the impact of change-related incidents upon service quality, and consequently improve the day-to-day operations of the organization._

And ISO as: 
_To ensure all changes are assessed, approved, implemented and reviewed in a controlled manner._

Towards regulators we have an obligation to capture and prove that all of the above is done in our change management process.
Different regulations have different requirements on what kind of evaluations and approvals to be done, what kind of information to be captured and how this should be reported. 

## Proposers

[Engineering Effectiveness](https://confluence.leovegas.com/display/PLAT/Engineering+Effectiveness)

## Duration

As this is a suggestion for a POC I want to keep the open feedback-loop short:
2020-02-05

## Proposal

### Assumptions/Axioms for this proposal to work:
- A Change is when something is merged to master
    - That means, when a PR is reviewed and approved and merged to master equals a Change. 
    - One could argue that a change is when something is put into production, however, in this proposal this would be when the merge to master happens. One advantage of this is that a master merge almoste exclusively happens after a SCR-approval. 
- A Release is when something is merged to master on a production configuration repo
    - That means, when a PR is reviewed and approved and merged to production config master equals a Release. 
- Github is our [CMDB](https://en.wikipedia.org/wiki/Configuration_management_database)
    - We would adopt that Github is our single point of truth when it comes to sofware and configuration. 

### Implemention points (TLDR):
- Start using [CODEOWNERS](https://help.github.com/en/github/creating-cloning-and-archiving-repositories/about-code-owners) extensively
    - By implementing a stardard where critical and compliance parts of the code are noted in the CODEOWNERS-file, we would be able to require approval (ie assign approvers) from different levels of compliance depending on the change made automatically. This means that if a developer for instance would edit a part of the codebase that is critical for Spain the "Spain-Compliance-Team" could be assigned as mandatory reviewers and approvers for the PR (Change). If the change is insignificant a peer would be accepted as approver.
    - We could implement that the assigned reviewers and approvers would get Slack-notifications upon being assigned
    - In order to implement this, Change and Compliance functions would have to educate the teams in what is critical and what is necessary to capture in the Change process for different regulations so teams can update their respective CODEOWNERS file. 
    - This can of course be used for other things aswell not related to compliance, for instance critical changes in services used by other teams (ie Data and Analytics might want to know when some certain changes are happening in specific databases or services).
    - Truth of compliance categorization of components would be the CODEOWNERS-file. 
- Implement PR-templates
    - When we do have a CODEOWNERS structure as above, we would implement automation to create specialized PR templates depending on what kind of reviewers that gets assigned when creating the PR. 
    - If the change is non-significant to all regulations we would implement the "basic" parts that are currently part of the SCR-process, approver would be a peer in the organization. 
    - If the change is significant to specific regulation a corresponding PR-teamplate would be used for this PR (and by that also other approvers; Change Manager, TCM, Compliance or similar).
- Data extraction and reporting
    - Upon merge to master (ie "Change") the information in the PR (affected components, risk evaluation, approvers etc.) would be extracted and stored together will all other information related to the change (like information about the deployment of it) in order to be searchable and produce reports targeted at different stakeholders (regulators, audit etc.)

By implementing this a developer wouldn't have to think or know if a change might be critical, as long as we maintain the definitions of what is critical in the CODEOWNERS-file, this would be automatically tagged when trying to do the code change. 

I think we could start this by trying it as a POC with one team or similar to see if it flies. 
 
## Unresolved Questions

There are a lot of unresolved questions, but here are a few:

- How long time would it take to implement to CODEOWNERS part above? This would require attention from the whole dev and product organization and also training of the teams (which in fact would be a really good thing).
- How should the PR-templates look? This is probably somehting that Change Management and Compliance need to decide, but it shouldn't be to hard to do this. 
- Is the organization willing to invest time in setting this up? I would imagine the potential benefits of it to speed up things later on that it would actually be worth it. 
- How do we keep the CODEOWNER-files up to date? I guess this is something that needs to be worked out, but one potential way of doing it is to do regular internal reviews with Change/Compliance-functions in every team explaining what is critical and the team can respond by updating accordingly (these kind of internal reviews should probably be done anyhow...) 

## Status

The current state of the RFC:

- **In Progress** : Review in progress.

