練習用環境の構築

## isuconユーザ作成
```sh
sudo yum install -y curl
sudo useradd isucon
sudo passwd isucon
sudo usermod -G wheel isucon

sudo mkdir -p /home/isucon/.ssh
curl https://github.com/inada-s.keys | sudo tee -a /home/isucon/.ssh/authorized_keys
sudo chown -R isucon:isucon /home/isucon/.ssh
sudo chmod 700 /home/isucon/.ssh
sudo chmod 600 /home/isucon/.ssh/authorized_keys

sudo yum -y update
sudo reboot
```

## isuconユーザで作業
```sh
sudo yum groupinstall -y "Development Tools"
sudo yum install -y kernel-devel kernel-headers
sudo yum install -y git curl screen wget tmux git tree make python-dev python-pip python3-dev python3-pip unzip zip graphviz sysstat
```

## FlameGraph
```sh
wget https://github.com/brendangregg/FlameGraph/archive/master.zip
unzip master.zip
sudo mv FlameGraph-master/* /usr/local/bin/
rm -r FlameGraph-master
rm master.zip
```

## Golang
```sh
wget https://dl.google.com/go/go1.11.linux-amd64.tar.gz && sudo tar -C /usr/local -xzf go1.11.linux-amd64.tar.gz && rm go1.11.linux-amd64.tar.gz
GOPATH=~/go
GOBIN=~/go/bin
mkdir -p ~/go
cat << EOF >> ~/.bash_profile
export PATH=$PATH:/usr/local/go/bin:$GOBIN
export GOPATH=$GOPATH
EOF
```

## Vim
```sh
sudo yum remove vim-enhanced
sudo yum install lua-devel ncurses-devel
sudo yum install -y ruby ruby-devel lua lua-devel luajit luajit-devel ctags mercurial python python-devel python3 python3-devel tcl-devel perl perl-devel perl-ExtUtils-ParseXS perl-ExtUtils-XSpp perl-ExtUtils-CBuilder perl-ExtUtils-Embed ncurses-devel
git clone https://github.com/vim/vim.git
cd vim
./configure --with-features=huge --enable-multibyte --enable-rubyinterp --enable-pythoninterp --enable-perlinterp --enable-luainterp --enable-gui=gtk2 --enable-cscope --with-tlib=ncurses --prefix=/usr
sudo make install

cd ~
curl https://raw.githubusercontent.com/Shougo/neobundle.vim/master/bin/install.sh > neobundle-install.sh
sh ./neobundle-install.sh
rm neobundle-install.sh

curl https://raw.githubusercontent.com/inada-s/isucon/master/.vimrc > ~/.vimrc
vim +NeoBundleInstall  +qall
vim +GoInstallBinaries +qall
```

## openresty
```sh
sudo yum -y install yum-utils
sudo yum-config-manager --add-repo https://openresty.org/package/centos/openresty.repo
sudo yum -y install openresty openresty-resty
```

