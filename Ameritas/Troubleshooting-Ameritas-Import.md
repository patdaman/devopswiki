<IMG src="https://www.ameritas.com/Corp_RespThemeS/themes/CorpRespTheme/images/corp/Ameritas_Bison_Tag_WEB_color_189x57.png"  alt="Ameritas" width="120" height="35"/>

# Table of Contents
  * [Acceptable Files](#prerequisites)
  * [View Import Jobs](#qes-import-jobs)
  * [Manual Import Step 1 - Connect to Storage Explorer](#connect-to-storage-explorer)
  * [Manual Import Step 2 - Upload files](#upload-files)
  * [Manual Import Step 3 - Monitor progress](#monitor-progress)
  * [Common Errors](#common-errors)
___
## Prerequisites
---
Acceptable files and their formats:
- Case.csv
- CONTRACT.txt
- innetwork_procedure_codes.csv
- language_codes.csv
- OPTOUT.txt
- outofnetwork_procedure_codes.csv
- PROVDATA.txt
- servarea.csv
- Univ Codes.csv

Accepted file names are based on the following regular expressions. For most up-to-date regex values navigate to `C:\Source\Omnius\Omnius.JobProcessingRole\App.config`
```
<add key="AmeritasProviderOrContractOrOptoutFileRegex" value="^(?:prov[_ ]?data|contract|opt[_ ]?out)\d*(?:\(\d+\))?\.txt(?:\(\d+\))?(?&lt;PGP&gt;\.pgp)?$" />
<add key="AmeritasContractFileRegex" value="^contract\d*(?&lt;Index1&gt;\(\d+\))?\.txt(?&lt;Index2&gt;\(\d+\))?$" />
<add key="AmeritasProviderFileRegex" value="^prov[_ ]?data\d*(?&lt;Index1&gt;\(\d+\))?\.txt(?&lt;Index2&gt;\(\d+\))?$" />
<add key="AmeritasOptOutFileRegex" value="^opt[_ ]?out\d*(?&lt;Index1&gt;\(\d+\))?\.txt(?&lt;Index2&gt;\(\d+\))?$" />
<add key="AmeritasLanguageFileRegex" value="^language[_ ]?codes(?:\(\d+\))?\.csv(?:\(\d+\))?(?&lt;PGP&gt;\.pgp)?$" />
<add key="AmeritasServiceAreaFileRegex" value="^serv[_ ]?area(?:\(\d+\))?\.csv(?:\(\d+\))?(?&lt;PGP&gt;\.pgp)?$" />
<add key="AmeritasUniversityFileRegex" value="^univ[_ ]?codes(?:\(\d+\))?\.csv(?:\(\d+\))?(?&lt;PGP&gt;\.pgp)?$" />
<add key="AmeritasCasingFileRegex" value="^case(?:\(\d+\))?\.csv(?:\(\d+\))?(?&lt;PGP&gt;\.pgp)?$" />
<add key="AmeritasInNetworkProcedureCodeFileRegex" value="^innetwork[_ ]?procedure[_ ]?codes(?:\(\d+\))?\.csv(?:\(\d+\))?(?&lt;PGP&gt;\.pgp)?$" />
<add key="AmeritasOutOfNetworkProcedureCodeFileRegex" value="^outofnetwork[_ ]?procedure[_ ]?codes(?:\(\d+\))?\.csv(?:\(\d+\))?(?&lt;PGP&gt;\.pgp)?$" />
```

**Before getting started** the following items are necessary to complete this process:
- Files to upload (Ex: `PROVDATA.TXT`, `CONTRACT.TXT`, and `OPTOUT.txt`. These 3 particular files must be uploaded together. An 'All Or Nothing' policy applies).
- Environment connection string to Microsoft Azure Storage Explorer (Storage Explorer)
- Running environment
___
## QES Import Jobs
To view Ameritas Import jobs in the database use the following query against the OmniusCore database.  
  ```SQL
  --use OmniusCore;
  DECLARE @searchHours INT = 24

  select
    JobType = jt.Name
    ,j.JobId
    ,DateQueued = j.DateQueued at time zone 'UTC' at time zone 'Central Standard Time'
    ,DateStarted = j.DateStarted at time zone 'UTC' at time zone 'Central Standard Time'
    ,Duration = CAST(COALESCE(j.DateFinished, j.DateQueuedForRetry, j.DateCancelled, GETUTCDATE()) - j.DateStarted AS TIME(2))
    ,DateCompleted = COALESCE(j.DateFinished, j.DateQueuedForRetry, j.DateCancelled, '9999-12-31') at time zone 'UTC' at time zone 'Central Standard Time'
    ,j.Payload
    ,j.ErrorMessage
    ,j.RetryCount
  from [Job] j
    inner join JobType jt on j.JobType = jt.Id
  where 1=1
    AND j.ClientId = 16
    AND (DATEDIFF(hour, j.DateStarted, GETUTCDATE()) <= @searchHours
      OR (j.DateStarted IS NULL AND j.DateQueued IS NOT NULL AND DATEDIFF(hour, j.DateQueued, GETUTCDATE()) <= @searchHours))
  order by DateCompleted desc, DateQueued desc
  ```
___

## Connect to Storage Explorer

- Click **Add Account** (top left)
- Select **Use a connection string**
- Follow steps to connect

![image.png](/.attachments/image-5408c512-352e-4ee7-bfe8-844eec5226ab.png)

**IMPORTANT!** Once connected, locate and select the `ameritas` blob storage container.
**WARNING!** Uploading files to this blob container will trigger an `Ameritas File Import` job. Only upload files that the job will recognize.

Once connected and the `ameritas` container has been selected the Explorer should look similar to this:
![image.png](/.attachments/image-596800ae-dd8d-4346-a960-b5dd075fd03d.png)

___
## Upload files ([Re-upload files](#re-upload-files))

**NOTE:** Copying a file from the archive folder is more reliable than uploading a file.
- Click **Upload** (top left of container window)
- Select file to upload
- Select Blob type as `Block Blob`
- Keep `Upload .vhd/vhdx files as page blobs (recommended)` checked
- Click **Upload**

If the desired environment is running this should kick off the `Ameritas File Import` job.

**WARNING!** Currently, daily provider data files `PROVDATA.TXT`, `CONTRACT.TXT`, and `OPTOUT.txt` have to be uploaded together, one after another.

![image.png](/.attachments/image-a765222b-61e1-4468-b6ec-bf8c7e379e3d.png)

___
## Re-Upload files

When a file is uploaded it is archived to `ameritas/Archive`. To re-upload this file copy it from the Archive folder to the main `ameritas` blob container and then rename the file.

If the desired file isn't visible within the Archive folder, click **Load more** to load more files:
![image.png](/.attachments/image-abbee722-75d0-43dc-97fe-6e3a633b1365.png)

If the desired environment is running this should kick off the `Ameritas File Import` job.

**WARNING!** Currently, weekly provider data files `PROVDATA.TXT` and `CONTRACT.TXT` have to be uploaded together, one after another.

___
## Monitor progress

Monitor progress via 
- SEQ (http://questlogging01.cloudapp.net:2357/#/events?signal=signal-804,signal-69&filter=ameritas)
- Ameritas Dashboard within the Quest cloud solution
- [QES (Omnius Core) Database](#qes-import-jobs)

**NOTE:** A fully imported and published log output looks like this.
![image.png](/.attachments/image-b5b24217-7b1c-434e-8e6f-7671427842e3.png)

## Common Errors
---
- Occasionally, the Azure Functions BLOB trigger will not fire when a file is pasted.  Update the metadata of a file to trigger the Azure Function.
![image.png](/.attachments/image-fd5da08b-87e2-4773-ae53-08e35902f9c3.png)
- A lookup key error here means that the provider and contract files are mismatched.
![image.png](/.attachments/image-0da88a9a-5a40-4fcb-b88b-31c422504de0.png)
- Here are examples of some failures
  - 1:32PM: unknown contract file name (i.e. "CONTRACT(504)_1.TXT")
  - 2:25PM, 2:39PM, and 2:39PM: wrong contract file size, resulting in PROVDATA.TXT and CONTRACT.TXT being mismatched
![image.png](/.attachments/image-aa88b353-ebae-41d5-b26b-9ee5f7edcb9a.png)
