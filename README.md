#  Basit Ağ Tarayıcı — Scapy ile ARP Scan

Bu repo, `scapy` kullanarak yerel ağdaki cihazları ARP istekleriyle tarayan **basit ve öğretici** bir Python scripti içerir.  
*(English: A simple ARP network scanner using Scapy for discovering devices on the local network.)*

---

##  Öne Çıkanlar
- Küçük ve anlaşılır bir ARP tabanlı ağ tarayıcı.
- Tek IP veya IP aralığı (/24 gibi) ile kullanılabilir.
- Eğitim ve laboratuvar amaçlıdır — **izinli** ortamlarda kullanın.

---

##  Gereksinimler
- Python 3
- scapy

Kurulum (Linux):
```bash
sudo pip3 install scapy
```
> Not: Scriptin çalışması için root yetkisi gereklidir (`sudo` ile çalıştırın).

---

##  Kullanım

1. Script dosyasını oluştur (ör. `network_scan.py`) ve içine aşağıdaki kodu yapıştır.
2. Terminalde çalıştır:

```bash
sudo python3 network_scan.py -i 192.168.1.0/24
```

Parametreler:
- `-i` veya `--ipaddress` : Taranacak IP adresi veya CIDR aralığı (ör. `192.168.1.0/24` veya tek IP `192.168.1.10`).

---

##  Kod 

```python
import scapy.all as scapy
import optparse

#1) arp_request
#2) broadcast
#3) response

def get_user_input():
    parse_object = optparse.OptionParser()
    parse_object.add_option("-i","--ipaddress", dest="ip_address",help="Enter IP Address")

    (user_input,arguments) = parse_object.parse_args()

    if not user_input.ip_address:
        parse_object.error("Please specify an IP address or range (use -i).")

    return user_input

def scan_my_network(ip):
    arp_request_packet = scapy.ARP(pdst=ip)
    # scapy.ls(scapy.ARP())
    broadcast_packet = scapy.Ether(dst="ff:ff:ff:ff:ff:ff")
    # scapy.ls(scapy.Ether())
    combined_packet = broadcast_packet/arp_request_packet
    (answered_list,unanswered_list) = scapy.srp(combined_packet,timeout=1,verbose=False)
    answered_list.summary()

if __name__ == "__main__":
    user_ip_address = get_user_input()
    scan_my_network(user_ip_address.ip_address)
```

---

##  Örnek Çalıştırma & Çıktı

Çalıştırma:
```bash
sudo python3 network_scan.py -i 192.168.1.0/24
```

Örnek çıktı (scapy formatına göre):
```
Ether / ARP 192.168.1.1 is-at aa:bb:cc:dd:ee:ff
Ether / ARP 192.168.1.10 is-at 11:22:33:44:55:66
...
```

---

##  Uyarılar & Etik
- Bu araç yalnızca **izinli test ortamlarında** ve eğitim amaçlı kullanılmalıdır.  
- İzinsiz ağ tarama/keşif işlemleri yasa dışıdır ve etik dışıdır — sorumluluk sahibine aittir.  
- Root yetkisi gerektirir; dikkatli kullanın.

---

##  Hata Giderme İpuçları
- `Permission denied` hatası: `sudo` ile çalıştırın.
- `scapy` bulunamıyor: `sudo pip3 install scapy`
- Beklenmeyen sonuçlar: `timeout` değerini artırmayı deneyin (`srp(..., timeout=2)` gibi).
- Bazı ağlarda güvenlik duvarları ARP yanıtlarını engelleyebilir.

---

##  Yazar
**Enes Nergiz**  
Bilgisayar Mühendisliği Öğrencisi | Ağ & Siber Güvenlik Meraklısı  
 İletişim: [energiz2310@gmail.com]  
 GitHub: [https://github.com/enesnergiz](https://github.com/enesnergiz)

---

##  Etiketler
`python` `scapy` `network-scan` `arp` `security` `linux` `educational`
