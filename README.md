# Neo4j_Disk_Encryption

export encrypted_file_path='/home/neo4j/secretfs'

export encrypted_file_size=20G

export encryption_password='neo4j'

export loop_device=$(losetup -f)

dd of=$encrypted_file_path bs=$encrypted_file_size count=0 seek=1                                                                        

chmod 600 $encrypted_file_path

sudo losetup $loop_device $encrypted_file_path

sudo cryptsetup -y luksFormat $loop_device

sudo mke2fs -j -O dir_index /dev/mapper/neo4j_data

sudo mkdir -p /mnt/secretfs

sudo mount /dev/mapper/neo4j_data /mnt/secretfs

sudo chmod -R 700 /mnt/secretfs

sudo chown -R $db_user:$db_user /mnt/secretfs

mv /home/neo4j/neo4j-enterprise-4.4.5/data /mnt/secretfs

sudo cryptsetup -v status neo4j_data
## Execution

![image](https://user-images.githubusercontent.com/77326619/183654246-06286c04-502e-42c1-a92a-0a06d55fa611.png)

## Create a file for NEO4J  data storage

![image](https://user-images.githubusercontent.com/77326619/183654349-44380a2c-4e93-463a-a3d4-230103b54451.png)

## ●	Change the permission of the file so that only the owner of the file (that is, only the NEO4J user who created the file in the previous step) will be able to access it:

![image](https://user-images.githubusercontent.com/77326619/183654426-57749001-3029-466e-ba8a-e0401bfaf3e8.png)

## Associate a loopback device with the file:

![image](https://user-images.githubusercontent.com/77326619/183654482-bd807637-eebb-4d0b-81a0-7d40549c4ca0.png)

## Encrypt storage in the device. cryptsetup will use the Linux device mapper to create, in this case, $encrypted_file_path . Initialize the volume and set a password interactively with the password you set to $encryption_password :

![image](https://user-images.githubusercontent.com/77326619/183654538-cdf618ab-96f0-431d-8185-c8d8d453b2e1.png)

![image](https://user-images.githubusercontent.com/77326619/183654571-37276073-9356-4a5d-bd11-e9635fdf367f.png)

## Open the partition, and create a mapping to $encrypted_file_path 

![image](https://user-images.githubusercontent.com/77326619/183654652-0c46642f-7596-48f0-b6c3-a1e5e692acfc.png)

## Create a file system and verify its status:

![image](https://user-images.githubusercontent.com/77326619/183654733-7e9548cd-031d-4e3a-b475-3175c5f088ca.png)

## Mount the new file system to /mnt/secretfs:

![image](https://user-images.githubusercontent.com/77326619/183654812-6ff7e2b7-7a24-4f9f-9425-9024013a3430.png)

![image](https://user-images.githubusercontent.com/77326619/183654827-8b2699d2-a9af-4c6c-ac7e-564946c8ed2b.png)

![image](https://user-images.githubusercontent.com/77326619/183654862-9f3d545d-2768-4ddc-abe9-582bd9c3c57a.png)

## Change data path directories in neo4j.conf

![image](https://user-images.githubusercontent.com/77326619/183654916-24fd646d-aa9f-4a05-a869-c0e4de396b47.png)

## Check status 

![image](https://user-images.githubusercontent.com/77326619/183654941-6b465790-19d1-4152-b1b2-5a7e35c24650.png)

sudo cryptsetup -v status neo4j_data

Run neo4j

./bin/neo4j start 

![image](https://user-images.githubusercontent.com/77326619/183655031-100ea5c3-5af1-48f1-9257-a835c9188944.png)

https://askubuntu.com/questions/1032546/should-i-use-luks1-or-luks2-for-partition-encryption







