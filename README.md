# Aether
Zero trust storage that shards your data across object stores.

Multi-part uploads are sharded across a GCS bucket and Azure Blob Storage. 
The file is split into chunks and uploaded to the object stores. 
For download, the file is then reassembled from the chunks.

## Example

```bash
# Upload file
$ curl -X POST -F "file=@my.data" http://127.0.0.1:5000/upload                    

{
  "file_id": "5jo0j", 
  "message": "Upload successful"
}

# Some chunks in gsutil
$ gsutil ls gs://romil-aether
gs://romil-aether/5jo0j_chunk_0
gs://romil-aether/5jo0j_chunk_2
gs://romil-aether/5jo0j_chunk_4

# Rest of the chunks on azure blob storage. Not shown here.

# Download file with the file_id
$ curl http://127.0.0.1:5000/download/5jo0j --output download.data    

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 5120k  100 5120k    0     0  3594k      0  0:00:01  0:00:01 --:--:-- 3603k
```

## Installation and usage

### Install
```bash
pip install -e .
```

### Run

Aether is configured through environment variables.
```bash
export GCS_BUCKET_NAME='' # Your GCS bucket name
export AZURE_CONNECTION_STRING='' # Your Azure connection string
export AZURE_CONTAINER_NAME='' # Your Azure container name
export CHUNK_SIZE=1 # 1MB default chunk sizes for sharding

python -m aether.server
```

Then use curl to upload and download files as shown in the example above.
