# Manual de instalação e configuração do FreeBSD 15.1-RELEASE no VirtualBox

## 1. Objetivo

Este manual documenta a criação, configuração e validação de uma máquina virtual com **FreeBSD 15.1-RELEASE** e **XFCE** no Oracle VirtualBox.

A atividade original de virtualização propõe a instalação de uma distribuição Linux leve. Nesta execução, a escolha foi adaptada para o FreeBSD com o objetivo de ampliar o estudo: em vez de repetir uma instalação Linux já familiar, foi utilizado outro sistema operacional Unix-like, permitindo observar diferenças reais de administração, organização do sistema-base, gerenciamento de pacotes, inicialização, shells e integração com o hipervisor.

O resultado final é uma VM gráfica utilizável para estudo, com:

- FreeBSD 15.1-RELEASE;
- XFCE 4.20 em X11;
- integração com o VirtualBox Guest Additions;
- resolução dinâmica e modo tela cheia;
- clipboard bidirecional;
- Firefox;
- Fastfetch;
- terminal configurado;
- recuperação administrativa via Single User Mode.

> **Nota:** alguns detalhes da interface do VirtualBox podem variar entre versões. Os comandos apresentados foram validados no ambiente descrito abaixo.

---

## 2. Ambiente utilizado

### 2.1. Sistema hospedeiro

| Item | Configuração |
|---|---|
| Sistema hospedeiro | Arch Linux x86_64 |
| Hipervisor | Oracle VirtualBox 7.2.18 |
| Processador físico | Intel Core i5-1335U |
| GPU | Intel Iris Xe |
| Memória física do host | 8 GiB |

### 2.2. Máquina virtual

| Item | Configuração |
|---|---|
| Nome | `FreeBSD-XFCE` |
| Sistema convidado | FreeBSD 15.1-RELEASE |
| Hostname | `freebsd-xfce` |
| Usuário comum | `bsd` |
| vCPUs | 4 |
| RAM | aproximadamente 4 GiB |
| Disco virtual | aproximadamente 50 GiB |
| Sistema de arquivos observado | UFS |
| Rede | NAT |
| Controlador gráfico | VBoxSVGA |
| Memória de vídeo | 256 MiB |
| Aceleração 3D | desativada |
| Ambiente gráfico | XFCE 4.20 |
| Window manager | Xfwm4 / X11 |
| Shell do usuário | `/bin/sh` |

Uma captura do ambiente final pode ser adicionada em:

```text
imagens/01-desktop-final.png
```

Exemplo de inclusão no Markdown:

```md
![Desktop final do FreeBSD](imagens/01-desktop-final.png)
```

---

## 3. Criação da máquina virtual

No VirtualBox, foi criada uma VM do tipo FreeBSD de 64 bits.

A configuração final utilizada foi:

- 4 processadores virtuais;
- 4 GiB de RAM;
- disco virtual de aproximadamente 50 GiB;
- rede NAT;
- um monitor virtual;
- 256 MiB de VRAM;
- controlador **VBoxSVGA**;
- aceleração 3D desativada.

O controlador gráfico merece atenção especial. A documentação do FreeBSD recomenda **VBoxSVGA** para convidados FreeBSD executando Xorg. O uso de VMSVGA pode fazer o Xorg selecionar drivers inadequados ou apresentar comportamento incorreto.

---

## 4. Instalação do FreeBSD

A instalação foi realizada a partir da imagem oficial do FreeBSD utilizando o instalador `bsdinstall`.

Durante a instalação foram definidos:

- hostname: `freebsd-xfce`;
- usuário comum: `bsd`;
- senha do usuário;
- senha administrativa do `root`;
- sistema de arquivos UFS;
- configuração de rede;
- fuso horário e opções básicas do sistema.

O usuário `bsd` foi mantido no grupo administrativo `wheel`.

Para verificar:

```sh
id
```

Saída observada:

```text
uid=1001(bsd) gid=1001(bsd) groups=0(wheel),1001(bsd)
```

A versão instalada foi validada com:

```sh
freebsd-version -kru
```

Resultado:

```text
15.1-RELEASE
15.1-RELEASE
15.1-RELEASE
```

