---
title: Checking user access for a module
---
This document describes how the system determines if a user can access a specific module. The process begins with a request to check access, retrieves the module's permissions and the user's group from the database, and then makes an access decision based on these factors.

# Where is this flow used?

This flow is used multiple times in the codebase as represented in the following diagram:

```mermaid
graph TD;
      632cdedb2fef9ac1425b7f13b33b1b5e9f74c8b16bfbc45e731b64aa59f02104(Form/frmUserChangePassword.frm::Form_Unload) --> 943db170f902b17f3e7c1fcc2d49aca7f82ea5a5eb3bc7ea246cc6f77df93d85(Module/modFunction.bas::UserAccessModule):::mainFlowStyle

6d5dfaa1535df03c2e73af44d3d5f08664c1a96b6cef4ca1650ab07ed9b64539(Form/frmUserChangePassword.frm::cmdOK_Click) --> 943db170f902b17f3e7c1fcc2d49aca7f82ea5a5eb3bc7ea246cc6f77df93d85(Module/modFunction.bas::UserAccessModule):::mainFlowStyle

f9ae32f25f27c084655bcf4815d4a37946a485e140be3fc266b2ccd63a052a14(Form/frmUserLogin.frm::cmdOK_Click) --> 943db170f902b17f3e7c1fcc2d49aca7f82ea5a5eb3bc7ea246cc6f77df93d85(Module/modFunction.bas::UserAccessModule):::mainFlowStyle


classDef mainFlowStyle color:#000000,fill:#7CB9F4
classDef rootsStyle color:#000000,fill:#00FFF4
classDef Style1 color:#000000,fill:#00FFAA
classDef Style2 color:#000000,fill:#FFFF00
classDef Style3 color:#000000,fill:#AA7CB9

%% Swimm:
%% graph TD;
%%       632cdedb2fef9ac1425b7f13b33b1b5e9f74c8b16bfbc45e731b64aa59f02104(<SwmPath>[Form/frmUserChangePassword.frm](Form/frmUserChangePassword.frm)</SwmPath>::Form_Unload) --> 943db170f902b17f3e7c1fcc2d49aca7f82ea5a5eb3bc7ea246cc6f77df93d85(<SwmPath>[Module/modFunction.bas](Module/modFunction.bas)</SwmPath>::<SwmToken path="Module/modFunction.bas" pos="84:4:4" line-data="Public Function UserAccessModule(intModuleID As Integer, Optional strUserID As String = &quot;&quot;) As Boolean">`UserAccessModule`</SwmToken>):::mainFlowStyle
%% 
%% 6d5dfaa1535df03c2e73af44d3d5f08664c1a96b6cef4ca1650ab07ed9b64539(<SwmPath>[Form/frmUserChangePassword.frm](Form/frmUserChangePassword.frm)</SwmPath>::cmdOK_Click) --> 943db170f902b17f3e7c1fcc2d49aca7f82ea5a5eb3bc7ea246cc6f77df93d85(<SwmPath>[Module/modFunction.bas](Module/modFunction.bas)</SwmPath>::<SwmToken path="Module/modFunction.bas" pos="84:4:4" line-data="Public Function UserAccessModule(intModuleID As Integer, Optional strUserID As String = &quot;&quot;) As Boolean">`UserAccessModule`</SwmToken>):::mainFlowStyle
%% 
%% f9ae32f25f27c084655bcf4815d4a37946a485e140be3fc266b2ccd63a052a14(<SwmPath>[Form/frmUserLogin.frm](Form/frmUserLogin.frm)</SwmPath>::cmdOK_Click) --> 943db170f902b17f3e7c1fcc2d49aca7f82ea5a5eb3bc7ea246cc6f77df93d85(<SwmPath>[Module/modFunction.bas](Module/modFunction.bas)</SwmPath>::<SwmToken path="Module/modFunction.bas" pos="84:4:4" line-data="Public Function UserAccessModule(intModuleID As Integer, Optional strUserID As String = &quot;&quot;) As Boolean">`UserAccessModule`</SwmToken>):::mainFlowStyle
%% 
%% 
%% classDef mainFlowStyle color:#000000,fill:#7CB9F4
%% classDef rootsStyle color:#000000,fill:#00FFF4
%% classDef Style1 color:#000000,fill:#00FFAA
%% classDef Style2 color:#000000,fill:#FFFF00
%% classDef Style3 color:#000000,fill:#AA7CB9
```

