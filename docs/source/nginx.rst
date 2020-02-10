Hosting websites using Nginx
============================

Hosting a static website
------------------------

Installing and lauching Nginx
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This section is based on this `tutorial <https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-debian-10>`_ from Digitalocean.

We'll start by installing Nginx::

   $ sudo apt update
   $ sudo apt install nginx

If everything goes right, Nginx should have launched itself.

You can check its status using::

   $ systemctl status nginx | grep 'Active: .* \(.*\)'

If you see ``Active: active (running) since ...``, it means Nginx is running!
Otherwise try to run::

   $ systemctl start nginx # this may requires sudo rights

Setting Nginx to serve the files
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

For the purpose of this tutorial, we will generate a doc using Sphinx (see the tutorial `here <http://docs.bastien-antoine.fr/docs_sphinx.html>`_ to know how to do so).

The first step is to setup the configuration file for Nginx.

Start by creating a new file inside ``/etc/nginx/sites-available/``::

   $ cd /etc/nginx
   $ vim sites-available/docs.my-website.com

.. note::

   You can name your configuration file what you want but it's highly suggested to use a distinctable name. For example if the files are meant to be served under ``docs.my-website.com``, you can (and should) name it ``docs.my-website.com``.

In this file, enter the following::

   server {
      listen 80;
      listen [::]:80;

      root /root/folder/of/the/docs/build;
      index index.html index.htm;

      server_name docs.mywebsite.com;

      location / {
         try_files $uri $uri/ =404
      }
   }

Customize the following values based on your configuration:

- ``root``: the full path for the ``build`` folder of your Sphinx project
- ``server_name``: the url under which the files will be accessed, usually the IP address of you server, or its domain name

We can now try our new file and enable it!

We need to start by makinga symlink of the file we created inside ``/etc/nginx/sites-enabled/``::

   $ ln -s /etc/nginx/sites-available/docs.my-website.com /etc/nginx/sites-enabled/

.. note::

   Be sure to use full path when creating the symlink, and not absolute paths.

You can test the configuration file you added by typing::

   $ nginx -t

If you see::

   nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
   nginx: configuration file /etc/nginx/nginx.conf test is successful

It's all good, and we're only a few steps away from having our files served!

We can now reload nginx to enable the new configuration file::

   $ systemctl reload nginx # this may requires sudo rights

We check that everything was realoaded fine::

   $ systemctl status nginx # this may requires sudo rights

If there's no problem, the files are served properly!

Hosting a Django project using Gunicorn
---------------------------------------
To be filled soon!
