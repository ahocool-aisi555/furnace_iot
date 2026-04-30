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


Metode tampilan : LED 4 digit 7-segmen <br>
Metode kontrol : ON/OFF, P, PI, PD, PID<br>
Spesifikasi Input : Thermocouple: K(CA), J(IC), E(CR), T(CC), B(PR), R(PR), S(PR), N(NN), C(TT), G(TT), L(IC), U(CC), Platinel II<br>
RTD: DPt100Ω, DPt50Ω, JPt100Ω, Cu100Ω, Cu50Ω, Nikel 120Ω<br>
Analog: 0-100mV, 0-5V, 1-5V, 0-10V
0-20mA, 4-20mA<br>
Siklus sampling : 50ms<br>
Output kontrol 1 : Relai(250VAC~ 3A)<br>
Pilihan output : Alarm 1, Transmission(DC4-20mA)<br>
Catu daya : 100-240VAC~ 50/60Hz<br>
Komunikasi Data : Rs485 Modbus RTU<br>
Tingkat proteksi : IP65(panel depan)<br>

[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/-z_Roo9Zg1I/0.jpg)](https://www.youtube.com/watch?v=-z_Roo9Zg1I) <br> kilk gambar untuk ke video youtube
                                                                                              

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
RS485 dapat di model cascade ke banyak perangkat  dengan jarak jauh ( ~1 km). 

# Akses TK4S melalui Modbus ke Protos PW11

IOT Gateway Protos PW11 tidak memiliki mode Modbus Master seperti halnya pada model PW21, dan seting untuk dapat akses di level modbus-RTU nya TK4S tidak bisa lewat web console.
Solusinya dibutuhkan download software console IOT master dari website nya protos (jika masih ada websitenya, saya akan simpan di drive jika membutuhkan).<br>
Untuk mengakses TK4S secara modbus RTU di mode slave maka dapat diakali dengan menulis His script, dan inipun setelah berkutat lama mencari caranya (syukur ketemu).<br>
Berikut ini sekilas script untuk akses TK4S dan kemudian dilempar ke profile Gateway bernama netp.<br>

```pseudocode
#Array urutan modbus RTU untuk perintah baca suhu pada TK4S
baca_suhu = [0x01, 0x04, 0x03, 0xE8, 0x00, 0x01, 0xB1, 0xBA]

#routine ketika ada data masuk di uart lalu parsing datanya
RECV UART uart0
    RAW = INPUT
    LEN = RAW.length()

    IF (LEN < 7) #Jika lebar data kurang
        RETURN(FALSE)
    END
	
	  ADDR_HEX = RAW.charAt(0)
	  FC = RAW.charAt(1)
	  BYTE_SIZE = RAW.charAt(2)
    
    IF (ADDR_HEX == 0x01)    # jika alamat modbus sesuai 0x01
 	    # contoh penerimaan data  01 04 02 00 1F F8 F8 
		
		  IF (FC != 0x04) # FC 04 = Read Input Registers 
			  RETURN(FALSE) 
		  END  
        
		  IF (BYTE_SIZE != 0x02) # 2 data bytes expected
			   RETURN(FALSE) 
	    END  

       # Extract high and low bytes (big-endian: [3]=high, [4]=low)
       TEMP_HIGH = RAW.charAt(3)
       TEMP_LOW = RAW.charAt(4)

       TEMP_HIGHH = TEMP_HIGH << 8
       TEMP_RAW = TEMP_HIGHH | TEMP_LOW
       PAYLOAD =  TEMP_RAW.prtString()

      SEND(SOCK, netp, PAYLOAD) # Kirim text suhu ke profile netp

	   END
	
	 RETURN(TRUE)
END

# Ambil data tiap 10 detik
TIMER read_temp 10000
     
    SEND(UART,uart0, baca_suhu) # Kirim modbus ke uart
END
```

# Monitoring IOT

[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/OqzBcb2V9yw/0.jpg)](https://www.youtube.com/watch?v=OqzBcb2V9yw)
<br> Klik pada gambar untuk ke youtubenya
# On Progress

..Harap bersabar masih dalam penyusunan github, pelan-pelan saya kumpulkan materinya
