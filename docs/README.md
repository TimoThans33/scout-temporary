# Scout-docs

This repo contains all the documentation on the scout.

---
## 0.1 Usage
You can start a basic http server in the docs folder.
```
python3 -m http.server 5555
```
Use your favourite web-browser and browse to your local ip (0.0.0.0) on specified port in previous command.

---
## 0.2 Systemd service
copy the service file into the systemd folder.
```
cp cx_scout_docs.service /etc/systemd/system/
```
enable and start the service.
```
sudo systemctl enable cx_scout_docs.service
sudo systemctl start cx_scout_docs.service
```
browse to the correct ip address with gui.

---
## 0.3 Guidelines
Some general rules and naming conventions used in this repository.
> scout = the actual robot
> Odrive (do not use motorcontroller)
> the graphical user interface is referred to as scout gui
> use correct sub(-sub)chapters (up to 3 like 3.0.1)
> end every SUBchapter (0.3) with a markdown line (---)

Thank for your help in keeping our scout documentation up-to-date (;
---