# Checking User Permissions for a Module

```mermaid
%%{init: {"flowchart": {"defaultRenderer": "elk"}} }%%
flowchart TD
    node1["Request user access for module"] --> node2["Opening the Database Connection and Logging Errors"]
    click node1 openCode "Module/modFunction.bas:84:91"
    
    node2 --> node3{"Is there access data for the module?"}
    click node3 openCode "Module/modFunction.bas:92:95"
    node3 -->|"Yes"| node4{"Is there user data for the user?"}
    click node4 openCode "Module/modFunction.bas:107:111"
    node3 -->|"No"| node5["Access denied (return False)"]
    click node5 openCode "Module/modFunction.bas:101:104"
    node4 -->|"Yes"| node6{"Which group does the user belong to?"}
    click node6 openCode "Module/modFunction.bas:112:119"
    node4 -->|"No"| node5
    node6 -->|"Group 1"| node7["Return Group 1 access flag"]
    node6 -->|"Group 2"| node7["Return Group 2 access flag"]
    node6 -->|"Group 3"| node7["Return Group 3 access flag"]
    node6 -->|"Group 4"| node7["Return Group 4 access flag"]
    click node7 openCode "Module/modFunction.bas:113:119"

classDef HeadingStyle fill:#777777,stroke:#333,stroke-width:2px;
click node2 goToHeading "Opening the Database Connection and Logging Errors"
node2:::HeadingStyle

%% Swimm:
%% %%{init: {"flowchart": {"defaultRenderer": "elk"}} }%%
%% flowchart TD
%%     node1["Request user access for module"] --> node2["Opening the Database Connection and Logging Errors"]
%%     click node1 openCode "<SwmPath>[Module/modFunction.bas](Module/modFunction.bas)</SwmPath>:84:91"
%%     
%%     node2 --> node3{"Is there access data for the module?"}
%%     click node3 openCode "<SwmPath>[Module/modFunction.bas](Module/modFunction.bas)</SwmPath>:92:95"
%%     node3 -->|"Yes"| node4{"Is there user data for the user?"}
%%     click node4 openCode "<SwmPath>[Module/modFunction.bas](Module/modFunction.bas)</SwmPath>:107:111"
%%     node3 -->|"No"| node5["Access denied (return False)"]
%%     click node5 openCode "<SwmPath>[Module/modFunction.bas](Module/modFunction.bas)</SwmPath>:101:104"
%%     node4 -->|"Yes"| node6{"Which group does the user belong to?"}
%%     click node6 openCode "<SwmPath>[Module/modFunction.bas](Module/modFunction.bas)</SwmPath>:112:119"
%%     node4 -->|"No"| node5
%%     node6 -->|"Group 1"| node7["Return Group 1 access flag"]
%%     node6 -->|"Group 2"| node7["Return Group 2 access flag"]
%%     node6 -->|"Group 3"| node7["Return Group 3 access flag"]
%%     node6 -->|"Group 4"| node7["Return Group 4 access flag"]
%%     click node7 openCode "<SwmPath>[Module/modFunction.bas](Module/modFunction.bas)</SwmPath>:113:119"
%% 
%% classDef HeadingStyle fill:#777777,stroke:#333,stroke-width:2px;
%% click node2 goToHeading "Opening the Database Connection and Logging Errors"
%% node2:::HeadingStyle
```

<SwmSnippet path="/Module/modFunction.bas" line="84">

---

In <SwmToken path="Module/modFunction.bas" pos="84:4:4" line-data="Public Function UserAccessModule(intModuleID As Integer, Optional strUserID As String = &quot;&quot;) As Boolean">`UserAccessModule`</SwmToken>, we start by figuring out which user we're checking (using the provided ID or the global one), then we prep to pull access rights for the module from the database. We need to call <SwmToken path="Module/modFunction.bas" pos="93:1:1" line-data="    OpenDB">`OpenDB`</SwmToken> next because the access checks depend on reading from the database tables (<SwmToken path="Module/modFunction.bas" pos="91:12:12" line-data="    strSQL = &quot;SELECT * FROM ModuleAccess&quot;">`ModuleAccess`</SwmToken> and <SwmToken path="Module/modFunction.bas" pos="108:13:13" line-data="    strSQL = strSQL &amp; &quot; FROM UserData&quot;">`UserData`</SwmToken>).

