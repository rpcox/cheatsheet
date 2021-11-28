## curl
---

    curl <url>                                          # GET
    curl -I <url>                                       # HEAD
    curl -I --http2 <url>                               # HTTP2 support check
    curl -X POST <url>                                  # POST
    curl --dns-servers <address>                        # use specific DNS server
    curl -H "Header: value" <url>                       # add header fields
    curl -H "@file" <url>                               # add header fields from file
    curl -i <url>                                       # show response header
    curl -k <url>                                       # don't verify server cert
    curl -4|-6 <url>                                    # verify only IPv4 or v6 addresses
    curl <url>/file -o filename                         # save file as filename
    curl <url>/file -O                                  # save file as original filename
    curl --referer URL <url>                            # set referer to URL
    curl --user-agent <agent>  <url>                    # set user agent to <agent>
    curl -L <url>                                       # follow redirects
    curl --upload-file <file> <url>                     # upload file to <url>
    curl -r start-end -o myfile https://site./file      # retrieve the byte range (start-end) of a file
    curl -D file <url>                                  # save response header in file
    curl -D - <url> -o /dev/null                        # test download time, write header to stdout



### POST JSON to <url>

    curl -d '{"key1": "value1", "key2": "value2"}' -H "Content-Type: application/json" <url>
    curl -d @file.json -H "Content-Type: application/json" <url>

### File upload to <url>

    curl -F file=@"path/to/data.txt" <url>
    curl -F files=@"path/to/file1" -F files=@"path/to/file2" <url>

