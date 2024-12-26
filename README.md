# Change default DocumentRoot of Apache WEb server by using Ansible Automation:
![Screenshot 2024-03-08 154000](https://github.com/Pratikshinde55/Ansible-httpd-documentRoot/assets/145910708/ba936d35-a6b9-41fd-89e5-b99b25c5b8f7)

### Apache webserver(httpd):
It is a free and open-source Web server Software. It's one of the most widely used web servers in the world, known for its reliability and robustness.
Apache is capable of serving static and dynamic content on the World Wide Web.

- Note: 

" /etc/httpd/ " or  "/etc/apache2/ directory" is common location httpd configuration file.

In this project, I can create custom Document Root in httpd by using ansible-playbook

## By Ansible Adhoc command: [Manual way]

1. Install httpd package :

       yum install httpd -y

2. Go inside Httpd internal configuration file , In this file we can create custom documentRoot, "/etc/httpd/conf.d" this httpd Default configuration file location:

       cd /etc/httpd/conf.d

3. Create own name or custom name folder inside Httpd configuration file("/etc/httpd/conf.d"), i am creating "my.conf" file & in this file i put my custom documentRoot:

my.conf file put :-->  DocumentRoot  /var/www/pratik

       vi  my.conf

Now apache web server read from this filder , It means from browser we connect this apache filder & in this folder we deploy own webpages.

4. After file created then need to restart httpd service (when new settings created ):

       systemctl reload httpd

## By Ansible automation:  [Ansible-playbook]

I take three AWS Cloud instances Amazon linux EC2, One instance make Ansible-Master & remaining Instances to make hosts or managed nodes.

![Screenshot 2024-03-07 193702](https://github.com/Pratikshinde55/Ansible-httpd-documentRoot/assets/145910708/eff832e0-d874-4ad5-8013-4a27da87aed4)

In ansible-master node install ansible-core and coonect with target node by ssh key Authentication :

https://github.com/Pratikshinde55/Ansible-setup-onAWS.git (Refer this link for SSH connection )

1. Create file or web page which will weploy ,(index.html) on ansible master node:

![Screenshot 2024-03-07 194123](https://github.com/Pratikshinde55/Ansible-httpd-documentRoot/assets/145910708/53254826-2125-4971-ba8e-f3d1a952c6ca)

2. Create playbook ( "myweb.yml" my playbook name) :

       vim myweb.yml

![Screenshot 2024-03-07 194019](https://github.com/Pratikshinde55/Ansible-httpd-documentRoot/assets/145910708/232536a4-f6dd-4919-a208-b6dc840f24c0)

- playbook explanation:

 A. Here set variables (vars) which for my tasks :-

 "webpage" variable for my local code or webpage which want to copy and deploy
 " packageName" variable for package that i want to install
 " documentDir" variable for my own custom documentRoot


            - hosts: web
              vars: 
                webpage: "index.html"
                packageName: "httpd"
                documentDir: "/var/www/pratik"


 B. Install Httpd package here use variable packageName


          tasks:
            - name: "Install httpd package"
              package:
                name: "{{ packageName }}"  
                state: present

 C.  create document root : 
  
 File module is used to create directory & use state is directory,  This Task createte directory as /var/www/pratik in ansible target nodes :


          - name: "Create Document root for httpd package"
            file: 
              state: directory
              path: "{{ documentDir }}"

 D. This is very important step Httpd configuration file change: (DocumentRoot)
 
 Here Copy module used to  create "my.conf" file at destination "/etc/httpd/conf.d" & and in this "/etc/httpd/conf.d/my.conf" file 'content' attribute
 create DocumentRoot = documentDir "/var/www/pratik":  



           - name: "Create or copy new path in config file"
             copy:
               dest: /etc/httpd/conf.d/my.conf
               content: |
                 DocumentRoot {{ documentDir }}


 E. Deploy local webpage "index.html to target nodes destination documentDir "/var/www/pratik/"


          - name: "Deploy webpage"
            copy:
              src: "{{ webpage }}"
              dest: "{{ documentDir }}"


 F. Reload httpd service, After any change make in httpd configuration file then need to reload or resatrt httpd service then new settings apply:


         
          - name: "Reload service httpd"
            service:
              name: "httpd"
              state: reloaded
              enabled: true


Step-4. Run ansible playbook 


        #ansible-playbook myweb.yml


![Screenshot 2024-03-07 193938](https://github.com/Pratikshinde55/Ansible-httpd-documentRoot/assets/145910708/0469443f-6110-4918-9f5e-ab404410d3a3)


⚡Here we can see that automation takes place on Manged node1 & node2 :

At ansible target nodes check what changes happened:

![Screenshot 2024-03-07 194252](https://github.com/Pratikshinde55/Ansible-httpd-documentRoot/assets/145910708/64df6891-ed15-4ac4-a3d2-b59e2c7b2023)


![Screenshot 2024-03-07 194309](https://github.com/Pratikshinde55/Ansible-httpd-documentRoot/assets/145910708/bd7ecaeb-cae0-4451-b880-3525d5a60f25)


🌟 From google or outside world we can able to connect target NOde1 & node2:


Ansible Target node 1 : public IP of EC2 instance : node1 : 3.7.69.254

![Screenshot 2024-03-07 194326](https://github.com/Pratikshinde55/Ansible-httpd-documentRoot/assets/145910708/dd65746a-2ca4-4f91-b27c-33fe6eadf01a)


Ansible Target node 2 : public IP of EC2 instance : node2 : 13.201.93.84 

![Screenshot 2024-03-07 194334](https://github.com/Pratikshinde55/Ansible-httpd-documentRoot/assets/145910708/aea89e62-5bf1-4f8e-9bf6-5f413931c0ca)