```visual basic
Public Function UserAccessModule(intModuleID As Integer, Optional strUserID As String = "") As Boolean
    Const mstrMethod As String = "UserAccessModule"
    Dim rst As ADODB.Recordset
    Dim strSQL As String
    Dim blnGroup(1 To 4) As Boolean
On Error GoTo CheckErr
    If strUserID = "" Then strUserID = gstrUserID
    strSQL = "SELECT * FROM ModuleAccess"
    strSQL = strSQL & " WHERE ModuleID = " & intModuleID
    OpenDB
    Set rst = OpenRS(strSQL)
    If Not rst.EOF Then
        blnGroup(1) = rst!Group1
        blnGroup(2) = rst!Group2
        blnGroup(3) = rst!Group3
        blnGroup(4) = rst!Group4
    Else
        blnGroup(1) = False
        blnGroup(2) = False
        blnGroup(3) = False
        blnGroup(4) = False
    End If
    CloseRS rst
    strSQL = "SELECT UserGroup"
    strSQL = strSQL & " FROM UserData"
    strSQL = strSQL & " WHERE UserID = '" & strUserID & "'"
    Set rst = OpenRS(strSQL)
    If Not rst.EOF Then
        If rst!UserGroup = 1 Then
            UserAccessModule = blnGroup(1)
        ElseIf rst!UserGroup = 2 Then
            UserAccessModule = blnGroup(2)
        ElseIf rst!UserGroup = 3 Then
            UserAccessModule = blnGroup(3)
```

---

</SwmSnippet>

## Opening the Database Connection and Logging Errors

```mermaid
%%{init: {"flowchart": {"defaultRenderer": "elk"}} }%%
flowchart TD
    node1["Attempt to open hotel database using path and password"]
    click node1 openCode "Module/modDatabase.bas:26:30"
    node1 --> node2{"Was connection successful?"}
    click node2 openCode "Module/modDatabase.bas:30:32"
    node2 -->|"Yes"| node3["Database connection established"]
    click node3 openCode "Module/modDatabase.bas:31:31"
    node2 -->|"No"| node4["Show error message with error number and description"]
    click node4 openCode "Module/modDatabase.bas:33:33"
    node4 --> node5["Log error details"]
    click node5 openCode "Module/modDatabase.bas:34:34"

classDef HeadingStyle fill:#777777,stroke:#333,stroke-width:2px;

%% Swimm:
%% %%{init: {"flowchart": {"defaultRenderer": "elk"}} }%%
%% flowchart TD
%%     node1["Attempt to open hotel database using path and password"]
%%     click node1 openCode "<SwmPath>[Module/modDatabase.bas](Module/modDatabase.bas)</SwmPath>:26:30"
%%     node1 --> node2{"Was connection successful?"}
%%     click node2 openCode "<SwmPath>[Module/modDatabase.bas](Module/modDatabase.bas)</SwmPath>:30:32"
%%     node2 -->|"Yes"| node3["Database connection established"]
%%     click node3 openCode "<SwmPath>[Module/modDatabase.bas](Module/modDatabase.bas)</SwmPath>:31:31"
%%     node2 -->|"No"| node4["Show error message with error number and description"]
%%     click node4 openCode "<SwmPath>[Module/modDatabase.bas](Module/modDatabase.bas)</SwmPath>:33:33"
%%     node4 --> node5["Log error details"]
%%     click node5 openCode "<SwmPath>[Module/modDatabase.bas](Module/modDatabase.bas)</SwmPath>:34:34"
%% 
%% classDef HeadingStyle fill:#777777,stroke:#333,stroke-width:2px;
```

<SwmSnippet path="/Module/modDatabase.bas" line="23">

---

In <SwmToken path="Module/modDatabase.bas" pos="23:4:4" line-data="Public Sub OpenDB()">`OpenDB`</SwmToken>, we set up and open the database connection using the configured path and password. If anything fails, we log the error to a text file by calling <SwmToken path="Module/modDatabase.bas" pos="34:1:1" line-data="    LogErrorText &quot;Error&quot;, mstrMethod, Err.Description">`LogErrorText`</SwmToken> in the next module.

