# Encryption and Downtime

* For most resources, encryption can only be enabled at creation.
* **EFS** - if you want to encrypt an EFS that is already exist, you'll need to create a new EFS and migrate your data.
* **RDS** - if you want to encrypt existing RDS, you need to create new encrypted database and migrate your data.
* **EBS** - encryption must be selected at creation time
  * you can not encrypt an unencrypted volume or unencrypt an encrypted volume.
  * you can migrate data between encrypted and unencrypted volumes \(e.g. using `rsync` or `Robocopy`\)
  * if you want to encrypt an existing volume, you can create a snapshot, copy the snapshot and apply encryption at the same time to give you an encrypted snapshot. Then restore the encrypted snapshot to a new encrypted volume.
* **S3 buckets** - you can enable encryption on your buckets at any time.
* **S3 objects** - you can enable individual S3 object encryption at any time.

