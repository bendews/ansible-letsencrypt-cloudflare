# LetsEncrypt-Cloudflare

This role simplifies the process of renewing LetsEncrypt Certificates when utilising Cloudflare as a DNS provider.

## Requirements

- Python >= 2.6
- OpenSSL (will automatically be installed if supported)

## Role Variables
Available variables are listed below, along with default values (see `defaults/main.yml` for many more variables that can be modified)

### Required Fields:

    letsencrypt_email: ""
    cloudflare_email: ""
    cloudflare_api_key: "" # 'Global' API key for specified Cloudflare account
    cloudflare_domain: "" # Cloudflare hosted DNS zone for entries to be created under

### Important Notes:
By default, the role will use the inventory hostname as the Common Name to request a certificate, and place all generated/recieved certificate files in `/etc/ssl/[Certificate Common Name]`, and all LetsEncrypt account files in `/etc/ssl/lets_encrypt`. These paths can all be overridden (see `defaults/main.yml`).

Furthermore, the certificate files can also be copied and renamed to another location after generation, by modifying any of the following variables:

    copy_csr_full_path: ""
    copy_crt_full_path: ""
    copy_key_full_path: ""
    copy_intermediate_full_path: "" # Requires include_intermediate set to 'yes'
    copy_fullchain_full_path: "" # Requires include_intermediate set to 'yes'

The role also defaults to using the "staging" letsencrypt endpoints which will generate functionally correct yet untrusted certificates. In order to generate valid certificates, set:  

    letsencrypt_production: yes


Generated files can be removed after use by specifying the following variable:

    cleanup_all: yes

# Example Playbook

    - hosts: servers
      tasks:
        - name: Renew/Download new SSL certificates
          include_role:
            name: letsencrypt-cloudflare
          vars:
            letsencrypt_email: "a@abc.com"
            cloudflare_email: "a@abc.com"
            cloudflare_domain: "abc.com"
            cloudflare_api_key: "AAABBBCCCDDDEEE111222333"
            letsencrypt_production: yes
            include_intermediate: yes


# TODO:

- Add support for multiple CN's

# License

MIT

# Author Information

Created in 2017 by [Ben Dews](bendews.com)