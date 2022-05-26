# Workflows

## [Storyboard Execution](https://www.notion.so/StoryBoard-Execution-0bd30d0542f642e5a5929565be98cc6b)
Stores the **data of the workflow**
- Captures participants and what happened
- When a user creates a workflow they actually create an execution

### Template

```json
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "id": "/1.0/entities/metadata/"firmnameExecution.json,
  "description": Description,
  "title": FirmName Execution,
  "type": "object",
  "allOf": [
    {
      "$ref": "/1.0/entities/metadata/entity.json"
    },
    {
      "$ref": "/1.0/entities/metadata/core_storyboard_execution.json"
    }
  ],
  "properties": {
     firmnameExecution: { // custom properties
      "properties": {
        status: { // common property
          "type": "string"
        },
        hasStarted: { // typical property
          "type": "boolean"
        },
        isCompleted: { // typical property
          "type": "boolean"
        },
        primaryIdentifier: { // common property - BOL/Shipping #...
          "type": "boolean"
        },
        documentEdge: { // typical property (connects to custom doc)
          "$ref": "/1.0/entities/metadata/entity.json#/definitions/edge",
          "label": "Document",
          "entityType": "/1.0/entities/metadata/entity.json"
        }
      },
      "type": "object"
    },
    "core_storyboard_execution": {
      "type": "object",
      "properties": {
        "name": {
          "$ref": "/1.0/entities/metadata/entity.json#/definitions/sequence",
          "type": "string",
          "isFilterable": true,
          "label": "Workflow number",
          "default": "Generating Workflow name",
          "placeholderSequence": "Generating Workflow name"
        },
        "id": {
          "$ref": "/1.0/entities/metadata/entity.json#/definitions/sequence",
          "type": "string",
          "default": "Generating Workflow name",
          "placeholderSequence": "Generating Workflow name"
        }
      }
    }
  },
  "uiSchema": {
    "mobile": {
      "entityCreationSection": {
        "children": [
          {
            "children": [
              {
                "formula": "'Generating Workflow name'",
                "isEditable": false,
                "isVisible": false,
                "type": "ui:computedProperty",
                "value": "core_storyboard_execution.name"
              },
              {
                "label": "GPS Coordinate",
                "type": "ui:geolocation",
                "value": "core_storyboard_execution.createdAt.geolocation"
              }
            ],
            "title": "Workflow Information",
            "type": "ui:section"
          },
          {
            "children": [
              {
                "formula": "core_storyboard_execution.name",
                "isEditable": false,
                "isVisible": false,
                "type": "ui:computedProperty",
                "value": "core_storyboard_execution.id"
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
            "conditionals": [
              {
                "action": "goNext()",
                "test": "isTimeout && !_.isEmpty(core_storyboard_execution.name)",
                "type": "ui:view"
              }
            ],
            "mapStateToProps": {
              "actionHooks": "actionHooks"
            },
            "timeoutMs": 1000,
            "type": "ui:conditional"
          }
        ],
        "mapStateToProps": {
          "title": "entityType"
        },
        "metadata": {
          "showGeolocation": true
        },
        "type": "ui:view"
      }
    },
    "web": {
      "components": {
        "tasksTab": {
          "type": "ui:view",
          "tabLabel": "Tasks",
          "disabled": {
            "type": "expression",
            "value": "entity.core_storyboard_execution.status === 'Canceled' || entity.core_storyboard_execution.status === 'Completed'"
          },
          "isGridContent": true,
          "children": [
            {
              "type": "ui:workflowManager",
              "mapActionsToProps": {
                "onClose": "onClose",
                "onStoryboardComplete": {
                  "type": "expression",
                  "value": "showToast('This workflow is now complete, returning to the work queue.').then(() => { returnToQueue(); })"
                },
                "onWorkflowCancel": {
                  "type": "expression",
                  "value": "setExtras('activeTabId', 1)"
                }
              }
            }
          ]
        },
        "detailsTab": {
          "type": "ui:view",
          "className": "grid-content pt3",
          "tabLabel": "Details",
          "children": [
            {
              "type": "ui:section",
              "title": "Workflow Details",
              "children": [
                {
                  "label": "Status",
                  "type": "ui:inputField",
                  "value": firmnameExecution.custom<status>Property,
                  "isDisabled": true
                },
                {
                  "label": "Started?",
                  "type": "ui:checkboxField",
                  "value": firmnameExecution.custom<hasStarted>Property,
                  "isDisabled": true
                },
                {
                  "label": "Completed?",
                  "type": "ui:checkboxField",
                  "value": firmnameExecution.custom<isComplete>Property,
                  "isDisabled": true
                },
                {
                  "label": "Posted Date",
                  "type": "ui:inputField",
                  "value": "creationDate",
                  "isDisabled": true
                },
                {
                  "type": "ui:section",
                  "title": "Shipment Details",
                  "children": [
                    {
                      "label": "Document ID", // typically have document(s)
                      "isDisabled": true,
                      "showInlinePreview": true,
                      "type": "ui:entitySelectField",
                      "value": firmnameExecution.<ref to tractor maintence document property>,
                      "inlinePreviewSchema": "uiSchema.web.entityPreviewConcise"
                    }
                  ]
                }
              ]
            }
          ]
        },
        "activityTab": {
          "type": "ui:view",
          "tabLabel": "Activity",
          "className": "grid-block br b--light-gray",
          "children": [
            {
              "type": "ui:feed",
              "legacyEventsToShow": [
                "11111111-0000-0000-0000-000000000006"
              ],
              "mapStateToProps": {
                "workflowEvents": "entity.core_storyboard_execution.events"
              }
            }
          ]
        }
      },
      "entityDetailCard": {
        "type": "ui:entityDetailCard",
        "removeDeleteButton": true,
        "showWorkflowCancelButton": {
          "type": "expression",
          "value": "entity.core_storyboard_execution.status !== 'Canceled'"
        },
        "mapActionsToProps": {
          "onClose": "onClose",
          "afterWorkflowCancel": {
            "type": "expression",
            "value": "showToast('This workflow has been cancelled'); setExtras('activeTabId', 1);"
          }
        },
        "mapStateToProps": {
          "entity": "entity"
        },
        "enableSchemaOverride": true,
        "header": {
          "type": "ui:entityHeader",
          "showOpenInNewTabButton": true,
          "children": [
            {
              "type": "ui:cardHeaderItem",
              "className": "u-overflowHidden",
              "description": {
                "type": "expression",
                "value": "entity.entityType"
              },
              "mapStateToProps": {
                "title": "entity.displayName"
              }
            },
            {
              "type": "ui:cardHeaderItem",
              "className": "c-cardHeader-item--grow u-overflowHidden",
              "description2": "Status",
              "mapStateToProps": {
                "title": "entity.core_storyboard_execution.status",
                "description": "description"
              }
            }
          ]
        },
        "children": [
          {
            "type": "ui:tabs",
            "className": "grid-block",
            "mapStateToProps": {
              "selectedTab": "extras.activeTabId"
            },
            "defaultTab": {
              "type": "expression",
              "value": "((entity.core_storyboard_execution.status === 'Canceled' || entity.core_storyboard_execution.status === 'Completed') && 1) || 0"
            },
            "children": [
              {
                "$ref": "#/uiSchema/web/components/tasksTab"
              },
              {
                "$ref": "#/uiSchema/web/components/detailsTab"
              },
              {
                "$ref": "#/uiSchema/web/components/activityTab"
              }
            ]
          }
        ]
      },
      "entityDetailCardBodyFullScreen": {
        "children": [
          {
            "children": [
              {
                "mapStateToProps": {
                  "selectedTab": "extras.activeTabId"
                },
                "defaultTab": {
                  "type": "expression",
                  "value": "((entity.core_storyboard_execution.status === 'Canceled' || entity.core_storyboard_execution.status === 'Completed') && 1) || 0"
                },
                "type": "ui:tabs",
                "children": [
                  {
                    "$ref": "#/uiSchema/web/components/tasksTab"
                  },
                  {
                    "$ref": "#/uiSchema/web/components/detailsTab"
                  }
                ]
              },
              {
                "type": "ui:tabs",
                "children": [
                  {
                    "$ref": "#/uiSchema/web/components/activityTab"
                  }
                ]
              }
            ],
            "defaultSize": "50%",
            "type": "ui:splitPane"
          }
        ],
        "className": "grid-block",
        "type": "ui:view"
      }
    }
  },
  "metadata": {
    "hasRevisionHistory": true,
    "namespace": firmnameExecution,
    "isSearchable": true,
    "autoCreate": true // allows workflow execution to be created without users doing anything
  },
  "uniqueId": UUID, // randomly generate
  "mixins": {
    "active": [
      {
        "entityId": "11111111-0000-0000-0000-000000000000",
        "displayName": "Entity"
      },
      {
        "entityId": "11111111-0000-0000-0000-000000000001",
        "displayName": "Entity Schema"
      }
    ],
    "inactive": []
  },
  "owner": {
    "firm": {
      "entityId": "00000000-0000-0000-0000-000000000000",
      "displayName": "Vector Firm"
    },
    "user": {
      "entityId": "11111111-1111-1111-1111-111111111111",
      "displayName": "System User"
    }
  }
}
```


