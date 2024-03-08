# Ansible-httpd-documentRoot

🌟 Change default DocumentRoot in Apache by using Ansible Automation 🌟

![Screenshot 2024-03-08 154000](https://github.com/Pratikshinde55/Ansible-httpd-documentRoot/assets/145910708/ba936d35-a6b9-41fd-89e5-b99b25c5b8f7)



💫Apache webserver(httpd):

It is a free and open-source web server software. It's one of the most widely used web servers in the world, known for its reliability and robustness.
Apache is capable of serving static and dynamic content on the World Wide Web.

" /etc/httpd/ " or  "/etc/apache2/ directory" is common location 


                In this project , i can create custom Documet Root in httpd by using ansible 

❄️ By Ansible Adhoc command: (manual way) ❄️

1. Install httpd package :

        #yum install httpd -y

2.  Go inside Httpd internal configuration file , In this file we can create custom documentRoot, "/etc/httpd/conf.d" this httpd Default configuration file location:


        #cd /etc/httpd/conf.d

3. Create own name or custom name folder inside Httpd configuration file("/etc/httpd/conf.d"), i am creating "my.conf" file & in this file i put my custom documentRoot:

   my.conf file put :-->  DocumentRoot  /var/www/pratik


        #vi  my.conf

Now apache web server read from this filder , It means from browser we connect this apache filder & in this folder we deploy own webpages.

4. After file created then need to restart httpd service (when new settings created ):


       #systemctl reload httpd



❄️ By Ansible automation : ( Ansible-playbook) ❄️



















   
