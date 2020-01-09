# Workflow-TaskForm

This is an enhancement to the *EnsembleWorkflow* (https://github.com/intersystems/Workflow) used for a Proof of Concept at Prince of Wales Hospital to integrate with HL7 messages from bedside devices.

It consists of:
* Restful web API for InterSystems Ensemble Workflow
* Production in IRIS for Health

## IRIS Setup

+ Set up new namespace – e.g. SESLHD - with the interoperability checkbox ticked.
+ Attach to new DB – e.g. SESLHD
+ Resource name for SESLHD – new resource = %DB_SESLHD
    + READ only Permission (Assumption here is that the ‘Write’ will only be via integration workflow, and not the REST APIs.)
+ New users created – (e.g. NurseMary, NursePeter)
+ New role created - e.g. WorkflowRole
+ Assign NurseMary to new role WorkflowRole
+ WorkflowRole is assigned to %DB_SESLHD role
+ Role %DB_SESLHD is assigned to the SQL table privileges as below:
    + EnsLib_Workflow.ActionDefinition
    + EnsLib_Workflow.RoleDefinition
    + EnsLib_Workflow.RoleMembership
    + EnsLib_Workflow.TaskRequest
    + EnsLib_Workflow.TaskRequest_FormValues
    + EnsLib_Workflow.TaskResponse
    + EnsLib_Workflow.TaskResponse_FormValues
    + EnsLib_Workflow.UserDefinition
    + EnsLib_Workflow.Worklist

## Installation
1. Deploy the production configuration in the namespace created in the previous section
2. Import and compile this project into the same namespace.
3. Create a web-application for REST in the Portal Management System (for ex. /csp/workflow/rest). Set dispatch class to Workflow.REST, Authentication methods to 'Unauthorized' and 'Password'.
4. (Optional) add Workflow package mapping if you need to query another namespace.


## Requests

These are the possible requests to web application (add param ?Namespace={Desired Namespace} to query another namespace cubes):

| URL                         | Type | Body (JSON)                 | Response  | Description                    |
|-----------------------------|------|-----------------------------|-----------|--------------------------------|
| login                       | POST | { "Username":"CacheUser","Password":"CachePassword" }| JSON| Current user|
| tasks                       | GET  |                             | JSON      | Unassigned tasks or assigned to current user |
| tasks/:id                   | GET  |                             | JSON      | EnsLib.Workflow.Worklist object|
| tasks/:id                   | POST |{EnsLib.Workflow.Worklist object}|JSON   | Processing of modified object  |
| test                        | GET  |                             | JSON      | Test info                      |