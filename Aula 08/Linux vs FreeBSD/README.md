# Linux e FreeBSD: semelhanças, diferenças e curiosidades

## 1. Introdução

Linux e FreeBSD são sistemas Unix-like e, à primeira vista, podem parecer muito semelhantes. Ambos oferecem shell, permissões no estilo Unix, usuários e grupos, processos, sockets, sistemas de arquivos hierárquicos, ferramentas de linha de comando, servidores, ambientes gráficos e uma grande quantidade de software livre.

Apesar disso, a forma como cada projeto é construído e administrado é diferente.

A distinção mais importante é conceitual:

- **Linux** é, estritamente, um kernel. As distribuições combinam esse kernel com bibliotecas, ferramentas, gerenciadores de pacotes, init systems, ambientes gráficos e outros componentes vindos de muitos projetos.
- **FreeBSD** é desenvolvido como um sistema operacional completo, com kernel, libc, ferramentas de userland, documentação e sistema-base mantidos de forma integrada pelo mesmo projeto.

Neste texto, o termo "Linux" é usado no sentido prático de **distribuições que utilizam o kernel Linux**, e não para sugerir que todas as distribuições possuam exatamente o mesmo userland.

---

## 2. O que os dois têm em comum

Linux e FreeBSD pertencem à tradição dos sistemas Unix-like.

Em ambos é comum encontrar:

- hierarquia de diretórios iniciada em `/`;
- usuários e grupos;
- usuário `root`;
- permissões `rwx`;
- processos e sinais;
- pipes e redirecionamento;
- shells;
- sockets;
- SSH;
- ferramentas como `ls`, `cp`, `mv`, `grep`, `sed`, `awk`, `find` e `tar`;
- programação em C;
- bibliotecas compartilhadas;
- X11 e ambientes gráficos;
- servidores Web, bancos de dados e ferramentas de desenvolvimento;
- virtualização;
- compilação de software a partir do código-fonte.

Por isso, um usuário experiente de Linux normalmente reconhece rapidamente o ambiente FreeBSD, mesmo encontrando diferenças nos detalhes dos comandos e na administração do sistema.

---

## 3. Kernel Linux x sistema FreeBSD

### Linux

Linux é o kernel responsável por:

- escalonamento;
- gerenciamento de memória;
- drivers;
- sistemas de arquivos;
- rede;
- processos;
- interfaces de sistema para o espaço de usuário.

Uma distribuição monta o restante do sistema ao redor dele.

Exemplos de distribuições:

- Arch Linux;
- Debian;
- Ubuntu;
- Fedora;
- Gentoo;
- Alpine Linux.

Essas distribuições podem usar componentes diferentes entre si.

### FreeBSD

O FreeBSD é distribuído como um sistema integrado.

O projeto mantém conjuntamente:

- kernel;
- libc;
- ferramentas fundamentais;
- utilitários administrativos;
- sistema `rc`;
- documentação;
- man pages;
- partes importantes do userland.

Isso cria uma separação clara entre:

```text
FreeBSD base system
```

e:

```text
third-party software
```

Essa diferença de filosofia aparece em praticamente toda a administração do sistema.

---

## 4. Sistema-base e software de terceiros

No FreeBSD, os componentes do sistema-base ficam principalmente em locais como:

```text
/bin
/sbin
/usr/bin
/usr/sbin
/etc
```

Software instalado posteriormente por `pkg` ou Ports utiliza predominantemente:

```text
/usr/local/bin
/usr/local/sbin
/usr/local/lib
/usr/local/etc
```

Por exemplo, um shell instalado externamente pode aparecer como:

```text
/usr/local/bin/zsh
```

Em muitas distribuições Linux, essa fronteira entre sistema-base e software de terceiros não é tão explícita. O gerenciador de pacotes da distribuição normalmente gerencia quase todo o sistema.

---

## 5. Gerenciamento de pacotes

### Linux

Não existe um único gerenciador de pacotes universal.

Alguns exemplos:

| Distribuição | Gerenciador |
|---|---|
| Debian / Ubuntu | `apt` / `dpkg` |
| Arch Linux | `pacman` |
| Fedora | `dnf` / RPM |
| openSUSE | `zypper` / RPM |
| Alpine | `apk` |

