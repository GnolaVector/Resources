# Vector 101

## Terminology

#### Entities
- Just about everything and each has its own UUID

#### Firms
- Companies (can also be divisions or business units)
- Has their own packets, doc types, users, views, custom workflows…

#### Users
- Members / Managers / Admins / Super Admins (Vector employees)
- **Adding Users:**
  - **Members** - can self register using a company code
  - **Managers/Admins** - must be created by invitation

#### App Bundles
- Contain data about the various doc type schemas that are available to users

#### Documents
- Each standard Document Type consists of:
  - Load Number
  - Recipients
  - Attachments
  - Location

#### Packets
- Combo documents

---

## Industry Knowledge

### Shippers
- Manufacturers (Clorox)

### Receivers
- Retails (Home Depot)

### Carriers
- Truckers

### Parcels
- Small/Med packages
- FedEx / UPS / USPS / DHL

---


## Operations

### Phases

#### Proposal / Discovery
- 1 to 2 meetings to determine if/how our product will fit business use-cases
- Which of the technical asks can be tackled easily vs need more discussion
- Notes gathered for testing and follow-ups

#### Statement of Work (SoW)
- Note compiled from meetings in discovery phase are transformed into a [SoW](https://docs.google.com/document/d/1nzP1jvlkPpHBGoVI9nww-zGJ9EhdNzp0jXryYEkNHHw/edit)
- Purpose is to:
  - Provide background info to understand the solution
  - Describe current process/tech and proposed process with outlined goals/objectives
  - Define scope of work (detailed steps at semi0technical level)


#### Work


#### Wrap-up


---

## Tech

**Front End:** React / React Native / Typescript
**Back End:** Java / Elastic Search


### Java
- Servers
- Fires - JOB / AFTER

#### Triggers
- Anything that happens on document upload
  - Custom FTPs are mainly for changes to filename or # of files to send
	
`IntegrationTypeModule.java`
- Where you add imports after completing new java files

`.yaml` = Config file (Firm account settings)
  - Host / UN / PW / FirmId
  - Port (22 is standard for SFTP)


---

## SetUps

### [Creating Firms](https://paper.dropbox.com/doc/Vector-Basic-Firm-Configuration-BMVJ6hMFGGiqjhob3gLYL#:uid=509611726809676689400718&h2=Firms)

##### in SuperAdmin:
- **‘+’ —> ‘Firm Setup’**
  - `Firm Name`
  - `Email` - Vector employees can use to access Admin view
  - `Company Code` - used by a firm’s employees to join their fleet/account without needing to be invited

##### in Repo:
- **Create Firm Directory** —> `schemas/custom/firmName`


### Vector Connector
Capability to connect a customer’s On Premise TMS database

#### Installation

1. Follow instructions from [here](https://docs.google.com/document/d/1H1EdyiMCpDAQq0NL4fc5UQw_Z_ChpkWXPN9UUsYFB8c/edit)


### API Docs

#### Step 1: Copy Template

- Make a Copy of the [Template](https://docs.google.com/document/d/1ZdTn74mpghJ9nzJT9_F232Ncci00Ev2GlzjPBKJj8qk/edit#heading=h.nfy1z7aiot4g)

#### Step 2: Crete User in Super Admin
- Log into Super Admin
- Create User 
  - **First Name:** *API*
  - **Last Name:** *USER*
  - **Firm:** Select correct Firm
  - **Email:** *firmnamapiuser@withvector.com*
  - **Password:** Something random (dont want to be able to guess)

#### Step 3: Grab Auth Token
- Open an *incognito window*
- Log in to Firm with *API USER* Credentials
- Open Dev Tools and navigate to ***Network*** tab
- Find any `query` and click on it
- Navigate to ***Headers*** tab
- Copy **token** from `authorization: Bearer`

#### Step 4: Replace Firm and Auth Bearer Tokens
- Replace Firm Names with correct name (2)
- Replace Auth Bearer tokens (3?)
- Exit out of Incognito Window **WITHOUT SIGNING OUT**

---

## Local Env

| Parameter                                                       | Type                                | 
| :-------------------------------------------------------------- | :---------------------------------- |
| `/usr/local/etc/elasticsearch-5.6.0/bin/elasticsearch -d`       | Start ES                            |
| `ps -ef | grep elastic`                                         | Checks to see if ES is running      |
| `psql <db_name>`                                                | Opens interactive psql terminal     |
| `\q`                                                            | Exits interactive psql terminal     |
| `cd Vector/vector/apps/server` then `./gradlew run`             | Starts Server                       |
| `cd Vector/vector/apps/webapp` then `npm run watch:local`       | Starts Server                       |

---

## Git

### Branching
`git checkout -b GnolaVector/VD-####`

### Commits
```
git commit
i
[VD-####] commit message
:wq
```

### PRs
- Manually add # for Jira
- Add Reviewers (A,Ke,M,D,Ka)
- Share PR link to Slack SE Channel

---

### Resources
- [Operational Guide](https://docs.google.com/document/d/1zI_mweHRnuT32f5zgsHe7x8ZB1UvBUtBaCmhuwlIMtk/edit#)
- [JSON Docs](https://paper.dropbox.com/doc/JSON-Schema-Documentation-aIDih1OVb3Gd0yXqg0aBd)
- [Vector Firm Config](https://www.notion.so/Vector-Basic-Firm-Configuration-efe087a5cade4215a8585b7c17a4b4fb)