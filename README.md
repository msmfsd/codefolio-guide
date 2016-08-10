## <a name="apiguide"></a>Codefolio + Codefolio API Guide

![alt text](https://raw.githubusercontent.com/msmfsd/es2015-example/master/cf-logo%402x.png "Codefolio logo")

 1. [Project overview](#guide1)
 2. [Install Codefolio API locally](#guide2)
 3. [Install Codefolio locally](#guide3)
 4. [Edit your folio with Codefolio CMS](#guide4)
 5. [Configure Codefolio API for production](#guide5)
 6. [Configure Codefolio for production](#guide6)
 7. [Production server requirements](#guide7)
 8. [Publish your folio to production server (Linux)](#guide8)
 9. [Technology credits](#guide9)

## <a name="guide1">1 - Project overview
Codefolio project is an open source build-your-own folio website & CMS for developers to showcase their skills and work. It is an online CV heavily skewed towards developers with features such as:

#### Codefolio API
 - RESTful Node API backend with NoSQL
 - CORS security & server logs
 - Public API endpoints secured with API key
 - Private API endpoints secured with JWT auth

#### Codefolio
 - Single-page React/Redux frontend application
 - Responsive mobile-first layout and functionality
 - Easy to use CMS to fully personalise your folio
 - Full URL routes via React Router and browser history
 - Modern design based on Material Design Light
 - Developer profile with avatar, links & technology icons
 - Gravitar integration
 - Unlimited developer projects
 - Project repo links
 - Project repo stars
 - Project technology chips
 - Project code snippet display

> Codefolio & Codefolio API are seperate projects that connect with each other to create your developer folio. Codefolio is the static front-end website & CMS, built with React, that displays your folio to the public and allows you to login and edit it. Codefolio API is a RESTful API server, built with NodeJS and MongoDB, that performs CRUD operations on data requested by your Codefolio.

#### Why have a seperate frontend and backend?

The API and front-end application are separated for a few reasons:

1. Large projects are easier to manage with seperate repos.
2. Faster page loading, security and best practice - [see this article](https://medium.com/@keithwhor/how-to-build-a-single-page-application-web-stack-that-works-the-baa-architecture-25c1ad941097#.b1pvnyigl).
3. To showcase a CMS with an external API built with only Javascript.

**To publish your folio to a production server you will need some experience with server admin. On steps 5 to 8 this guide documents deploying to a single Linux server with Node running on port 8080 and Apache running on default port 80. You could easily deploy to many other server configurations like publishing your API to a Heroku Node dyno and your static frontend to Nginx http server.**


## <a name="guide2">2 - Install Codefolio API locally

- Go to the [Codefolio API](https://github.com/msmfsd/codefolio-api) repo on Github and follow the README file
- Use Postman app or your browser to test the API on http://localhost:8080/

## <a name="guide3">3 - Install Codefolio locally

- Go to the [Codefolio](https://github.com/msmfsd/codefolio) repo on Github and follow the README file
- Open http://localhost:3000/ in your browser

## <a name="guide4">4 - Edit your folio with Codefolio CMS

Once you have installed and configured your Codefolio API and your Codefolio then [http://localhost:3000/](http://localhost:3000/) will display a dummy profile and no projects. To modify and create your folio follow these steps:

1. Create your administrator by loading [http://localhost:3000/register](http://localhost:3000/register). Ensure you enter your email correctly and remember your password.
2. Login to the CMS admin area with your credentials by loading [http://localhost:3000/login](http://localhost:3000/login).
3. The admin dashboard at [http://localhost:3000/admin](http://localhost:3000/admin) contains links to edit your administrator, profile and projects.
4. First you should update your profile by selecting the Profile Settings `EDIT` button. Set your avatar, update all your details and contact, location and web links. Dont forget to set your tech icons to the technologies you use as a developer.
5. Now return to the admin dashboard and add some projects to showcase your work by clicking on the `CREATE NEW PROJECT` button in the projects area - add some screenshots, description, links and a code snippet for optimum display.
6. Your created projects will appear in the admin dashboard and can be edited and deleted.

## <a name="guide5">5 - Configure Codefolio API for production

##### NODE ENV VARS
When you installed your local Codefolio API project you would have renamed the ***.env.example*** file to ***.env*** and edited the variables to your environment. Your .env file will need to be uploaded to your production server so read the comments in the file carefully and make sure all the values are correct.

## <a name="guide6">6 - Configure Codefolio for production

##### CONFIG
When you installed your local Codefolio project you would have copied the API_KEY value from your Codefolio API .env file to ***config.js***. For production make sure you edit the API_PROD_URL with your production node server URL eg. ***http://your-production-api-node-server.com:8080***

## <a name="guide7">7 - Production server requirements

The scope of this guide is currently limited to publishing your Codefolio to a single Linux Ubuntu server and we will install current versions of Apache, Node, NPM and Mongo. You could deploy to many types of servers in many different ways. To follow this guide exactly you will need:

- Linux server
- Ubuntu 14+
- Sudo user & ssh access
- Domain name pointing to your server

## <a name="guide8">8 - Publish your folio to production server (Linux)

> TIP: For my production server I created an [AWS EC2](https://aws.amazon.com/ec2/) instance with an [AWS Route 53](https://aws.amazon.com/route53/) domain name connected to an [AWS Elastic IP](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html).

1. SSH into your production server as a sudo user:

  `$ ssh yourusername@yourprodserver`

2. Install git if you haven't done so previously:

  `$ apt-get install git`

3. Install Apache if you haven't done so previously:

  `$ sudo apt install apache2`

4. Go your home user directory, mine is named ubuntu but for the rest of this guide we will call it username, make sure you insert your actual user folder name for these commands:

  `$ cd /home/yourusername`

5. Clone or FTP your Codefolio API repo (here I am using the project repo on github so from here on our API directory will be called **codefolio-api**):

  `$ git clone https://github.com/msmfsd/codefolio-api.git`

6. You should now have your API code deployed to **/home/yourusername/codefolio-api/**

7. If your production server does not have Node & NPM then install them:

  `$ curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -`

  `$ sudo apt-get install -y nodejs`

8. Now install MongoDB v3+ for Ubuntu 14 by following the official Mongo guide: https://docs.mongodb.com/v3.0/tutorial/install-mongodb-on-ubuntu/

9. Ensure MongoDB service is started:

  `$ sudo service mongod start`

10. Move into your API directory:

  `$ cd /home/yourusername/codefolio-api`

11. Install project dependencies:

  `$ npm install`

12. You could now just run **node server.js** to fire up the API though I highly recommend you install [pm2](http://pm2.keymetrics.io/docs/usage/quick-start/ "pm2") to manage that for us and ensure node is always running:

  `$ sudo npm install pm2@latest -g`

13. Now tell pm2 to start the server, monitor it and restart it if needed:

  `$ pm2 startup`

14. Test your API is up and running by loading your domain server + our Node port eg. http://my-prod-server.com:8080. It should return:

  `{"success":true,"message":"API"}`

15. OK the API is complete!! Lets get the static Codefolio website/CMS uploaded to connect to that API - create a sites folder to house it:

  `$ mkdir /home/yourusername/sites`

16. Go to the that directory:

  `$ cd /home/yourusername/sites`

17. If you haven't already then create your Codefolio production build on your local machine by running `npm run build` - that will create a **dist** folder ready for deploying.

18. Upload that entire ***dist*** folder via FTP or git to your production server's sites folder we just created. So the root folder for your Codefolio site will now be **/home/yourusername/sites/dist**

18. Now we need to point Apache to that location to serve the static files - usually it will be pointing to /var/www/html or some other directory. Open your Apache sites default config file:

  `$ vim /etc/apache2/sites-available/000-default.conf`

19. Edit the file - first by replacing the DocumentRoot line with the folling line:

  `DocumentRoot /home/yourusername/sites/dist`

20. And second by pasting the following code before the closing VirtualHost tag:
	```
  <Directory /home/yourusername/sites/dist>
	      Options Indexes FollowSymLinks MultiViews
	      AllowOverride All
	      Order allow,deny
	      allow from all
	</Directory>
  ```

21. Save and close the config file.

22. To allow full application routing, browser refresh and direct page loads of projects via HTML5 browser history and React Router we need to add a .htaccess file to your folio sites root folder:

  `$ vim /home/yourusername/sites/dist/.htaccess`

23. Paste the following into that newly created .htaccess file:

  ```
  RewriteEngine on
	RewriteBase /
	RewriteRule ^index\.html$ - [L]
	RewriteCond %{REQUEST_FILENAME} !-f
	RewriteCond %{REQUEST_FILENAME} !-d
	RewriteRule . /index.html [L]
  ```

24.  Save and close the .htaccess file.

25. We now need to enable Apache mod_rewrite:

  `$ sudo a2enmod rewrite`

  `$ sudo service apache2 restart`

26. You may need to restart the API node process via pm2:

  `$ cd /home/yourusername/codefolio-api`

  `$ pm2 restart all`

27. Now open your Codefolio by loading your domain in a browser: [http://my-prod-server.com](http://my-prod-server.com)

28. To use the CMS remember to first register your administrator: [http://my-prod-server.com/register](http://my-prod-server.com/register)

28. You can now edit your production folio by logging in to the admin: [http://my-prod-server.com/login](http://my-prod-server.com/login)

## <a name="guide9">9 - Technology credits

Codefolio is built with:

##### Codefolio API

- Node Js
- Express Js
- Passport Js
- MongoDb
- Mongoose

##### Codefolio

- React
- Redux
- React Router
- Redux Form
- React CSS Modules
- PostCSS
- Highlight Js
- Materialize CSS
- Webpack