### FreeBSD

O FreeBSD possui o gerenciador binário:

```sh
pkg
```

Exemplos:

```sh
pkg update
```

```sh
pkg upgrade
```

```sh
pkg install firefox
```

Além disso existe a **Ports Collection**, que permite compilar software a partir do código-fonte com patches e opções próprias do FreeBSD.

Exemplo conceitual:

```sh
cd /usr/ports/categoria/programa
make install clean
```

Packages e Ports são duas interfaces para o ecossistema de software de terceiros do FreeBSD.

---

## 6. Shells

### Linux

O shell padrão depende da distribuição.

Bash é extremamente comum, mas não universal.

Exemplos possíveis:

```text
bash
ash
dash
zsh
fish
```

### FreeBSD

Na VM deste trabalho, o usuário utiliza:

```text
/bin/sh
```

O FreeBSD também inclui shells no sistema-base, enquanto Bash, Zsh e Fish podem ser instalados como software adicional.

Exemplo:

```sh
pkg install bash zsh fish
```

E normalmente aparecem em:

```text
/usr/local/bin/bash
/usr/local/bin/zsh
/usr/local/bin/fish
```

Um script escrito estritamente para `sh` tende a ser mais portável; um script que utiliza extensões específicas do Bash pode não funcionar em `/bin/sh`.

---

## 7. Userland GNU x userland BSD

Uma diferença frequentemente percebida por quem migra de uma distribuição GNU/Linux é que vários comandos possuem implementações diferentes.

Exemplo clássico: `sed`.

Em muitas distribuições GNU/Linux:

```sh
sed -i 's/foo/bar/g' arquivo
```

No FreeBSD:

```sh
sed -i '' 's/foo/bar/g' arquivo
```

Outros comandos podem possuir opções e formatos diferentes:

- `stat`;
- `date`;
- `find`;
- `ps`;
- `tar`;
- ferramentas de rede.

Isso não significa que um esteja "certo" e o outro "errado": são implementações diferentes, com histórias e extensões próprias.

---

## 8. Inicialização e serviços

### Linux

O sistema de inicialização depende da distribuição.

Hoje, `systemd` é muito comum, mas não é obrigatório.

Outros exemplos:

- OpenRC;
- runit;
- s6;
- SysV-style init.

Em systemd:

```sh
systemctl enable sshd
systemctl start sshd
```

### FreeBSD

O FreeBSD utiliza seu sistema tradicional `rc`.

Configurações persistentes normalmente são colocadas em:

```text
/etc/rc.conf
```

Para habilitar um serviço:

```sh
sysrc sshd_enable="YES"
```

Para iniciar:

```sh
service sshd start
```

Para verificar:

```sh
service sshd status
```

Esse modelo é relativamente simples e baseado em scripts de inicialização e variáveis de configuração.

---

## 9. Gerenciamento de usuários

Em distribuições Linux é comum encontrar:

```sh
useradd
usermod
groupadd
```

No FreeBSD, uma ferramenta central é:

```sh
pw
```

Exemplo:

```sh
pw groupmod wheel -m usuario
```

O grupo `wheel` é especialmente importante para usuários que precisam utilizar `su` para assumir `root`.

---

## 10. `sudo` não é obrigatório no FreeBSD

Em várias distribuições Linux de desktop, o uso de `sudo` é tão comum que pode parecer parte essencial do Unix.

No FreeBSD, `sudo` é software adicional.

É possível administrar o sistema com:

```sh
su -
```

E instalar `sudo` apenas se desejado:

```sh
pkg install sudo
```

Isso evidencia novamente a separação entre sistema-base e aplicações externas.

---

## 11. `/proc`, `/sys` e `sysctl`

### Linux

Dois pseudo-filesystems possuem grande importância:

```text
/proc
/sys
```

`/proc` expõe informações de processos e kernel.

`/sys` representa grande parte do modelo de dispositivos e objetos do kernel através de `sysfs`.

### FreeBSD

O FreeBSD não depende de `/proc` da mesma maneira.

Muitas informações e parâmetros são acessados através de interfaces como:

```sh
sysctl
```

Exemplo:

```sh
sysctl hw.model
```

