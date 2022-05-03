# Vector 101

---

## Research

#### Shippers
- Manufacturers (Clorox)

#### Receivers
- Retails (Home Depot)

#### Carriers
- Truckers

---

## Basics

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

## Tech

#### Branching
`git checkout -b GnolaVector/VD-####`

#### Commits
```
git commit
i
[VD-####] commit message
:wq
```

#### PRs
- Manually add # for Jira
- Add Reviewers (A,Ke,M,D,Ka)
- Share PR link to Slack SE Channel

---

## Languages

### Java
- Servers / Triggers
	
  IntegrationTypeModule.java
	Fires - JOB / AFTER

### React / React Native / Typescript
- Client

---

## Local Env

| Parameter                                                 | Type                           | 
| :-------------------------------------------------------- | :----------------------------- |
| `/usr/local/etc/elasticsearch-5.6.0/bin/elasticsearch -d` | Start ES                       |
| `ps -ef | grep elastic`                                   | Checks to see if ES is running |
| `psql <db_name>`                                          | Opens interactive terminal     |
| `\q`                                                      | Exits interactive terminal     |
| `cd Vector/vector/apps/server` then `./gradlew run`       | Starts Server                  |

---

### Resources
[JSON Docs](https://paper.dropbox.com/doc/JSON-Schema-Documentation-aIDih1OVb3Gd0yXqg0aBd)
[Vector Firm Config](https://www.notion.so/Vector-Basic-Firm-Configuration-efe087a5cade4215a8585b7c17a4b4fb)
