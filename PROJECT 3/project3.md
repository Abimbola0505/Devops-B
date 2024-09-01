# Documentation

Please reference Project1 for guidance on spinning up an Ubuntu server, as well as creating and associating an elastic IP address with your server, among other tasks.

- Spin up your 3 ubuntu servers. Ensure you clearly name them so you don't make mistakes.

![onee](images/onee.png)

# Install Nginx and Setup Your Website

- Download your website template from your preferred website by navigating to the website, locating the template you want.

- Right click and select Inspect from the drop down menu.

![two](images/two.png)

- Click on the Network tab.

- Click the Download button

![three](images/three.png)

- right click on the website name  that has .zip at the end i.e 2135_mini_finance.zip

-Select Copy and click on Copy URL.

![four](images/four.png)

- To install Nginx, execute the following commands on your terminal.

sudo apt update

sudo apt upgrade

sudo apt install nginx

- Start your Nginx server by running the sudo systemctl start nginx command, enable it to start on boot by executing sudo systemctl enable nginx, and then confirm if it's running with the sudo systemctl status nginx command.

![five](images/five.png)

**!! NOTE**

- Install Nginx on both web server terminals. These are the terminals you're using to manage the servers hosting the two distinct website contents that the load balancer will distribute traffic to.

- Visit your instances IP address in a web browser to view the default Nginx startup page.

- Execute sudo apt install unzip to install the unzip tool and run the following command to download and unzip your website files sudo curl -o /var/www/html/2135_mini_finance.zip https://www.tooplate.com/zip-templates/2135_mini_finance.zip && sudo unzip -d /var/www/html/ /var/www/html/2135_mini_finance.zip && sudo rm -f /var/www/html/2135_mini_finance.zip.

![six](images/six.png)


![seven](images/seven.png)

- o set up your website's configuration, start by creating a new file in the Nginx sites-available directory. Use the following command to open a blank file in a text editor: sudo nano /etc/nginx/sites-available/finance.

- Copy and paste the following code into the open text editor.

server {
    listen 80;
    server_name example.com www.example.com;

    root /var/www/html/example.com;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}

- Edit the root directive within your server block to point to the directory where your downloaded website content is stored.

![eight](images/eight.png)

- Save the edited root directive by clicking cntrl X on your keyboard, type Y for yes. lastly click enter .

- Create a symbolic link for both websites by running the following command. sudo ln -s /etc/nginx/sites-available/finance /etc/nginx/sites-enabled/

![nine](images/nine.png)

- Run the sudo nginx -t command to check the syntax of the Nginx configuration file, and when successful run the sudo systemctl restart nginx command.

![ten](images/ten.png)

![eleven](images/eleven.png)

- Repeat the process for the second website.

**NOTE!!!**

To better identify the impact of your changes, connect to the second server in a new terminal window. There, update the website content with something different. When you reload your website, the load balancer will distribute traffic, potentially sending you to the updated version on the second server. This will make the differences between the two versions clearer.

- Download your website template for **interior** from your preferred website by navigating to the website, locating the template you want.

- Right click and select Inspect from the drop down menu.

- Click on the Network tab and Click the download button 


![twelve](images/twelve.png)

- Right click on the website name that ends with .zip and Select Copy and click on Copy URL.

![thirteen](images/thirteen.png)

- To install Nginx, execute the following commands on your terminal.

sudo apt update

sudo apt upgrade

sudo apt install nginx

![fourteen](images/fourteen.png)

- Start your Nginx server by running the sudo systemctl start nginx command, enable it to start on boot by executing sudo systemctl enable nginx, and then confirm if it's running with the sudo systemctl status nginx command.

![fifteen](images/fifteen.png)

**NOTE:** To better identify the impact of your changes, connect to the second server in a new terminal window. There, update the website content with something different. When you reload your website, the load balancer will distribute traffic, potentially sending you to the updated version on the second server. This will make the differences between the two versions clearer.

- Execute sudo apt install unzip to install the unzip tool and run the following command to download and unzip your website files sudo curl -o /var/www/html/2133_moso_interior.zip https://www.tooplate.com/zip-templates/2133_moso_interior.zip && sudo unzip -d /var/www/html/ /var/www/html/2133_moso_interior.zip && sudo rm -f /var/www/html/2133_moso_interior.zip

![sixteen](images/sixteen.png)

![seventeen](images/seventeen.png)

- To set up your website's configuration, start by creating a new file in the Nginx sites-available directory. Use the following command to open a blank file in a text editor: sudo nano /etc/nginx/sites-available/interior.

- Copy and paste the following code into the open text editor.

server {
    listen 80;
    server_name placeholder.com www.placeholder.com;

    root /var/www/html/placeholder.com;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}

- Edit the root directive within your server block to point to the directory where your downloaded website content is stored.

![eighteen](images/eighteen.png)

- Save the edited root directive by clicking cntrl X on your keyboard, type Y for yes. lastly click enter .

