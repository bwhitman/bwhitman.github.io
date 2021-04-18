---
title: "Make and attach an EBS volume on EC2 instance boot"
date: "2009-02-07"
---

This is all you have to do…

create\_mount\_ebs() {
	# Make and mount an EBS. Note: your instance has to be on us-east-1a.
	SIZE="$1" # in GB
	log "creating and mounting a $1 GB EBS to /vol"
	VOL=\`ec2-create-volume -z us-east-1a -s $SIZE | awk '{print $2}'\`

	# Check for it on a loop..
	AVAILABLE="unavailable"
	while \[ "$AVAILABLE" != "available" \]
		do
			AVAILABLE=\`ec2-describe-volumes $VOL | awk '{print $5}'\`
		done
		
	# Now attach it
	INSTANCE=\`curl [http://169.254.169.254/latest/meta-data/instance-id](http://169.254.169.254/latest/meta-data/instance-id) 2> /dev/null\`
	ec2-attach-volume -d /dev/sdh -i $INSTANCE $VOL	
	apt-get install -y xfsprogs
	mkfs.xfs /dev/sdh
	echo "/dev/sdh /vol xfs noatime 0 0" >> /etc/fstab
	mkdir /vol
	mount /vol
}
