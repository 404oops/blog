---
title: M3U Playlists (And how I mangled them into serving me)
description: ""
date: 2025-09-29T16:35:48.205Z
preview: ""
draft: false
tags:
  - configuration
  - opinion
categories:
  - computers
slug: m3u-playlists-mangled-serving
---

M3U playlists are one technological marvel that start with an `#EXTM3U` token and end with links.

Alright, that's a shitty explanation, but it really is like that. Sadly, the internet is full of bullshit and frankly I have to work around *that* bullshit so that it works.

Let's start with the obvious: Radios. Internet radio is a very awesome concept - MP3 Streams inside links. Most run on outdated SHOUTcast DNAS servers (which `dro` of the WACUP team said is so outdated that the SHOUTcast website restreams radios to Icecast servers, but that's another story I'd rather not get into)

As for Playlists, They're comprised of metadata and streams. There's not a concise standard for M3U files, but there is something that all players understand.

I took several IR stations that I liked and compiled them to a list:

```text
#EXTINF:0 tvg-logo="https://radios.404oops.com/lola.png" tvg-country="SR" tvg-type="Beograd",Lola
https://streaming.tdiradio.com/radiolola.mp3
#EXTINF:0 tvg-logo="https://radios.404oops.com/bumbum.png" tvg-country="SR" tvg-type="Beograd",BUM BUM
http://185.22.144.158:8000/
#EXTINF:0 tvg-logo="https://radios.404oops.com/antena.png" tvg-country="SR" tvg-type="Kruševac",ANTENA
http://www.antenaradio.rs:4560/
#EXTINF:0 tvg-logo="https://radios.404oops.com/puls.png" tvg-country="SR" tvg-type="Grocka",PULS
https://stream.iradio.pro/proxy/radiopuls?mp=/stream/
#EXTINF:0 tvg-logo="https://radios.404oops.com/play.png" tvg-country="SR" tvg-type="Beograd",PLAY
https://stream.playradio.rs:8443/play.mp3
#EXTINF:0 tvg-logo="https://radios.404oops.com/naxiboem.png" tvg-country="SR" tvg-type="Beograd",NAXI BOEM
https://naxidigital-boem128ssl.streaming.rs:8162/
#EXTINF:0 tvg-logo="https://radios.404oops.com/belami.png" tvg-country="SR" tvg-type="Niš",Belle Amie
http://92.60.239.23:8000/
#EXTINF:0 tvg-logo="https://radios.404oops.com/dzenarika.png" tvg-country="SR" tvg-type="Čačak",Dženarika
http://136.243.134.202:8000/
#EXTINF:0 tvg-logo="https://radios.404oops.com/bubamara.png" tvg-country="SR" tvg-type="Svrljig",Bubamara
http://92.60.230.200:5010/
#EXTINF:0 tvg-logo="https://radios.404oops.com/kolubara.png" tvg-country="SR" tvg-type="Lajkovac",Kolubara
https://media.radioexs.com/stream/radiokolubara
#EXTINF:0 tvg-logo="https://radios.404oops.com/velvet.png" tvg-country="GR" tvg-type="Thessaloniki",VELVET
https://metromedia.live24.gr/velvet968thess
#EXTINF:0 tvg-logo="https://radios.404oops.com/ble.png" tvg-country="GR" tvg-type="Thessaloniki",BLE
http://radio.lancom.gr:8006/stream1
#EXTINF:0 tvg-logo="https://radios.404oops.com/fear.png" tvg-country="NL" tvg-type="Amsterdam",FEAR
https://stream.fear.fm/
```

Now by "compile", I really mean it. I used a codeblock I made with Python to compute a JSON as well as some pictures I grabbed for each station. What you see up there is what the Python code compiled, which looks like this:

