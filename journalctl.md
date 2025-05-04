## journalctl

### show time in UTC +0

    journalctl --utc

### view journal records from current boot

    journalctl -b

### list system boots

    journalctl --list-boots
    IDX BOOT ID                          FIRST ENTRY                 LAST ENTRY
    -1 7e87a0d5b2c9466db16230714d4e49cc Sat 2025-04-19 08:18:31 MST Fri 2025-05-02 20:04:32 MST
    0 f6605720327942d4a2349d6c9a78d6bd Fri 2025-05-02 13:04:51 MST Sun 2025-05-04 11:04:18 MST

### view journal records by boot index

    journalctl -b -1

### view journal records by boot id

    journalctl -b f6605720327942d4a2349d6c9a78d6bd

### time windows

    journalctl --since "2025-05-04 08:15:00"
    journalctl --since "2025-05-03" --until "2025-05-04 03:00"
    journalctl --since yesterday
    journalctl --since 06:00 --until '1 hour ago'

### journal records by service

    journalctl -u grafana-server -u nginx

### journal records by field

See systemd.journal-fields(7)

    journalctl \_PID=1234
    journalctl \_UID-1234
    journalctl \_GID-1234
    journalctl PRIORITY=6  (syslog levels 0 - 7)

### show all journal records related to an executable

    journalctl /usr/bin/kdumpctl

### kernel messages only

    journalctl -k
    journalctl --dmesg

### show entries interleaved from all available journals (including remote ones)

    journalctl --merge

### output

    journalctl [OPTIONS] -o FORMAT

    short : default
    short-full : timestamp as 'Sun 2025-05-04 12:01:01 MST'
    short-iso : timestamp as '2025-05-04T12:02:50-0700'
    short-iso-precise : timestamp as '2025-05-04T12:04:28.857986-0700'
    short-precise : timestamp as 'May 04 12:25:51.798891'
    short-monotonic : timestamp as '[145307.610353]'
    short-delta : timestamp as [ short-monotonic <time difference to the previous entry>]

    [147371.686023                ] revolver CROND[128186]: (root) CMD (run-parts /etc/cron.hourly)
    [147371.691782 <    0.005759 >] revolver run-parts[128189]: (/etc/cron.hourly) starting 0anacron
    [147371.697259 <    0.005477 >] revolver run-parts[128195]: (/etc/cron.hourly) finished 0anacron

    short-unix : timestamp as 1746389004.218441
    verbose : structured with all fields as labels present
    export : serialized for backup
    json : json object with all verbose fields
    json-pretty : pretty print json
    json-sse : json output wrapped in data object 'data:{...}'
    json-seq : json prefixed with 0x1E (^)
    cat : just messages
    with-unit : short but use unit and user names


