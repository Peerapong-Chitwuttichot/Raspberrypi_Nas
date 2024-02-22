# Raspberry Pi Network Attached Storage (NAS)

รายละเอียด: โปรเจคนี้เป็นการนำตัวของ Raspberry Pi มาทำเป็น NAS เพื่ออำนวยความสะดวก และมีความสามารถในการจัดการไฟล์สิทธิ์การเข้าถึงไฟล์ รวมไปถึงลดค่าใช้จ่าย

โดยจะทำการแบ่ง การ Setup หรือ ติดตั้ง ออกเป็นส่วนดังนี้

----------------------------------------------

# Raspberry Pi

1.ทำการแฟลช OS อย่าง Raspbian ลงไปยังตัว SD CARD หรือ Flash Drive หรือ Thumb Drive (เพื่อที่จะนำไป Boot OS กับตัว Raspberry Pi)
ในที่นี้เราใช้เป็น 


![image](https://github.com/Peerapong-Chitwuttichot/Raspberrypi_Nas/assets/142074845/fca8d860-d0ea-4201-a2c4-3d57bb798f02)

link: https://www.raspberrypi.com/software/


2.นำไปต่อเข้ากับ Raspberry Pi และทำการ Boot OS

----------------------------------------------

# Raspbian

1.เมื่อสามารถติดตั้งหรือเปิด Raspbian ได้แล้ว ให้นำตัวของ SSD|HDD|External SSD or HDD มาต่อ และทำการตรวจเช็ค

*ทำการอัพเดทก่อน


```
sudo apt update
sudo apt upgrade
```



![image](https://github.com/Peerapong-Chitwuttichot/Raspberrypi_Nas/assets/142074845/636af61a-a07a-472c-bf3b-5bd9be016666)


2.ทำการสร้าง Partition (จากรูป 2.1) และ Format Partition (จากรูป 2.2) เป็น ext4 เพื่อให้ตัว Raspbian รองรับการทำงาน


```
sudo fdisk /dev/xxx | สร้าง Partition


sudo mkfs.ext4 /dev/xxx1 | Format Partition


sudo nano /etc/fstab
/dev/xxx1 /mnt/xxx1/ ext4 defaults,noatime 0 1 | ตั้งค่าการ Config Partition ให้ทำการ Mount


sudo mount /dev/xxx1 | mount Partition


sudo mkdir /mnt/sda1/shared
sudo chmod -R 777 /mnt/sda1/shared | สร้างและให้สิทธิ์ Folder
```


รูปที่ 2.1

![image](https://github.com/Peerapong-Chitwuttichot/Raspberrypi_Nas/assets/142074845/411d57fe-43d3-41be-b29c-31f110ac47d6)


รูปที่ 2.2

![image](https://github.com/Peerapong-Chitwuttichot/Raspberrypi_Nas/assets/142074845/4686a9f5-4e4c-4410-9453-26fb24fd1cc2)

3.ทำการ Mount และ ตั้งค่าการ Mount


![image](https://github.com/Peerapong-Chitwuttichot/Raspberrypi_Nas/assets/142074845/83bd3b64-a560-4654-95a9-cbb6d4599f63)


4.ทำการให้สิทธิ์แก่ Folder ที่จะแชร์


![image](https://github.com/Peerapong-Chitwuttichot/Raspberrypi_Nas/assets/142074845/dfe2ed10-b135-4adf-923d-df1093b6a95f)

# Raspbian --> ติดตั้ง Samba Package

Note Samba เป็นตัวกลางที่เราจะนำมาใช้ ในการที่จะให้ Device ต่างๆ เข้าถึงข้อมูลใน Storage ของเรา ผ่านเครื่อข่าย(Network) ได้

1.ติดตั้ง Samba Package


```
sudo apt install samba samba-common-bin | ติดตั้ง Samba Package
```

2.ทำการ Setting Config Samba (ตั้งค่าสิทธิ์ต่างๆ และ การทำงานของ NAS)


```
sudo nano /etc/samba/smb.conf


[shared]
path=/mnt/sda1/shared
writeable=Yes
create mask=0777
directory mask=0777
public=no
```

3.ทำการ Restart Samba


```
sudo systemctl restart smbd
```



![image](https://github.com/Peerapong-Chitwuttichot/Raspberrypi_Nas/assets/142074845/030f79a6-961b-40fd-9924-19d03dd86438)




# Raspbian --> ติดตั้ง Samba Package --> ให้สิทธิ์

1.เพิ่มสิทธิ์ให้ User และ ตั้งค่า Password ให้สามารถเข้าถึง NAS ของเราได้


```
sudo smbpasswd -a username | แทนที่ username ด้วยชื่อ User
```


เท่านี้ก็จะสามารถเข้าถึง Raspberry Pi NAS ของเราได้ ผ่าน OS ต่างๆ เช่น Window , macOS , iOS , Andriod(ใช้แอพ)



![image](https://github.com/Peerapong-Chitwuttichot/Raspberrypi_Nas/assets/142074845/25a99346-6c13-4118-81f8-0631b4263843)






![image](https://github.com/Peerapong-Chitwuttichot/Raspberrypi_Nas/assets/142074845/01654cb8-d1d4-4af7-b82f-71209aac0ce2)






# รูปชิ้นงาน (ของฉันเอง)




![aaa](https://github.com/Peerapong-Chitwuttichot/Raspberrypi_Nas/assets/142074845/2ba6143b-f33d-412b-9856-ac62c0c6305a)







อ้างอิงข้อมูลจาก :  https://www.raspberrypi.com/tutorials/nas-box-raspberry-pi-tutorial/