---


## [Storyboard Plan](https://www.notion.so/Storyboard-Plan-4e53b867b86947058ab39a235d7220e9) (`core_storyboard_plan`)

The **tasks/work** to be performed by **users**
- Where we determine the user experience
- Who does what and what to do when waiting for another user

### Template

```json
{
  "core_storyboard_plan": {
    "execution": {
      "displayName": "",
      "entityId": ""
    },
    "id": "",
    "name": "",
    "permissions": [],
    "scenes": [],
    "serverComputedProperties": [],
    "settings": {
      "resumeOnActive": true/false
    },
    "stories": [],
    "tasks": []
  },
  "entity": {},
  "mixins": {
    "active": [
      {
        "displayName": "Entity",
        "entityId": "11111111-0000-0000-0000-000000000000"
      },
      {
        "displayName": "Storyboard Plan",
        "entityId": "0b770bb1-501c-4408-a477-fe409e9f5f6b"
      }
    ],
    "inactive": []
  },
  "owner": {
    "firm": {
      "entityId": "00000000-0000-0000-0000-000000000000",
      "displayName": "Vector Firm"
    },
    "user": {
      "entityId": "11111111-1111-1111-1111-111111111111",
      "displayName": "System User"
    }
  },
  "uniqueId": ""
}
```


