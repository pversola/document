# Add Users

List users

```
getent passwd | awk -F: '{print $1}'
```

### Ubuntu

```
sudo useradd -G sudo somboon.s
sudo useradd -G sudo chankrit.t
sudo useradd -G sudo nirapan.n
sudo useradd -G sudo todsawat.f
sudo useradd -G sudo santichai.p

sudo useradd -G sudo somboon.s && sudo useradd -G sudo chankrit.t && sudo useradd -G sudo nirapan.n && sudo useradd -G sudo todsawat.f && sudo useradd -G sudo santichai.p

sudo usermod -aG docker somboon.s && sudo usermod -aG docker chankrit.t && sudo usermod -aG docker nirapan.n && sudo usermod -aG docker todsawat.f && sudo usermod -aG docker santichai.p

sudo passwd somboon.s
sudo passwd chankrit.t
sudo passwd nirapan.n
sudo passwd todsawat.f
sudo passwd santichai.p
```

```
for i in somboon.s chankrit.t nirapan.n todsawat.f santichai.p; do sudo useradd -G sudo $i; sudo usermod -aG $i $i; sudo mkdir /home/$i; sudo chown $i:$i /home/$i; done

for i in somboon.s chankrit.t nirapan.n todsawat.f santichai.p; do sudo usermod -aG $i $i; sudo mkdir /home/$i; sudo chown $i:$i /home/$i; done

for i in somboon.s chankrit.t nirapan.n todsawat.f santichai.p; do echo "$i:Et@xSP1nfr@"  | sudo chpasswd; done

for i in somboon.s chankrit.t nirapan.n todsawat.f santichai.p; do sudo chown $i:$i /home/$i; done
```

### Centos

```
sudo useradd -G wheel somboon.s
sudo useradd -G wheel chankrit.t
sudo useradd -G wheel nirapan.n
sudo useradd -G wheel todsawat.f
sudo useradd -G wheel santichai.p

sudo passwd somboon.s
sudo passwd chankrit.t
sudo passwd nirapan.n
sudo passwd todsawat.f
sudo passwd santichai.p
```