```visual basic
Public Sub OpenDB()
Const mstrMethod As String = "OpenDB"
On Error GoTo CheckErr
    Set ACN = New ADODB.Connection
    ACN.Provider = "Microsoft.Jet.OLEDB.4.0"
    ACN.ConnectionString = "Data Source=" & gstrDatabasePath
    ACN.Properties("Jet OLEDB:Database Password") = gstrPassword
    ACN.Open
    Exit Sub
CheckErr:
    MsgBox Err.Number & " - " & Err.Description, vbExclamation, mstrMethod
    LogErrorText "Error", mstrMethod, Err.Description
```

---

</SwmSnippet>

<SwmSnippet path="/Module/modTextFile.bas" line="34">

---

<SwmToken path="Module/modTextFile.bas" pos="34:4:4" line-data="Public Sub LogErrorText(FileName As String, pstrNote As String, Optional pstrError As String)">`LogErrorText`</SwmToken> appends error details (timestamp, note, and error message) to a text file in the app directory. If logging itself fails, it pops up a message box with the error.

```visual basic
Public Sub LogErrorText(FileName As String, pstrNote As String, Optional pstrError As String)
On Error GoTo SE
    Open App.Path & "\" & FileName & ".txt" For Append As #1
    If pstrError = "" Then
        Print #1, vbCrLf & FormatDateAndTime(Now) & vbCrLf & pstrNote
    Else
        Print #1, vbCrLf & FormatDateAndTime(Now) & vbCrLf & pstrNote & vbCrLf & pstrError
    End If
    Close #1
    Exit Sub
SE:
    MsgBox "Error #" & Err.Number & vbCrLf & Err.Description, vbExclamation, App.Title
End Sub
```

---

</SwmSnippet>

<SwmSnippet path="/Module/modDatabase.bas" line="35">

---

We just came back from <SwmToken path="Module/modFunction.bas" pos="338:1:1" line-data="    LogErrorText &quot;Error&quot;, mstrMethod, Err.Description">`LogErrorText`</SwmToken> in <SwmToken path="Module/modFunction.bas" pos="93:1:1" line-data="    OpenDB">`OpenDB`</SwmToken>. After logging the error, the function simply ends—no retries or extra handling here.

```visual basic
    'LogErrorDB "Sub", mstrModule, mstrMethod, Err.Number, Err.Description
End Sub
```

---

</SwmSnippet>

## Fetching User Group and Returning Access Decision

<SwmSnippet path="/Module/modFunction.bas" line="118">

---

We just got back from <SwmToken path="Module/modFunction.bas" pos="93:1:1" line-data="    OpenDB">`OpenDB`</SwmToken> in <SwmToken path="Module/modFunction.bas" pos="119:1:1" line-data="            UserAccessModule = blnGroup(4)">`UserAccessModule`</SwmToken>. Now, we check the user's group and return the corresponding group permission. If the user isn't found, access is denied. We close the recordset with <SwmToken path="Module/modFunction.bas" pos="124:1:1" line-data="    CloseRS rst">`CloseRS`</SwmToken> next to clean up resources.

```visual basic
        Else ' rst!UserGroup = 4
            UserAccessModule = blnGroup(4)
        End If
    Else
        UserAccessModule = False
    End If
    CloseRS rst
```

---

</SwmSnippet>

## Cleaning Up Recordset Resources

