# Claude Web Onboarding – Dual-WAN VLAN Setup

## Wer ich bin
Dieter Horst, SYSTEMHAUS HORST. Baue MIRA/EVY alleine. Server im Obergeschoss, Router im Erdgeschoss.

## Das Vorhaben
Zweite Internetleitung (Vodafone Glasfaser) zusätzlich zur bestehenden Telekom-Leitung einrichten. Beide Leitungen über **ein einziges Kabel** nach oben zum Server bringen.

## Aktuelle Situation

```
ERDGESCHOSS                           OBERGESCHOSS
                                      
[Telekom Fritz!Box]                   [DASBIEST]
        │                                  │
        └──── [Zyxel Switch] ════════════ [Intel X710]
                    │                  (ein Kabel)
        ┌──────────┘
        │
   (weitere Geräte)
```

## Ziel-Situation

```
ERDGESCHOSS                           OBERGESCHOSS
                                      
[Telekom Fritz!Box]───┐               [DASBIEST]
                      │                    │
                 [Zyxel Switch] ═════════ [Intel X710]
                      │               VLAN 10 = Telekom
[Vodafone Fritz!Box]──┘               VLAN 20 = Vodafone
   (NEU - Glasfaser)
```

## Hardware vorhanden

| Gerät | Details | Standort |
|-------|---------|----------|
| **Telekom Fritz!Box** | Bestandsleitung DSL | EG |
| **Vodafone Fritz!Box** | NEU, Glasfaser | EG |
| **Zyxel GS1900-24E** | 24-Port Managed Switch, VLAN-fähig | EG |
| **DASBIEST** | Windows 11, Intel X710-TL (10G, VLAN-fähig) | OG |
| **1× Netzwerkkabel** | Zwischen EG und OG | verlegt |

## Zyxel GS1900-24E Zugangsdaten

- **IP:** 192.168.1.1 (Standard)
- **Login:** admin / 1234
- **Webinterface:** http://192.168.1.1

## Geplante VLAN-Konfiguration

| VLAN ID | Name | Zyxel Ports | Beschreibung |
|---------|------|-------------|--------------|
| 10 | Telekom | Port 1 (Untagged), Port 24 (Tagged) | Bestandsleitung |
| 20 | Vodafone | Port 2 (Untagged), Port 24 (Tagged) | Neue Glasfaser |

**Port 24** = Uplink zum DASBIEST (Tagged Trunk, beide VLANs)

## Geplante Verkabelung

| Zyxel Port | Gerät |
|------------|-------|
| Port 1 | Telekom Fritz!Box LAN |
| Port 2 | Vodafone Fritz!Box LAN |
| Port 24 | Kabel hoch → DASBIEST (X710) |

## Auf DASBIEST zu tun

1. Intel X710 Treiber/PROSet installiert
2. VLAN 10 anlegen → IP von Telekom-Fritz (DHCP oder statisch)
3. VLAN 20 anlegen → IP von Vodafone-Fritz (DHCP oder statisch)
4. Optional: Hyper-V virtuelle Switches pro VLAN für VMs

## Offene Fragen

- [ ] Vodafone Fritz!Box Modell?
- [ ] Welche IP-Bereiche nutzen die Fritz!Boxen? (Standard: 192.168.178.x)
- [ ] Soll DASBIEST beide Leitungen gleichzeitig nutzen (Load Balancing) oder Failover?
- [ ] Brauchen VMs Zugang zu beiden Netzen?

## Mögliche Nutzungsszenarien

| Szenario | Beschreibung |
|----------|--------------|
| **Failover** | Telekom primary, Vodafone springt ein bei Ausfall |
| **Load Balancing** | Traffic auf beide verteilen |
| **Trennung** | VMs auf Vodafone, Host auf Telekom |
| **Backup-Route** | Bestimmte Dienste über Vodafone |

---

## Session-Ziel

Wenn die Vodafone-Leitung da ist: Schritt-für-Schritt das VLAN-Setup durchführen.

1. Zyxel konfigurieren
2. Fritz!Boxen anschließen
3. DASBIEST VLANs einrichten
4. Testen
5. Optional: Hyper-V Netzwerk anpassen

---

*Warte auf Dieters Go wenn die Leitung geschaltet ist.*
