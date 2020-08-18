# Mount&Blade Dedicated Server Systemd Daemon

## Installation

The easiest way with wget for Warband:

```
sudo wget https://raw.githubusercontent.com/kanlas-net/Mount_Blade_Systemd/master/warband%40.service -P /etc/systemd/system
sudo systemctl daemon-reload
```

and for WFAS

```
sudo wget https://raw.githubusercontent.com/kanlas-net/Mount_Blade_Systemd/master/wfas%40.service -P /etc/systemd/system
sudo systemctl daemon-reload
```

Also you need to create user *mbserver* with home folder (to store wine config files)

`useradd mbserver -m`

Don't folget to give him write permissions to server logs folder. You can just give ownership:

`chown -R mbserver:mbserver /path/to/mb/server/Logs`

The rest server files may be readable only, the game doesn't write anything to its root folder

## Content

In general you may need to edit only this lines:
```
ExecStart=/usr/bin/screen -dmS Warband sh -c 'wineconsole --backend=curses /opt/warband/mb_warband_dedicated.exe -r %i.txt -m Native'
ExecStop=/usr/bin/screen -X -S Warband quit
```

`wineconsole` is used instead of `wine` because of input problems after server is launched

In parts `screen -dmS Warband` and `screen -X -S Warband` session with name *"Warband"* is created, change if you want

`/opt/warband/mb_warband_dedicated.exe` is absolute path to server exe file

`-m Native` game module name

## Usage

To start service you need to use server config name as argument. So if I have *Duel.txt* config file, i will run it like this:

`systemctl start warband@Duel`

Use `sudo -u mbserver screen -r Warband` or `sudo -u mbserver screen -r WFAS` to connect wineconsole session and *Crtl+A* then *Crtl+D* to detach from it.

