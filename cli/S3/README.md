## [AWS CLI Command Reference](https://awscli.amazonaws.com/v2/documentation/api/latest/index.html)

### S3 copy local to remote
- copy one file:
aws s3 cp /path/to/local/file s3://my-bucket-name/destination/path
- many files:
aws s3 cp /path/to/local/files s3://my-bucket-name/destination/path --recursive --exclude "*" --include "file1.jpg" --include "*.pdf"
- directory:
aws s3 cp /path/to/local/directory s3://my-bucket-name/destination/path --recursive

### S3 copy remote to local
- all files with JPG extension from all directories to local current directory:
aws s3 cp s3://my-bucket-name . --recursive --exclude "*" --include "*.JPG"