```mermaid
%%{init: {"flowchart": {"defaultRenderer": "elk"}} }%%
flowchart TD
    node1["Request to close recordset"] --> node2{"Is recordset provided?"}
    click node1 openCode "Module/modDatabase.bas:1337:1340"
    node2 -->|"No"| node6["No action needed"]
    click node2 openCode "Module/modDatabase.bas:1340:1341"
    node2 -->|"Yes"| node3{"Is recordset open?"}
    click node3 openCode "Module/modDatabase.bas:1342:1344"
    node3 -->|"Yes"| node4["Close recordset"]
    click node4 openCode "Module/modDatabase.bas:1343:1344"
    node3 -->|"No"| node5["Release recordset resources"]
    click node5 openCode "Module/modDatabase.bas:1345:1346"
    node4 --> node5
    node5 --> node7["Resources released"]
    click node7 openCode "Module/modDatabase.bas:1346:1347"
    node6 --> node7
classDef HeadingStyle fill:#777777,stroke:#333,stroke-width:2px;

%% Swimm:
%% %%{init: {"flowchart": {"defaultRenderer": "elk"}} }%%
%% flowchart TD
%%     node1["Request to close recordset"] --> node2{"Is recordset provided?"}
%%     click node1 openCode "<SwmPath>[Module/modDatabase.bas](Module/modDatabase.bas)</SwmPath>:1337:1340"
%%     node2 -->|"No"| node6["No action needed"]
%%     click node2 openCode "<SwmPath>[Module/modDatabase.bas](Module/modDatabase.bas)</SwmPath>:1340:1341"
%%     node2 -->|"Yes"| node3{"Is recordset open?"}
%%     click node3 openCode "<SwmPath>[Module/modDatabase.bas](Module/modDatabase.bas)</SwmPath>:1342:1344"
%%     node3 -->|"Yes"| node4["Close recordset"]
%%     click node4 openCode "<SwmPath>[Module/modDatabase.bas](Module/modDatabase.bas)</SwmPath>:1343:1344"
%%     node3 -->|"No"| node5["Release recordset resources"]
%%     click node5 openCode "<SwmPath>[Module/modDatabase.bas](Module/modDatabase.bas)</SwmPath>:1345:1346"
%%     node4 --> node5
%%     node5 --> node7["Resources released"]
%%     click node7 openCode "<SwmPath>[Module/modDatabase.bas](Module/modDatabase.bas)</SwmPath>:1346:1347"
%%     node6 --> node7
%% classDef HeadingStyle fill:#777777,stroke:#333,stroke-width:2px;
```

<SwmSnippet path="/Module/modDatabase.bas" line="1337">

---

In <SwmToken path="Module/modDatabase.bas" pos="1337:4:4" line-data="Public Sub CloseRS(rst As ADODB.Recordset)">`CloseRS`</SwmToken>, we make sure the recordset is closed and dereferenced. If something goes wrong, we log the error to a text file by calling <SwmToken path="Module/modDatabase.bas" pos="1350:1:1" line-data="    LogErrorText &quot;Error&quot;, mstrMethod, Err.Description">`LogErrorText`</SwmToken> next.

```visual basic
Public Sub CloseRS(rst As ADODB.Recordset)
Const mstrMethod As String = "CloseRS"
On Error GoTo CheckErr
    If rst Is Nothing Then
    Else
        If rst.State = adStateOpen Then
            rst.Close
        End If
        Set rst = Nothing
    End If
    Exit Sub
CheckErr:
    MsgBox Err.Number & " - " & Err.Description, vbExclamation, mstrMethod
    LogErrorText "Error", mstrMethod, Err.Description
```

---

</SwmSnippet>

<SwmSnippet path="/Module/modDatabase.bas" line="1351">

---

We just came back from <SwmToken path="Module/modFunction.bas" pos="338:1:1" line-data="    LogErrorText &quot;Error&quot;, mstrMethod, Err.Description">`LogErrorText`</SwmToken> in <SwmToken path="Module/modFunction.bas" pos="106:1:1" line-data="    CloseRS rst">`CloseRS`</SwmToken>. After logging, the function ends—no further action is taken.

```visual basic
    'LogErrorDB "Sub", mstrModule, mstrMethod, Err.Number, Err.Description
End Sub
```

---

</SwmSnippet>

## Finalizing Access Check and Closing Connection

<SwmSnippet path="/Module/modFunction.bas" line="125">

---

We just returned from <SwmToken path="Module/modFunction.bas" pos="106:1:1" line-data="    CloseRS rst">`CloseRS`</SwmToken> in <SwmToken path="Module/modFunction.bas" pos="84:4:4" line-data="Public Function UserAccessModule(intModuleID As Integer, Optional strUserID As String = &quot;&quot;) As Boolean">`UserAccessModule`</SwmToken>. Now we close the database connection with <SwmToken path="Module/modFunction.bas" pos="125:1:1" line-data="    CloseDB">`CloseDB`</SwmToken> to free up resources before exiting the function.

```visual basic
    CloseDB
    Exit Function
CheckErr:
```

---

</SwmSnippet>

## Releasing Database Connection Resources