- Create a symbolic link for both websites by running the following command. sudo ln -s /etc/nginx/sites-available/interior /etc/nginx/sites-enabled/

![nineteen](images/nineteen.png)

- Run the sudo nginx -t command to check the syntax of the Nginx configuration file, and when successful run the sudo systemctl restart nginx command.

![twenty](images/twenty.png)

**NOTE!!!**

- On your first server, run sudo rm /etc/nginx/sites-enabled/default, and on your second server, run sudo rm /etc/nginx/sites-enabled/default. This will delete the default site-enabled folders and enable Nginx to serve content from your specified website directories. If you don't delete these default folders, you'll continue to see the default Nginx page.

![twenty1](images/twenty1.png)

![twenty2](images/twenty2.png)

- Run the sudo systemctl restart nginx command to restart your server.

- Check both IP addresses to confirm your website is up and running.

![twenty3](images/twenty3.png)

![twenty4](images/twenty4.png)

# Configure your Load balancer

- Install Nginx on the server you want to use as a load balancer i.e sudo apt update, sudo apt upgrade , sudo apt install nginx ,sudo systemctl start nginx, sudo systemctl enable nginx, and execute sudo systemctl status nginx to ensure it's running.

![twenty5](images/twenty5.png)

- Execute sudo nano /etc/nginx/nginx.conf to edit your Nginx configuration file

- Add the following within the http block.

upstream cloudghoul {
    server 1;
    server 2;
    # Add more servers as needed
}

server {
    listen 80;
    server_name <your domain> www.<your domain>;

    location / {
        proxy_pass http://cloudghoul;
    }
}


**Note!!!**

Replace the necessary placeholders as shown in the picture above. Substitute <server 1> and <server 2> with the actual PRIVATE IP addresses of your servers. Also, replace <your domain> www.<your domain> with your root domain and subdomain name, and update proxy_pass and the other relevant fields accordingly.

![twenty6](images/twenty6.png)

- Run sudo nginx -t to check for syntax error.

![twenty7](images/twenty7.png)

- Apply the changes by restarting Nginx: sudo systemctl restart nginx

![twenty8](images/twenty8.png)

# Create An A Record

To make your website accessible via your domain name rather than the IP address, you'll need to set up a DNS record. I did this by buying my domain from IONOS and then moving hosting to AWS Route 53, where I set up an A record.

**Note!!!**

Visit Project1 for instructions on how to create a hosted zone.

- Point your domain's A records to the IP address of your Nginx load balancer server.

- In route 53, select the domain name and click on Create record.

![twenty9](images/twenty9.png)

- Paste your IP address➀ and then click on Create records➁ to create the root domain.

![thirty](images/thirty.png)

![thirty1](images/thirty1.png)

- Click on create record again, to create the record for your sub domain.

- Paste your IP address➀, input the Record name(www➁) and then click on Create records➂.

![thirty2](images/thirty2.png)

![thirty3](images/thirty3.png)

- Go to the terminal you used in setting your first website and run sudo nano /etc/nginx/sites-available/finance to edit your settings. Enter the name of your domain and then save your settings.

![thirty4](images/thirty4.png)

- Save the setting by clicking control X , type Y and click enter. if done correctly , you should get the below screenshot.


![thirty5](images/thirty5.png)

- Restart your nginx server by running the sudo systemctl restart nginx command.

![thirty6](images/thirty6.png)

- Go to the terminal you used in setting your second website and run sudo nano /etc/nginx/sites-available/interior to edit your settings. Enter the name of your domain and then save your settings.

![thirty7](images/thirty7.png)

![thirty8](images/thirty8.png)

- Save the setting by clicking control X , type Y and click enter. if done correctly , you should get the below screenshot.

![thirty9](images/thirty9.png)

- Restart your nginx server by running the sudo systemctl restart nginx command.

![forty](images/forty.png)

- Go to your domain name in a web browser to verify that your website is accessible.

![forty1](images/forty1.png)

- Reload the webpage to ensure the load balancer distributes traffic evenly between your servers.

![forty2](images/forty2.png)

# Install certbot and Request For an SSL/TLS Certificate


- Install certbot on Loadbalancer because your A record was created using the loadbalancer . Install certbot on loadblancer by executing the following commands: sudo apt update sudo apt install python3-certbot-nginx

- Execute the sudo certbot --nginx command to request your certificate. Follow the instructions provided by certbot and select the domain name for which you would like to activate HTTPS.

- You should get a congratulatory message that says https has been successfully enabled.

![fort3](images/forty3.png)

- Access your website to verify that Certbot has successfully enabled HTTPS.

![forty4](images/forty4.png)

![forty5](images/forty5.png)

- It is recommended to renew your LetsEncrypt certificate at least every 60 days or more frequently. You can test renewal command in dry-run mode: sudo certbot renew --dry-run

![forty6](images/forty6.png)



# The End Of Project 3.





