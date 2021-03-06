# upload-to-s3
The Upload to S3 WordPress plugin automatically adds to specific text input fields the ability to select a file from your local machine, upload it directly to an AWS S3 bucket that the user has access to, and populate the text field with the public URL of the uploaded file. The file is never saved on the WordPress server. This is useful for working with large media files, such as podcasts and videos, that you want to store remotely without filling up your server disk.

## Usage
Add the CSS class 'upload_to_s3' to any text input and the S3 file uploader will automatically appear connected to that text field. Selecting and uploading a file will put the file in your S3 bucket and will put the public URL of your file into the text field. You can apply the class to multiple text input fields on a page, and each will work independently.

### Settings
Plugin setup is in the WordPress 'Settings' menu under 'Upload to S3 Settings'.  The S3 file uploader will not appear unless the name of an AWS S3 bucket is saved in the settings and some access credentials are entered and saved.

### Shared or individual access credentials

Upload access to your bucket is setup in AWS via the AWS 'IAM' system. Any user(s) who have access to the bucket will need an IAM account and a key/secret pair. You may create a single user and enter that key pair in the fields above, or you may setup indiviudal IAM accounts for any users you want to allow to upload to the S3 bucket. In this latter case, the user's key pair will be maintainted in his or her own profile page.

If there is no key pair set (either the global pair if you use shared credentials, or the individual users' key pair if you use individual credentials), the S3 file uploader will not appear.

### AWS S3 Bucket setup
Setting up an Amazon Web Services (AWS) account and using their S3 service to create a 'bucket' for file storage is beyond the scope of this documentation.  I have a blog post detailing how to do this in some detail at 

http://ieg.wnet.org/blog/setting-up-an-aws-s3-bucket-for-read-only-web-access/

The post above also goes into detail about creating and managing users for AWS services using their Identity and Access Management (IAM) system.

One particularly tricky part of S3 Bucket setup is getting the 'policy' working right.  If you are using a user or group policy to manage read/write access to the bucket (a good idea), make sure your policy includes permission for 'S3:PutObjectAcl'.  Here's a sample group policy for a bucket named 'files.xyz.com':

    {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::files.xyz.com"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:PutObjectAcl",
                "s3:GetObject",
                "s3:GetObjectVersion",
                "s3:DeleteObject",
                "s3:DeleteObjectVersion"
            ],
            "Resource": "arn:aws:s3:::files.xyz.com/*"
        }
    ]
    }

## Credits

The AWS CORS file upload stuff is adapted from Carson McDonald's work on this blog post
http://www.ioncannon.net/programming/1539/direct-browser-uploading-amazon-s3-cors-fileapi-xhr2-and-signed-puts/


