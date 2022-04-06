# tar+gz
Taring and Zipping

Untar a file
```
tar -xvf {filename}
```

UnZip a file
```
gunzip (filename}
```

create a tar file
```
tar -cvf {filename.tar} {files_to_compress_into_filename*}
```

create a tar file with the same permissions as the original
```
tar -cvfp {filename.tar} {files_to_compress_into_filename*}
```

zip up file
```
gzip -v {filename}
```

create a ziped up tar file.
```
tar -cvzf {filename.tar.gz} {files_to_compress_into_filename*}
```
