# SMB

#### **Ringkasan SMB dan Samba**

1. **SMB (Server Message Block)**
   * Protokol client-server yang digunakan untuk berbagi file, direktori, dan sumber daya jaringan seperti printer dan router.
   * Utama pada sistem operasi Windows, mendukung kompatibilitas mundur (downward-compatible).
   * Menggunakan TCP untuk komunikasi, termasuk **three-way handshake** sebelum koneksi terjalin.
   * Akses dikendalikan menggunakan **Access Control Lists (ACL)** yang mengatur hak akses seperti baca, eksekusi, dan akses penuh.
   * **Port TCP**:
     * NetBIOS (SMB lama): 137, 138, 139.
     * CIFS/SMB 1.0 dan seterusnya: 445.
2.  **Versi SMB**

    | **Versi**     | **Didukung**               | **Fitur Utama**                         |
    | ------------- | -------------------------- | --------------------------------------- |
    | **CIFS**      | Windows NT 4.0             | Komunikasi via NetBIOS.                 |
    | **SMB 1.0**   | Windows 2000               | Koneksi langsung via TCP.               |
    | **SMB 2.0**   | Windows Vista, Server 2008 | Peningkatan performa, caching, signing. |
    | **SMB 2.1**   | Windows 7, Server 2008 R2  | Mekanisme locking.                      |
    | **SMB 3.0**   | Windows 8, Server 2012     | Multichannel, enkripsi end-to-end.      |
    | **SMB 3.1.1** | Windows 10, Server 2016    | Integrity check, enkripsi AES-128.      |
3. **Samba**
   * Implementasi alternatif SMB untuk sistem berbasis Unix/Linux.
   * Menggunakan protokol **CIFS** (dialek SMB), memungkinkan komunikasi dengan sistem Windows.
   * **Samba Versi 3**: Mendukung integrasi penuh dengan domain Active Directory.
   * **Samba Versi 4**: Bisa berfungsi sebagai Active Directory domain controller.
   * Daemon penting:
     * **smbd**: Mengatur server SMB.
     * **nmbd**: Mengelola blok pesan NetBIOS.
4. **NetBIOS**
   * API untuk jaringan komputer yang menyediakan nama unik untuk perangkat di jaringan.
   * Mendukung nama pendaftaran melalui **NetBIOS Name Server (NBNS)** atau **WINS (Windows Internet Name Service)**.
5. **Workgroup dalam Jaringan SMB**
   * Workgroup adalah grup komputer dengan sumber daya bersama di jaringan SMB.
   * Beberapa workgroup dapat ada sekaligus dalam satu jaringan.

**Kesimpulan**\
SMB adalah protokol berbagi sumber daya jaringan yang digunakan luas di Windows, dengan Samba sebagai implementasi untuk Linux/Unix. CIFS adalah versi lama SMB, sedangkan versi baru SMB menawarkan fitur modern seperti enkripsi dan koneksi multichannel.
