# nagios-commands
Send mail alerts with custom icons and colors from nagios commands.cfg


```
define command {
    command_name    notify-host-by-email
    command_line    /usr/bin/printf "%b" "<html><body>State: $HOSTSTATE$<br><br><table><tr><td>Host:</td><td> $HOSTALIAS$<td></tr><tr><td>Address:</td><td>$HOSTADDRESS$</td></tr><tr><td>Date/Time:</td><td> $LONGDATETIME$</td></tr><tr><td>Add Info:</td><td>$HOSTOUTPUT$</td></tr><tr><td>Tech Notes:</td><td style='color:green\;font-weight: bold\;'>$NOTIFICATIONCOMMENT$</td></tr></table></body></html>" |/bin/sed 's/State: DOWN/\&nbsp\;<b style=color:red\;background-color:white> \&#x2622\; <\/b>/g' | /bin/sed 's/State: UP/<b style=color:green\;background-color:white>\&nbsp\; \&#x2705\; <\/b>/g' |  /bin/sed 's/State: WARNING/<b style=color:green\;background-color:white>\&nbsp\; \&#x9888\; <\/b>/g' | /usr/bin/sendemail -f "user@domain.xyz" -u "$HOSTALIAS$ is [$HOSTSTATE$]"  -t $CONTACTEMAIL$  -s "smtp.office365.com:587" -xu "user@domain.xyz"     -xp "password" -o tls=yes
}


define command {
    command_name    notify-service-by-email
    command_line    /usr/bin/printf "<html><body>%b" "State: $SERVICESTATE$<br><table><tr><td>Host:</td><td> <b>$SERVICEDESC$</b></td></tr><tr><td> Gateway:</td><td> $HOSTALIAS$</td></tr></b> <tr><td>\n\nDate/Time:</td><td> $LONGDATETIME$</td></tr><tr><td>Info:</td><td> $SERVICEOUTPUT$</td></tr><tr><td>Tech Notes:</td><td style='color:blue\;font-weight: bold\;'>$NOTIFICATIONCOMMENT$</td></tr></table><div style='color:white'>$NOTIFICATIONTYPE$</div></body></html>" | /bin/sed 's/State: OK/<b style=color:green\;background-color:white>\&nbsp\; \&#x2705\; <\/b>/g' | /bin/sed 's/State: CRITICAL/\&nbsp\;<b style=color:red\;background-color:white> \&#x2622\; <\/b>/g' |/bin/sed 's/State: WARNING/<b style=color:green\;background-color:white>\&nbsp\; \&#x26A0\; <\/b>/g' | /usr/bin/sendemail -f "user@domain.xyz" -u "$SERVICEDESC$ [$SERVICESTATE$]"  -t $CONTACTEMAIL$  -s "smtp.office365.com:587" -xu "user@domain.xyz"     -xp "password" -o tls=yes
}

```
