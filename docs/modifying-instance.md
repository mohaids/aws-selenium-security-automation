# Once SSH'd into the VM:

Install the necessary packages:
```bash
sudo apt update
sudo apt install -y xfce4 xfce4-goodies tigervnc-standalone-server dbus-x11 xterm x11-xserver-utils
```

#### Set Up vncserver

First on your personal machine (not the instance), install [RealVNC Viewer](https://www.realvnc.com/en/connect/download/viewer/windows/?lai_vid=VVzDK0Pe0iV4&lai_sr=0-4&lai_sl=l&lai_p=1&lai_na=921110)

Now, back to your instance:

```bash
vncserver
```

You will be required to create a password here. Enter it, and confirm it.

When asked "Would you like to enter a view-only password (y/n)?", enter 'n'.

Kill default session:
```bash
vncserver -kill :1
```

Edit start up file:
```bash
nano ~/.vnc/xstartup
```

Replace the existing contents with these lines, save + exit:
```text
#!/bin/bash
unset SESSION_MANAGER
unset DBUS_SESSION_BUS_ADDRESS
export XKL_XMODMAP_DISABLE=1
export DISPLAY=:1
xrdb $HOME/.Xresources
dbus-launch --exit-with-session startxfce4
```

Make script executable:
```bash
chmod +x ~/.vnc/xstartup
```

Clean logs:
```bash
rm -rf ~/.vnc/*.log ~/.vnc/*.pid
```

Restart the server:
```bash
vncserver :1
```

If the output is something like this:
```output
New Xtigervnc server 'ip-172-31-23-21.us-east-2.compute.internal:1 (ubuntu)' on port 5901 for display :1.
Use xtigervncviewer -SecurityTypes VncAuth -passwd /tmp/tigervnc.TfXFnO/passwd :1 to connect to the VNC server.
```

Then you should be good to run the following command:

Replace "<ec-ip-address>", with your EC2's Public IP address.

```bash
ssh -L 5901:localhost:5901 -i key.pem ubuntu@<ec-ip-address>
```

Open RealVNC Viewer on your personal machine, and if you already don't have localhost:5901 as an option, enter it in the search bar to make it such.

Now it'll warn you about "Unencrypted connection"

> explain why this is fine

Click "Continue"

Now it'll prompt you to "Authenticate to VNC Server" - use the password you created from earlier to authenticate.

Now you should successfully have access to your machine via GUI! 🚀