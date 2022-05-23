# Documents

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

---

## Document Breakdown

### Properties

```json
"properties": {
  "custom_FIRMNAME_documents_DOCTYPE": {
    "properties" : {}, // custom_FIRMNAME_documents_DOCTYPE.propertyName
    "type": "object"
  },
  "document": {
    "properties": {}, // document.attachments, document.name
    "required": [ "propertyName" ],
    "type": "object"
  }
}
```

#### Document Properties
- Inherited from `"$ref": "/1.0/entities/metadata/document.json"`
  - Also includes `recipients` and `location` properties (not shown in JSON)

```json
"document": {
  "properties": {
    "attachments": { // document.attachments
      "properties": {
        "files": { // attachments.files
          "minItems": 1, // must have 1 file minimum
          "type": "array"
        }
      },
      "required": [ "files"], // files must be included
      "type": "object"
    },
    "name": { // Load # - default label
      "isFilterable": true, // allows to be filtered
      "keyboardType": "numeric",
      "label": "", // used to overwrite Load # label
      "pattern": "", // custom RegExpt (OPTIONAL) to enforce input validation on format
      "type": "string"
    }
  },
  "required": [ "attachments" ], // attachments must be included
  "type": "object"
}
```

#### Custom Properties

```json
"custom_FIRMNAME_documents_DOCTYPE": { // Custom Properties for Custom Doc
  "properties" : {
    "customProperty": {
      "type": "array/boolean/intger/null/number/object/string", // REQUIRED
    }
  },
  "type": "object"
}
```

| Property       | Options                                          | Notes                | Required? |
| :------------- | :----------------------------------------------- | -------------------- | :-------: |
| `type`         | `array/boolean/intger/null/number/object/string` | Datatype of property | Yes       |
| `label`        | `""`                                             | Name of property     | No        |
| `isFilterable` | `true/false`                                     | Allows for filtering | No        |
| `isSortable`   | `true/false`                                     | Allows for sorting   | No        |


---

### UI Schema

```json
"uiSchema": {
  "mobile": {
    "entityCreationSection": {},
    "entityDetailSection": {}
  },
  "web": {
    "entityDetailCardBodyPreview": {},
    "entityDetailCardBodyFullScreen": {},
    "entityCreationSection": {},
    "entityDetailSection": {}
  }
}
```

### Mobile

#### Entity Create Section

```json
"entityCreationSection": {
  "children": [
    {
      "children": [ // Also includes Load # and Location
        { // Attachments
          "errorPath": "document.attachments",
          "type": "ui:fileInput",
          "value": "document.attachments.files"
        },
        { // Recipients
          "mapActionsToProps": {
            "onChange": "handleRecipientsChange"
          },
          "mapStateToProps": {
            "value": "recipients"
          },
          "type": "ui:recipientsField"
        }
      ],
      "type": "ui:section" // Default name is name of the document
    },
    {
      "children": [ // Add custom properties/components here
        {
          "type":"ui:", // type of component
          "value":"" // Property this maps to (custom_doc.customProp)
        }
      ],
      "title": "", // Custom Section Title
      "type": "ui:section" // Custom Section
    }
  ],
  "mapStateToProps": {
    "title": "entityType"
  },
  "metadata": {
    "enableAutoGrayscale": true,
    "enablePageDetection": true,
    "identifiers": [
      {
        "path": "document.name"
      }
    ]
  },
  "type": "ui:view"
},
```

![Mobile Create](../_assets/mobileBOLCreate.PNG)


#### Entity Detail Section

```json
"entityDetailSection": {
  "children": [
    { // Attachment(s) displayed at top
      "type": "ui:attachmentViewer",
      "value": "document.attachments"
    },
    { // Section #1
      "children": [
        { // Load #
          "type": "ui:inputField",
          "value": "document.name"
        },
        { // Doc Name / Type
          "label": "Document type",
          "mapStateToProps": {
            "value": "entityType"
          },
          "type": "ui:inputField"
        },
        { // Posted By (User)
          "label": "Posted By",
          "mapStateToProps": {
            "value": "createdBy.displayName"
          },
          "type": "ui:inputField"
        },
        { // Posted Date (Creation Date)
          "type": "ui:inputField",
          "value": "creationDate"
        },
        { // Received on (Date Uploaded)
          "type": "ui:inputField",
          "value": "document.uploadDate"
        },
        { // Location
          "label": "Location",
          "type": "ui:addressField",
          "value": "document.address"
        },
        { // GPS
          "label": "GPS",
          "type": "ui:locationField",
          "value": "document.address"
        }
      ],
      "title": "Load Information", // Section Title
      "type": "ui:section"
    }
  ],
  "type": "ui:view"
}
```