### Basics

```json
"execution": { // Ties in Execution File
  "displayName": "Gs Trucking Tractor Execution", // Name of Execution File
  "entityId": "d3789c07-f111-42c3-9c55-19b89a6ed464" // Entity ID of Execution File
},
"id": "gsTrucking_tractor_plan", // StoryBoard Plan ID
"name": "Gs Trucking Tractor Plan", // StoryBoard Plan Name
"settings": {
  "resumeOnActive": "true/false" // For persistent workflows -- opens most recent active workflow
}
```

### [Permissions](https://www.notion.so/Permissions-82714ab8cf644446b46ca555c8db06b2)
Used to **define who has access to a specific story or plan**
- **Plan Level:** The user/firm can see ***ANY workflow***
- **Story Level:** The user/firm can see ***ONLY this workflow***

```json
"permissions": [ // Multiple objects within array are treated as OR statements
  {
    "firm": { // Anyone in this firm can access this story/plan
      "displayName": "G's Trucking",
      "entityId": "0fd4fef9-09f9-44d7-9647-8bce34a4ec02"
    }
  },
  { // Multiple objects within object are treated as AND statements
    "firm": { // This person must belong to the firm AND
      "displayName": "G's Trucking",
      "entityId": "0fd4fef9-09f9-44d7-9647-8bce34a4ec02"
    },
    "position": { // Have this specific position to access story/plan
      "entityId": "<POSITION UUID>",
      "displayName": "<POSITION NAME>"
    }
  },
]
```

### [Stories](https://www.notion.so/Stories-1711551846094e2fa2300685e7b4ca30)
Container of **scenes**, typically performed by a **single "actor"** and form a directed acyclic graph so there is a start and end with no loops (can be nested)
- An ***actor*** (driver, guard, clerk, etc.) can be completed by multiple people (multiple guards can complete the "guard story")
- A ***user*** should only have access to 1 story but can have multiple stories if needed


