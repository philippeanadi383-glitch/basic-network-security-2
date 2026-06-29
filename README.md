# 🔥 Basic Firewall Lab — Cisco Packet Tracer

## Objectif
Simuler un firewall basique avec des ACL étendues sur un routeur Cisco pour filtrer le trafic réseau : autoriser uniquement le HTTP vers un serveur web et bloquer tout autre trafic.

## Outils
- Cisco Packet Tracer

## Topologie
```
[PC0] 192.168.1.10 ──┐
[PC1] 192.168.1.11 ──┤── Switch (2960-24TT) ── Routeur (ISR4331) ── Serveur Web (10.0.0.10)
[PC2] 192.168.1.12 ──┘
```

## Configuration

### Interfaces du routeur
| Interface | IP | Rôle |
|---|---|---|
| GigabitEthernet0/0/0 | 192.168.1.1 /24 | Réseau interne (LAN) |
| GigabitEthernet0/0/1 | 10.0.0.1 /24 | Réseau serveur (DMZ) |

### Règles ACL (Access Control List 100)
```
access-list 100 permit tcp 192.168.1.0 0.0.0.255 host 10.0.0.10 eq 80
access-list 100 permit tcp host 10.0.0.10 192.168.1.0 0.0.0.255 established
access-list 100 deny ip any any
```

- ✅ **Autorisé** : trafic HTTP (port 80) depuis le réseau interne vers le serveur web
- ✅ **Autorisé** : réponses HTTP du serveur (established)
- ❌ **Bloqué** : tout autre trafic (ping ICMP, Telnet, etc.)

### Application de l'ACL
```
interface GigabitEthernet0/0/0
 ip access-group 100 in
```

## Résultats des tests
| Test | Résultat |
|---|---|
| PC0 → HTTP vers 10.0.0.10 | ✅ Page web accessible |
| PC0 → Ping vers 10.0.0.10 | ❌ Bloqué par l'ACL |

## Concepts démontrés
- Filtrage de trafic avec ACL étendue (Extended ACL)
- Principe du moindre privilège (deny all par défaut)
- Firewall stateless sur routeur Cisco
- Séparation réseau LAN / DMZ