```mermaid
%%{init: {"flowchart": {"defaultRenderer": "elk"}} }%%
flowchart TD
    node1{"Does database connection exist?"}
    click node1 openCode "Module/modDatabase.bas:58:64"
    node1 -->|"No"| node5["End"]
    node1 -->|"Yes"| node2{"Is connection open?"}
    click node2 openCode "Module/modDatabase.bas:60:62"
    node2 -->|"Yes"| node3["Close connection"]
    click node3 openCode "Module/modDatabase.bas:61:61"
    node2 -->|"No"| node4["Release connection object"]
    node3 --> node4
    node4 --> node5["End"]
    click node4 openCode "Module/modDatabase.bas:63:63"
    click node5 openCode "Module/modDatabase.bas:65:70"

classDef HeadingStyle fill:#777777,stroke:#333,stroke-width:2px;

%% Swimm:
%% %%{init: {"flowchart": {"defaultRenderer": "elk"}} }%%
%% flowchart TD
%%     node1{"Does database connection exist?"}
%%     click node1 openCode "<SwmPath>[Module/modDatabase.bas](Module/modDatabase.bas)</SwmPath>:58:64"
%%     node1 -->|"No"| node5["End"]
%%     node1 -->|"Yes"| node2{"Is connection open?"}
%%     click node2 openCode "<SwmPath>[Module/modDatabase.bas](Module/modDatabase.bas)</SwmPath>:60:62"
%%     node2 -->|"Yes"| node3["Close connection"]
%%     click node3 openCode "<SwmPath>[Module/modDatabase.bas](Module/modDatabase.bas)</SwmPath>:61:61"
%%     node2 -->|"No"| node4["Release connection object"]
%%     node3 --> node4
%%     node4 --> node5["End"]
%%     click node4 openCode "<SwmPath>[Module/modDatabase.bas](Module/modDatabase.bas)</SwmPath>:63:63"
%%     click node5 openCode "<SwmPath>[Module/modDatabase.bas](Module/modDatabase.bas)</SwmPath>:65:70"
%% 
%% classDef HeadingStyle fill:#777777,stroke:#333,stroke-width:2px;
```

<SwmSnippet path="/Module/modDatabase.bas" line="55">

---

In <SwmToken path="Module/modDatabase.bas" pos="55:4:4" line-data="Public Sub CloseDB()">`CloseDB`</SwmToken>, we close and release the database connection if it's open. If something goes wrong, we log the error to a text file by calling <SwmToken path="Module/modDatabase.bas" pos="68:1:1" line-data="    LogErrorText &quot;Error&quot;, mstrMethod, Err.Description">`LogErrorText`</SwmToken> next.

```visual basic
Public Sub CloseDB()
Const mstrMethod As String = "CloseDB"
On Error GoTo CheckErr
    If ACN Is Nothing Then
    Else
        If ACN.State = adStateOpen Then
            ACN.Close
        End If
        Set ACN = Nothing
    End If
    Exit Sub
CheckErr:
    MsgBox Err.Number & " - " & Err.Description, vbExclamation, mstrMethod
    LogErrorText "Error", mstrMethod, Err.Description
```

---

</SwmSnippet>

<SwmSnippet path="/Module/modDatabase.bas" line="69">

---

We just came back from <SwmToken path="Module/modFunction.bas" pos="338:1:1" line-data="    LogErrorText &quot;Error&quot;, mstrMethod, Err.Description">`LogErrorText`</SwmToken> in <SwmToken path="Module/modFunction.bas" pos="125:1:1" line-data="    CloseDB">`CloseDB`</SwmToken>. After logging, the function ends—no further action is taken.

```visual basic
    'LogErrorDB "Sub", mstrModule, mstrMethod, Err.Number, Err.Description
End Sub
```

---

</SwmSnippet>

## Error Handling and Cleanup in Access Check

<SwmSnippet path="/Module/modFunction.bas" line="128">

---

We just returned from <SwmToken path="Module/modFunction.bas" pos="125:1:1" line-data="    CloseDB">`CloseDB`</SwmToken> in <SwmToken path="Module/modFunction.bas" pos="84:4:4" line-data="Public Function UserAccessModule(intModuleID As Integer, Optional strUserID As String = &quot;&quot;) As Boolean">`UserAccessModule`</SwmToken>. If we're in the error handler, we call <SwmToken path="Module/modFunction.bas" pos="128:1:1" line-data="    CloseRS rst">`CloseRS`</SwmToken> again to make sure the recordset is cleaned up, even after an error.

```visual basic
    CloseRS rst
```

---

</SwmSnippet>

<SwmSnippet path="/Module/modFunction.bas" line="129">

---

