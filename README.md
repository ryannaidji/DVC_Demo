# DVC Demo 🚀

## What is DVC? 🤔
DVC (Data Version Control) is an open-source tool designed to streamline the management and versioning of data, machine learning models, and pipelines in data science and machine learning projects. DVC extends the capabilities of Git, enabling efficient handling of large files and datasets. It is one of the essential tools that MLOps professionals should know to efficiently manage machine learning workflows.

## Why Use DVC? 🌟
- **Version Control for Data**: Just as Git tracks changes in code, DVC allows you to version control large datasets and models.
- **Reproducibility**: Ensures that experiments and results can be reproduced with the exact same datasets and configurations.
- **Collaboration**: Facilitates seamless sharing of datasets and ML models among team members.
- **Remote Storage Integration**: Works with various remote storage solutions (e.g., Google Drive, AWS S3, Azure Blob Storage) to offload large files.

## How is DVC Useful in Machine Learning and AI Projects? 🤖
Machine learning and AI workflows involve numerous experiments with datasets, models, and hyperparameters. DVC helps by:
- Keeping track of changes in datasets and model files.
- Providing an easy way to revert to previous versions of data and models.
- Allowing seamless integration with cloud storage to manage large files outside the Git repository.

## References 📚
- [Official DVC Documentation](https://dvc.org/doc)
- [DVC GitHub Repository](https://github.com/iterative/dvc)

---

## DEMO 🛠️
In this demo, we will demonstrate how to use DVC to version control data in a machine learning workflow. We'll be leveraging Google Drive as our remote storage solution for simplicity and accessibility. The steps include initializing DVC, downloading data, setting up remote storage, and managing dataset versions efficiently.

### 0. Install DVC with Google Drive Support 🛠️
Before starting, install DVC with Google Drive support using pip:
```bash
pip install 'dvc[gdrive]'
```

### 1. Initialize Git and DVC ⚙️
```bash
git init
dvc init
```

### 2. Download and Add Data 📂
```bash
dvc get https://github.com/iterative/dataset-registry get-started/data.xml -o data/data.xml
dvc add ./data/data.xml
git add ./data/.gitignore ./data/data.xml.dvc
git commit -m "Add raw data"
```

### 3. Set Up Google Drive Remote Storage ☁️

Create a new directory in your Google Drive. Change the access parameters to allow editing for people who have the share link.

1. Add your Google Drive as remote storage:
   ```bash
   dvc remote add -d storage gdrive://YOUR_DRIVE_ID
   ```
2. Configure your service account:
   - Go to [Google Cloud](https://console.cloud.google.com/)
   - Create a new project
   - Go to the service accounts section and create a service account
   - Create a new key from this service account
   - Download the key file in JSON format
   - Save the key file in your `.dvc/` directory and rename it to `credentials.json`.
   - Add `credentials.json` to `.dvc/.gitignore`.
   ```bash
   dvc remote modify storage gdrive_use_service_account true
   dvc remote modify storage gdrive_service_account_json_file_path .dvc/credentials.json
   git commit .dvc/config -m "Configure remote storage"
   ```
3. Push your data to remote storage:
   ```bash
   dvc push
   ```

### 4. Retrieve Data After Cleanup 🧹
1. Delete the cache and data file:
   ```bash
   rm -r .dvc/cache data/data.xml
   ```
2. Pull data from remote storage:
   ```bash
   dvc pull
   ```
> ⚠️ **Warning**
> If you encounter the error `This file has been identified as malware or spam and cannot be downloaded`:
> 
> ```bash
> dvc remote modify storage gdrive_acknowledge_abuse true
> git commit .dvc/config -m "Acknowledge abuse flag"
> dvc pull
> ```

### 5. Update Data ✏️
1. Modify `data/data.xml` to add new content.
2. Track changes with DVC:
   ```bash
   dvc add ./data/data.xml
   git add ./data/data.xml.dvc
   git commit -m "Dataset updates"
   dvc push
   ```
Now you have multiple versions of your data stored in Google Drive.

### 6. View and Revert Data Versions 🔄
1. Check Git commit history:
   ```bash
   git log --oneline
   ```
2. Revert to a previous version of your dataset:
   ```bash
   git checkout HEAD^1 data/data.xml.dvc
   dvc checkout
   ```
3. Commit the reverted version:
   ```bash
   git add data/data.xml.dvc
   git commit -m "Revert dataset updates"
   ```

---

## Conclusion 🎯
Git handles version control for your code, while DVC extends this capability to your datasets and ML models, enabling efficient and reproducible workflows for machine learning and AI projects. DVC is a critical tool for MLOps, bridging the gap between code versioning and data versioning for scalable and collaborative machine learning development.
