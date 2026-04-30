# Furnace PID IOT Control System
Perancangan Kontrol Pengapian Gas Furnace / Peleburan Logam Dengan Kontrol PID dan IOT<br>
Bekerjasama dengan Lab Teknik Mesin Undiksha - Singaraja Bali<br>
Berhasil Masuk Final Nasional Program Penelitian Laboran <br>

<img width="921" height="657" alt="github1" src="https://github.com/user-attachments/assets/c9ba87b0-19b7-4c2c-8ec5-978805e34c9d" />

# Cara Kerja Sistem Secara Umum

1. Alat yang dirancang menggunakan alat kontrol industri siap pakai tanpa melibatkan mikrokontroler <br>
2. Sistem akan melakukan kontrol aliran gas ke pembakar / burner dengan output suhu yang dapat ditentukan pengguna dan akan dicapai suhu yang sesuai pengan prinsip PID<br>
3. Sistem juga akan memberikan pelaporan secara IOT melalui Modbus IOT Gateway ke monitoring yang dapat di remote dari manapun
4. Terdapat pengaman sensor kebocoran gas yang akan memutus aliran gas jika terdeteksi<br>

<img width="2258" height="1584" alt="flowchart_gun" src="https://github.com/user-attachments/assets/482c074c-f88c-44a8-af09-e9842c6ab972" />



# Layout Diagram

<img width="2258" height="1584" alt="diag_gun" src="https://github.com/user-attachments/assets/788b28db-01b7-43f6-aacc-e53dd445450f" />


<br>

<img width="2272" height="1557" alt="sch_furnace" src="https://github.com/user-attachments/assets/b9f1753c-5cb2-4c98-80d1-01960d4aa069" />

# Kontrol Suhu PID TK4S

[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/-z_Roo9Zg1I/0.jpg)](https://www.youtube.com/watch?v=-z_Roo9Zg1I)

# Protos PW 11 IOT Gateway

<img width="697" height="560" alt="protosPW11" src="https://github.com/user-attachments/assets/624044dc-5baf-43dc-a044-bb15d649137c" />

<br>
Specs: <br>
802.11bgn Wireless Standard <br>
STA/AP/AP+STA Mode <br>
TCP/UDP/MQTT/HTTP/WebSocket Protocol <br>
Modbus TCP to RTU, Modbus Master Function <br>
RS485 To WiFi Conversion <br>
Webpage Easy Configuration or PC IOTService Tool <br>
Security Protocol Such As TLS/AES/DES3 <br>
Industrial Temperature: -40 to +85˚ C <br>

Seri PW11 hanya sebagai jembatan komunikasi Modbus RTU (PLC) menuju komunikasi berbasis TCP/IP.
Butuh PLC kontroler sebagai Modbus master atau alat I/O modbus dapat me-schedule pengiriman data.
RS485 dapat di model cascade ke banyak perangkat  dengan jarak jauh ( ~1 km). Akan dibahas cara settingnya pada github tersendiri.  


# Monitoring IOT

[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/OqzBcb2V9yw/0.jpg)](https://www.youtube.com/watch?v=OqzBcb2V9yw)

# On Progress

..Harap bersabar masih dalam penyusunan github