```json
"stories": [
  { // Single Story usually for a SINGLE ACTOR
   "id": "gsTrucking_driver_story", // Identifier of a User's story -- REQUIRED
   "name": "Gs Trucking Driver Story", // Name of the story
   "permissions": [ // Who can access this story -- REQUIRED
      {
        "firm": {
          "displayName": "G's Trucking",
          "entityId": "0fd4fef9-09f9-44d7-9647-8bce34a4ec02"
        },
      }
   ], 
   "platforms": ["mobile", "web", "all"], // Where this story is accessible -- 1 REQUIRED, Default is all
   "sceneRoutes": [ // Scenes of the story (in order) -- REQUIRED
     { // 1st Scene
       "sourceId": "__ROOT__", // Where you're COMING FROM -- REQUIRED (__ROOT__ will always be first entry)
       "destinations": [ // Where you're GOING TO -- REQUIRED
         {
           "entryPredicates": [], // If actor is allowed to enter this scene
           "exitPredicates": [], // What is needed to exit this scene
           "inputMappings": [],
           "name": "Driver Welcome Scene", // Name of Next Scene
           "pathId": "scene_welcome_driver" // Next Scene's SourceId
         },
       ]
     }
   ],
   "settings": {
     "resumeOnActive": "true/false"  // Is this workflow persistent? -- False
   }
  },
]
```

#### Story Template

```json
{
  "id": "",
  "name": "",
  "permissions": [
    {
      "firm": {
        "displayName": "",
        "entityId": ""
      },
    }
  ], 
  "platforms": ["mobile", "web", "all"],
  "sceneRoutes": [
    {
      "sourceId": "__ROOT__",
      "destinations": [ 
        {
          "entryPredicates": [],
          "exitPredicates": [],
          "inputMappings": [],
          "name": "",
          "pathId": ""
        },
      ]
    },
    {
      "sourceId": "",
      "destinations": [
        {
          "pathId": "__EXIT__"
        }
      ]
    }
  ],
  "settings": {
    "resumeOnActive": true/false
  }
}
```


### [Scenes](https://www.notion.so/Scenes-8b5667f5b5e54e58a78ebb476b4c4637)
Container of **tasks** that **consists of a single `uiView` which can break off into tasks**


```json
"scenes": [
  {
    "id": "pathId_from_sceneRoutes", // Identifier of the Scene (should match pathId from sceneRoutes) -- REQUIRED
    "name": "Name of Scene from sceneRoutes", // Name of the Scene (should match name from sceneRoutes)
    "isEntryCheckpoint": true, // Determines if a user can navigate back after ENTERING a view -- True allows users to click 'back' and go to entityList
    "isExitCheckpoint": false, // Determines if a user can navigate back after COMPLETING / EXITING a view
    "taskRoutes": [ // Top-level task routes for this scene (Removing creates a view only scene)
      { // First entry is starting task
        "sourceId": "__ROOT__", // Where you're COMING FROM -- REQUIRED (__ROOT__ will always be first entry)
        "destinations": [ // Where you're GOING TO -- REQUIRED
          {
            "inputMappings": [],
            "outputMappings": [],
            "name": "Enter BOL Task", // Name of task
            "pathId": "task_enter_bol_number" // Id of the task (will be sourceId of next taskRoute)
          }
        ]
      }
    ],
    "uiView": { // View for the scene (not required)
      "mobile": {
        "uiSchema": {
          "children": [], 
          "type": "ui:view"
        }
      }
    }
  }
]
```

#### Scene Template

```json
{
  "id": "",
  "name": "",
  "taskRoutes": [ 
    { 
      "sourceId": "__ROOT__",
      "destinations": [
        {
          "inputMappings": [],
          "outputMappings": [],
          "name": "",
          "pathId": ""
        }
      ],
    }
  ],
  "uiView": {
    "mobile": {
      "uiSchema": {
        "children": [], 
        "type": "ui:view"
      }
    }
  },
  "isEntryCheckpoint": true/false,
  "isExitCheckpoint": true/false
}
```


### [Tasks](https://www.notion.so/Tasks-31153029886f4d6fbcb3620f946d94cb)
Define the **actions** that can be performed **within a scene by the user** such as creating/editing entities
- Create new document
- Edit an existing doc
- Edit execution details

```json
{
  "actionType": "create/edit/share", // Type of action being performed -- REQUIRED
  "id": "", // Identifier of the Task (should match pathId from taskRoutes) -- REQUIRED
  "name": "", // Name of Task (should match name from taskRoutes)
  "uiView": { // View of this task (not required)
    "mobile": {
      "uiSchema": {
        "itemOptions": {
          "title": ""
          },
        "task'Create/Edit/Share'Section": {
          "children": [], 
          "type": "ui:view"
        },
        "validationSchema": {
          "$schema": "http://json-schema.org/draft-07/schema#",
          "properties": {},
          "required": [],
          "type": "object"
        }
      }
    }
  },
  "isEntryCheckpoint": true/false, // Determines if a user can navigate back after ENTERING a view -- True allows users to click 'back' and go to entityList
  "isExitCheckpoint": true/false // Determines if a user can navigate back after COMPLETING / EXITING a view
}
```

