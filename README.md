# Lab11Web
Nama  : Muflih Salda Maulana

Nim  : 312410527

Kelas  : TI.24.A5

---

# **Keterangan Screenshot Praktikum 11**

---

### **1 Struktur Folder Project**

<img width="752" height="386" alt="strukturfolder" src="https://github.com/user-attachments/assets/dd018060-3b16-4e2e-adef-3c7f4f1a2cd9" />


**Keterangan:**
Screenshot ini menampilkan struktur folder project `lab11_php_oop` yang dibuat pada folder `htdocs`. Struktur folder terdiri dari folder `class`, `module`, dan `template` serta file pendukung seperti `index.php`, `config.php`, dan `.htaccess`. Struktur ini menunjukkan penerapan konsep modularisasi pada aplikasi PHP.

---

### **2 Konfigurasi Database**

<img width="996" height="324" alt="image" src="https://github.com/user-attachments/assets/274fee30-853c-4b94-8777-3554535ef8b0" />

**Keterangan:**
Screenshot ini menampilkan database `latihan_oop` beserta tabel `artikel` yang digunakan pada praktikum ini. Tabel `artikel` berfungsi untuk menyimpan data artikel yang terdiri dari kolom `id`, `judul`, dan `isi`.

---

### **3 File .htaccess**

<img width="386" height="224" alt="image" src="https://github.com/user-attachments/assets/263342ba-bb89-4eaf-b3cf-18539a20a50b" />


**Keterangan:**
Screenshot ini menampilkan konfigurasi file `.htaccess` yang digunakan untuk mengaktifkan fitur routing menggunakan `mod_rewrite`. Konfigurasi ini berfungsi untuk mengarahkan seluruh permintaan URL ke file `index.php` sebagai router utama aplikasi.

---

### **4 File index.php (Routing Utama)**

<img width="627" height="458" alt="image" src="https://github.com/user-attachments/assets/91740a5b-bfc5-496d-b8fe-9f89f851924f" />

**Keterangan:**
Screenshot ini menampilkan kode pada file `index.php` yang berperan sebagai gerbang utama aplikasi. File ini bertugas untuk membaca URL, menentukan modul dan halaman yang diakses, serta memanggil file modul yang sesuai.

---

### **5 Class Database.php**
```php
    <?php
class Database
{
    protected $host;
    protected $user;
    protected $password;
    protected $db_name;
    protected $conn;

    public function __construct()
    {
        $this->getConfig();
        $this->conn = new mysqli(
            $this->host,
            $this->user,
            $this->password,
            $this->db_name
        );

        if ($this->conn->connect_error) {
            die("Koneksi gagal: " . $this->conn->connect_error);
        }
    }

    private function getConfig()
    {
        include("config.php");
        $this->host = $config['host'];
        $this->user = $config['username'];
        $this->password = $config['password'];
        $this->db_name = $config['db_name'];
    }

    public function query($sql)
    {
        return $this->conn->query($sql);
    }

    public function get($table, $where = null)
    {
        $sql = "SELECT * FROM $table";
        if ($where) {
            $sql .= " WHERE $where";
        }
        return $this->conn->query($sql)->fetch_assoc();
    }

    public function insert($table, $data)
    {
        foreach ($data as $key => $val) {
            $columns[] = $key;
            $values[] = "'$val'";
        }

        $columns = implode(",", $columns);
        $values  = implode(",", $values);

        $sql = "INSERT INTO $table ($columns) VALUES ($values)";
        return $this->conn->query($sql);
    }

    public function update($table, $data, $where)
    {
        foreach ($data as $key => $val) {
            $update[] = "$key='$val'";
        }

        $update = implode(",", $update);
        $sql = "UPDATE $table SET $update WHERE $where";
        return $this->conn->query($sql);
    }
}
```

**Keterangan:**
kode class `Database` yang digunakan untuk mengelola koneksi database dan menjalankan perintah SQL. Class ini menerapkan konsep Object Oriented Programming (OOP) untuk mempermudah pengelolaan data.

---