O FreeBSD também utiliza `devfs` para dispositivos em `/dev`.

Essa diferença fica clara para scripts que assumem a existência de arquivos específicos em `/proc` ou `/sys`.

---

## 12. Rede

Os dois sistemas possuem stacks TCP/IP maduras, mas as ferramentas tradicionais diferem.

### Linux moderno

É comum usar:

```sh
ip addr
```

```sh
ip route
```

### FreeBSD

Ferramentas tradicionais continuam importantes:

```sh
ifconfig
```

```sh
netstat -rn
```

```sh
route
```

Configuração persistente costuma passar por:

```text
/etc/rc.conf
```

---

## 13. Firewalls

### Linux

Atualmente, o framework central é o Netfilter, normalmente administrado com:

```text
nftables
```

Sistemas mais antigos ou compatibilidade ainda podem envolver `iptables`.

### FreeBSD

O FreeBSD oferece opções como:

- PF;
- IPFW.

O administrador pode escolher a ferramenta de acordo com o objetivo do sistema.

---

## 14. Sistemas de arquivos

### Linux

O ecossistema Linux suporta muitos sistemas de arquivos, entre eles:

- ext4;
- XFS;
- Btrfs;
- F2FS.

OpenZFS também pode ser utilizado, embora não faça parte do kernel Linux mainline.

### FreeBSD

Dois sistemas particularmente associados ao FreeBSD são:

- UFS;
- OpenZFS.

O FreeBSD possui integração oficial extensa com OpenZFS, incluindo suporte pelo instalador e recursos administrativos.

A VM deste trabalho utiliza UFS.

---

## 15. ZFS

O OpenZFS é um dos recursos mais conhecidos no ecossistema FreeBSD.

Ele combina conceitos de:

- sistema de arquivos;
- gerenciamento de volumes;
- checksums;
- snapshots;
- clones;
- compressão;
- pools de armazenamento.

O FreeBSD também integra ZFS com Jails e Boot Environments.

---

## 16. Containers: Jails x namespaces/cgroups

### Linux

Containers Linux modernos são construídos principalmente sobre recursos do kernel como:

- namespaces;
- cgroups;
- capabilities;
- seccomp.

Ferramentas como Docker, Podman e LXC utilizam esses mecanismos.

### FreeBSD

O FreeBSD possui **Jails**.

Uma jail isola ambientes no mesmo kernel FreeBSD e pode restringir:

- processos;
- usuários;
- rede;
- filesystem;
- recursos visíveis.

Jails não são máquinas virtuais completas: compartilham o kernel com o host.

O FreeBSD também consegue integrar Jails com datasets ZFS.

---

## 17. Virtualização

### Linux

Tecnologias comuns:

- KVM;
- QEMU;
- VirtualBox;
- VMware;
- Xen.

### FreeBSD

O FreeBSD pode:

- funcionar como convidado em VirtualBox;
- funcionar como convidado em outros hipervisores;
- atuar como host com `bhyve`;
- utilizar QEMU e VirtualBox em cenários suportados.

Neste trabalho, o FreeBSD foi executado como guest no VirtualBox sobre Arch Linux.

---

## 18. Licenciamento

### Kernel Linux

O kernel Linux é distribuído sob GPLv2.

A GPL é uma licença copyleft: ao distribuir trabalhos derivados cobertos pela licença, existem obrigações de disponibilização do código-fonte sob os termos aplicáveis.

As distribuições Linux completas contêm software sob muitas licenças diferentes.

### FreeBSD

O Projeto FreeBSD procura manter um sistema operacional completo majoritariamente sob licenças BSD permissivas.

Licenças BSD permitem ampla reutilização, inclusive em produtos proprietários, desde que as condições da licença sejam respeitadas.

Essa diferença de filosofia de licenciamento teve impacto histórico na adoção de código BSD por outros produtos e projetos.

---

## 19. Compiladores

Muitas distribuições Linux utilizam GCC como compilador padrão tradicional, embora Clang também seja amplamente disponível.

No FreeBSD moderno, Clang/LLVM possui papel central no sistema-base.

Isso é mais um exemplo da diferença entre:

```text
um sistema completo mantido como um projeto
```

e:

