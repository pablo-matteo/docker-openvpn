## Inicializa los archivos de configuracion y certificados

```bash
docker-compose run --rm openvpn ovpn_genconfig -u udp://<IP-SERVIDOR>
docker-compose run --rm openvpn ovpn_initpki
```

## Arregla tus permisos (puede no ser necesario si ya estás haciendo todo con root)

```bash
sudo chown -R $(whoami): ./openvpn-data
```

## Inicia el contenedor de OpenVPN

```bash
docker-compose up -d
```

## Puedes ver los logs de contenedor con:

```bash
docker-compose logs -f
```

## Generar un certificado de cliente

```bash
export CLIENTNAME="el_nombre_del_cliente"
```
con contraseña

```bash
docker-compose run --rm openvpn easyrsa build-client-full $CLIENTNAME
```

sin contraseña

```bash
docker-compose run --rm openvpn easyrsa build-client-full $CLIENTNAME nopass
```

## Crea el archivo de configuración del cliente

```bash
docker-compose run --rm openvpn ovpn_getclient $CLIENTNAME > $CLIENTNAME.ovpn
```

## Revoca el certificado de un cliente
Dejando los archivos crt, key y req.

```bash
docker-compose run --rm openvpn ovpn_revokeclient $CLIENTNAME
Borrando los correspondientes archivos crt, key y req.
```

```bash
docker-compose run --rm openvpn ovpn_revokeclient $CLIENTNAME remove
```

## Contributing
Pull requests are welcome! :)

## License
[MIT](https://choosealicense.com/licenses/mit/)