![Mobile Detail](../_assets/mobileBOLDetail.jpeg)

### Web

#### Entity Create Section

```json
"entityCreationSection": {
  "children": [
    {
      "type": "ui:formTable",
      "children": [ // Add Properties here
        { // Load Number
          "placeholder": "Load Number",
          "type": "ui:inputField",
          "value": "document.name"
        },
        { // Recipients
          "mapActionsToProps": {
            "onChange": "handleRecipientsChange"
          },
          "mapStateToProps": {
            "value": "recipients"
          },
          "type": "ui:recipientsField"
        },
        { // Attachments
          "inputClassName": "ph2",
          "isHorizontalLayout": true,
          "isMultiFile": true,
          "label": "Attachments",
          "type": "ui:fileField",
          "value": "document.attachments.files",
          "valuePath": "document.attachments.files"
        },
        { // Location
          "isDisabled": true,
          "label": "Location",
          "type": "ui:geolocationField",
          "value": "document.address.geolocation"
        },
        { // Custom Property
          label : customPropertyName,
          type : ui:XYZ,
          value: custom_FIRMNAM_documents_DOCTYPE.customPropertyName
        }
      ]
    }
  ],
  "className": "mv3 grid-content",
  "type": "ui:view"
},
```

![Web Create](../_assets/webBOLCreate.png)


#### Entity Detail Section

```json
"entityDetailSection": {
  "children": [
    { // Section #1
      "children": [
        {
          "type": "ui:formTable",
          "className": "u-bumperBottom",
          "children": [
            { // Load #
              "type": "ui:inputField",
              "value": "document.name"
            },
            { // Doc Type (BOL/Custom Doc Type)
              "label": "Document Type",
              "type": "ui:documentTypeSelectField",
              "value": "mixins"
            },
            { // Processed
              "type": "ui:checkboxField",
              "value": "document.processed"
            }
          ]
        },
        { // Still in Section #1 but has a line break between above
          "type": "ui:formTable",
          "children": [
            { // Posted By (User)
              "isDisabled": true,
              "showLinkIcon": false,
              "type": "ui:entitySelectField", // Disabled by default
              "value": "createdBy"
            },
            { // Posted Date (Creation Date)
              "isDisabled": true,
              "type": "ui:inputField", // Disabled by default
              "value": "creationDate"
            },
            { // Received on (Date Uploaded)
              "isDisabled": true,
              "type": "ui:inputField", // Disabled by default
              "value": "document.uploadDate"
            }
          ]
        }
      ],
      "isCollapsable": true,
      "title": "Document Details", // Section Title
      "type": "ui:section"
    },
    { // Section #2
      "children": [
        {
          "type": "ui:formTable",
          "children": [
            { // Location
              "isDisabled": true,
              "showEditableFields": false,
              "label": "Location",
              "type": "ui:addressField",
              "value": "document.address"
            },
            { // GPS
              "isDisabled": true, // Hidden by default
              "label": "GPS",
              "type": "ui:locationField",
              "value": "document.address.geolocation"
            }
          ]
        }
      ],
      "isCollapsable": true,
      "title": "Location", // Section Title
      "type": "ui:section"
    },
    { // Section #3
      "children": [ // Components
        { // Attachments
          "type": "ui:filesViewer",
          "isHorizontalLayout": false,
          "value": "document.attachments"
        }
      ],
      "title": "Attachments", // Section Title
      "hideHeaderBorder": false,
      "type": "ui:section"
    }
    // Add new Sections Here
    {
      "children":[], // Add custom components here
      "type": "ui:section"
    }
  ],
  "type": "ui:view"
}
```

![Web Detail](../_assets/webBOLDetail.png)

---

### UI Components

<table>
<tr>
<td> <strong>Component</strong> </td> <td> <strong>Code Snippet</strong> </td>  <td> <strong>Rendering</strong> </td> <td> <strong>Notes / Additional Properties</strong> </td>
</tr>