```text
uma distribuição montada a partir de muitos componentes selecionados
```

---

## 20. Atualização do sistema

### Linux

Normalmente o gerenciador de pacotes da distribuição controla:

- kernel;
- bibliotecas;
- userland;
- aplicações.

Exemplo conceitual em Debian/Ubuntu:

```sh
apt update
apt full-upgrade
```

### FreeBSD

Existe uma distinção maior entre:

1. sistema-base;
2. pacotes de terceiros.

O comando:

```sh
pkg upgrade
```

é voltado ao conjunto de pacotes instalados pelo `pkg`.

Essa separação reforça a arquitetura administrativa do FreeBSD.

---

## 21. Documentação

Uma característica marcante do FreeBSD é a integração entre:

- Handbook;
- man pages;
- documentação do sistema-base;
- documentação de APIs do kernel;
- Ports.

O Handbook funciona como uma referência central para instalação, rede, armazenamento, segurança, virtualização, desktop e administração.

No Linux, existe documentação excelente, porém distribuída entre:

- documentação do kernel;
- documentação da distribuição;
- man pages;
- projetos individuais;
- documentação de systemd, GNU, desktop environments etc.

---

## 22. Compatibilidade com software Linux

O FreeBSD possui um mecanismo de compatibilidade binária com Linux.

Isso permite executar determinados binários Linux em FreeBSD quando a compatibilidade e as bibliotecas necessárias estão configuradas.

Também existem Linux Jails, que combinam o mecanismo de compatibilidade com um userland Linux dentro de uma jail.

Isso não transforma o FreeBSD em Linux: o kernel continua sendo FreeBSD.

---

## 23. Hardware e desktop

De maneira geral, distribuições Linux possuem um ecossistema maior de:

- drivers de hardware recente;
- suporte de fabricantes;
- software proprietário;
- jogos;
- ferramentas desktop;
- aplicações comerciais.

O FreeBSD possui um ecossistema desktop menor, embora ambientes como XFCE, KDE e outros estejam disponíveis.

Em servidores, redes, storage, firewalls e ambientes Unix tradicionais, o FreeBSD possui forte presença técnica.

Esses pontos não definem um "vencedor"; representam diferenças de ecossistema e prioridades.

---

## 24. Comandos equivalentes

| Objetivo | Linux comum | FreeBSD |
|---|---|---|
| instalar pacote | `apt install`, `pacman -S`, `dnf install` | `pkg install` |
| atualizar pacotes | varia por distribuição | `pkg upgrade` |
| serviço | `systemctl` em systemd | `service` |
| habilitar serviço | `systemctl enable` | `sysrc ..._enable="YES"` |
| interfaces de rede | `ip addr` | `ifconfig` |
| rotas | `ip route` | `netstat -rn`, `route` |
| alterar usuário | `usermod` | `pw usermod` |
| alterar grupo | `groupmod` | `pw groupmod` |
| elevar para root | `sudo`, `su` | `su`, `sudo` opcional |
| parâmetros do kernel | `/proc`, `/sys`, `sysctl` | principalmente `sysctl` |
| software compilado | depende da distro | Ports Collection |

---

## 25. O que foi mais familiar na prática

Durante o uso do FreeBSD, muitos comandos pareceram imediatamente familiares:

```sh
cd
pwd
ls
cp
mv
rm
cat
grep
find
chmod
chown
mount
ps
kill
ssh
```

Também permaneceram familiares conceitos como:

- root;
- home directories;
- permissões;
- processos;
- sinais;
- pipes;
- arquivos de configuração;
- shells;
- X11.

Isso confirma que Linux e FreeBSD compartilham a tradição Unix, mesmo com implementações diferentes.

---

## 26. O que mais chamou atenção na prática

Na VM deste trabalho, as diferenças mais visíveis foram:

1. `sudo` não estava instalado;
2. o shell padrão era `/bin/sh`;
3. Bash não fazia parte da instalação base;
4. software externo ficava em `/usr/local`;
5. `pkg` substituía os gerenciadores conhecidos de distribuições Linux;
6. serviços eram controlados por `service` e `sysrc`;
7. o sistema-base era claramente separado dos pacotes de terceiros;
8. os Guest Additions exigiram atenção ao controlador `VBoxSVGA`;
9. ferramentas BSD possuem pequenas diferenças de sintaxe em relação às variantes GNU;
10. o Single User Mode permitiu recuperar o sistema sem mídia externa.

