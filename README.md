# LANGKAH - LANGKAH PRAKTIKUM 

1. Membuat folder dengan nama lab9_php_modular, dan kita membuat file dengan nama header.php di dalam folder lab9_php_modular
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Contoh Modularisasi</title>
    <link href="style.css" rel="stylesheet" type="text/stylesheet"
media="screen" />
</head>
<body>
    <div class="container">
        <header>
            <h1>Modularisasi Menggunakan Require</h1>
        </header>
        <nav>
            <a href="home.php">Home</a>
            <a href="about.php">Tentang</a>
            <a href="kontak.php">Kontak</a>
        </nav>
```
2. Membuat file dengan nama footer.php
```
        <footer>
            <p>&copy; 2021, Informatika, Universitas Pelita Bangsa</p>
        </footer>
    </div>
</body>
</html>
```
3. Membuat file dengan nama home.php
```
<?php require('header.php'); ?>

<div class="content">
    <h2>Ini Halaman Home</h2>
    <p>Ini adalah bagian content dari halaman.</p>
</div>

<?php require('footer.php'); ?>
```
4. Membuat file dengan nama about.php
```
<?php require('header.php'); ?>

<div class="content">
    <h2>Ini Halaman About</h2>
    <p>Ini adalah bagian content dari halaman.</p>
</div>

<?php require('footer.php'); ?>
```
<img width="960" alt="Screenshot (231)" src="https://github.com/Birrhamm/lab9_web/assets/115520530/4723250f-997b-42af-8bdd-61a6ebc62df3">


Pertanyaan dan Tugas
Implementasikan konsep modularisasi pada kode program praktikum 8 tentang database, sehingga setiap halamannya memiliki template tampilan yang sama.

1. Membuat folder baru dengan nama tugas_modular di dalam folder lab9_php_modular, lalu ambil file koneksi.php dan hapus.php dari praktikum 8 dan di simpan di lab9_php_modular/tugas_modular
```
<?php
$host = "localhost";
$user = "root";
$pass = "";
$db = "latihan1";
$conn = mysqli_connect($host, $user, $pass, $db);
if ($conn == false)
{
    echo "Koneksi ke server gagal.";
    die();
} #else echo "Koneksi berhasil";
?>
```
```
<?php
include_once 'koneksi.php';
$id = $_GET['id'];
$sql = "DELETE FROM data_barang WHERE id_barang = '{$id}'";
$result = mysqli_query($conn, $sql);
header('location: index.php');
?>
```
2. Membuat file dengan nama header_index.php
```
<?php
include("koneksi.php");
$no = 1;
// query untuk menampilkan data
$sql = 'SELECT * FROM data_barang';
$result = mysqli_query($conn, $sql);

?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="css/bootstrap.min.css" />
    <script src="js/bootstrap.min.js"></script>
    <title>Data Barang</title>
</head>
<body>
    <div class="container">
        <h1 align="center">Data Barang</h1>
		<table class="table table-striped table-sm">
			<td><td>
		</table>
        <div class="main">
		<div class="btn-toolbar mb-2 mb-md-10">
           <a class="btn btn-primary" href="tambah.php" role="button">Tambah Barang</a>
		</div> 

```
3. Membuat footer_index.php
```
</div>
</body>
</html>
```
4. Membuat file index.php
```
<?php
	require('header_index.php');
?>
<div class="table-responsive">
				<table class="table table-striped table-hover">
				<thead>
					<tr>
						<th>No</th>
						<th>Gambar</th>
						<th>Nama Barang</th>
						<th>Katagori</th>
						<th>Harga Jual</th>
						<th>Harga Beli</th>
						<th>Stok</th>
						<th>Aksi</th>
					</tr>
					<?php if($result): ?>
					<?php while($row = mysqli_fetch_array($result)): ?>
					<tr>
						<td><?php echo $no++ ?></td>
						<td ><img src="gambar/<?= $row['gambar'];?>" alt="<?=$row['nama'];?>"></td>
						<td><?= $row['nama'];?></td>
						<td><?= $row['kategori'];?></td>
						<td><?= $row['harga_jual'];?></td>
						<td><?= $row['harga_beli'];?></td>
						<td><?= $row['stok'];?></td>
						<td>
						<a class="btn btn-success" href="ubah.php?id=<?= $row['id_barang'];?>" role="button">Ubah</a>
						<a class="btn btn-danger" href="hapus.php?id=<?= $row['id_barang'];?>" role="button">Hapus</a>
					</td>
					</tr>
					<?php endwhile; else: ?>
					<tr>
						<td colspan="7">Belum ada data</td>
					</tr>
					<?php endif; ?>
				</thead>
            </table>
        </div>
    </div>
