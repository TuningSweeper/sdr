# Logging RTLSDR-Airband

This patch enables three logging features on [RTLSDR-Airband](https://github.com/charlie-foxtrot/RTLSDR-Airband). Its ugly and not yet worth a PR, but maybe someday. Right now it works on my machine. sample.conf shows how to enable these features.


## Level monitor

Level monitor reports information using UDP to target host and port at 8 Hz. This could be useful when building real-time user interfaces. Currently the data contains measurement time, current frequency, signal strength, squelch level, noise floor and squelch open bit. For real-time data I use the local timezone.

Sample data stream looks like this:
```
20240516-112943.019, 145.100, -39.7, -33.3, -42.9, 0
20240516-112943.144, 145.100, -41.0, -33.3, -42.9, 0
20240516-112943.269, 145.100, -39.3, -33.3, -42.9, 0
20240516-112943.395, 145.100, -39.5, -33.3, -42.9, 0
20240516-112943.520, 145.100, -19.1, -33.3, -42.8, 0
20240516-112943.644, 145.100,  -9.5, -33.3, -42.8, 1
20240516-112943.769, 145.100,  -9.5, -33.3, -42.8, 1
20240516-112943.895, 145.100,  -9.5, -33.3, -42.8, 1
20240516-112944.021, 145.100,  -9.5, -33.3, -42.8, 1
20240516-112944.146, 145.100,  -9.5, -33.3, -42.8, 1
20240516-112944.271, 145.100,  -9.5, -33.3, -42.8, 1
```


## Signal level on split files

When recording transmissions on automatically split files, this option adds the average signal strength on the filename.

```
$ ls -l recordings
total 76
-rw-r--r-- 1 user user 13859 May 16 11:20 signal-1451-_20240516_082053_avg-33.1_.mp3
-rw-r--r-- 1 user user 17533 May 16 11:26 signal-1451-_20240516_082647_avg-20.7_.mp3
-rw-r--r-- 1 user user 14510 May 16 11:27 signal-1451-_20240516_082702_avg-19.1_.mp3
-rw-r--r-- 1 user user  8766 May 16 11:27 signal-1451-_20240516_082719_avg-18.1_.mp3
-rw-r--r-- 1 user user  9641 May 16 11:29 signal-1451-_20240516_082943_avg-9.2_.mp3
```


## Transmission log

Transmission log stores basic details on each signal that opens the squelch. If the defined filename has the keywork DATE, it will be replaced with the current date.

```
$ cat txlog-2024-05-16.txt
2024-05-16 08:20:53 freq 145.100, duration 6.2 s, avg level -33.1 dBFS, squelch level -33.1 dBFS, noise floor -42.7 dBFS
2024-05-16 08:26:47 freq 145.100, duration 9.4 s, avg level -20.7 dBFS, squelch level -33.6 dBFS, noise floor -43.2 dBFS
2024-05-16 08:27:02 freq 145.100, duration 8.1 s, avg level -19.1 dBFS, squelch level -33.1 dBFS, noise floor -42.7 dBFS
2024-05-16 08:27:19 freq 145.100, duration 5.0 s, avg level -18.1 dBFS, squelch level -33.8 dBFS, noise floor -43.3 dBFS
2024-05-16 08:29:43 freq 145.100, duration 5.9 s, avg level -9.2 dBFS, squelch level -33.3 dBFS, noise floor -42.8 dBFS
```


## Applying the patch and building the RTLSDR-Airband

Check dependencies at [RTLSDR-Airband wiki](https://github.com/charlie-foxtrot/RTLSDR-Airband/wiki/Installation#local-build)

```
wget patch
wget https://github.com/charlie-foxtrot/RTLSDR-Airband/archive/refs/tags/v5.0.9.tar.gz
tar xvfz v5.0.9.tar.gz
cd RTLSDR-Airband-5.0.9/
patch -p1 < ../airband.patch
mkdir build
cmake ../
cd build
make
```