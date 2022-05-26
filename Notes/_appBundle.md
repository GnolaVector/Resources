# AppBundles

### [Creating AppBundles](https://paper.dropbox.com/doc/Vector-Basic-Firm-Configuration-BMVJ6hMFGGiqjhob3gLYL#:uid=050877809394612151869479&h2=App-Bundles)

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

### Template

```json
{
  "applicationBundle": {
    "extends": [
      {
        "displayName": "Base Bundle",
        "entityId": "11111111-0000-0000-0000-200000000000"
      },
      {
        "displayName": "DMS Bundle",
        "entityId": "11111111-0000-0000-0000-200000000001"
      }
    ],
    "firm": {
      "displayName": Firm Display Name,
      "entityId": Firm UUID (from SuperAdmin)
    },
    "name": Firm Name Bundle,
    "schemas": {
      "/1.0/entities/metadata/billOfLading.json": {
        "_operation": "DELETE",
        "entityId": "11111111-0000-0000-0000-000000000026"
      },
      "/1.0/entities/metadata/freightBill.json": {
        "_operation": "DELETE",
        "entityId": "11111111-0000-0000-0000-000000000039"
      },
      "/1.0/entities/metadata/fuelAdvance.json": {
        "_operation": "DELETE",
        "entityId": "11111111-0000-0000-0000-000000000040"
      },
      "/1.0/entities/metadata/invoice.json": {
        "_operation": "DELETE",
        "entityId": "11111111-0000-0000-0000-000000000051"
      },
      "/1.0/entities/metadata/lumperReceipt.json": {
        "_operation": "DELETE",
        "entityId": "11111111-0000-0000-0000-000000000055"
      },
      "/1.0/entities/metadata/map.json": {
        "_operation": "DELETE",
        "entityId": "11111111-0000-0000-0000-000000000059"
      },
      "/1.0/entities/metadata/osd.json": {
        "_operation": "DELETE",
        "entityId": "11111111-0000-0000-0000-000000000064"
      },
      "/1.0/entities/metadata/other.json": {
        "entityId": "11111111-0000-0000-0000-000000000065",
        "_operation": "DELETE"
      },
      "/1.0/entities/metadata/photograph.json": {
        "entityId": "11111111-0000-0000-0000-000000000077",
        "_operation": "DELETE"
      },
      "/1.0/entities/metadata/pickTicket.json": {
        "_operation": "DELETE",
        "entityId": "11111111-0000-0000-0000-000000000078"
      },
      "/1.0/entities/metadata/proofOfDelivery.json": {
        "entityId": "11111111-0000-0000-0000-000000000080",
        "_operation": "DELETE"
      },
      "/1.0/entities/metadata/purchaseOrder.json": {
        "_operation": "DELETE",
        "entityId": "11111111-0000-0000-0000-000000000081"
      },
      "/1.0/entities/metadata/rateConfirmation.json": {
        "_operation": "DELETE",
        "entityId": "11111111-0000-0000-0000-000000000082"
      },
      "/1.0/entities/metadata/tankWashTicket.json": {
        "_operation": "DELETE",
        "entityId": "11111111-0000-0000-0000-000000000090"
      },
      "/1.0/entities/metadata/threePrior.json": {
        "_operation": "DELETE",
        "entityId": "11111111-0000-0000-0000-000000000092"
      },
      "/1.0/entities/metadata/weightTag.json": {
        "_operation": "DELETE",
        "entityId": "11111111-0000-0000-0000-000000000101"
      },
      /1.0/entities/metadata/customDocumentName.json: {
        "orderPriority": #,
        "entityId": customUUID,
        "displayName": documentName
      }
    }
  },
  "mixins": {
    "active": [
      {
        "displayName": "Entity",
        "entityId": "11111111-0000-0000-0000-000000000000"
      },
      {
        "displayName": "Application Bundle",
        "entityId": "11111111-0000-0000-0000-000000000211"
      }
    ],
    "inactive": []
  },
  "owner": {
    "firm": {
      "displayName": "Vector Firm",
      "entityId": "00000000-0000-0000-0000-000000000000"
    },
    "user": {
      "displayName": "Gianni Nola",
      "entityId": "7f0f396e-69c3-4774-a077-66f8ce629e3a"
    }
  },
  "precomputation": {
    "displayName": Bundle Name Here
  },
  "uniqueId": New UUID
}
```