```py
import json

BASEURL = "https://radios.404oops.com"

def load_config():
  """Load configuration from a JSON file."""
  with open("radios.json", 'r') as file:
    config = json.load(file)
  return config

config = load_config()
for station in config:
  print(f"""Found station with:
  Name: {station['name']},
  URL: {station['url']},
  Logo: {station['logo']}""")

with open("index.m3u", 'w') as file:
  for station in config:
    file.write(
      f'#EXTINF:0 tvg-logo="{BASEURL}/{station["logo"]}" tvg-country="{station["country"]}" tvg-type="{station["city"]}",{station["name"]}\n'
    )
    file.write(f"{station['url']}\n")

with open("index.xml", 'w') as file:
  file.write('<?xml version="1.0" encoding="UTF-8"?>\n')
  file.write('<tv>\n')
  for station in config:
    file.write(f'  <channel id="{station["name"]}">\n')
    file.write(f'    <display-name>{station["name"]}</display-name>\n')
    file.write(f'    <icon src="{BASEURL}/{station["logo"]}" />\n')
    file.write(f'    <country>{station["country"]}</country>\n')
    file.write(f'    <category>{station["city"]}</category>\n')
    file.write('  </channel>\n')
    file.write('</tv>\n')

with open("basic.m3u", 'w') as file:
  for station in config:
    file.write(f"#EXTINF:0,{station['country']} - {station['name']}/{station['city']}\n")
    file.write(f"{station['url']}\n")
```

Now this is a really shitty way to automate stations from a JSON file whose schema you can already guess, but whatever.

```json
[
  {
    "name": "Lola",
    "country": "SR",
    "city": "Beograd",
    "url": "https://streaming.tdiradio.com/radiolola.mp3",
    "logo": "lola.png"
  },
  {
    "name": "BUM BUM",
    "country": "SR",
    "city": "Beograd",
    "url": "http://185.22.144.158:8000/",
    "logo": "bumbum.png"
  },
  {
    "name": "ANTENA",
    "country": "SR",
    "city": "Kruševac",
    "url": "http://www.antenaradio.rs:4560/",
    "logo": "antena.png"
  },
  {
    "name": "PULS",
    "country": "SR",
    "city": "Grocka",
    "url": "https://stream.iradio.pro/proxy/radiopuls?mp=/stream/",
    "logo": "puls.png"
  },
  {
    "name": "PLAY",
    "country": "SR",
    "city": "Beograd",
    "url": "https://stream.playradio.rs:8443/play.mp3",
    "logo": "play.png"
  },
  {
    "name": "NAXI BOEM",
    "country": "SR",
    "city": "Beograd",
    "url": "https://naxidigital-boem128ssl.streaming.rs:8162/",
    "logo": "naxiboem.png"
  },
  {
    "name": "Belle Amie",
    "country": "SR",
    "city": "Niš",
    "url": "http://92.60.239.23:8000/",
    "logo": "belami.png"
  },
  {
    "name": "Dženarika",
    "country": "SR",
    "city": "Čačak",
    "url": "http://136.243.134.202:8000/",
    "logo": "dzenarika.png"
  },
  {
    "name": "Bubamara",
    "country": "SR",
    "city": "Svrljig",
    "url": "http://92.60.230.200:5010/",
    "logo": "bubamara.png"
  },
  {
    "name": "Kolubara",
    "country": "SR",
    "city": "Lajkovac",
    "url": "https://media.radioexs.com/stream/radiokolubara",
    "logo": "kolubara.png"
  },
  {
    "name": "VELVET",
    "country": "GR",
    "city": "Thessaloniki",
    "url": "https://metromedia.live24.gr/velvet968thess",
    "logo": "velvet.png"
  },
  {
    "name": "BLE",
    "country": "GR",
    "city": "Thessaloniki",
    "url": "http://radio.lancom.gr:8006/stream1",
    "logo": "ble.png"
  },
  {
    "name": "FEAR",
    "country": "NL",
    "city": "Amsterdam",
    "url": "https://stream.fear.fm/",
    "logo": "fear.png"
  }
]
```

Now there's also Docker. Docker is useful because I have an NGINX Proxy Manager instance and also need this to be replicable and to work as part of the build setup. Which means that there's ALSO a Dockerfile:

