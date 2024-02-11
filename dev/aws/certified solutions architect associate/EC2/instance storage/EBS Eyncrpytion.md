- When you create an encrypted EBS volume, you get the following:  
	- Data at rest is encrypted inside the volume  
	- All the data in flight moving between the instance and the volume is encrypted
	- All snapshots are encrypted  
	- All volumes created from the snapshot
- Encryption and decryption are handled transparently (you have nothing to do)
- Encryption has a minimal impact on latency
- EBS Encryption leverages keys from KMS (AES-256)
- Copying an unencrypted snapshot allows encryption
- Snapshots of encrypted volumes are encrypted

## Encrypt an unencrypted EBS volume
- Create an EBS snapshot of the volume
- Encrypt the EBS snapshot ( using copy )
- Create new ebs volume from the snapshot ( the volume will also be encrypted )
- Now you can attach the encrypted volume to the original instance

There's a shortcut:
- Create an EBS snapshot of the volume(a volume that is not encrypted)
- Selected the unencrypted snapshot and `Actions` > `Create volume from snapshot`, and when creating a volume, just enable `Encrypt this volume`
