
- EBS volumes are **network drives** with good but “limited” performance
- **If you need a high-performance hardware disk, use EC2 Instance Store**
- Better I/O performance
- EC2 Instance Store lose their storage if they’re stopped (ephemeral)
- Good for buffer / cache / scratch data / temporary content (not for long-term storage)
- Risk of data loss if hardware fails
- Backups and Replication are your responsibility

## Example
IOPS = I/O per second
![[Pasted image 20240210111849.png]]