<?php
	require('footer_index.php');
?>
```
<img width="470" alt="pp" src="https://github.com/Birrhamm/lab9_web/assets/115520530/26a88606-113e-4fa7-a3a5-01e5aac2e138">

5. Membuat header_tambah.php
```
<?php
error_reporting(E_ALL);
include_once 'koneksi.php';

if (isset($_POST['submit']))
{
	$nama = $_POST['nama'];
	$kategori = $_POST['kategori'];
	$harga_jual = $_POST['harga_jual'];
	$harga_beli = $_POST['harga_beli'];
	$stok = $_POST['stok'];
	$file_gambar = $_FILES['file_gambar'];
	$gambar = null;
	if ($file_gambar['error'] == 0)
	{
		$filename = str_replace(' ', '_',$file_gambar['name']);
		$destination = dirname(__FILE__) .'/gambar/' . $filename;
		if(move_uploaded_file($file_gambar['tmp_name'], $destination))
		{
		$gambar = 'gambar/' . $filename;;
		}
	}
	$sql = 'INSERT INTO data_barang (nama, kategori,harga_jual, harga_beli, stok, gambar) ';
	$sql .= "VALUE ('{$nama}', '{$kategori}','{$harga_jual}', '{$harga_beli}', '{$stok}', '{$gambar}')";
	$result = mysqli_query($conn, $sql);
	header('location: index.php');
}
?>

<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<link rel="stylesheet" href="css/bootstrap.min.css" />
    <script src="js/bootstrap.min.js"></script>
	<title>Tambah Barang</title>
	<style>
	form div > label {
			display: inline-block;
			width: 100px;
			height: 30px;
	}
	form div > label {
			display: inline-block;
			width: 100px;
			height: 50px;
	}
	form input[type="text"], form textarea {
			border: 1px solid;
	}
	form input[type="submit"] {
		border: 1px solid #2E8B57;
		background-color: #2E8B57;
		color: #ffffff;
		font-weight: bold;
		padding: 5px 15px;
	}
	</style>
</head>
	<div class="container">
	<h1 align="center">Tambah Barang</h1>
```
6. Membuat file footer_tambah.php
```
</div>
</fieldset>
</body>
</html>
```
7. Membuat file tambah.php
```
<?php
	require('header_tambah.php');
?>
<table class="table table-striped table-sm">
			<td><td>
		</table>
	<div class="main">
		<form method="post" action="tambah.php" enctype="multipart/form-data">
			<div class="input">
				<label>Nama Barang</label>
				<input type="text" name="nama" />
			</div>
			<div class="input">
				<label>Kategori</label>
					<select name="kategori">
					<option value="Komputer">Komputer</option>
					<option value="Elektronik">Elektronik</option>
					<option value="Hand Phone">Hand Phone</option>
				</select>
			</div>
			<div class="input">
				<label>Harga Jual</label>
				<input type="text" name="harga_jual" />
			</div>
			<div class="input">
				<label>Harga Beli</label>
				<input type="text" name="harga_beli" />
			</div>
			<div class="input">
				<label>Stok</label>
				<input type="text" name="stok" />
			</div>
			<div class="input">
				<label>File Gambar</label>
				<input type="file" name="file_gambar" />
			</div>
			<div class="submit">
				<input type="submit" name="submit" value="Simpan" />
			</div>
		</form>
	</div>
	
<?php
	require('footer_tambah.php');
?>
```
<img width="960" alt="Screenshot (229)" src="https://github.com/Birrhamm/lab9_web/assets/115520530/e01b1827-d46d-4f4f-8749-1d9ba552f44f">

8. Membuat file header_ubah.php
```
<?php
error_reporting(E_ALL);
include_once 'koneksi.php';