```dockerfile
# Use official Python image to run init.py
FROM python:3.11-slim AS builder

WORKDIR /app
COPY . .
RUN python init.py

# Use official Nginx image for serving files
FROM nginx:alpine

COPY --from=builder /app /usr/share/nginx/html/

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

I didn't bother hiding the source, what you see is what you get.

### Here comes TV!

TV is the worst thing to ever touch the Internet. I stand behind this always. But, alas, I still have to compile and compute because, surprise surprise, there's bullshit that needs watching.

Specifically, the [IPTV-Org](https://iptv-org.github.io/) GitHub page is one source of channels that I can tolerate. But I have yet to build code that features all channels that work always.

TV is different. Since Radios I put in the playlist are always validated by, well, the fact that I'm the one curating them, GitHub repos are rarely maintained for channels like this. It SUCKS.

So I built a script that validates this.

```py
import requests
import subprocess
import os
from xml.etree.ElementTree import Element, SubElement, tostring, ElementTree

M3U_URLS = [
    "https://iptv-org.github.io/iptv/countries/rs.m3u",
    "https://iptv-org.github.io/iptv/languages/srp.m3u"
]

WORKING_STREAMS = {
    "Red TV": "https://edge8.pink.rs/redtv/playlist.m3u8",
    "TV Belle Amie": "http://195.252.83.95:1935/live/web/playlist.m3u8",
    "Kurir TV": "https://static.am.mediaoutcast.com/storage/nQJnjJkO/nQJnjJkO/stream/O68x4o8g/720p/720p.m3u8",
}

M3U_INDEX_PATH = "index.m3u"
TVXML_PATH = "index.xml"

# Helper to check if a stream is downloadable
def is_downloadable(url):
    try:
        r = requests.get(url, stream=True, timeout=10)
        if r.status_code == 200:
            return True
    except Exception:
        pass
    return False

# Helper to check if a stream is playable with ffmpeg
def is_playable_ffmpeg(url):
    try:
        result = subprocess.run([
            "ffmpeg", "-hide_banner", "-loglevel", "error", "-i", url, "-t", "1", "-f", "null", "-"
        ], stdout=subprocess.PIPE, stderr=subprocess.PIPE, timeout=15)
        return result.returncode == 0
    except Exception:
        return False

# Parse M3U and extract channels
def parse_m3u(url):
    channels = []
    try:
        r = requests.get(url, timeout=15)
        lines = r.text.splitlines()
        name, meta = None, {}
        for line in lines:
            if line.startswith("#EXTINF:"):
                info = line.split(',', 1)
                meta = {'extinf': info[0], 'name': info[1] if len(info) > 1 else ''}
                name = meta['name']
            elif line and not line.startswith('#'):
                if name:
                    channels.append({'name': name, 'url': line, 'meta': meta})
                name, meta = None, {}
    except Exception:
        pass
    return channels

# Write M3U file
def write_m3u(channels, path):
    with open(path, 'w') as f:
        f.write("#EXTM3U\n")
        for ch in channels:
            f.write(f"{ch['meta'].get('extinf', '#EXTINF:-1')},{ch['name']}\n{ch['url']}\n")

# Write TVXML file
def write_tvxml(channels, path):
    root = Element('tv')
    for ch in channels:
        c = SubElement(root, 'channel', id=ch['name'])
        display_name = SubElement(c, 'display-name')
        display_name.text = ch['name']
        stream = SubElement(c, 'stream')
        stream.text = ch['url']
    tree = ElementTree(root)
    tree.write(path, encoding='utf-8', xml_declaration=True)

