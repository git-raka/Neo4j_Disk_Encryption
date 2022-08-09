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
