# Install Tailscale on the Steam Deck

I installed [with these instructions from Tailscale](https://tailscale.com/blog/steam-deck) in the section `systemd-run`, starting "The easiest way to hoist this service into systemd would be to use systemd-run systemd-run lets you create one-off systemd jobs..."

The paths used in the official post are inconsistent, after `.local`, `share` is added sometimes. Here are all the scripts but correct and consolidated. I'll write some script that does all the install, adds the alias, creates a script in the home folder ~eventually. I should also set up the blog again, make this a post.

```
mkdir -p ~/.local/tailscale/steamos
cd ~/.local/tailscale/steamos
wget https://pkgs.tailscale.com/stable/tailscale_1.24.2_amd64.tgz
tar xzf tailscale_1.24.2_amd64.tgz
cd tailscale_1.24.2_amd64
```

```
sudo systemd-run \  
   --service-type=notify \  
   --description="Tailscale node agent" \  
   -u tailscaled.service \  
   -p ExecStartPre="/home/deck/.local/tailscale/steamos/tailscale_1.24.2_amd64/tailscaled --cleanup" \  
   -p ExecStopPost="/home/deck/.local/tailscale/steamos/tailscale_1.24.2_amd64/tailscaled --cleanup" \  
   -p Restart=on-failure \  
   -p RuntimeDirectory=tailscale \  
   -p RuntimeDirectoryMode=0755 \  
   -p StateDirectory=tailscale \  
   -p StateDirectoryMode=0700 \  
   -p CacheDirectory=tailscale \  
   -p CacheDirectoryMode=0750 \  
   "/home/deck/.local/tailscale/steamos/tailscale_1.24.2_amd64/tailscaled" \  
   "--state=/var/lib/tailscale/tailscaled.state" \  
   "--socket=/run/tailscale/tailscaled.sock"  
sudo /home/deck/.local/tailscale/steamos/tailscale_1.24.2_amd64/tailscale up --operator=deck --qr
```

Log In.

```
sudo /home/deck/.local/share/tailscale/steamos/tailscale_1.24.2_amd64/tailscale up --operator=deck --qr
```

Set an alias to access Tailscale more easily.

```
alias tailscale="/home/deck/.local/tailscale/steamos/tailscale_1.24.2_amd64/tailscale"
```

