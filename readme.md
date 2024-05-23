# bittorrentv2-alternative

[bittorrent v2](https://blog.libtorrent.org/2020/09/bittorrent-v2/) has poor adoption

large trackers like [thepiratebay.org](https://thepiratebay.org/) only support bittorrent v1



## info/sha256.txt

the file `info/sha256.txt` stores sha256 hashes of all files in the torrent

to generate this file, you can use `info-hashes.sh`

```sh
#!/usr/bin/env bash
# hash all files in workdir
out=info/sha256.txt
if [ -e "$out" ]; then
  echo "error: output file exists: $out"
  exit 1
fi
mkdir -p info
find . -type f -not -path './info/*' -printf '%P\n' | LC_ALL=C sort | xargs sha256sum >"$out"
echo "done $out"
```



## why info/sha256.txt

this allows file-deduplication across torrents, aka [content-addressed storage](https://en.wikipedia.org/wiki/Content-addressable_storage)

CAS is one benefit of bittorrentv2



## why the info directory

because traditionally, metadata is stored in `.nfo` files, `nfo` as in `info`

but that format is too limiting, its better to use an `info` directory,  
so we can also store non-text files, like images or documents



## why sha256

- because [sha1 is broken](https://crypto.stackexchange.com/questions/3690/why-is-sha-1-considered-broken)
- because [sha256 is not broken](https://crypto.stackexchange.com/questions/47809/why-havent-any-sha-256-collisions-been-found-yet)
- because sha512 would be a waste of disk space ([npm uses sha512 in its lockfiles](https://stackoverflow.com/a/78488187/10440128))
- because popular tools use sha256:
  [bittorrentv2](https://blog.libtorrent.org/2020/09/bittorrent-v2/),
  [git](https://stackoverflow.com/questions/60087759/git-is-moving-to-new-hashing-algorithm-sha-256-but-why-git-community-settled-on),
  [bitcoin](https://en.bitcoin.it/wiki/SHA-256),
  ...



## why not bittorrentv2

[why Info hash V2 or Hybrid torrents are not being in circulation yet? what are your thoughts to promote the technology's mass adoption?](https://old.reddit.com/r/Piracy/comments/td37u5/why_info_hash_v2_or_hybrid_torrents_are_not_being/)

Even though most popular P2P clients have introduced support for v2 hashing & also hybrid hashing [both v1 and v2 URI in magnet or torrent] why haven't uploaders are not able to adopt to this yet?

- Many people use older Clients which don't support v2
- Debian Stable qBittorrent is at 4.2.5
- Some Issues which were introduced with libtorrent 2 (Memory Managment and other smaller bugs)
- No backwards compatibilty, v1 Clients can't download v2 Torrents.
  - solved by hybrid
- benefit from BTv2 (more secure hashing, more compact directory handling, potentially improved search/indexing from file based hashing)
- It's unlikely we'll jump straight to V2 any time soon, which means hybrid is the only possibility.
- hybrid torrents require padding to be added to the v1 component, which isn't that widely supportedhybrid torrents require padding to be added to the v1 component, which isn't that widely supportedhybrid torrents require padding to be added to the v1 component, which isn't that widely supported
- BTv2 adds some nice improvements, but unfortunately, v1 has been around for so long, with all manner of clients, that transitioning, even via hybrid torrents, will have some growing pains.

[Info hash V2 or Hybrid torrents technology for masses adoption](https://old.reddit.com/r/DataHoarder/comments/td3i5k/info_hash_v2_or_hybrid_torrents_technology_for/)

- Because for most people, v1 is all they need. SHA1 attacks are possible, but not really practical. Sharing files between torrents would be nice, but it's not essential. And while v2 can handle much larger torrents, few people handle multi-hundred-GB torrents in their everyday activities.
- As in many things, it's hard for an improved technology to displace a technology which is good-enough and already in widespread use.

[Torrent format: Hybrid, v1 and V2](https://old.reddit.com/r/qBittorrent/comments/uiwchy/torrent_format_hybrid_v1_and_v2/)
