# Azure-Files-and-Data-Protection
Build a cloud-based file server replacement for DMP Consulting using Azure Files while implementing data protection and recovery mechanisms.  The focus of this project is ensuring business data can be recovered from accidental deletion, corruption, or user error.

---

**Objectives Covered**
Create and configure a file share in Azure Files

Configure identity-based access for Azure Files

Configure snapshots for Azure Files

Configure soft delete for Azure Files

Manage files and folders

Understand Azure Files vs Blob Storage

---

**Scenario**

DMP Consulting currently stores department files on an aging on-premises file server.

Management wants to migrate to Azure Files to gain:

Centralized cloud storage
Simplified file sharing
Built-in recovery capabilities
Reduced infrastructure management

As the Azure Administrator, you will deploy Azure Files and implement protection mechanisms to safeguard business data.

---

**Architecture Overview**

<img width="561" height="262" alt="diagram drawio" src="https://github.com/user-attachments/assets/78b3e5df-08d1-442e-8153-dafd3f5b4a5f" />

---

**Create Storage Account**
Lets create a new storage account

az storage account create --name dmpfilestorage --resource-group Storage-RG --location centralus --sku Standard_LRS --kind StorageV2 --tags owner=devon department=IT

<img width="1132" height="707" alt="image" src="https://github.com/user-attachments/assets/6fec7f50-052f-48ba-a827-055d65832902" />


Next we'll go and create a file share.

az storage share create --account-name dmpfilestorage --name dmp-shared-files 

<img width="1132" height="711" alt="image" src="https://github.com/user-attachments/assets/c1a5782e-796d-48f1-b5ba-dc64745101d7" />

The Quota size is a bit big lets reconfigure it to something more reasonable.

az storage share update --account-name dmpfilestorage --name dmp-shared-files --quota 100

<img width="1131" height="712" alt="image" src="https://github.com/user-attachments/assets/d16f8441-c44d-4ac7-a6b5-93814ea3010f" />

---

**Create Department Folders**
Inside the file share lets create some folders: Finance, Marketing, and IT

az storage directory create --account-name dmpfilestorage --share-name dmp-shared-files --name IT && az storage directory create --account-name dmpfilestorage --share-name dmp-shared-files --name Marketing && az storage directory create --account-name dmpfilestorage --share-name dmp-shared-files --name Finance

<img width="1132" height="707" alt="image" src="https://github.com/user-attachments/assets/ab907db8-bb4f-4644-86ed-2968ff499aa8" />


I am also going to use a LLM to generate some fake documents.

<img width="432" height="581" alt="image" src="https://github.com/user-attachments/assets/60c20bc2-d972-47d9-a060-e084409ee27b" />

Now i am uploading them into my shared files folder

<img width="1132" height="711" alt="image" src="https://github.com/user-attachments/assets/20df40c3-1ae6-4327-80ad-b4d5b1c3e8b7" />

Finance: budget.xlsx and payroll.xlsx

Marketing: campaign-plan.docx

IT: asset-inventory.xlsx and network-diagram.pdf

This simulates a real company file share structure.

---

**Connect to Azure File Share**
I had issues connecting will try agian later.

Open Connect Option --> Choose Windows --> Map Network Drive --> Verify Access

Azure Files can be mounted similarly to traditional file servers.

---

**Create File Share Snapshot**
We want to protect files against accidental modification.

az storage share snapshot --account-name dmpfilestorage --name dmp-shared-files

<img width="1132" height="705" alt="image" src="https://github.com/user-attachments/assets/07fbb9be-1d43-47f4-9573-7ece1911f011" />

Snapshots provide point-in-time recovery.

---

**Restore Files from Snapshot**
Lets delete a file and then restore it using snapshots.  

az storage remove --account-name "dmpfilestorage" --share-name "dmp-shared-files" --path "IT" --recursive

<img width="1135" height="711" alt="image" src="https://github.com/user-attachments/assets/c1b6591d-0e52-4994-a247-fba43e04e1a4" />

Now will use the snapshot "restore" button to recover the deleted folder.

<img width="1137" height="712" alt="image" src="https://github.com/user-attachments/assets/9e020ae0-0417-4c8a-818d-c93eeeec727a" />

This simulates recovering from user error.

---