# Main logic
def main():
    import re
    all_channels = []
    for m3u_url in M3U_URLS:
        print(f"Fetching M3U: {m3u_url}")
        all_channels.extend(parse_m3u(m3u_url))
    # Remove duplicates by name
    seen = {}
    for ch in all_channels:
        # Strip qualities in parentheses from name
        name = re.sub(r"\s*\([^)]*\)", "", ch['name']).strip()
        ch['name'] = name
        seen[name] = ch
    all_channels = list(seen.values())

    # Filter out channels containing 'Radio' or 'Not 24/7'
    filtered_channels = []
    for ch in all_channels:
        if ("Radio" in ch['name']) or ("Not 24/7" in ch['name']) or ("Radio" in ch['url']) or ("Not 24/7" in ch['url']):
            print(f"Filtered out: {ch['name']} ({ch['url']})")
            continue
        filtered_channels.append(ch)
    all_channels = filtered_channels

    working, replaced, failed = [], [], []
    for ch in all_channels:
        url = ch['url']
        name = ch['name']
        print(f"Checking stream: {name} ({url})")
        if is_downloadable(url):
            print(f"  Downloadable: PASS")
            if is_playable_ffmpeg(url):
                print(f"  FFmpeg playable: PASS")
                working.append(ch)
            else:
                print(f"  FFmpeg playable: FAIL")
                # Try fallback
                if name in WORKING_STREAMS:
                    fallback_url = WORKING_STREAMS[name]
                    print(f"  Trying fallback: {fallback_url}")
                    if is_downloadable(fallback_url) and is_playable_ffmpeg(fallback_url):
                        ch['url'] = fallback_url
                        replaced.append(ch)
                        print(f"  Fallback: PASS")
                    else:
                        failed.append(ch)
                        print(f"  Fallback: FAIL")
                else:
                    failed.append(ch)
        else:
            print(f"  Downloadable: FAIL")
            # Try fallback
            if name in WORKING_STREAMS:
                fallback_url = WORKING_STREAMS[name]
                print(f"  Trying fallback: {fallback_url}")
                if is_downloadable(fallback_url) and is_playable_ffmpeg(fallback_url):
                    ch['url'] = fallback_url
                    replaced.append(ch)
                    print(f"  Fallback: PASS")
                else:
                    failed.append(ch)
                    print(f"  Fallback: FAIL")
            else:
                failed.append(ch)

    # Add WORKING_STREAMS channels not already present
    for name, url in WORKING_STREAMS.items():
        # Strip qualities in parentheses
        name_stripped = re.sub(r"\s*\([^)]*\)", "", name).strip()
        if name_stripped not in [ch['name'] for ch in working + replaced]:
            ch = {'name': name_stripped, 'url': url, 'meta': {'extinf': f'#EXTINF:-1,{name_stripped}'}}
            print(f"Checking WORKING_STREAMS: {name_stripped} ({url})")
            if is_downloadable(url):
                print(f"  Downloadable: PASS")
                if is_playable_ffmpeg(url):
                    print(f"  FFmpeg playable: PASS")
                    working.append(ch)
                else:
                    print(f"  FFmpeg playable: FAIL")
                    failed.append(ch)
            else:
                print(f"  Downloadable: FAIL")
                failed.append(ch)

    # Write outputs
    write_m3u(working + replaced, M3U_INDEX_PATH)
    write_tvxml(working + replaced, TVXML_PATH)

    # Print report
    print("\nWorking streams:")
    for ch in working:
        print(f"  {ch['name']}: {ch['url']}")
    print("\nReplaced streams:")
    for ch in replaced:
        print(f"  {ch['name']}: {ch['url']}")
    print("\nFailed streams:")
    for ch in failed:
        print(f"  {ch['name']}: {ch['url']}")

if __name__ == "__main__":
    main()
```

The power of LLMs is astonishing since they can follow standards that I can't be bother looking up.

This is "Barely Legal" but it is legal nontheless. All of these streams are picked up from open sources and Cat-Catch from official sites with unprotected streams.

This script scrolls through this list and validates if the TV channels listed actually work in 2 instances:

1. If they are downloadable (as in 200 OK or have data in them)
2. If they're streamable from FFmpeg (which is what Jellyfin uses)

If both of these pass, then the stream is included. If not, then it resorts to a fallback, which is included in the `WORKING_STREAMS` "constant" (it's part of Python programmer semantics, not really a syntax thing, it's still a variable)

### What about after the script gets executed?

As per the Dockerfile, the files that get computed get stuffed into the NGINX Public-facing folder. The index.html is default, but the files are definitely there.

Try them! See what they have in store:

- Radios:
  - [https://radios.404oops.com/index.m3u](https://radios.404oops.com/index.m3u)
  - [https://radios.404oops.com/basic.m3u](https://radios.404oops.com/basic.m3u)
  - [https://radios.404oops.com/index.xml](https://radios.404oops.com/index.xml)
- TVs:
  - [https://tvs.vps.404oops.com/index.m3u](https://tvs.vps.404oops.com/index.m3u)
  - [https://tvs.vps.404oops.com/index.xml](https://tvs.vps.404oops.com/index.xml)

  Thanks for reading