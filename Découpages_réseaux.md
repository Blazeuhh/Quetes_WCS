## Découpage du Réseau 172.16.1.0/24

### Découpage Symétrique

| Pôle                | CIDR           | Adresse Réseau   | Adresse de Diffusion |
|---------------------|----------------|------------------|----------------------|
| Pôle Informatique   | 172.16.1.0/26  | 172.16.1.0       | 172.16.1.63          |
| Pôle Développement  | 172.16.1.64/26 | 172.16.1.64      | 172.16.1.127         |
| Pôle  Administratif     | 172.16.1.128/26| 172.16.1.128     | 172.16.1.191         |
| Pôle Technicien   | 172.16.1.192/26| 172.16.1.192     | 172.16.1.255         |

### Découpage Asymétrique

| Pôle                | CIDR           | Adresse Réseau   | Adresse de Diffusion |
|---------------------|----------------|------------------|----------------------|
| Pôle Informatique   | 172.16.1.0/26  | 172.16.1.0       | 172.16.1.63          |
| Pôle Administratif  | 172.16.1.64/27 | 172.16.1.64      | 172.16.1.95          |
| Pôle Technicien     | 172.16.1.96/27 | 172.16.1.96      | 172.16.1.127         |
| Pôle Développement  | 172.16.1.128/28| 172.16.1.128     | 172.16.1.143         |

