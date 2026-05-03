# Waybar Config

Configuração do [Waybar](https://github.com/Alexays/Waybar) otimizada para Hyprland, com barra no estilo "pílulas" flutuantes e ícones Nerd Fonts.

## Visual

```
[ CPU  RAM  Workspaces ]   [ 󰂱 Fone  Seg, 01 Jan 2025  12:00 ]   [ Vol  Rede  Bluetooth  Privacy ]
```

## Funcionalidades

| Módulo | Descrição |
|---|---|
| **CPU** | Uso em tempo real (intervalo 10s) |
| **Memória** | RAM usada / total (intervalo 30s) |
| **Workspaces** | Workspaces do Hyprland; clique para ativar |
| **Relógio** | Data e hora; clique direito para alternar calendário mensal/anual; scroll para navegar |
| **Volume** | Volume atual com ícone por dispositivo; scroll para ajustar; clique abre `pavucontrol` |
| **Rede** | Ícone por tipo de conexão (ethernet/wifi); vermelho quando desconectado |
| **Bluetooth** | Mostra nome do dispositivo conectado; clique abre `blueman-manager` |
| **Privacy** | Indicadores de screenshare e microfone ativos |

## Dependências

### Obrigatórias

| Pacote | Função |
|---|---|
| `waybar` | A barra em si |
| `hyprland` | Compositor Wayland (módulos `hyprland/*`) |
| `pipewire` + `wireplumber` | Backend de áudio (módulo `pulseaudio`) |
| `bluez` + `bluez-utils` | Daemon Bluetooth |
| Nerd Fonts (qualquer variante) | Ícones usados nos módulos |
| JetBrains Mono (Nerd Font) | Fonte principal da barra |

### Opcionais (para interatividade)

| Pacote | Função |
|---|---|
| `blueman` | GUI de Bluetooth (abre ao clicar no módulo) |
| `pavucontrol` | Controle de volume/dispositivos (abre ao clicar no volume) |

## Instalação

### Arch Linux

```bash
# Dependências obrigatórias
sudo pacman -S waybar pipewire wireplumber bluez bluez-utils

# Fontes
sudo pacman -S ttf-jetbrains-mono-nerd

# Opcionais
sudo pacman -S blueman pavucontrol

# Habilitar Bluetooth
sudo systemctl enable --now bluetooth

# Instalar config
git clone https://github.com/salaroli/waybar ~/.config/waybar
```

### Fedora

```bash
sudo dnf install waybar pipewire wireplumber bluez

# Fontes (via repositório copr ou manual)
sudo dnf copr enable peterwu/iosevka
sudo dnf install jetbrains-mono-fonts-all

sudo dnf install blueman pavucontrol

sudo systemctl enable --now bluetooth

git clone https://github.com/salaroli/waybar ~/.config/waybar
```

### Ubuntu / Debian

```bash
sudo apt install waybar pipewire wireplumber bluez

# Fontes: baixar manualmente de https://www.nerdfonts.com/font-downloads
# Extrair em ~/.local/share/fonts/ e rodar: fc-cache -fv

sudo apt install blueman pavucontrol

sudo systemctl enable --now bluetooth

git clone https://github.com/salaroli/waybar ~/.config/waybar
```

### NixOS

```nix
environment.systemPackages = with pkgs; [
  waybar
  pipewire
  wireplumber
  bluez
  blueman
  pavucontrol
  nerd-fonts.jetbrains-mono
];

services.blueman.enable = true;
hardware.bluetooth.enable = true;
```

```bash
git clone https://github.com/salaroli/waybar ~/.config/waybar
```

## Controle de volume com pavucontrol

O `pavucontrol` abre ao clicar no módulo de volume e permite:
- Ajustar volume de saída e entrada separadamente
- Selecionar dispositivos de output (caixas, fones, HDMI)
- Selecionar dispositivos de input (microfones)
- Ver e controlar o volume por aplicativo

Para abrir como janela flutuante no Hyprland, adicione ao `hyprland.conf`:

```
windowrulev2 = float, class:^(pavucontrol)$
windowrulev2 = size 700 450, class:^(pavucontrol)$
windowrulev2 = center, class:^(pavucontrol)$
```

## Interface de rede

O módulo detecta automaticamente o tipo de conexão:
- `󰊗` — Ethernet conectada
- `󰖩` — Wi-Fi conectado
- `󰤭` — Desconectado (ícone fica vermelho via CSS)

A interface padrão está fixada em `enp8s0`. Ajuste em `config` se necessário:

```json
"network": {
    "interface": "sua-interface"
}
```

Para descobrir o nome da sua interface: `ip link show`
