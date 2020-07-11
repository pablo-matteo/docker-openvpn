Inicializa los archivos de configuracion y certificados
docker-compose run --rm openvpn ovpn_genconfig -u udp://<IP-SERVIDOR>
docker-compose run --rm openvpn ovpn_initpki

Arregla tus permisos (puede no ser necesario si ya estás haciendo todo con root)
sudo chown -R $(whoami): ./openvpn-data

Inicia el contenedor de OpenVPN
docker-compose up -d

Puedes ver los logs de contenedor con:
docker-compose logs -f

Generar un certificado de cliente
export CLIENTNAME="el_nombre_del_cliente"
# con contraseña
docker-compose run --rm openvpn easyrsa build-client-full $CLIENTNAME
# sin contraseña
docker-compose run --rm openvpn easyrsa build-client-full $CLIENTNAME nopass

Crea el archivo de configuración del cliente
docker-compose run --rm openvpn ovpn_getclient $CLIENTNAME > $CLIENTNAME.ovpn

Revoca el certificado de un cliente
# Dejando los archivos crt, key y req.
docker-compose run --rm openvpn ovpn_revokeclient $CLIENTNAME
# Borrando los correspondientes archivos crt, key y req.
docker-compose run --rm openvpn ovpn_revokeclient $CLIENTNAME remove