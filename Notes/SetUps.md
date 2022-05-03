# SetUps

## [Creating Firms](https://paper.dropbox.com/doc/Vector-Basic-Firm-Configuration-BMVJ6hMFGGiqjhob3gLYL#:uid=509611726809676689400718&h2=Firms)

##### in SuperAdmin:
- **‘+’ —> ‘Firm Setup’**
  - `Firm Name`
  - `Email` - Vector employees can use to access Admin view
  - `Company Code` - used by a firm’s employees to join their fleet/account without needing to be invited

##### in Repo:
- **Create Firm Directory** —> `schemas/custom/firmName`

---

## [Creating AppBundles](https://paper.dropbox.com/doc/Vector-Basic-Firm-Configuration-BMVJ6hMFGGiqjhob3gLYL#:uid=050877809394612151869479&h2=App-Bundles)

##### in Repo:
- **Create AppBundle Directory + File** —> `schemas/custom/firmName/appBundle/appBundle.json`

##### in SuperAdmin:
- Navigate to **App Bundles** and locate firm's bundle
- Copy `JSON` and **paste into repo**
  - Can also copy/paste Boilerplate DMS Bundle from [JSON Schema Docs](https://paper.dropbox.com/doc/JSON-Schema-Documentation-aIDih1OVb3Gd0yXqg0aBd)) and manually fill out sections
    - `"firm.displayName"` - name of firm created
    - `"firm.entityId"` - UUID of firm found in SuperAdmin JSON
    - `"name"` - *firmName Bundle*
    - `"uniqueId"` - New/Random UUID

---

## [Creating Documents](https://paper.dropbox.com/doc/Vector-Basic-Firm-Configuration-BMVJ6hMFGGiqjhob3gLYL#:uid=501518658042055556634776&h2=Document-Types)

##### in SuperAdmin:
- Log into **Firm's User**
- Click on **User** --> **Account & Settings** --> **Document Types** --> **Create New Document**
-  Copy updated appBundle into repo

##### in Repo:
- **Create Documents Directory + File(s)** —> `schemas/custom/firmName/documents/newDoc.json`
- Copy/paste Boilerplate Schema v2 from [JSON Schema Docs](https://paper.dropbox.com/doc/JSON-Schema-Documentation-aIDih1OVb3Gd0yXqg0aBd)) and fill out sections
- After customizations are done, copy entire file

##### in SuperAdmin:
- Click on **Add Entity Schema** button (make sure you're logged in as SA)
- Paste code and click save

##### in Repo:
- Add new document to `appBundle.json`

##### in SuperAdmin:
- Replace appBundle JSON
