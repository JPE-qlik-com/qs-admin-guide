---
layout: default
title: Remove Unused Streams
parent: Streams
grand_parent: Asset Management
nav_order: 2
---

# Remove Unused Streams <i class="fas fa-dolly-flatbed fa-xs" title="Shipped | Native Capability"></i> <i class="fas fa-file-code fa-xs" title="API | Requires Script"></i>
{:.no_toc}

## Applicable Environments
{:.no_toc}
* All

## Goal
{:.no_toc}

Sometimes we move applications from one stream to another and, we do not notice that the stream is now empty. Or we create a stream waiting for applications get published to it and, they do not get published. It is always a good practice to delete unused Streams.

## Table of Contents
{:.no_toc}

* TOC
{:toc}

-------------------------

## QMC - Manually Check Published Apps For Each Stream  <i class="fas fa-dolly-flatbed fa-xs" title="Shipped | Native Capability"></i>

In the QMC, select **Streams**:

[![remove_unused_stream_01.png](images/remove_unused_stream_01.png)](https://raw.githubusercontent.com/qs-admin-guide/qs-admin-guide/master/docs/asset_management/streams/images/remove_unused_stream_01.png)

You will now see all the **Streams**. To view all applications that were published to a specific Stream, select the Stream name and enter to edit mode.

[![remove_unused_stream_02.png](images/remove_unused_stream_02.png)](https://raw.githubusercontent.com/qs-admin-guide/qs-admin-guide/master/docs/asset_management/streams/images/remove_unused_stream_02.png)

In **Edit** mode, select the **Apps** tab.

[![remove_unused_stream_03.png](images/remove_unused_stream_03.png)](https://raw.githubusercontent.com/qs-admin-guide/qs-admin-guide/master/docs/asset_management/streams/images/remove_unused_stream_03.png)

You will now see applications that were published to this stream.

[![remove_unused_stream_04.png](images/remove_unused_stream_04.png)](https://raw.githubusercontent.com/qs-admin-guide/qs-admin-guide/master/docs/asset_management/streams/images/remove_unused_stream_04.png)

This process should be repeated for each **Stream** to find unused streams.
In case you have a considerable quantity of streams, consider to use the [CLI method](#get-list-of-unused-streams-qlik-cli-).

-------------------------

## Get List of Unused Streams (Qlik CLI) <i class="fas fa-file-code fa-xs" title="API | Requires Script"></i>

The below script snippet requires the [Qlik CLI](../../tooling/qlik_cli.md).

The script will bring all streams and the number of applications inside of each one. The script will then store the output into the location of your choice in either csv or json format.

### Script
```powershell
# Function to collect streams and number of applications inside

# Parameters
# Assumes default credentials are used for the Qlik CLI Connection
$computerName = 'machineName'
$virtualProxyPrefix = '/default' # leave empty if windows auth is on default VP
$filePath = 'C:\'
$fileName = 'output'
$outputFormat = 'json'

$outFile = ($filePath + $fileName + '.' + $outputFormat)
$computerNameFull = ($computerName + $virtualProxyPrefix).ToString()

# Main
Connect-Qlik -ComputerName $computerNameFull -UseDefaultCredentials -TrustAllCerts
$streamJson = Get-QlikStream -raw -full
$appStreamIds = Get-QlikApp -filter "published eq true" | foreach{$_.stream.id} | Sort-Object | Get-Unique
$emptyStreamIDs = ($streamJson | foreach{$_.id}) | ?{$appStreamIds -notcontains $_}
$streamEmptyJson = $streamJson | ?{$emptyStreamIDs -contains $_.id}

(&{If($emptyStreamIDs.length -gt 0) {$("Empty Streams Found: " + $emptyStreamIDs.count) ; $streamEmptyJson} Else {"No Empty Streams Found"}})
If ($emptyStreamIDs.length) {
    (&{If($outputFormat.ToLower() -eq 'csv') {$streamEmptyJson | ConvertTo-Csv -NoTypeInformation | Set-Content $outFile} Else {$streamEmptyJson | ConvertTo-Json | Set-Content $outFile}})
}
```

### Example Output
```
[
    {
        "id":  "bbab7440-e2c6-4fea-8939-d23cabb16fb0",
        "createdDate":  "2020-03-13T13:27:55.453Z",
        "modifiedDate":  "2020-03-13T13:27:55.453Z",
        "modifiedByUserName":  "QLIKSENSE\\qlik",
        "customProperties":  [

                             ],
        "owner":  {
                      "id":  "24990cfb-d3f3-4086-979a-9bd2ff81c5f6",
                      "userId":  "qlik",
                      "userDirectory":  "QLIKSENSE",
                      "name":  "qlik",
                      "privileges":  null
                  },
        "name":  "Sales Team",
        "tags":  [

                 ],
        "privileges":  null,
        "schemaPath":  "Stream"
    },
    {
        "id":  "74ab818b-c6ab-4924-aa31-d34867babd53",
        "createdDate":  "2020-03-13T13:56:48.796Z",
        "modifiedDate":  "2020-03-13T13:56:48.796Z",
        "modifiedByUserName":  "QLIKSENSE\\qlik",
        "customProperties":  [

                             ],
        "owner":  {
                      "id":  "24990cfb-d3f3-4086-979a-9bd2ff81c5f6",
                      "userId":  "qlik",
                      "userDirectory":  "QLIKSENSE",
                      "name":  "qlik",
                      "privileges":  null
                  },
        "name":  "HR",
        "tags":  [

                 ],
        "privileges":  null,
        "schemaPath":  "Stream"
    }
]
```

**Tags**

#monthly