if (isset($_POST['submit']))
{
    $id = $_POST['id'];
    $nama = $_POST['nama'];
    $kategori = $_POST['kategori'];
    $harga_jual = $_POST['harga_jual'];
    $harga_beli = $_POST['harga_beli'];
    $stok = $_POST['stok'];
    $file_gambar = $_FILES['file_gambar'];
    $gambar = null;

    if ($file_gambar['error'] == 0)
    {
        $filename = str_replace(' ', '_', $file_gambar['name']);
        $destination = dirname(_FILE_) . '/gambar/' . $filename;
        if (move_uploaded_file($file_gambar['tmp_name'], $destination))
        {
            $gambar = 'gambar/' . $filename;;
        }
    }

    $sql = 'UPDATE data_barang SET ';
    $sql .= "nama = '{$nama}', kategori = '{$kategori}', ";
    $sql .= "harga_jual = '{$harga_jual}', harga_beli = '{$harga_beli}', stok = '{$stok}' ";
    if (!empty($gambar))
        $sql .= ", gambar = '{$gambar}' ";
    $sql .= "WHERE id_barang = '{$id}'";
    $result = mysqli_query($conn, $sql);

    header('location: index.php');
    }

    $id = $_GET['id'];
    $sql = "SELECT * FROM data_barang WHERE id_barang = '{$id}'";
    $result = mysqli_query($conn, $sql);
    if (!$result) die('Error: Data tidak tersedia');
    $data = mysqli_fetch_array($result);

    function is_select($var, $val) {
        if ($var == $val) return 'selected="selected"';
        return false;
}
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
	<link rel="stylesheet" href="css/bootstrap.min.css" />
    <script src="js/bootstrap.min.js"></script>
    <title>Ubah Barang</title>
	<style>
	form div > label {
			display: inline-block;
			width: 100px;
			height: 30px;
	}
	form div > label {
			display: inline-block;
			width: 100px;
			height: 50px;
	}
	form input[type="text"], form textarea {
			border: 1px solid;
	}
	form input[type="submit"] {
		border: 1px solid #2E8B57;
		background-color: #2E8B57;
		color: #ffffff;
		font-weight: bold;
		padding: 5px 15px;
	}
	</style>
</head>
<body>
<div class="container">
    <h1 align="center">Ubah Barang</h1>
```
9. Membuat file footer_ubah.php
```
</div>
</body>
</html>
```
10. Membuat file ubah.php
```
<?php
	require('header_ubah.php');
?>		
		<table class="table table-striped table-sm">
			<td><td>
		</table>
    <div class="main">
        <form method="post" action="ubah.php" enctype="multipart/form-data">
            <div class="input">
                <label>Nama Barang</label>
                <input type="text" name="nama" value="<?php echo $data['nama'];?>"/>
            </div>
            <div class="input">
                <label>Kategori</label>
                <select name="kategori">
                    <option <?php echo is_select ('Komputer', $data['kategori']);?> value="Komputer">Komputer</option>
                    <option <?php echo is_select ('Komputer', $data['kategori']);?> value="Elektronik">Elektronik</option>
                    <option <?php echo is_select ('Komputer', $data['kategori']);?> value="Hand Phone">Hand Phone</option>
                </select>
            </div>
            <div class="input">
                <label>Harga Jual</label>
                <input type="text" name="harga_jual" value="<?php echo $data['harga_jual'];?>"/>
            </div>
            <div class="input">
                <label>Harga Beli</label>
                <input type="text" name="harga_beli" value="<?php echo $data['harga_beli'];?>"/>
            </div>
            <div class="input">
                <label>Stok</label>
                <input type="text" name="stok" value="<?php echo $data['stok'];?>"/>
            </div>
            <div class="input">
                <label>File Gambar</label>
                <input type="file" name="file_gambar" />
            </div>
            <div class="submit">
                <input type="hidden" name="id" value="<?php echo $data['id_barang'];?>" />
                    <input type="submit" name="submit" value="Simpan" />
            </div>
        </form>
    </div>
<?php
	require('footer_ubah.php');
?>
```
<img width="960" alt="Screenshot (230)" src="https://github.com/Birrhamm/lab9_web/assets/115520530/41767bcf-a983-4d17-a782-704d556c4a62">