We just returned from <SwmToken path="Module/modFunction.bas" pos="106:1:1" line-data="    CloseRS rst">`CloseRS`</SwmToken> in <SwmToken path="Module/modFunction.bas" pos="130:1:1" line-data="    UserAccessModule = False">`UserAccessModule`</SwmToken>. Now we call <SwmToken path="Module/modFunction.bas" pos="129:1:1" line-data="    CloseDB">`CloseDB`</SwmToken> again to make sure the DB connection is closed after an error, set the return value to False, and show an error message.

```visual basic
    CloseDB
    UserAccessModule = False
    MsgBox Err.Number & " - " & Err.Description, vbExclamation, mstrMethod
```

---

</SwmSnippet>

<SwmSnippet path="/Module/modFunction.bas" line="132">

---

We just returned from <SwmToken path="Module/modFunction.bas" pos="125:1:1" line-data="    CloseDB">`CloseDB`</SwmToken> in <SwmToken path="Module/modFunction.bas" pos="84:4:4" line-data="Public Function UserAccessModule(intModuleID As Integer, Optional strUserID As String = &quot;&quot;) As Boolean">`UserAccessModule`</SwmToken>. Finally, we log the error details to the database with <SwmToken path="Module/modFunction.bas" pos="133:1:1" line-data="    LogErrorDB &quot;Function&quot;, mstrModule, mstrMethod, Err.Number, Err.Description">`LogErrorDB`</SwmToken> for tracking and debugging.

```visual basic
    'LogError "Error", mstrMethod, Err.Description
    LogErrorDB "Function", mstrModule, mstrMethod, Err.Number, Err.Description
End Function
```

---

</SwmSnippet>

# Logging Errors to the Database with Timestamps

```mermaid
%%{init: {"flowchart": {"defaultRenderer": "elk"}} }%%
flowchart TD
    node1{"Is error type specified?"}
    click node1 openCode "Module/modFunction.bas:311:311"
    node1 -->|"Yes"| node2["Prepare error log entry (error code, description, module, method, user, timestamp)"]
    click node2 openCode "Module/modFunction.bas:312:327"
    node1 -->|"No"| node3["Set error type to 'Unknown'"]
    click node3 openCode "Module/modFunction.bas:311:311"
    node3 --> node2
    node2 --> node4["Record error in database"]
    click node4 openCode "Module/modFunction.bas:328:333"
    node4 --> node5["Complete"]
    click node5 openCode "Module/modFunction.bas:334:335"

classDef HeadingStyle fill:#777777,stroke:#333,stroke-width:2px;

%% Swimm:
%% %%{init: {"flowchart": {"defaultRenderer": "elk"}} }%%
%% flowchart TD
%%     node1{"Is error type specified?"}
%%     click node1 openCode "<SwmPath>[Module/modFunction.bas](Module/modFunction.bas)</SwmPath>:311:311"
%%     node1 -->|"Yes"| node2["Prepare error log entry (error code, description, module, method, user, timestamp)"]
%%     click node2 openCode "<SwmPath>[Module/modFunction.bas](Module/modFunction.bas)</SwmPath>:312:327"
%%     node1 -->|"No"| node3["Set error type to 'Unknown'"]
%%     click node3 openCode "<SwmPath>[Module/modFunction.bas](Module/modFunction.bas)</SwmPath>:311:311"
%%     node3 --> node2
%%     node2 --> node4["Record error in database"]
%%     click node4 openCode "<SwmPath>[Module/modFunction.bas](Module/modFunction.bas)</SwmPath>:328:333"
%%     node4 --> node5["Complete"]
%%     click node5 openCode "<SwmPath>[Module/modFunction.bas](Module/modFunction.bas)</SwmPath>:334:335"
%% 
%% classDef HeadingStyle fill:#777777,stroke:#333,stroke-width:2px;
```

<SwmSnippet path="/Module/modFunction.bas" line="307">

---

In <SwmToken path="Module/modFunction.bas" pos="307:4:4" line-data="Public Sub LogErrorDB(LogType As String, LogModule As String, LogMethod As String, ErrorNumber As String, Optional ErrorDescription As String)">`LogErrorDB`</SwmToken>, we build an SQL INSERT to log error details, including a formatted timestamp. We call <SwmToken path="Module/modFunction.bas" pos="321:15:15" line-data="    strSQL = strSQL &amp; &quot;#&quot; &amp; FormatDateAndTime(Now) &amp; &quot;#,&quot;">`FormatDateAndTime`</SwmToken> next to get the right date/time string.

