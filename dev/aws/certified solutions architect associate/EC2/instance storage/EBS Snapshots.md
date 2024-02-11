Makes a backup (snapshot) of your EBS volume at a point in time  
- Not necessary to detach volume to do snapshot, but recommended
- Can copy snapshots across AZ or Region
![[Pasted image 20240210101946.png]]

## Features

### EBS Snapshot Archive  
- Move a Snapshot to an ”archive tier” that is 75% cheaper
- Takes within 24 to 72 hours for restoring the archive
![[Pasted image 20240210102038.png]]

### Recycle Bin for EBS Snapshots
- Setup rules to retain deleted snapshots so you can recover them after an accidental deletion
- Specify retention (from 1 day to 1 year)
![[Pasted image 20240210102107.png]]

### Fast Snapshot Restore (FSR)
- Force full initialization of snapshot to have no latency on the first use (costs a lot)

## Hands on

### Create a snapshot
Here, we can create a snapshot of our the EBS volume
![[Pasted image 20240210102336.png]]

### Create volume from snapshot
We can also create a volume for the snapshot and give a different AZ if needed.
![[Pasted image 20240210103525.png]]

### Create Retention Rule for Recycle bin
Here, click on `Recycle Bin` to go to Recycle bin to create a retention rule. Inside creating retention rule, you can specify the retention period by selecting 1 - 365 days.
![[Pasted image 20240210103226.png]]

If we accidentally delete the snapshot of the volume,
![[Pasted image 20240210103349.png]]

Since we created a recycle bin with certain retention rule, we can restore:
![[Pasted image 20240210103432.png]]
This will restore the snapshot we just deleted.