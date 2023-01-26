Refer to:

  https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-gunicorn-and-nginx-on-ubuntu-22-04
  
for more information

# To test UWSGI locally

	# UserFireWall
	sudo ufw allow 5001 

	# Run server
	uwsgi --socket 0.0.0.0:5001 --protocol=http -w run:app

	# Test
	http://157.245.247.123:5001/

# Setup systemd unit file to automatically start uWSGI

	# Create file
	sudo nano /etc/systemd/system/gusmontanocom.service

		# Contents of file
		[Unit]
		Description=uWSGI instance to serve gusmontanocom
		After=network.target

		[Service]
		User=root
		Group=www-data
		WorkingDirectory=/gusmontanocom/
		Environment="PATH=/gusmontanocom/gusmontanocomenv/bin"
		ExecStart=/gusmontanocom/gusmontanocomenv/bin/uwsgi --ini gusmontanocom.ini

		[Install]
		WantedBy=multi-user.target

	# Start service
	sudo systemctl start gusmontanocom

	# Enable service (Difference against starting?)
	sudo systemctl enable gusmontanocom

	# Check status of service
	sudo systemctl status gusmontanocom

# Configuring NGINX

	# Create server block configuration

	sudo nano /etc/nginx/sites-available/gusmontanocom

	# Link NGINX to server block, check for errors and other stuff

```
  sudo ln -s /etc/nginx/sites-available/gusmontanocom /etc/nginx/sites-enabled
  sudo nginx -t
  sudo systemctl restart nginx
  sudo ufw delete allow 5001
  sudo ufw allow 'Nginx Full'
```

# See status

>`sudo systemctl status nginx.service`

# Restart when code changes have been made

>`sudo service gusmontanocom restart`
	
# View error logs

	`cd var/log/nginx`
	`cat error.log`
	
# Other resets

	```
	systemctl daemon-reload
	sudo systemctl restart nginx
	sudo systemctl reload nginx
	sudo service gusmontanocom restart
	
	```
	
