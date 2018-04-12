# 安裝modsecurity

sudo apt-get update
sudo apt-get install libapache2-modsecurity -y

sudo systemctl reload apache2
sudo  service apache2 reload
sudo apachectl -M | grep --color security2

sudo mv /etc/modsecurity/modsecurity.conf-recommended /etc/modsecurity/modsecurity.conf
sudo service apache2 reload

sudo sed -i "s/SecRuleEngine DetectionOnly/SecRuleEngine On/" /etc/modsecurity/modsecurity.conf
sudo sed -i "s/SecResponseBodyAccess On/SecResponseBodyAccess Off/" /etc/modsecurity/modsecurity.conf

sudo vim /etc/apache2/mods-enabled/security2.conf

------------------------------------------------------------------------------------

<IfModule security2_module>
        # Default Debian dir for modsecurity's persistent data
        SecDataDir /var/cache/modsecurity

        # Include all the *.conf files in /etc/modsecurity.
        # Keeping your local configuration in that directory
        # will allow for an easy upgrade of THIS file and
        # make your life easier
        IncludeOptional /etc/modsecurity/*.conf
        ------------------------------------------------------------------------------------
        IncludeOptional "/usr/share/modsecurity-crs/*.conf"
        IncludeOptional "/usr/share/modsecurity-crs/activated_rules/*.conf"
        //新增兩條新規則
        ------------------------------------------------------------------------------------
</IfModule>
------------------------------------------------------------------------------------

sudo service apache2 reload

------------------------------------------------------------------------------------

sudo cp /usr/share/modsecurity-crs/base_rules/modsecurity_crs_41_sql_injection_attacks.conf 
/usr/share/modsecurity-crs/activated_rules/
啟用sql_injection規則

------------------------------------------------------------------------------------
sudo service apache2 reload