---

## 5. Administração básica: `root`, `su` e ausência de `sudo`

Uma diferença percebida logo no início foi a ausência do `sudo` na instalação base.

No FreeBSD, é possível assumir uma sessão administrativa com:

```sh
su -
```

O comando solicita a senha do usuário `root`.

O usuário comum deve pertencer ao grupo `wheel` para utilizar `su` para `root` na configuração padrão.

A instalação de `sudo` é opcional:

```sh
pkg install sudo
```

Nesta VM foi mantida a possibilidade de administração via `su -`, o que também serviu para evidenciar a diferença entre componentes do sistema-base e softwares instalados posteriormente.

---

## 6. Gerenciamento de pacotes

O gerenciador de pacotes binários utilizado no FreeBSD é o `pkg`.

Atualização do catálogo:

```sh
pkg update
```

Atualização dos pacotes instalados:

```sh
pkg upgrade -y
```

Instalação de um software:

```sh
pkg install nome-do-pacote
```

Exemplo utilizado posteriormente:

```sh
pkg install -y fastfetch
```

Além dos pacotes binários, o FreeBSD possui a **Ports Collection**, um sistema de Makefiles, patches e metadados que permite compilar aplicações a partir do código-fonte com opções customizadas.

---

## 7. Shell padrão

O usuário `bsd` utiliza:

```sh
echo $SHELL
```

Resultado:

```text
/bin/sh
```

Bash, Zsh e Fish não precisam fazer parte do sistema-base. Eles podem ser instalados como pacotes de terceiros, normalmente em `/usr/local/bin`.

Exemplo:

```sh
pkg install bash zsh fish
```

Essa separação é uma característica importante do FreeBSD: programas externos ao sistema-base são instalados principalmente sob `/usr/local`.

---

## 8. Recuperação de senha pelo Single User Mode

Durante o uso da VM, a senha administrativa foi esquecida. A recuperação foi feita sem reinstalar o sistema.

### 8.1. Entrar no modo de usuário único

No menu de boot do FreeBSD foi selecionado **Single User Mode**.

Quando apareceu:

```text
Enter full pathname of shell or RETURN for /bin/sh:
```

foi pressionado `Enter`.

### 8.2. Verificar o sistema de arquivos

Após uma interrupção não limpa, o sistema apresentou aviso de desmontagem incorreta. Foi utilizado:

```sh
fsck -y /
```

Ao final:

```text
***** FILE SYSTEM IS CLEAN *****
***** FILE SYSTEM MARKED CLEAN *****
```

### 8.3. Remontar a raiz com escrita

```sh
mount -uw /
```

### 8.4. Alterar senha

Para o usuário administrativo:

```sh
passwd root
```

Para o usuário comum:

```sh
passwd bsd
```

### 8.5. Reiniciar

```sh
reboot
```

Esse procedimento demonstrou uma diferença importante entre simplesmente utilizar uma interface gráfica e compreender a inicialização e recuperação de um sistema Unix-like.

---

## 9. XFCE e ambiente gráfico

A VM utiliza:

- XFCE 4.20;
- Xfwm4;
- X11;
- Xfce Terminal.

O ambiente final pode ser conferido por:

```sh
fastfetch
```

ou:

```sh
echo "$XDG_CURRENT_DESKTOP"
```

No uso da VM, o XFCE foi escolhido por ser leve, simples e adequado a um ambiente virtualizado com poucos recursos.

---

## 10. VirtualBox Guest Additions

Para integrar melhor o convidado FreeBSD ao VirtualBox, foi instalado:

```sh
pkg install virtualbox-ose-additions-72
```

Na instalação utilizada, o pacote reportou a série 7.2.x.

Em seguida foram habilitados os serviços:

```sh
sysrc vboxguest_enable="YES"
```

```sh
sysrc vboxservice_enable="YES"
```

A presença do módulo foi verificada com:

```sh
kldstat | grep -i vbox
```

Exemplo observado:

```text
vboxguest.ko
```

O serviço principal foi verificado com:

```sh
service vboxservice status
```

E os componentes da sessão gráfica com:

```sh
pgrep -laf 'VBoxClient|VBoxDRMClient'
```

Foram observados processos relacionados a:

- clipboard;
- drag-and-drop;
- seamless mode;
- sessão gráfica do VirtualBox.

---

## 11. Diagnóstico do problema de resolução

Inicialmente, a VM estava limitada a `1024x768`.

O diagnóstico foi feito com:

```sh
xrandr --current
```

e:

```sh
grep -Ei 'vboxvideo|vesa|scfb|modesetting|LoadModule' /var/log/Xorg.0.log
```

O log mostrou que o Xorg estava utilizando o driver VESA:

```text
VESA(0): Virtual size is 1024x768
VESA(0): Setting up VESA Mode ... (1024x768)
```

Ao mesmo tempo, o driver do VirtualBox estava instalado:

```sh
ls -lh /usr/local/lib/xorg/modules/drivers/vboxvideo_drv.so
```

### 11.1. Tentativa que não funcionou

Foi criado temporariamente um arquivo que forçava manualmente o driver `vboxvideo`.

Essa alteração fez a sessão gráfica falhar durante a inicialização. O arquivo foi recuperado pelo Single User Mode e renomeado para:

```text
20-vboxvideo.conf.disabled
```

A lição importante foi: **não forçar o driver antes de confirmar o controlador gráfico apresentado pelo VirtualBox**.

### 11.2. Causa real

No host, foi verificada a configuração da VM:

```sh
VBoxManage showvminfo "FreeBSD-XFCE" --machinereadable
```

O controlador estava como:

```text
graphicscontroller="vmsvga"
```

Depois da correção, ficou:

```text
graphicscontroller="vboxsvga"
accelerate3d="off"
vram=256
```

Após iniciar novamente, o Xorg passou a detectar automaticamente o driver correto:

```text
Matched vboxvideo as autoconfigured driver 0
LoadModule: "vboxvideo"
VBoxVideo(0): VirtualBox guest additions video driver version 7.2
```

E o `xrandr` passou a permitir resoluções muito maiores:

```text
maximum 32766 x 32766
```

---

## 12. Resolução dinâmica e tela cheia

Com `VBoxSVGA` e `vboxvideo`, o VirtualBox passou a fornecer modos dinâmicos de resolução.

Exemplo:

```sh
xrandr --current
```

Saída observada em modo janela:

```text
current 1920 x 967
VGA-0 connected primary 1920x967
```

Em tela cheia, foi validado:

```text
current 1920 x 1080
VGA-0 connected primary 1920x1080
```

### 12.1. Workaround para inicialização em baixa resolução

Em alguns boots, o VirtualBox iniciava temporariamente com modos como `720x400`. Para tornar a correção automática, foi criado:

```text
~/.local/bin/vbox-xfce-resize-fix.sh
```

Conteúdo final:

```sh
#!/bin/sh
sleep 5
xrandr --output VGA-0 --mode 1920x1080 >/dev/null 2>&1
sleep 1
xfdesktop --quit 2>/dev/null
sleep 1
xfdesktop >/dev/null 2>&1 &
```

O script foi marcado como executável:

```sh
chmod +x ~/.local/bin/vbox-xfce-resize-fix.sh
```

E adicionado ao autostart do XFCE:

```text
~/.config/autostart/vbox-xfce-resize-fix.desktop
```

com:

```ini
[Desktop Entry]
Type=Application
Name=VirtualBox XFCE Resize Fix
Comment=Sincroniza o desktop XFCE com a resolução dinâmica do VirtualBox
Exec=/home/bsd/.local/bin/vbox-xfce-resize-fix.sh
Terminal=false
StartupNotify=false
X-GNOME-Autostart-enabled=true
OnlyShowIn=XFCE;
```

---

## 13. Clipboard e drag-and-drop

Os Guest Additions permitem:

- compartilhamento de clipboard;
- integração do mouse;
- sincronização de tempo;
- redimensionamento da janela;
- seamless mode.

No host, clipboard e drag-and-drop foram configurados como bidirecionais.

O componente de clipboard pode ser verificado no guest com:

```sh
pgrep -laf 'VBoxClient --clipboard'
```

Para colar texto no Xfce Terminal:

```text
Ctrl + Shift + V
```

Para copiar:

```text
Ctrl + Shift + C
```

---

## 14. Fastfetch

O Fastfetch foi instalado com:

```sh
su -
```

```sh
pkg install -y fastfetch
```

Depois:

```sh
exit
```

Para executá-lo:

```sh
fastfetch
```

### 14.1. Inicialização automática

O `/bin/sh` foi configurado para carregar `~/.shrc`.

No `~/.profile`:

```sh
ENV="$HOME/.shrc"
export ENV
```

No `~/.shrc`, o Fastfetch é chamado automaticamente com uma seleção de informações úteis e logo do FreeBSD totalmente vermelho.

A configuração utilizada exibe:

- título;
- sistema operacional;
- host;
- kernel;
- uptime;
- shell;
- display;
- desktop environment;
- window manager;
- terminal;
- CPU;
- GPU;
- memória;
- disco;
- IP local;
- bateria.

Uma captura recomendada:

```text
imagens/02-fastfetch.png
```

---

## 15. Terminal maior e centralizado

Para evitar quebra das linhas do Fastfetch, o Xfce Terminal foi configurado para abrir com geometria maior.

Configuração:

```sh
xfconf-query -c xfce4-terminal -p /misc-default-geometry -n -t string -s '120x34'
```

```sh
xfconf-query -c xfce4-terminal -p /misc-inherit-geometry -n -t bool -s false
```

O posicionamento de novas janelas foi configurado no Xfwm4:

```sh
xfconf-query -c xfwm4 -p /general/placement_mode -s center
```

```sh
xfconf-query -c xfwm4 -p /general/placement_ratio -s 100
```

Resultado: novas janelas do terminal abrem maiores, centralizadas e comportam o Fastfetch sem quebra visual.

---

## 16. Firefox

O navegador escolhido foi o Firefox devido à boa disponibilidade e integração no FreeBSD.

Instalação:

```sh
su -
```

```sh
pkg install -y firefox
```

```sh
exit
```

Execução:

```sh
firefox
```

Para criar um atalho na área de trabalho:

```sh
mkdir -p ~/Desktop
```

```sh
cp /usr/local/share/applications/firefox.desktop ~/Desktop/Firefox.desktop
```

```sh
chmod +x ~/Desktop/Firefox.desktop
```

Uma captura recomendada:

```text
imagens/03-firefox.png
```

---

## 17. Wallpaper

Uma imagem relacionada ao Beastie/FreeBSD foi transferida do host para o guest e aplicada como papel de parede no XFCE.

A resolução-alvo da VM em tela cheia é:

```text
1920x1080
```

Esse passo não altera a funcionalidade do sistema, mas ajuda a identificar visualmente o ambiente e deixa a entrega final mais organizada.

---

## 18. Comandos úteis de validação

### Versão do sistema

```sh
freebsd-version -kru
```

### Identidade e grupos

```sh
id
```

### Resolução

```sh
xrandr --current
```

### Guest Additions

```sh
kldstat | grep -i vbox
```

```sh
pgrep -laf 'VBoxClient|VBoxService'
```

### Pacotes

```sh
pkg info
```

### Informações resumidas

```sh
fastfetch
```

---

## 20. Resultado final

A máquina virtual terminou com os seguintes recursos funcionais:

- FreeBSD 15.1-RELEASE;
- XFCE 4.20;
- usuário comum `bsd`;
- administração via `root`/`su`;
- rede funcional;
- Guest Additions;
- VBoxSVGA + vboxvideo;
- resolução 1920x1080 em tela cheia;
- clipboard bidirecional;
- integração do ponteiro;
- Firefox;
- Fastfetch automático;
- terminal maior e centralizado;
- atalho do Firefox na área de trabalho;
- wallpaper personalizado;
- mecanismo de recuperação via Single User Mode testado.

A atividade permitiu estudar não apenas a criação de uma VM, mas também troubleshooting real de boot, Xorg, drivers gráficos, serviços, permissões, pacotes, shells e integração host/guest.

---
