# Lab11web


# struktur 
<img width="253" height="635" alt="image" src="https://github.com/user-attachments/assets/c04ee909-dc34-4201-9fa7-022df37ffd41" />


### 1. Sistem Routing Front-Controller

Seluruh permintaan akses web dipusatkan pada file `index.php`. Sistem menangkap alamat melalui `PATH_INFO`, lalu memecahnya untuk menentukan modul yang dimuat. Jika URL kosong, sistem otomatis mengarahkan ke modul `artikel`.

**Kodingan di `index.php`:**

```php
$path = isset($_SERVER['PATH_INFO']) ? $_SERVER['PATH_INFO'] : '/artikel/index';
$segments = explode('/', trim($path, '/'));

$mod = isset($segments[0]) && $segments[0] != '' ? $segments[0] : 'artikel';
$page = isset($segments[1]) && $segments[1] != '' ? $segments[1] : 'index';

$file = "module/" . $mod . "/" . $page . ".php";

```

### 2. Implementasi Pemrograman Berorientasi Objek (OOP)

Logika kodingan memisahkan fungsi database ke dalam sebuah Class agar kode lebih rapi dan bisa digunakan berulang kali (*reusable*).

**Kodingan di `class/Database.php`:**

```php
class Database {
    protected $conn;
    public function __construct() {
        global $config;
        $this->conn = new mysqli($config['host'], $config['user'], $config['password'], $config['db_name']);
    }
    public function query($sql) {
        return $this->conn->query($sql);
    }
}

```

### 3. Struktur Konten dan Skema Tabel

Sistem mengambil data dari database di mana kolom `nama` difungsikan sebagai **Badge Penulis**, kolom `judul` sebagai kepala berita, dan kolom `alamat` sebagai isi artikel.

**Kodingan pemanggilan data di `module/artikel/index.php`:**

```php
<span class="badge-penulis"><?= $row['nama']; ?></span>
<h3><?= $row['judul']; ?></h3>
<p><?= substr($row['alamat'], 0, 150); ?>...</p>

```

### 4. Layout Dinamis dengan Flexbox

Antarmuka menggunakan Flexbox untuk membagi area antara Sidebar (navigasi) dan Main Content (isi). Hal ini memastikan navigasi selalu ada di samping konten yang berubah-ubah.

**Kodingan di `index.php`:**

```php
<div class="wrapper" style="display: flex; gap: 20px;">
    <aside style="flex: 1;"> </aside>
    <main style="flex: 3;">
        <?php include $file; // Memuat isi modul secara dinamis ?>
    </main>
</div>

```

### 5. Alur Logika CRUD (Update Data)

Operasi CRUD memungkinkan perubahan data secara langsung ke database. File `ubah.php` menangkap ID dari URL, menampilkan data lama, lalu mengirimkan perintah `UPDATE`.

**Kodingan di `module/artikel/ubah.php`:**

```php
$id = $_GET['id'];
$result = $db->query("SELECT * FROM users WHERE id = '{$id}'");
$data = $result->fetch_assoc();

if (isset($_POST['submit'])) {
    $sql = "UPDATE users SET judul = '{$judul}', nama = '{$nama}', alamat = '{$alamat}' WHERE id = '{$id}'";
    $db->query($sql);
    header("Location: /lab11_php_oop/artikel/index");
}

```

---



# Berikut hasil nya ketika di run

<img width="1432" height="954" alt="image" src="https://github.com/user-attachments/assets/ee91882b-3a18-4f8d-8f8a-f7f16b6dfd2b" />

<img width="1350" height="698" alt="image" src="https://github.com/user-attachments/assets/b91e7524-6bb3-4424-ac65-06a4c3f5ce4a" />

<img width="1396" height="918" alt="image" src="https://github.com/user-attachments/assets/bd194c49-3a3d-48c7-885d-6962efb46e2c" />
