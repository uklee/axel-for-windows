# AXEL for Windows

## About

[**Axel**](https://github.com/axel-download-accelerator/axel) is a lightweight CLI download accelerator. While its more powerful alternative, [**aria2**](https://github.com/aria2/aria2), offers a wider range of features, Axel is significantly easier to use when the goal is *just download acceleration*. However, Axel is originally written using POSIX APIs, which makes it incompatible with Windows. This repository provides a version of Axel that works on Windows.

Porting POSIX-based code to Windows typically involves removing all POSIX dependencies (e.g., `arpa/inet.h`) and rewriting them using Windows-native APIs. However, since Axel relies heavily on POSIX for its functionality, this would essentially require a complete rewrite. Instead, we use **MSYS2** to allow minimal code modifications while enabling Axel to run on Windows.

---

## Building Axel on MSYS2

1. **Install MSYS2**  
   If you haven’t already, install MSYS2 from [https://www.msys2.org](https://www.msys2.org).

2. **Open MSYS shell**  
   You must run the **MSYS** environment, not UCRT or MINGW shells, because only the MSYS environment supports the necessary POSIX APIs.

3. **Install required packages**  
   Run the following command in the MSYS shell:
   ```bash
   pacman -S gcc autotools gettext pkg-config openssl openssl-devel
   ```

4. **Clone this repository**  
   ```bash
   git clone https://github.com/uklee/axel-for-windows.git
   cd axel-for-windows
   ```

5. **Build Axel**  
   ```bash
   autoreconf -i
   ./configure
   make
   ```

6. **The `axel.exe` binary should now be available for use.**  

---

## Using Axel on Native Windows (Outside MSYS)

Although you can use the built axel.exe inside the MSYS environment, with a few additional steps you can run it directly from native Windows.

1. **Create a folder and copy the following files into it:**  
   ```
   axel.exe
   msys64/usr/bin/msys-2.0.dll
   msys64/usr/bin/msys-crypto-3.dll
   msys64/usr/bin/msys-ssl-3.dll
   ```

2. **Obtain an SSL certificate file**  
   To enable OpenSSL's certificate verification, copy cert.pem from:

   `msys64/usr/ssl/cert.pem`, or

   download from [https://curl.se/docs/caextract.html](https://curl.se/docs/caextract.html).

3. **Set the environment variable**  
   Add the following to your Windows environment variables:
   ```
   SSL_CERT_FILE=<path-to-cert.pem>
   ```

4. **Run Axel**  
   You can now run `axel.exe` from that folder or add the folder to your `%PATH%` to use it system-wide.
