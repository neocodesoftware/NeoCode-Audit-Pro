# NeoCode-Audit-Pro 
- it's so good you can take a 'nap' -

FileMaker Pro NOW has a native, plug-in free way to audit changes to your database.  This is a Technology Preview of NeoCode-Audit-Pro tool. It logs inserts, updates, and deletes to a separate FileMaker file without use of plug-ins. It is developed and tested on FileMaker Pro 15-- older versions are not supported.

FILES
* Task_Audit.fmp12 - Sample FileMaker Starter Solution that has had NeoCode-Audit-Pro integrated.
* NAP_Service.fmp12 - The Audit engine that pulls the logged event data out of Task_Audit.fmp12 and stores the events.

## FEATURES & LIMITATIONS
What the files will and will not do...

### FEATURE HIGHLIGHTS
* Logs 'Audit Events' when users Insert, Update and Delete
* Audit Events are logged to memory; NAP_Service then intermittently commits events to disk.
* Supports cascading delete & related record creation.
* Supports record revert.
* Task_Audit.fmp12 events are logged in memory. 
* NAP_Service.fmp12 manages garbage collection while committing events to disk.
* Uses ExecuteSQL() to dynamically capture database schema; changes to database design are captured.
* Minimal integration steps (to be documented).

### LIMITATIONS
Known bugs, incomplete and unsupported features.  This is essentially our to-do list. Not in any particular order.

* No implementation documentation.
* Does not support shared deployment (single user).
* Does not support FileMaker Go, FileMaker WebDirect.
* Does not support server side data events (audits user events only).
* No error handling.
* Does not support multiple windows.
* Does not log insert/update events trigged by importing data.
* Does not log container data-- yet.
* Does not support repeating fields.
* Does not support commas in data.
* Does not log table name.
* No implementation of rollback. (Does not support audit recovery)



## EXAMPLE
Excerpt of the custom function that logs audit events into global variables.
```
Let ([
    ...
    //####    JSON     ####//
    ~json_new = "{" & ~json_sql_logtype & ~json_sql_pkid_name & ~Json_sql_pkid_value & ~json_sql_schema & ~json_sql_data & "}"
    ];
    Case (

    //##### DELETE #####//
    ~Action = "DELETE" ;
    _Aud_GV_Log_JSONData ( 1 ; ~json_new  ; ~LogCount ) &
    _Aud_GV_Log_Max ( 1 ; ~LogCount ) ;


    //##### INSERT #####//
    ~Rec_Open  = 1 ; //and ~Rec_CreateTS  ≠ ~Rec_ModTS  ;
    _Aud_GV_Log_JSONData ( 1 ; ~json_new  ; ~LogCount ) &
    _Aud_GV_Log_Max ( 1 ; ~LogCount  )  ;
   

    //##### UPDATE #####//
    ~Rec_Open   >=  2 and ~Rec_ModTS >= $$Rec_ModCnt ;
    _Aud_GV_Log_JSONData ( 1 ; ~json_new   ; ~LogCount ) &
    _Aud_GV_Log_Max ( 1 ; ~LogCount  ) ;
   
    //##### DEFAULT  #####//
    ""
    )  //END Case
   
) //END Let
```

## INSTALLATION
### Requirements
    FileMaker Pro 15
    Tasks_Audit.fmp12 (this would be replaced by your file)
    NAP_Service.fmp12 (this is where your audit events are stored)
   
### Credentials
Opens automatically with the following credentials-- record changes CANNOT be logged using Admin (Full Access) account.

#### Audit Enabled (must be enabled to log events)
* user: "NAPUser"
* pass: ""

#### Full Access  (does not log)
* user: "Admin"
* pass: ""


## TESTS
1. Open Tasks_Audit.fmp12
1. Insert|Update|Delete records
1. From file menu, select "Window > Show Windows > NeoCode Audit Pro - Service"
1. Click the button "Show Audit Log"

 
## USAGE

## REFERENCES
DataBuzz - http://www.databuzz.com.au/using-executesql-to-query-the-virtual-schemasystem-tables/

## Contributors
Paul Nelson
Joshua Paul

## LICENSE
MIT
