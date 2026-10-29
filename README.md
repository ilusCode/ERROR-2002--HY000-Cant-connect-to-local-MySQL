# 🛠️ Solución: ERROR 2002 (HY000) — Can't connect to local MySQL server through socket

Script en Bash que soluciona el error más común al intentar conectarse a MySQL en Linux:

```text
ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/var/run/mysqld/mysqld.sock' (2)
```

![Shell](https://img.shields.io/badge/Shell-Bash-4EAA25?logo=gnubash&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?logo=mysql&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?logo=linux&logoColor=black)

---

## 📌 ¿Por qué ocurre?

El cliente `mysql` se conecta al servidor local a través de un archivo socket en
`/var/run/mysqld/mysqld.sock`. El error aparece cuando:

- El directorio `/var/run/mysqld` no existe (es común tras un reinicio, ya que `/var/run` se limpia).
- El directorio existe pero **no pertenece al usuario `mysql`**, así que el servidor no puede crear el socket.
- El servicio de MySQL está detenido.

## ⚙️ ¿Qué hace el script?

| Paso | Comando | Descripción |
|------|---------|-------------|
| 1 | `mkdir /var/run/mysqld` | Crea el directorio del socket |
| 2 | `touch /var/run/mysqld/mysqld.sock` | Crea el archivo del socket |
| 3 | `chown -R mysql /var/run/mysqld` | Asigna el directorio al usuario `mysql` |
| 4 | `/etc/init.d/mysql restart` | Reinicia el servicio de MySQL |

## 🚀 Uso

1. Clona el repositorio:

   ```bash
   git clone https://github.com/ilusCode/ERROR-2002--HY000-Cant-connect-to-local-MySQL.git
   cd ERROR-2002--HY000-Cant-connect-to-local-MySQL
   ```

2. Ejecuta el script con permisos de administrador (el nombre del archivo tiene espacios, por eso va entre comillas):

   ```bash
   sudo bash "ERROR 2002 (HY000): Can't connect to local MySQL server through socket.sh"
   ```

3. Comprueba que ya puedes conectarte:

   ```bash
   mysql -u root -p
   ```

## 🔧 Solución manual

Si prefieres ejecutar los pasos tú mismo:

```bash
sudo mkdir -p /var/run/mysqld
sudo chown -R mysql:mysql /var/run/mysqld
sudo service mysql restart      # o: sudo systemctl restart mysql
```

## ✅ Requisitos

- Distribución Linux basada en Debian/Ubuntu (o similar) con MySQL o MariaDB instalado.
- Permisos de `sudo`.

## ⚠️ Si el error persiste

- Revisa el estado del servicio: `sudo systemctl status mysql`
- Revisa el log de errores: `sudo tail -n 50 /var/log/mysql/error.log`
- Verifica la ruta del socket en la configuración (`/etc/mysql/my.cnf` o `/etc/mysql/mysql.conf.d/mysqld.cnf`), en la opción `socket=`.

## 👤 Autor

**Eduardo Fernández Morales** — [@ilusCode](https://github.com/ilusCode) · [LinkedIn](https://www.linkedin.com/in/eduardo-fernandez-morales/)