<tr>
  <td> <strong>Help Block</strong> </td>
  <td>

  ```json
  {
    "type": "ui:helpBlock", 
    "helptext": "This is what the..."
  }
  ```

  </td>
  <td>
    <img src="../_assets/helper.png"  >
  </td>
  <td></td>
</tr>

<tr>
  <td> <strong>Input Field</strong> </td>
  <td>

  ```json
  {
    "type": "ui:inputField"
  }
  ```

  </td>
  <td>
    <img src="../_assets/inputField.png">
  </td>
  <td>

  ```json
  {
    "value": "customDocName.propertyThisMapsTo", // REQUIRED
    "label": "", // What shows on lefthand side
    "placeholder": "", // What shows on righthand side
    "isDisabled": true/false, // allows for editing
    "autoCorrect": true/false, // MOBILE ONLY
    "autoFocus": true/false // ENTITY CREATION ONLY
  }
  ```

  </td>
</tr>

<tr>
  <td> <strong>Input Field (Multi-Row)</strong> </td>
  <td>

  ```json
  {
    "type": "ui:inputField", // Mobile
    "multiline": true // Mobile
    "type": "ui:textareaField", // Web
  }
  ```

  </td>
  <td>
    <img src="../_assets/multiRow.png">
  </td>
  <td>

  ```json
  {
    "value": "customDocName.propertyThisMapsTo" // REQUIRED
  }
  ```

  </td>
</tr>

<tr>
  <td> <strong>Section</strong> </td>
  <td>

  ```json
  {
    "type": "ui:section"
  }
  ```

  </td>
  <td>

  </td>
  <td>
  
  ```json
  {
    "title":"" // Section Title
  }
  ```

  </td>
</tr>

<tr>
  <td> <strong>Select</strong> </td>
  <td>

  ```json
  {
    "type": "ui:select", // Mobile
    "type": "ui:selectField" // Web
  }
  ```

  </td>
  <td>

  </td>
  <td>
  
  ```json
 
  ```

  </td>
</tr>

<tr>
  <td> <strong>Checkbox</strong> </td>
  <td>

  ```json
  {
    "type": "ui:switch", // Mobile
    "type": "ui:checkboxField" // Web
  }
  ```

  </td>
  <td>

  </td>
  <td>
  
  ```json
 
  ```

  </td>
</tr>

</table>

---