#### Task Template

```json
{
  "actionType": "",
  "id": "",
  "name": "",
  "uiView": {
    "mobile": {
      "uiSchema": {
        "itemOptions": {
          "title": ""
          },
        "taskCreateSection": {
          "children": [], 
          "type": "ui:view"
          },
        "validationSchema": {
          "$schema": "http://json-schema.org/draft-07/schema#",
          "properties": {},
          "required": [],
          "type": "object"
        }
      }
    }
  },
  "isEntryCheckpoint": true/false,
  "isExitCheckpoint": true/false
}
```



### [Server Computed Properties](https://www.notion.so/Server-Computed-Properties-f34fe59b1d104a438db33907154b3de9)
Place to **manage custom or out-of-the-box workflow statuses**
- `destination: ""` - the property to modify
- `options: []` - triggers that determine what to change `destination` property to
  - `conditional: ""`
  - `result: ""` - what the `destination` property maps out to be
  - *the code is read top down, so whenever one of the options evaluates to `true`, this is the new status that will be computed by the server*

```json
"serverComputedProperties": [
  { // controls the OUT-OF-THE-BOX "workflow status" property
    "destination": "core_storyboard_execution.status", 
    "options": [
      {
        "conditional": "$.[?(@.core_storyboard_execution.status == 'Canceled')]", // based on OUT-OF-THE-BOX "workflow status"
        "result": "Canceled"
      },
      {
        "conditional": "$.[?(@.conagraOutboundExecution.deliveryStatus == 'Completed')]", // based on CUSTOM "delivery status"
        "result": "Completed"
      },
      {
        "conditional": "$.[?(@.conagraOutboundExecution.hasStarted == true)]",
        "result": "Active"
      },
      {
        "conditional": "$", // no actions have taken place
        "result": "Pending" // "workflow status" gets set to Pending initially
      }
    ]
  },
  { // controls the CUSTOM "deliveryStatus" property
    "destination": "conagraOutboundExecution.deliveryStatus", 
    "options": [
      {
        "conditional": "$.[?(@.conagraOutboundExecution.isWorkflowCompleted == true)]",
        "result": "Completed"
      },
      {
        "conditional": "$.[?(@.conagraOutboundExecution.isLate == true)]",
        "result": "Rejected"
      },
      {
        "conditional": "$.[?(@.conagraOutboundExecution.hasPhotos == true)]",
        "result": "Awaiting Confirmation"
      },
      {
        "conditional": "$.[?(@.conagraOutboundExecution.pickupDoorAssigned == true)]",
        "result": "Dock Door / Yard Assigned"
      },
      {
        "conditional": "$.[?(@.conagraOutboundExecution.isEarly == false && @.conagraOutboundExecution.within60Miles == true )]",
        "result": "Active Check In"
      },
      {
        "conditional": "$",
        "result": "New"
      }
    ]
  },
]
```




```json
{
  "id": "scene_driver_appointment_details",
  "name": "Driver Appointment Details Scene",
  "taskRoutes": [
    {
      "destinations": [
        {
          "inputMappings": [
            {
              "destination": "entityId", // Used to manipulate Execution File / Properties
              "formula": "self.uniqueId"
            }
          ],
          "name": "Driver Appointment Details Task",
          "pathId": "task_driver_appointment_details"
        }
      ],
      "sourceId": "__ROOT__"
    }
  ]
},
{
  "id": "scene_driver_inbound_details",
  "name": "Driver Inbound Details Scene",
  "taskRoutes": [
    {
      "destinations": [
        {
          "inputMappings": [
            {
              "destination": "entityId", // Used to manipulate Document
              "value": "3b676d94-6f9e-4103-869a-ddc0d1b4e964" // UUID of Document being manipulated
            }
          ],
          "name": "Driver Inbound Details Task",
          "pathId": "task_driver_inbound_details"
        }
      ],
      "sourceId": "__ROOT__",
    }
  ]
},
```