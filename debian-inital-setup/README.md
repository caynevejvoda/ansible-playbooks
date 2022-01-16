# Ansible Playbook for inital Setup a Debian Vagrant Box
## Disclaimer: This Readme is work in progress

Dieses Ansible Playbook automatisiert die grundlegende Einrichtung einer Debian 11 Vagrant VM.
Das Playbook läuft nur mit Debian bzw. Ubuntu based Distributionen und wurde nur unter Debian 11 getestet. 

## Was installiert das Playbook?

Es wird Automatisch folgende Software über apt auf der VM installiert:
* git
* zsh
* curl
* exa
* tcpdump
* unzip
* vim
* tar
* gzip

Zusätzlich wird der Plugin Manager [Oh My ZSH](https://ohmyz.sh/) über die Ansible Galaxy Role [gantsign/antige](https://galaxy.ansible.com/gantsign/antigen) für die ZShell installiert, welche folgende Plugins nachlädt.

* zsh-syntax-highlighting
* zsh-autosuggestions
* git
* common-aliases
* colored-man-pages
* z
* vi-mode
* docker

Anschließend wird der Vim Pluginmanager [vim-plug](https://github.com/junegunn/vim-plug) installiert welcher standardmäßig folgende Plugins Installiert. 

* onehalf Theme
* vim-airline
* vim-airline-themes
* nerdtree
* vim-floaterm

Die Plugins werden über das .vimrc file definiert.

## How to use this Playbook. 

### Requirements

* [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
* [VirtualBox Extension Pack](https://www.virtualbox.org/wiki/Downloads)
* [Vagrant](https://www.vagrantup.com/downloads)
* [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)

### Installation 

Als erstes wechseln wir in das Verzeichnis in dem wir unsere VM erstellen wollen. 
```
cd ~/vagrant/debian-initial-setup 
```
Dann initialisieren wir die gewünschte Vagrant Box.
```
vagrant init debian/bullseye64
```
Als mächstes kopieren wir den Inhalt der ansible-playbook.yaml und vimrc in gleichnamige Dateien in unserem  Arbeitsverzeichnis. 
Ist das geschehen können wir unser Vagrantfile anpassen. 
```
Vagrant.configure("2") do |config|

  config.vm.box = "debian/bullseye64"

  # tells Vagrant to execute the ansible Playbook by first Vagrant up oder vagrant provision 
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "zsh_playbook.yml"
  end

end
```
Dieser Codeblock sorgt dafür das Vagrant bei dem ersten `vagrant up` oder bei einem `vagrant provision` das Playbook auf der VM ausführt. 

Nach einem `Vagrant up` und einem `vagrant ssh` sind wir auch schon auf dem eingerichteten System. Wenn wir vim das erste mal öffnen kommt eine Fehlermeldung welche wir ignorieren können. In Vim öffnen wir mit `:` den Command modus aufrufen und geben den Befehl `PlugInstall` ein, dieser installiert alle Plugins die wir im .vimrc file definiert haben. Nach erfolgreicher Installation beenden wir den Editor mit `:qa`. Diesen Schritt müssen wir für den Benutzer Vagrant und Root vornehmen. Danach sollte die Fehlermeldung beim öffnen von Vim nicht mehr erscheinen. 

Damit ist die grundlegende Einrichtung auch abgeschlossen und wir haben die Zeit die wir für die erstellung einer VM mit Virtualbox brauchen von ca. einer Stunde auf 5 Minuten verkürzt. 