### **6 Class Form.php**
```php
<?php
class Form
{
    private $fields = [];
    private $action;
    private $submit;
    private $jumField = 0;

    public function __construct($action, $submit)
    {
        $this->action = $action;
        $this->submit = $submit;
    }

    public function addField($name, $label, $type = "text", $options = [])
    {
        $this->fields[$this->jumField] = [
            'name' => $name,
            'label' => $label,
            'type' => $type,
            'options' => $options
        ];
        $this->jumField++;
    }

    public function displayForm()
    {
        echo "<form action='{$this->action}' method='POST'>";
        echo "<table border='0' width='100%'>";

        foreach ($this->fields as $field) {
            echo "<tr>
                    <td>{$field['label']}</td>
                    <td>";

            switch ($field['type']) {
                case 'textarea':
                    echo "<textarea name='{$field['name']}'></textarea>";
                    break;

                case 'select':
                    echo "<select name='{$field['name']}'>";
                    foreach ($field['options'] as $v => $l) {
                        echo "<option value='$v'>$l</option>";
                    }
                    echo "</select>";
                    break;

                case 'radio':
                    foreach ($field['options'] as $v => $l) {
                        echo "<input type='radio' name='{$field['name']}' value='$v'> $l ";
                    }
                    break;

                case 'checkbox':
                    foreach ($field['options'] as $v => $l) {
                        echo "<input type='checkbox' name='{$field['name']}[]' value='$v'> $l ";
                    }
                    break;

                case 'password':
                    echo "<input type='password' name='{$field['name']}'>";
                    break;

                default:
                    echo "<input type='text' name='{$field['name']}'>";
            }

            echo "</td></tr>";
        }

        echo "<tr><td colspan='2'>
                <input type='submit' value='{$this->submit}'>
              </td></tr>";
        echo "</table></form>";
    }
}
```

**Keterangan:**
kode class `Form` yang digunakan untuk membuat form input secara dinamis. Class ini memungkinkan pembuatan berbagai jenis input seperti text, textarea, radio, checkbox, dan select.

---

### **7 Tampilan Halaman Data Artikel**

<img width="1357" height="423" alt="image" src="https://github.com/user-attachments/assets/d60736a0-ad0c-4e58-83f8-2dac84cbaf6b" />

**Keterangan:**
Screenshot ini menampilkan halaman data artikel yang diakses melalui URL `http://localhost/lab11_php_oop/artikel`. Halaman ini menampilkan seluruh data artikel yang tersimpan di dalam database dalam bentuk tabel.

---

### **8 Halaman Tambah Artikel**

<img width="813" height="377" alt="image" src="https://github.com/user-attachments/assets/8c7afc7d-5172-47f7-9a0a-d1120b7e12e9" />

**Keterangan:**
Screenshot ini menampilkan halaman form tambah artikel yang diakses melalui URL `http://localhost/lab11_php_oop/artikel/tambah`. Form ini dibuat menggunakan class `Form` dan digunakan untuk menginput data artikel baru.

---

### **9 Proses Penyimpanan Data Artikel**


<img width="738" height="192" alt="image" src="https://github.com/user-attachments/assets/8d940c01-acd0-4272-ad41-a58ba85f1268" />

**Keterangan:**
Screenshot ini menampilkan proses penyimpanan data artikel setelah tombol simpan ditekan. Data yang diinput berhasil disimpan ke dalam database dan ditampilkan pesan bahwa data berhasil disimpan.

---

### **10 Data Artikel Berhasil Masuk ke Database**

<img width="1364" height="267" alt="image" src="https://github.com/user-attachments/assets/b7159294-ad6f-41e3-9ee0-6fcb6f3faaeb" />

**Keterangan:**
Screenshot ini menampilkan isi tabel `artikel` pada database `latihan_oop` melalui phpMyAdmin. Hal ini menunjukkan bahwa proses insert data artikel berhasil dijalankan.

---
### **11 Tampilan Error Halaman Tidak Ditemukan**

<img width="571" height="268" alt="image" src="https://github.com/user-attachments/assets/b1730489-c3d6-40f5-b19d-1672e85b1504" />

**keterangan:**
Screenshot ini menampilkan pesan kesalahan ketika mengakses modul atau halaman yang tidak tersedia. Hal ini menunjukkan bahwa sistem routing telah berjalan dengan baik dalam menangani request yang tidak valid.

