document
========

	mkdir /mnt/source
	mount -o loop /<ISOFILEPATH>/debian-6.0.6-amd64-DVD-1.iso /mnt/source
	echo file:///mnt/source squeeze main contrib > /etc/apt/source.list.d/    ISO.list
