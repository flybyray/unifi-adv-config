# Configure UniFi - USG

An ansible way

> The config.gateway.json is a file that sits in the UniFi Network application filesystem and allows custom changes to the USG that aren't available in the web GUI. 

see: [UniFi - USG Advanced Configuration Using config.gateway.json](https://help.ui.com/hc/en-us/articles/215458888-UniFi-USG-Advanced-Configuration-Using-config-gateway-json)


## Usage

Check/Diff changes
```bash
ansible-playbook unifi-config.yml --check --diff
```

Apply config if usefull
```bash
ansible-playbook unifi-config.yml
```


## Customization

* `git clone <repourl>`

Files `hosts` and `config_gateway_json.yml` are ansible-vault encrypted.
The `ansible.cfg` defines a file `vault_password` which is in .gitignore and will hold the password for the encrypted files.

* Create the `vault_password` file. E.g. interactive shell example:

```bash
unset -v password
set +o allexport
IFS= read -rsp 'Ansible Vault Password: ' password < /dev/tty &&
  printf '%s' "$password" > vault_password
```

* (optional) `export EDITOR='code --wait';` this allows you to edit vault files in an editor called via `code`.

* Create or edit `hosts` with `ansible-vault edit hosts`.
  * If that fails, check your vault password.
  * Or if you are not the owner of the repo you need to create a your own `hosts` file.
    * `rm hosts;touch hosts;ansible-vault encrypt hosts`
    * `ansible-vault edit hosts`
    * example:
      ```ini
      [usg]
      usg-3p ansible_user=username ansible_password=password
      [uck]
      unifi-cloudkey ansible_user=username ansible_password=password
      ```

* Create or edit `config_gateway_json.yml` with `ansible-vault edit config_gateway_json.yml`.
  * If that fails, check your vault password.
  * Or if you are not the owner of the repo you need to create a your own `config_gateway_json.yml` file.
    * `rm hosts;touch hosts;ansible-vault encrypt hosts`
    * `ansible-vault edit config_gateway_json.yml`
    * example based on a [IPv6 und UniFi USG mit Telekom DSL[German]](https://www.bjoerns-techblog.de/2018/02/ipv6-und-unifi-usg/):
      ```yaml
      ---
      config_gateway_json:
        unifi-cloudkey:
          interfaces:
            ethernet:
              eth0:
                pppoe:
                  "0":
                    dhcpv6-pd:
                      prefix-only: "''"
      sitepath:
        unifi-cloudkey: ~
      ```


