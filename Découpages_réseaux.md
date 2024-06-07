# Découpage Réseau pour la Société Fictive

## 1. Découpage Symétrique

### Pôle Informatique
- CIDR: 172.16.1.0/26
- Adresse Réseau: 172.16.1.0
- Adresse de Diffusion: 172.16.1.63

### Pôle Développement
- CIDR: 172.16.1.64/26
- Adresse Réseau: 172.16.1.64
- Adresse de Diffusion: 172.16.1.127

### Pôle Administratif
- CIDR: 172.16.1.128/26
- Adresse Réseau: 172.16.1.128
- Adresse de Diffusion: 172.16.1.191

### Pôle Technicien
- CIDR: 172.16.1.192/26
- Adresse Réseau: 172.16.1.192
- Adresse de Diffusion: 172.16.1.255

## 2. Découpage Asymétrique

### Pôle Informatique
- CIDR: 172.16.1.0/26
- Adresse Réseau: 172.16.1.0
- Adresse de Diffusion: 172.16.1.63

### Pôle Administratif
- CIDR: 172.16.1.64/27
- Adresse Réseau: 172.16.1.64
- Adresse de Diffusion: 172.16.1.95

### Pôle Technicien
- CIDR: 172.16.1.96/28
- Adresse Réseau: 172.16.1.96
- Adresse de Diffusion: 172.16.1.111

### Pôle Développement
- CIDR: 172.16.1.112/28
- Adresse Réseau: 172.16.1.112
- Adresse de Diffusion: 172.16.1.127
