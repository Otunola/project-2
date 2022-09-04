# project-2  LEMP STACK IMPLEMENTATION
1. After the ubunter server creation on AWS,start off by updating your server’s package index with the following command : sudo apt update
2. then intall NgiNx  web server with the command : sudo apt install nginx
3. confirm the status of the installation with the command : sudo systemctl status nginx 
as shown below
<img width="562" alt="Screen Shot 2022-09-03 at 4 48 53 PM" src="https://user-images.githubusercontent.com/112595648/188312297-9238003b-4f2d-47d0-8e56-ceced8028ba7.png">

4. open the TCP port 80 of the EC2 instance on AWS as shown below so as to allow traffic flow on port 80, 

<img width="1109" alt="Screen Shot 2022-09-03 at 5 06 32 PM" src="https://user-images.githubusercontent.com/112595648/188312380-b413881f-d939-4239-9400-f856d6cfbea4.png">

5. checked if can access the port 80 from the ubuntu with the command : curl http://127.0.0.1:80
<img width="565" alt="Screen Shot 2022-09-03 at 5 19 06 PM" src="https://user-images.githubusercontent.com/112595648/188313080-f70d8f6a-7f44-48be-863c-f99365b86d05.png">

6. also try to access the server ip address on the URL

<img width="1095" alt="Screen Shot 2022-09-03 at 5 20 45 PM" src="https://user-images.githubusercontent.com/112595648/188313308-1af44cc6-1a7d-4b31-9d57-cce941377466.png">
