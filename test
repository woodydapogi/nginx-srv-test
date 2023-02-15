echo "Usecase: ./<script>.sh <name>.conf"

FILE_NAME=$1
NGINX_CONF="/etc/nginx/nginx.conf"
SITES="/etc/nginx/sites-available/"
SITES_ENABLED="/etc/nginx/sites-enabled/"
#IP_HOSTS=""

function main () {
        if [[ ! -f "$SITES/$FILE_NAME" ]]; then
                echo "$1 File Does Not Exist."
                cat > "$SITES$FILE_NAME" << EOF
server {
        listen 80 default_server;
        index index.html index.htm index.php;
        server_name _;

        root /var/www/html;

        location / {
                proxy_pass http://127.0.0.1;
        }

}
EOF

                ln -s "$SITES$FILE_NAME" "$SITES_ENABLED$FILE_NAME"

                nginx -t

                nginx -s reload
        fi
}

function check_nginx_service () {
        clear

        if systemctl --type=service --state=active | grep nginx.service; then
                echo "nginx service already installed."
                main
        else
                sudo apt-get install nginx -y

                clear

                systemctl start nginx
                systemctl enable nginx
                systemctl status nginx

                main
        fi

}

check_nginx_service
