# Snowball

### Snowball

* Snowball is a physical device used for transporting many terabytes or petabytes of data into and out of AWS
* Makes large scale of data transfer fast, easy and secure
* Comes in tamper-resistant enclosure
* Uses 256-bit encryption
* It's region specific, not for transporting data from one region to another

### When to use Snowball

* When you have many TB or PB of data to upload
* You don't want to make expensive upgrades to your network for a one-off data transfer
* If you frequently experience data backlog

  If you're in a physically isolated environment, high-bandwidth internet is not available or is cost-prohibitive

* If it takes more than a week to upload your data

### Snowball Edge

* Each Snowball Edge is a 100TB device, which also features onboard compute power which can be clustered to act as a single storage and compute pool.
* Designed to undertake local processing / edge computing, as well as data transfer.
* S3 compatible endpoint, supports NFS, and can also run Lambda functions as data is copied to the device.
* S3 buckets and Lambda functions come pre-configured on the device.