## Template

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "allOf": [
    {
      "$ref": "/1.0/entities/metadata/entity.json"
    },
    {
      "$ref": "/1.0/entities/metadata/document.json"
    }
  ],
  "description": Description,
  "entitySchema": {
  },
  "id": "/1.0/entities/metadata"/custom_FIRMNAME_documents_DOCTYPE.json,
  "metadata": {
    "hasRevisionHistory": true,
    "isAbstract": false,
    "isSearchable": true,
    "namespace": custom_FIRMNAME_documents_DOCTYPE
  },
  "mixins": {
    "active": [
      {
        "displayName": "Entity",
        "entityId": "11111111-0000-0000-0000-000000000000"
      },
      {
        "displayName": "Entity Schema",
        "entityId": "11111111-0000-0000-0000-000000000001"
      }
    ],
    "inactive": [
    ]
  },
  "owner": {
    "firm": {
      "displayName": Firm Display Name,
      "entityId": Firm UUID
    },
    "user": {
      "displayName": Username,
      "entityId": User UUID
    }
  },
  "properties": {
    custom_FIRMNAME_documents_DOCTYPE: {
      properties : {
        customPropertyName: {
          "type": array/boolean/intger/null/number/object/string,
          "label": Custom Property Name,
          "isFilterable":true,
          "isSortable":true
        }
      },
      "type": "object"
    },
    "document": {
      "properties": {
        "attachments": {
          "properties": {
            "files": {
              "minItems": 1,
              "type": "array"
            }
          },
          "required": [
            "files"
          ],
          "type": "object"
        },
        "name": { LOAD NUMBER
          "isFilterable": true,
          "keyboardType": "numeric",
          "label": <enter custom reference number label name, needed HERE to modify validation message>,
          "pattern": <enter custom RegExpt (OPTIONAL) to enforce input validation on load number format>,
          "type": "string"
        }
      },
      "required": [
        "attachments"
      ],
      "type": "object"
    }
  },
  "status": {
    "state": "new"
  },
  "title": Title of Document (what will show up),
  "type": "object",
  "uiSchema": {
    "mobile": {
      "entityCreationSection": {
        "children": [
          {
            "children": [
              {
                "errorPath": "document.attachments",
                "type": "ui:fileInput",
                "value": "document.attachments.files"
              },
              {
                "mapActionsToProps": {
                  "onChange": "handleRecipientsChange"
                },
                "mapStateToProps": {
                  "value": "recipients"
                },
                "type": "ui:recipientsField"
              }
            ],
            "type": "ui:section"
          },
          {
            "children": [
              {
                <add custom components here>: <mapped to various custom properties on this document entity>
              }
            ],
            "title": <enter the name of some custom section here>,
            "type": "ui:section"
          }
        ],
        "mapStateToProps": {
          "title": "entityType"
        },
        "metadata": {
          "enableAutoGrayscale": true,
          "enablePageDetection": true,
          "identifiers": [
            {
              "path": "document.name"
            }
          ]
        },
        "type": "ui:view"
      },
      "entityDetailSection": {
        "children": [
          {
            "type": "ui:attachmentViewer",
            "value": "document.attachments"
          },
          {
            "children": [
              {
                "type": "ui:inputField",
                "value": "document.name"
              },
              {
                "label": "Document type",
                "mapStateToProps": {
                  "value": "entityType"
                },
                "type": "ui:inputField"
              },
              {
                "label": "Posted By",
                "mapStateToProps": {
                  "value": "createdBy.displayName"
                },
                "type": "ui:inputField"
              },
              {
                "type": "ui:inputField",
                "value": "creationDate"
              },
              {
                "type": "ui:inputField",
                "value": "document.uploadDate"
              },
              {
                "label": "Location",
                "type": "ui:addressField",
                "value": "document.address"
              },
              {
                "label": "GPS",
                "type": "ui:locationField",
                "value": "document.address"
              }
            ],
            "title": "Load Information",
            "type": "ui:section"
          }
        ],
        "type": "ui:view"
      }
    },
    "web": {
      "entityDetailCardBodyPreview": {
        "children": [
          {
            "children": [
              {
                "children": [
                  {
                    "children": [
                      {
                        "$ref": "#/uiSchema/web/entityDetailSection"
                      }
                    ],
                    "className": "grid-content pt3",
                    "type": "ui:view"
                  }
                ],
                "className": "grid-block",
                "type": "ui:view"
              },
              {
                "type": "ui:shareBar"
              },
              {
                "mapStateToProps": {
                  "entity": "entity",
                  "errors": "errors",
                  "isVisible": "entity.isDirty"
                },
                "mapActionsToProps": {
                  "onCancelButtonClick": "handleRollback",
                  "onPrimaryButtonClick": "handleSaveEntity"
                },
                "type": "ui:entityFooter"
              }
            ],
            "className": "grid-block vertical u-overflowHidden",
            "tabLabel": "Details",
            "type": "ui:view"
          },
          {
            "children": [
              {
                "$ref": "/1.0/entities/metadata/entity.json#/uiSchema/web/entityActivitySection"
              }
            ],
            "tabLabel": "Activity",
            "className": "grid-block",
            "type": "ui:view"
          }
        ],
        "type": "ui:tabs"
      },
      "entityDetailCardBodyFullScreen": {
        "children": [
          {
            "children": [
              {
                "children": [
                  {
                    "children": [
                      {
                        "children": [
                          {
                            "children": [
                              {
                                "$ref": "#/uiSchema/web/entityDetailSection"
                              }
                            ],
                            "className": "grid-content pt3",
                            "type": "ui:view"
                          }
                        ],
                        "className": "grid-block",
                        "type": "ui:view"
                      },
                      {
                        "type": "ui:shareBar"
                      },
                      {
                        "mapStateToProps": {
                          "entity": "entity",
                          "errors": "errors",
                          "isVisible": "entity.isDirty"
                        },
                        "mapActionsToProps": {
                          "onCancelButtonClick": "handleRollback",
                          "onPrimaryButtonClick": "handleSaveEntity"
                        },
                        "type": "ui:entityFooter"
                      }
                    ],
                    "className": "grid-block vertical br b--light-gray u-overflowHidden",
                    "tabLabel": "Details",
                    "type": "ui:view"
                  },
                  {
                    "children": [
                      {
                        "$ref": "/1.0/entities/metadata/entity.json#/uiSchema/web/entityActivitySection"
                      }
                    ],
                    "tabLabel": "Activity",
                    "className": "grid-block br b--light-gray",
                    "type": "ui:view"
                  }
                ],
                "type": "ui:tabs"
              },
              {
                "children": [
                  {
                    "children": [
                      {
                        "children": [
                          {
                            "imagesPath": "document.attachments",
                            "type": "ui:imageEditorCarousel"
                          }
                        ],
                        "className": "grid-block u-bumperLeft--xl",
                        "type": "ui:view"
                      }
                    ],
                    "tabLabel": "Attachments",
                    "className": "grid-block",
                    "type": "ui:view"
                  }
                ],
                "type": "ui:tabs"
              }
            ],
            "primary": "first",
            "minSize": 500,
            "defaultSize": 500,
            "type": "ui:splitPane"
          }
        ],
        "className": "grid-block",
        "type": "ui:view"
      },
      "entityCreationSection": {
        "children": [
          {
            "type": "ui:formTable",
            "children": [ Add any Customizations here
              {
                "placeholder": "Load Number",
                "type": "ui:inputField",
                "value": "document.name"
              },
              {
                "mapActionsToProps": {
                  "onChange": "handleRecipientsChange"
                },
                "mapStateToProps": {
                  "value": "recipients"
                },
                "type": "ui:recipientsField"
              },
              {
                "inputClassName": "ph2",
                "isHorizontalLayout": true,
                "isMultiFile": true,
                "label": "Attachments",
                "type": "ui:fileField",
                "value": "document.attachments.files",
                "valuePath": "document.attachments.files"
              },
              {
                "isDisabled": true,
                "label": "Location",
                "type": "ui:geolocationField",
                "value": "document.address.geolocation"
              },
              {
                label : customPropertyName,
                type : ui:XYZ,
                value: custom_FIRMNAME_documents_DOCTYPE.customPropertyName
              }
            ]
          }
        ],
        "className": "mv3 grid-content",
        "type": "ui:view"
      },
      "entityDetailSection": {
        "children": [
          {
            "children": [
              {
                "type": "ui:formTable",
                "className": "u-bumperBottom",
                "children": [
                  {
                    "type": "ui:inputField",
                    "value": "document.name"
                  },
                  {
                    "label": "Document Type",
                    "type": "ui:documentTypeSelectField",
                    "value": "mixins"
                  },
                  {
                    "type": "ui:checkboxField",
                    "value": "document.processed"
                  }
                ]
              },
              {
                "type": "ui:formTable",
                "children": [
                  {
                    "isDisabled": true,
                    "showLinkIcon": false,
                    "type": "ui:entitySelectField",
                    "value": "createdBy"
                  },
                  {
                    "isDisabled": true,
                    "type": "ui:inputField",
                    "value": "creationDate"
                  },
                  {
                    "isDisabled": true,
                    "type": "ui:inputField",
                    "value": "document.uploadDate"
                  }
                ]
              }
            ],
            "isCollapsable": true,
            "title": "Document Details",
            "type": "ui:section"
          },
          {
            "children": [
              {
                "type": "ui:formTable",
                "children": [
                  {
                    "isDisabled": true,
                    "showEditableFields": false,
                    "label": "Location",
                    "type": "ui:addressField",
                    "value": "document.address"
                  },
                  {
                    "isDisabled": true,
                    "label": "GPS",
                    "type": "ui:locationField",
                    "value": "document.address.geolocation"
                  }
                ]
              }
            ],
            "isCollapsable": true,
            "title": "Location",
            "type": "ui:section"
          },
          {
            "children": [
              {
                "type": "ui:filesViewer",
                "isHorizontalLayout": false,
                "value": "document.attachments"
              }
            ],
            "title": "Attachments",
            "hideHeaderBorder": false,
            "type": "ui:section"
          }
        ],
        "type": "ui:view"
      }
    }
  },
  "uniqueId": New UUID
}
```