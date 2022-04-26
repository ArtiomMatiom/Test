![workflow status](https://github.com/recognizegroup/fiberworkz/actions/workflows/ci.yaml/badge.svg)

# VWT Fiberworkz

This repository contains the source code and infrastructure deployment for the VWT Fiberworkz project.

## INFO:

### Action (jobs):
[![Build](https://github.com/recognizegroup/fiberworkz/actions/workflows/build_test.yaml/badge.svg)](https://github.com/recognizegroup/fiberworkz/actions/workflows/build_test.yaml)

[![Build](https://github.com/recognizegroup/fiberworkz/actions/workflows/codeql-analysis.yaml/badge.svg)](https://github.com/recognizegroup/fiberworkz/actions/workflows/codeql-analysis.yaml)

[![Build](https://github.com/recognizegroup/fiberworkz/actions/workflows/ci.yaml/badge.svg)](https://github.com/recognizegroup/fiberworkz/actions/workflows/ci.yaml)

[![Build](https://github.com/recognizegroup/fiberworkz/actions/workflows/uitests.yaml/badge.svg)](https://github.com/recognizegroup/fiberworkz/actions/workflows/uitests.yaml)

### TODO: failed tests:

### Built With

* []() ...

## 🚀 Getting Started

To get a local copy up and running follow these simple steps.

### Development prerequisites

- [Download the Azure Cosmos DB Emulator](https://aka.ms/cosmosdb-emulator)
- Install azurite: `npm install -g azurite`, then `azurite` to run it before starting the backend.

- install the visual studio extension "SpecFlow for visual studio 2022"
   - for issues and discussions see: https://github.com/SpecFlowOSS/SpecFlow.VS/issues/
   - for documentation see : https://specflow.org/

## :rocket: Getting Started

### Installation

1. Clone the repository
   ```sh
   git clone git@github.com:recognizegroup/fiberworkz.git
   ```
2. Run `dotnet tool restore` in `fiberworkz` directory

### Terraform/Terragrunt

To run terragrunt locally

* Open powershell
* Set environment to use in variable
  ```
  $env:ENVIRONMENT = 'development'
  ```
* Login into azure (recognize tenant)
  ```
  az login
  ```
* Set correct subscription active (recognize tenant). In this example the id of volkerwessels-telecom-fiberworkz-dev is
  used.
  ```
  az account set --subscription=57399271-24ad-4ab8-b129-0fdd4ff386f7
  ```
* Goto terraform directory
  ```
   cd terraform
  ```
* Add environment variables
  ```
   TF_WORKING_DIR
   TF_CONTAINER_ACCESS_KEY
   TF_FRONTEND_ZIP_PATH
   TF_BACKEND_ZIP_PATH
  ```

* Run terragrunt
  ```
   terragrunt run-all apply
  ```

### Assigning roles to yourself

```sql
DECLARE @UserId uniqueidentifier;
SET @UserId = '...'; -- Insert your GUID from the Users table here
INSERT INTO UserRoles (Id, UserId, RoleId)
VALUES
(NEWID(), @UserId, '4654C46F-D5BA-46C4-A469-FB5B95CBF941'),
(NEWID(), @UserId, '408E8FC2-E506-45A1-963F-58C428FBE9F5'),
(NEWID(), @UserId, '590E26C7-3D79-4E0C-A4B6-FA70BB09E50E');
```

### Seeding schedules with unplanned tasks

Create an empty schedule, and ensure at least some connections have been selected to be planned. Then run the following script, after having changed the schedule id:

```sql
DECLARE @ScheduleId uniqueidentifier = 'Insert schedule id here';
DECLARE @TaskTypeId int = 8; -- Schouwen woning (tegelijk toestemming ophalen)

DECLARE @ProjectId uniqueidentifier = (SELECT ProjectId FROM Schedules WHERE Id = @ScheduleId);

INSERT INTO Tasks (ConnectionId, TaskTypeId, ProjectId, ScheduleId, Status)
SELECT Id, @TaskTypeId, @ProjectId, @ScheduleId, 0 FROM Connections WHERE ScheduleId = @ScheduleId;
```

You can now plan tasks in the planboard for the "Schouwen woning (tegelijk toestemming ophalen)" task type.

## :pencil2: Architecture

Visit [Lucid](https://lucid.app/lucidchart/769dcab5-46ad-4b67-90be-74c67a4dcc74/edit?invitationId=inv_ba682368-258e-4cd9-91c5-d6ed83470c02) for latest version of the design.

### Infra Architecture
![Architecture](architecture.png)

### Information Architecture & Capabilities 
![Architecture](InfoArchitecture.png)
