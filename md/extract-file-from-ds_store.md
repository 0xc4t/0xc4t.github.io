## Extract file from .DS_Store

A .DS_Store (Desktop Services Store) file is a hidden file created by the macOS operating system. It is used by Finder to store custom attributes of a folder, such as the positions of icons, the choice of background image, and other view options. Each directory in macOS can have its own .DS_Store file.

### Why does this file exist?
Why the .DS_Store file can appear on the website? because developers who use macOS forget to delete the .DS_Store file in their folder, and indirectly the file is also deployed on the website.

### Vulnerability discovery
By using FFUF (Fuzz Faster U Fool), I did a directory enumeration to find out what files/folders are on the website with a simple command like the following

```bash
ffuf -c -w ~/some/wordlists.txt -u https://example.com/FUZZ -v
```
![](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*zqk7OPLhPW7qsgdBz9asLg.png)



### How? Can I Extract This File?
When doing penetration testing on websites with the blackbox method, I often find this .DS_Store file when doing Fuzzing Directory / Content Discovery, I skipped this file because I didn’t know what the benefits were, because I was a little idle, I tried to research about this DS.Store, it turns out that this file can provide us with information on some of the content contained on the website.

```bash
git clone https://github.com/lijiejie/ds_store_exp
cd ds_store_exp
python3 ds_store_exp.py https://example.com/.DS_Store
```

![](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*KwTOKN9bwTKR6ILveH0wYQ.png)


### How does this tool work?

So when the .DS_Store file is decoded, several lists of file names automatically appear, and then the tools automatically request the file from the .DS_Store decode result, when the file exists, the response will be saved into a file and vice versa, when the file does not exist, it will automatically be skipped.

![](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*HMJF9h-eygtMRHE9VUSTDg.png)

### Refrence
* [https://github.com/lijiejie/ds_store_exp](https://github.com/lijiejie/ds_store_exp)

* [https://www.invicti.com/web-vulnerability-scanner/vulnerabilities/dsstore-file-found/](https://www.invicti.com/web-vulnerability-scanner/vulnerabilities/dsstore-file-found/)

* [https://xelkomy.medium.com/how-i-was-able-to-get-1000-bounty-from-a-ds-store-file-dc2b7175e92c](https://xelkomy.medium.com/how-i-was-able-to-get-1000-bounty-from-a-ds-store-file-dc2b7175e92c)