---

## 27. Curiosidades

### 27.1. Beastie

O mascote tradicional associado ao BSD é o **BSD Daemon**, conhecido informalmente como **Beastie**.

O nome "daemon" não se refere a demônio no sentido religioso; em sistemas Unix, daemon é um processo de background que presta algum serviço.

### 27.2. BSD é mais antigo que Linux

A família BSD nasceu da Berkeley Software Distribution, desenvolvida historicamente na Universidade da Califórnia em Berkeley a partir do Unix.

Linux surgiu posteriormente, em 1991, como um novo kernel Unix-like.

### 27.3. A API de sockets tem forte herança BSD

A interface de programação de sockets que influenciou profundamente a programação de redes Unix é tradicionalmente associada ao Berkeley sockets API.

Termos e chamadas como:

```text
socket()
bind()
listen()
accept()
connect()
```

fazem parte dessa herança.

### 27.4. FreeBSD pode hospedar Linux userlands

Com Linux Binary Compatibility, o FreeBSD pode executar determinados programas Linux e até criar Linux Jails, mantendo o kernel FreeBSD.

### 27.5. O sistema-base é versionado como uma unidade

Enquanto uma distribuição Linux combina versões de muitos projetos independentes, o FreeBSD publica releases do sistema-base como uma unidade, por exemplo:

```text
FreeBSD 15.1-RELEASE
```

---

## 28. Linux ou FreeBSD?

A resposta depende do objetivo.

### Distribuições Linux costumam ser vantajosas quando:

- é necessário suporte amplo a hardware recente;
- o foco é desktop e jogos;
- o software desejado possui suporte oficial apenas para Linux;
- existe dependência forte de Docker/Kubernetes;
- é desejado um ecossistema enorme de distribuições e ferramentas.

### FreeBSD pode ser especialmente interessante quando:

- é valorizado um sistema-base integrado;
- o foco envolve redes;
- storage e ZFS são importantes;
- Jails são úteis;
- documentação centralizada é desejada;
- a licença BSD e sua permissividade são relevantes;
- deseja-se estudar um Unix-like fora do ecossistema Linux.

Não se trata de definir qual sistema é "melhor", mas de compreender que eles fazem escolhas arquiteturais, administrativas e de licenciamento diferentes.

---

## 29. Conclusão

A experiência com a VM mostrou que a semelhança superficial entre Linux e FreeBSD esconde diferenças estruturais importantes.

No terminal, muitas tarefas são praticamente idênticas. Porém, ao administrar o sistema, aparecem diferenças claras:

- origem e organização do userland;
- sistema-base integrado;
- layout de software de terceiros;
- gerenciadores de pacotes;
- init e serviços;
- shells;
- interfaces do kernel;
- licenciamento;
- virtualização e containers;
- filosofia de desenvolvimento.

Para quem já conhece Linux, estudar FreeBSD é uma forma prática de separar conceitos realmente pertencentes à tradição Unix daqueles que são específicos do ecossistema GNU/Linux ou de uma distribuição.

---

## 30. Referências

### FreeBSD

- FreeBSD Handbook: https://docs.freebsd.org/en/books/handbook/
- FreeBSD Quickstart Guide for Linux Users: https://docs.freebsd.org/en/articles/linux-users/
- Packages and Ports: https://docs.freebsd.org/en/books/handbook/ports/
- Virtualization: https://docs.freebsd.org/en/books/handbook/virtualization/
- Jails and Containers: https://docs.freebsd.org/en/books/handbook/jails/
- ZFS: https://docs.freebsd.org/en/books/handbook/zfs/
- FreeBSD License Policies: https://docs.freebsd.org/en/articles/license-guide/

### Linux e GNU

- Linux Kernel Documentation: https://docs.kernel.org/
- GNU — Linux and the GNU System: https://www.gnu.org/gnu/linux-and-gnu.html
- Filesystem Hierarchy Standard 3.0: https://refspecs.linuxfoundation.org/FHS_3.0/fhs-3.0.html