```visual basic
Public Sub LogErrorDB(LogType As String, LogModule As String, LogMethod As String, ErrorNumber As String, Optional ErrorDescription As String)
    Const mstrMethod As String = "LogErrorDB"
    Dim strSQL As String
On Error GoTo CheckErr
    If LogType = "" Then LogType = "Unknown"
    strSQL = "INSERT INTO LogError ("
    strSQL = strSQL & " LogDateTime,"
    strSQL = strSQL & " LogErrorNum,"
    strSQL = strSQL & " LogErrorDescription,"
    strSQL = strSQL & " LogUserName,"
    strSQL = strSQL & " LogModule,"
    strSQL = strSQL & " LogMethod,"
    strSQL = strSQL & " LogType)"
    strSQL = strSQL & " VALUES ("
    strSQL = strSQL & "#" & FormatDateAndTime(Now) & "#,"
    strSQL = strSQL & " '" & CheckInput(ErrorNumber) & "',"
    strSQL = strSQL & " '" & CheckInput(ErrorDescription) & "',"
    strSQL = strSQL & " '" & gstrUserID & "',"
    strSQL = strSQL & " '" & CheckInput(LogModule) & "',"
    strSQL = strSQL & " '" & CheckInput(LogMethod) & "',"
    strSQL = strSQL & " '" & CheckInput(LogType) & "')"
```

---

</SwmSnippet>

<SwmSnippet path="/Module/modCommon.bas" line="134">

---

<SwmToken path="Module/modCommon.bas" pos="134:4:4" line-data="Public Function FormatDateAndTime(dtDate As Date) As String">`FormatDateAndTime`</SwmToken> just returns the date/time as a readable string like '01 Jan 2024 10:30:00 AM'.

```visual basic
Public Function FormatDateAndTime(dtDate As Date) As String
    FormatDateAndTime = Format(dtDate, "dd MMM yyyy hh:mm:ss AMPM")
End Function
```

---

</SwmSnippet>

<SwmSnippet path="/Module/modFunction.bas" line="328">

---

We just got the formatted timestamp from <SwmToken path="Module/modFunction.bas" pos="321:15:15" line-data="    strSQL = strSQL &amp; &quot;#&quot; &amp; FormatDateAndTime(Now) &amp; &quot;#,&quot;">`FormatDateAndTime`</SwmToken> in <SwmToken path="Module/modFunction.bas" pos="133:1:1" line-data="    LogErrorDB &quot;Function&quot;, mstrModule, mstrMethod, Err.Number, Err.Description">`LogErrorDB`</SwmToken>. Now we open the DB, start a transaction, insert the log, and commit it to keep things consistent.

```visual basic
    OpenDB
    With ACN
        .BeginTrans
        .Execute strSQL
        .CommitTrans
    End With
```

---

</SwmSnippet>

<SwmSnippet path="/Module/modFunction.bas" line="334">

---

We just finished the transaction in <SwmToken path="Module/modFunction.bas" pos="133:1:1" line-data="    LogErrorDB &quot;Function&quot;, mstrModule, mstrMethod, Err.Number, Err.Description">`LogErrorDB`</SwmToken>. Now we close the DB connection to clean up before exiting.

```visual basic
    CloseDB
    Exit Sub
CheckErr:
```

---

</SwmSnippet>

<SwmSnippet path="/Module/modFunction.bas" line="337">

---

We just returned from <SwmToken path="Module/modFunction.bas" pos="125:1:1" line-data="    CloseDB">`CloseDB`</SwmToken> in <SwmToken path="Module/modFunction.bas" pos="133:1:1" line-data="    LogErrorDB &quot;Function&quot;, mstrModule, mstrMethod, Err.Number, Err.Description">`LogErrorDB`</SwmToken>. If logging to the DB fails, we fall back to writing the error to a text file with <SwmToken path="Module/modFunction.bas" pos="338:1:1" line-data="    LogErrorText &quot;Error&quot;, mstrMethod, Err.Description">`LogErrorText`</SwmToken>.

```visual basic
    'MsgBox Err.Number & " - " & Err.Description, vbExclamation, mstrMethod
    LogErrorText "Error", mstrMethod, Err.Description
End Sub
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBc3Rhci1ob3RlbC12YjYlM0ElM0FtdWRhc2luMQ==" repo-name="star-hotel-vb6"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>
