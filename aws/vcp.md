# VPC (Virtual Private Cloud)
> คือ private network

## Subnets
> คือ Range IP ของ VPC สามารถแบ่งได้อย่างน้อง 1 เครือข่ายในแต่ละ Availability Zone

## Route Tables
> เส้นทางที่ใช้กำหนดตำแหน่งที่ใช้รับส่งข้อมูลเครือข่ายจาก Subnet หรือ Internet gateway

## Internet gateways
> ช่วยให้สามารถสื่อสารระหว่าง VPC กับอินเทอร์เน็ตได้

## DHCP Option Sets


## Elastic IPs

## Endpoints
> สามารถเชื่อมต่อระหว่าง VPC กับ บริการที่รับรอง โดยไม่ต้องใช้ Internet gateway, NAT device, VPN connection, or AWS Direct Connect connection  ดังนั้น VPC จะไม่ถูกเปิดเผยต่อ public internet

## Endpoint services
> สร้างการเชื่อมต่อระหว่าง VPC, VPC Endpoint, Load Balance มายัง Application

## NAT Gateway (Network Address Translation)
> เปิดเพื่อใช้งาน Instance ใน private subnet เพื่อเชื่อมต่อกับบริการภายนอก VPC แต่จะป้องกันไม่ให้บริการภายนอกมาเชื่อมต่อกับ Instance

 - Public NAT Gateway ช่วยให้เชื่อมต่อกับ Internet ได้
 - Private NAT Gateway ช่วยให้เชื่อมต่อกับ VPC อื่นหรือ on-premiss netwaork ได้

## Peering connections