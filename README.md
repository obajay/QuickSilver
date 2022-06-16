# QuickSilver
git clone https://github.com/ingenuity-build/quicksilver.git --branch v0.3.0

cd quicksilver

make install

quicksilverd version

#v0.3.0

quicksilverd init <name_node> --chain-id rhapsody-5

wget -O $HOME/.quicksilverd/config/genesis.json "https://raw.githubusercontent.com/ingenuity-build/testnets/main/rhapsody/genesis.json"

quicksilverd config chain-id rhapsody-5

external_address=$(wget -qO- eth0.me)

peers=""

sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/; s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.quicksilverd/config/config.toml

seeds="dd3460ec11f78b4a7c4336f22a356fe00805ab64@seed.rhapsody-5.quicksilver.zone:26656"

sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" $HOME/.quicksilverd/config/config.toml


sudo tee /etc/systemd/system/quicksilverd.service > /dev/null <<EOF
                                                                    
[Unit]
                                                                    
Description=quicksilver
                                                                    
After=network-online.target
                                                                    

                                                                    
[Service]
                                                                    
User=$USER
                                                                    
ExecStart=$(which quicksilverd) start
                                                                    
Restart=on-failure
                                                                    
RestartSec=3
                                                                    
LimitNOFILE=65535
                                                                    

                                                                    
[Install]
                                                                    
WantedBy=multi-user.target
                                                                    
EOF
                                                                    

sudo systemctl daemon-reload && \
sudo systemctl enable quicksilverd && \
sudo systemctl restart quicksilverd && sudo journalctl -u quicksilverd -f -o cat
