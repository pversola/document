### Load balancing methods

- **Round-Robin**: สลับวนไปเรื่อย ๆ ในแต่ละ request
  ```
  upstream www {
    server x.x.x.11:81;
    server x.x.x.12:81;
    server x.x.x.13:81;
  }
  ```
- **Least-Connection**: request ถัดไปจะถูกส่งไปยัง server ที่มี active connection น้อยที่สุด
  ```
  upstream {
    least_conn;
    server x.x.x.11:81;
    server x.x.x.12:81;
    server x.x.x.13:81;
  }
  ```
- **IP-hash**: ใช้ ip address ของ client เป็นตัวตัดสินใจว่าจะส่ง request ไปยัง server ไหน ต้องใช้วิธีนี้เพื่อที่จะทำ Session persistence
  ```
  upstream {
    ip_hash;
    server x.x.x.11:81;
    server x.x.x.12:81;
    server x.x.x.13:81;
  }
  ```

### Weighted load balancing

เป็นการส่ง request ไปยัง server แบบมีน้ำหนัก จะถูกส่ง request มากกว่า server อื่น

```
upstream {
  server x.x.x.11:81 weight=3;
  server x.x.x.12:81;
  server x.x.x.13:81;
}
```

### Health checks

nginx จะใช้ request จริงในการตรวจสอบว่า upstream server ยังใช้งานได้อยู่หรือไม่โดยมีตัวแปรสองค่าที่เดี่ยวข้องคือ

- **max_fails**: คือจำนวนครั้งที่ไม่สามารถติดต่อ server ได้แล้วจะถือว่า server นั้น down, server มี down จะไม่ถูกส่ง request ไปอีก ค่า default คือ 1
- **fail_timeout**: คือระยะเวลาที่หลังจาก server down แล้วจะส่ง request ไปตรวจสอบว่ายังติดต่อได้หรือไม่ถ้าติดต่อได้จะถือว่า server up มาอีกครั้งและส่ง request ไปยัง server นั้นตามปกติ ค่า default คือ 10 วินาที

```
upstream {
  server x.x.x.11:81 max_fails=3 fail_timeout=15s;
  server x.x.x.12:81;
  server x.x.x.13:81;
}
```
