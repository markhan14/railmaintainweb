<?php
// ===========================================
// Sistem Perawatan Aset KAI
// ============================================

define('DB_HOST', 'localhost');
define('DB_USER', 'root');
define('DB_PASS', '');
define('DB_NAME', 'railmaintainweb');

// Koneksi database
$conn = mysqli_connect(DB_HOST, DB_USER, DB_PASS, DB_NAME);
if (!$conn) {
    die("Koneksi database gagal: " . mysqli_connect_error());
}
mysqli_set_charset($conn, "utf8");

// Session
if (session_status() == PHP_SESSION_NONE) {
    session_start();
}

// ============================================
// FUNGSI-FUNGSI
// ============================================
function isLoggedIn() {
    return isset($_SESSION['user_id']);
}

function hasRole($role) {
    return isset($_SESSION['role']) && $_SESSION['role'] == $role;
}

function redirect($url) {
    header("Location: $url");
    exit();
}

function sanitize($data) {
    global $conn;
    return mysqli_real_escape_string($conn, htmlspecialchars(strip_tags(trim($data))));
}

function getAllUnits() {
    global $conn;
    $query = "SELECT u.*, d.Nama_Depo FROM UNIT u LEFT JOIN DEPO d ON u.Depo_Id = d.Id ORDER BY u.Unit_number";
    $result = mysqli_query($conn, $query);
    return mysqli_fetch_all($result, MYSQLI_ASSOC);
}

function getUnitById($id) {
    global $conn;
    $query = "SELECT u.*, d.Nama_Depo, d.Kode_Depo FROM UNIT u LEFT JOIN DEPO d ON u.Depo_Id = d.Id WHERE u.Id = $id";
    $result = mysqli_query($conn, $query);
    return mysqli_fetch_assoc($result);
}

function getMaintenanceRecords($unit_id = null) {
    global $conn;
    $where = $unit_id ? "WHERE mr.Unit_Id = $unit_id" : "";
    $query = "SELECT mr.*, u.Unit_number, teknisi.Name as Teknisi_Name 
              FROM MAINTENANCE_RECORDS mr
              JOIN UNIT u ON mr.Unit_Id = u.Id
              LEFT JOIN USERS teknisi ON mr.Teknisii_Id = teknisi.Id
              $where
              ORDER BY mr.Maintenance_date DESC";
    $result = mysqli_query($conn, $query);
    return mysqli_fetch_all($result, MYSQLI_ASSOC);
}

function getMaintenanceById($id) {
    global $conn;
    $query = "SELECT mr.*, u.Unit_number FROM MAINTENANCE_RECORDS mr JOIN UNIT u ON mr.Unit_Id = u.Id WHERE mr.Id = $id";
    $result = mysqli_query($conn, $query);
    return mysqli_fetch_assoc($result);
}

function getUnitStats() {
    global $conn;
    $query = "SELECT 
                COUNT(*) as total,
                SUM(CASE WHEN Status = 'active' THEN 1 ELSE 0 END) as layak,
                SUM(CASE WHEN Status = 'maintenance' THEN 1 ELSE 0 END) as perawatan,
                SUM(CASE WHEN Status = 'damaged' THEN 1 ELSE 0 END) as rusak
              FROM UNIT";
    $result = mysqli_query($conn, $query);
    return mysqli_fetch_assoc($result);
}

function getUserData($user_id) {
    global $conn;
    $query = "SELECT * FROM USERS WHERE Id = $user_id";
    $result = mysqli_query($conn, $query);
    return mysqli_fetch_assoc($result);
}

function getAllUsers() {
    global $conn;
    $query = "SELECT * FROM USERS ORDER BY Name";
    $result = mysqli_query($conn, $query);
    return mysqli_fetch_all($result, MYSQLI_ASSOC);
}

function getAllTeknisi() {
    global $conn;
    $query = "SELECT Id, Name FROM USERS WHERE Role = 'teknisi' AND Is_active = 1";
    $result = mysqli_query($conn, $query);
    return mysqli_fetch_all($result, MYSQLI_ASSOC);
}

function getAllDepo() {
    global $conn;
    $query = "SELECT * FROM DEPO ORDER BY Nama_Depo";
    $result = mysqli_query($conn, $query);
    return mysqli_fetch_all($result, MYSQLI_ASSOC);
}

function countMaintenanceByUnit($unit_id) {
    global $conn;
    $query = "SELECT COUNT(*) as total FROM MAINTENANCE_RECORDS WHERE Unit_Id = $unit_id";
    $result = mysqli_query($conn, $query);
    $row = mysqli_fetch_assoc($result);
    return $row['total'];
}

// ============================================
// PROSES FORM & DELETE
// ============================================

// Login
if (isset($_GET['page']) && $_GET['page'] == 'login' && $_SERVER['REQUEST_METHOD'] == 'POST') {
    $email = sanitize($_POST['email']);
    $password = $_POST['password'];
    
    $query = "SELECT * FROM USERS WHERE Email = '$email'";
    $result = mysqli_query($conn, $query);
    
    if ($row = mysqli_fetch_assoc($result)) {
        if ($row['Password'] == hash('sha256', $password)) {
            $_SESSION['user_id'] = $row['Id'];
            $_SESSION['name'] = $row['Name'];
            $_SESSION['email'] = $row['Email'];
            $_SESSION['role'] = $row['Role'];
            
            mysqli_query($conn, "UPDATE USERS SET Last_login = NOW() WHERE Id = " . $row['Id']);
            redirect('?page=dashboard');
        } else {
            $error = 'Password salah!';
        }
    } else {
        $error = 'Email tidak ditemukan!';
    }
}

// Register
if (isset($_GET['page']) && $_GET['page'] == 'register' && $_SERVER['REQUEST_METHOD'] == 'POST') {
    $name = sanitize($_POST['name']);
    $email = sanitize($_POST['email']);
    $password = $_POST['password'];
    $confirm = $_POST['confirm_password'];
    $role = sanitize($_POST['role']);
    $departemen = sanitize($_POST['departemen'] ?? '');
    $no_telepon = sanitize($_POST['no_telepon'] ?? '');
    
    if ($password !== $confirm) {
        $error = 'Password dan konfirmasi tidak cocok!';
    } elseif (strlen($password) < 6) {
        $error = 'Password minimal 6 karakter!';
    } else {
        $check = "SELECT Id FROM USERS WHERE Email = '$email'";
        if (mysqli_num_rows(mysqli_query($conn, $check)) > 0) {
            $error = 'Email sudah terdaftar!';
        } else {
            $hashed = hash('sha256', $password);
            $query = "INSERT INTO USERS (Name, Email, Password, Role, Departemen, No_Telepon, Is_active) 
                      VALUES ('$name', '$email', '$hashed', '$role', '$departemen', '$no_telepon', 1)";
            if (mysqli_query($conn, $query)) {
                $success = 'Pendaftaran berhasil! Silakan login.';
            } else {
                $error = 'Gagal mendaftar: ' . mysqli_error($conn);
            }
        }
    }
}

// Add Unit
if (isset($_GET['page']) && $_GET['page'] == 'add_unit' && $_SERVER['REQUEST_METHOD'] == 'POST') {
    if (!hasRole('admin') && !hasRole('manager')) {
        redirect('?page=dashboard');
    }
    $unit_number = sanitize($_POST['unit_number']);
    $type = sanitize($_POST['type']);
    $depo_id = intval($_POST['depo_id']);
    $tahun = sanitize($_POST['tahun_pembuatan']);
    $pabrikan = sanitize($_POST['pabrikan']);
    $status = sanitize($_POST['status']);
    
    $query = "INSERT INTO UNIT (Unit_number, Type, Depo_Id, Tahun_Pembuatan, Pabrikan, Status) 
              VALUES ('$unit_number', '$type', $depo_id, '$tahun', '$pabrikan', '$status')";
    if (mysqli_query($conn, $query)) {
        $success = 'Unit berhasil ditambahkan!';
    } else {
        $error = 'Gagal: ' . mysqli_error($conn);
    }
}

// Edit Unit
if (isset($_GET['page']) && $_GET['page'] == 'edit_unit' && $_SERVER['REQUEST_METHOD'] == 'POST') {
    if (!hasRole('admin') && !hasRole('manager')) {
        redirect('?page=dashboard');
    }
    $id = intval($_GET['id']);
    $unit_number = sanitize($_POST['unit_number']);
    $type = sanitize($_POST['type']);
    $depo_id = intval($_POST['depo_id']);
    $tahun = sanitize($_POST['tahun_pembuatan']);
    $pabrikan = sanitize($_POST['pabrikan']);
    $status = sanitize($_POST['status']);
    
    $query = "UPDATE UNIT SET 
              Unit_number = '$unit_number',
              Type = '$type',
              Depo_Id = $depo_id,
              Tahun_Pembuatan = '$tahun',
              Pabrikan = '$pabrikan',
              Status = '$status'
              WHERE Id = $id";
    if (mysqli_query($conn, $query)) {
        $success = 'Unit berhasil diperbarui!';
    } else {
        $error = 'Gagal: ' . mysqli_error($conn);
    }
}

// Add Maintenance
if (isset($_GET['page']) && $_GET['page'] == 'add_maintenance' && $_SERVER['REQUEST_METHOD'] == 'POST') {
    $unit_id = intval($_POST['unit_id']);
    $teknisi_id = intval($_POST['teknisi_id']);
    $date = sanitize($_POST['maintenance_date']);
    $type = sanitize($_POST['maintenance_type']);
    $level = sanitize($_POST['maintenance_level']);
    $damage = sanitize($_POST['damage_founds']);
    $parts = sanitize($_POST['parts_replaced']);
    $notes = sanitize($_POST['notes']);
    $biaya = floatval(str_replace(',', '', $_POST['biaya']));
    $status_result = sanitize($_POST['status_result']);
    
    $query = "INSERT INTO MAINTENANCE_RECORDS 
              (Unit_Id, Teknisii_Id, Maintenance_date, Maintenance_type, Maintenance_Level, 
               Damage_founds, Parts_replaced, Notes, Biaya, Status_result) 
              VALUES 
              ($unit_id, $teknisi_id, '$date', '$type', '$level',
               '$damage', '$parts', '$notes', $biaya, '$status_result')";
    if (mysqli_query($conn, $query)) {
        $success = 'Data maintenance berhasil disimpan!';
    } else {
        $error = 'Gagal: ' . mysqli_error($conn);
    }
}

// ============================================
// PROSES DELETE DATA
// ============================================

// 1. DELETE UNIT
if (isset($_GET['page']) && $_GET['page'] == 'delete_unit') {
    if (!hasRole('admin') && !hasRole('manager')) {
        redirect('?page=dashboard');
    }
    $id = intval($_GET['id']);
    $unit = getUnitById($id);
    if ($unit) {
        // Hapus semua maintenance records terkait
        mysqli_query($conn, "DELETE FROM MAINTENANCE_RECORDS WHERE Unit_Id = $id");
        // Hapus unit
        mysqli_query($conn, "DELETE FROM UNIT WHERE Id = $id");
        $_SESSION['message'] = "Unit " . $unit['Unit_number'] . " berhasil dihapus!";
        $_SESSION['message_type'] = "success";
    } else {
        $_SESSION['message'] = "Unit tidak ditemukan!";
        $_SESSION['message_type'] = "danger";
    }
    redirect('?page=units');
}

// 2. DELETE MAINTENANCE RECORD
if (isset($_GET['page']) && $_GET['page'] == 'delete_maintenance') {
    $id = intval($_GET['id']);
    $maintenance = getMaintenanceById($id);
    if ($maintenance) {
        $unit_id = $maintenance['Unit_Id'];
        mysqli_query($conn, "DELETE FROM MAINTENANCE_RECORDS WHERE Id = $id");
        $_SESSION['message'] = "Data maintenance untuk unit " . $maintenance['Unit_number'] . " berhasil dihapus!";
        $_SESSION['message_type'] = "success";
        redirect('?page=detail&id=' . $unit_id);
    } else {
        $_SESSION['message'] = "Data maintenance tidak ditemukan!";
        $_SESSION['message_type'] = "danger";
        redirect('?page=units');
    }
}

// 3. DELETE USER (Admin only)
if (isset($_GET['page']) && $_GET['page'] == 'delete_user') {
    if (!hasRole('admin')) {
        redirect('?page=dashboard');
    }
    $id = intval($_GET['id']);
    if ($id == $_SESSION['user_id']) {
        $_SESSION['message'] = "Anda tidak bisa menghapus akun sendiri!";
        $_SESSION['message_type'] = "danger";
        redirect('?page=users');
    }
    $user = getUserData($id);
    if ($user) {
        mysqli_query($conn, "DELETE FROM USERS WHERE Id = $id");
        $_SESSION['message'] = "User " . $user['Name'] . " berhasil dihapus!";
        $_SESSION['message_type'] = "success";
    } else {
        $_SESSION['message'] = "User tidak ditemukan!";
        $_SESSION['message_type'] = "danger";
    }
    redirect('?page=users');
}

// 4. DELETE ALL MAINTENANCE RECORDS for a unit
if (isset($_GET['page']) && $_GET['page'] == 'delete_all_maintenance') {
    if (!hasRole('admin') && !hasRole('manager')) {
        redirect('?page=dashboard');
    }
    $unit_id = intval($_GET['id']);
    $unit = getUnitById($unit_id);
    if ($unit) {
        $count = countMaintenanceByUnit($unit_id);
        mysqli_query($conn, "DELETE FROM MAINTENANCE_RECORDS WHERE Unit_Id = $unit_id");
        $_SESSION['message'] = "Semua $count data maintenance untuk unit " . $unit['Unit_number'] . " berhasil dihapus!";
        $_SESSION['message_type'] = "success";
        redirect('?page=detail&id=' . $unit_id);
    } else {
        $_SESSION['message'] = "Unit tidak ditemukan!";
        $_SESSION['message_type'] = "danger";
        redirect('?page=units');
    }
}

// Logout
if (isset($_GET['page']) && $_GET['page'] == 'logout') {
    $_SESSION = array();
    if (ini_get("session.use_cookies")) {
        $params = session_get_cookie_params();
        setcookie(session_name(), '', time() - 42000, $params["path"]);
    }
    session_destroy();
    redirect('?page=login');
}

// ============================================
// VIEW - HEADER
// ============================================
function renderHeader($title = 'RailMainTainWeb') {
    $is_public = in_array($_GET['page'] ?? 'home', ['login', 'register']);
    ?>
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title><?= $title ?> - RailMainTainWeb</title>
    <style>
        /* ========== CSS STYLE ========== */
        :root {
            --gold: #D4AF37;
            --gold-light: #F0D060;
            --gold-dark: #B8960F;
            --black: #0A0A0A;
            --black-light: #1A1A1A;
            --black-lighter: #2A2A2A;
            --white: #FFFFFF;
            --gray: #888888;
            --gray-light: #AAAAAA;
        }
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            font-family: 'Segoe UI', Tahoma, sans-serif;
            background: var(--black);
            color: var(--white);
            min-height: 100vh;
        }
        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: var(--black-light); }
        ::-webkit-scrollbar-thumb { background: var(--gold); border-radius: 4px; }
        
        .container { max-width: 1200px; margin: 0 auto; padding: 0 20px; }
        
        /* Auth */
        .auth-container {
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            background: linear-gradient(135deg, var(--black) 0%, var(--black-light) 100%);
            padding: 20px;
        }
        .auth-box {
            background: var(--black-light);
            border: 2px solid var(--gold);
            border-radius: 15px;
            padding: 40px;
            width: 100%;
            max-width: 420px;
            animation: fadeInUp 0.6s ease;
        }
        @keyframes fadeInUp {
            from { opacity: 0; transform: translateY(30px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .auth-logo { text-align: center; margin-bottom: 30px; }
        .auth-logo h1 { color: var(--gold); font-size: 28px; letter-spacing: 2px; }
        .auth-logo p { color: var(--gray-light); font-size: 14px; }
        .auth-box h2 { color: var(--gold); text-align: center; margin-bottom: 25px; font-size: 22px; }
        
        .form-group { margin-bottom: 20px; }
        .form-group label {
            display: block;
            color: var(--gray-light);
            margin-bottom: 6px;
            font-size: 14px;
            font-weight: 500;
        }
        .form-group input, .form-group select, .form-group textarea {
            width: 100%;
            padding: 12px 16px;
            background: var(--black);
            border: 1px solid var(--gray);
            border-radius: 8px;
            color: var(--white);
            font-size: 15px;
            transition: all 0.3s ease;
            font-family: inherit;
        }
        .form-group textarea { min-height: 100px; resize: vertical; }
        .form-group input:focus, .form-group select:focus, .form-group textarea:focus {
            border-color: var(--gold);
            outline: none;
            box-shadow: 0 0 0 3px rgba(212, 175, 55, 0.2);
        }
        
        .btn {
            width: 100%;
            padding: 14px;
            background: linear-gradient(135deg, var(--gold) 0%, var(--gold-dark) 100%);
            color: var(--black);
            border: none;
            border-radius: 8px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            text-transform: uppercase;
            letter-spacing: 1px;
        }
        .btn:hover { transform: translateY(-2px); box-shadow: 0 5px 20px rgba(212, 175, 55, 0.3); }
        .btn-secondary {
            background: transparent;
            border: 2px solid var(--gold);
            color: var(--gold);
        }
        .btn-secondary:hover { background: var(--gold); color: var(--black); }
        .btn-sm { padding: 8px 20px; font-size: 13px; width: auto; }
        .btn-danger {
            background: #ff4444;
            color: white;
            border: none;
        }
        .btn-danger:hover { background: #cc0000; }
        .btn-danger-sm {
            padding: 6px 14px;
            font-size: 13px;
            width: auto;
            background: #ff4444;
            color: white;
            border: none;
            border-radius: 6px;
            cursor: pointer;
        }
        .btn-danger-sm:hover { background: #cc0000; }
        
        .auth-link { text-align: center; margin-top: 20px; color: var(--gray-light); }
        .auth-link a { color: var(--gold); text-decoration: none; font-weight: 600; }
        .auth-link a:hover { text-decoration: underline; }
        
        .alert {
            padding: 12px 16px;
            border-radius: 8px;
            margin-bottom: 20px;
            font-size: 14px;
            animation: fadeInUp 0.3s ease;
        }
        .alert-danger { background: rgba(255,0,0,0.15); border: 1px solid #ff4444; color: #ff6b6b; }
        .alert-success { background: rgba(0,255,0,0.1); border: 1px solid #44ff44; color: #6bff6b; }
        .alert-warning { background: rgba(255,170,0,0.15); border: 1px solid #ffaa00; color: #ffaa00; }
        
        /* Navbar */
        .navbar {
            background: var(--black-light);
            border-bottom: 2px solid var(--gold);
            padding: 15px 0;
            position: sticky;
            top: 0;
            z-index: 1000;
        }
        .navbar .container { display: flex; align-items: center; justify-content: space-between; flex-wrap: wrap; }
        .navbar-brand { display: flex; align-items: center; gap: 12px; text-decoration: none; }
        .navbar-brand span { color: var(--gold); font-size: 22px; font-weight: 700; letter-spacing: 1px; }
        .navbar-menu { display: flex; align-items: center; gap: 20px; flex-wrap: wrap; }
        .navbar-menu a {
            color: var(--gray-light);
            text-decoration: none;
            font-size: 15px;
            padding: 8px 16px;
            border-radius: 6px;
            transition: all 0.3s ease;
        }
        .navbar-menu a:hover, .navbar-menu a.active {
            color: var(--gold);
            background: rgba(212, 175, 55, 0.1);
        }
        .navbar-menu .user-info { color: var(--gold); font-weight: 600; }
        .navbar-menu .btn-logout {
            background: transparent;
            border: 1px solid #ff4444;
            color: #ff4444;
            padding: 6px 18px;
            border-radius: 6px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-size: 14px;
        }
        .navbar-menu .btn-logout:hover { background: #ff4444; color: var(--white); }
        .nav-toggle { display: none; background: none; border: none; color: var(--gold); font-size: 28px; cursor: pointer; }
        
        /* Dashboard */
        .dashboard-header { padding: 30px 0 20px; border-bottom: 1px solid var(--black-lighter); }
        .dashboard-header h1 { color: var(--gold); font-size: 32px; }
        .dashboard-header p { color: var(--gray-light); }
        
        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            margin: 30px 0;
        }
        .stat-card {
            background: var(--black-light);
            border: 1px solid var(--black-lighter);
            border-radius: 12px;
            padding: 20px;
            text-align: center;
            transition: all 0.3s ease;
        }
        .stat-card:hover { border-color: var(--gold); transform: translateY(-5px); }
        .stat-card .number { font-size: 36px; font-weight: 700; color: var(--gold); }
        .stat-card .label { color: var(--gray-light); font-size: 14px; margin-top: 5px; }
        .status-dot { display: inline-block; width: 12px; height: 12px; border-radius: 50%; margin-right: 8px; }
        .status-dot.active { background: #44ff44; }
        .status-dot.maintenance { background: #ffaa00; }
        .status-dot.damaged { background: #ff4444; }
        
        /* Units */
        .page-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            padding: 20px 0;
            gap: 15px;
        }
        .page-header h2 { color: var(--gold); font-size: 26px; }
        .page-header .actions { display: flex; gap: 10px; flex-wrap: wrap; }
        
        .filter-bar {
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
            margin-bottom: 25px;
            padding: 15px;
            background: var(--black-light);
            border-radius: 10px;
            border: 1px solid var(--black-lighter);
        }
        .filter-bar input, .filter-bar select {
            padding: 10px 16px;
            background: var(--black);
            border: 1px solid var(--gray);
            border-radius: 6px;
            color: var(--white);
            flex: 1;
            min-width: 150px;
        }
        .filter-bar input:focus, .filter-bar select:focus {
            border-color: var(--gold);
            outline: none;
        }
        .filter-bar .btn { width: auto; padding: 10px 25px; }
        
        .unit-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
            gap: 20px;
            margin-top: 10px;
        }
        .unit-card {
            background: var(--black-light);
            border: 1px solid var(--black-lighter);
            border-radius: 12px;
            padding: 20px;
            transition: all 0.3s ease;
        }
        .unit-card:hover { border-color: var(--gold); transform: translateY(-3px); box-shadow: 0 5px 20px rgba(0,0,0,0.3); }
        .unit-card .unit-header { display: flex; justify-content: space-between; align-items: start; margin-bottom: 12px; }
        .unit-card .unit-number { font-size: 20px; font-weight: 700; color: var(--white); }
        .unit-card .unit-type { color: var(--gray-light); font-size: 13px; background: var(--black); padding: 2px 12px; border-radius: 20px; }
        .unit-card .unit-status {
            display: inline-block;
            padding: 4px 14px;
            border-radius: 20px;
            font-size: 13px;
            font-weight: 600;
        }
        .status-active { background: rgba(68,255,68,0.15); color: #44ff44; }
        .status-maintenance { background: rgba(255,170,0,0.15); color: #ffaa00; }
        .status-damaged { background: rgba(255,68,68,0.15); color: #ff4444; }
        .unit-card .unit-info { color: var(--gray-light); font-size: 14px; margin: 8px 0; }
        .unit-card .unit-actions {
            display: flex;
            gap: 10px;
            margin-top: 15px;
            padding-top: 15px;
            border-top: 1px solid var(--black-lighter);
            flex-wrap: wrap;
        }
        .unit-card .unit-actions a {
            padding: 6px 16px;
            border-radius: 6px;
            text-decoration: none;
            font-size: 13px;
            font-weight: 500;
            transition: all 0.3s ease;
        }
        .btn-detail { background: var(--gold); color: var(--black); }
        .btn-detail:hover { background: var(--gold-dark); }
        .btn-maintenance { background: transparent; border: 1px solid var(--gold); color: var(--gold); }
        .btn-maintenance:hover { background: var(--gold); color: var(--black); }
        .btn-edit { background: transparent; border: 1px solid #4a9eff; color: #4a9eff; }
        .btn-edit:hover { background: #4a9eff; color: var(--black); }
        .btn-delete { background: transparent; border: 1px solid #ff4444; color: #ff4444; }
        .btn-delete:hover { background: #ff4444; color: var(--white); }
        
        /* Detail */
        .detail-card {
            background: var(--black-light);
            border: 1px solid var(--gold);
            border-radius: 12px;
            padding: 25px;
            margin-bottom: 25px;
        }
        .detail-card h3 { color: var(--gold); margin-bottom: 15px; font-size: 20px; }
        .detail-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
        }
        .detail-item {
            padding: 10px;
            background: var(--black);
            border-radius: 8px;
        }
        .detail-item .label { color: var(--gray); font-size: 12px; text-transform: uppercase; letter-spacing: 0.5px; }
        .detail-item .value { color: var(--white); font-size: 16px; margin-top: 4px; }
        
        /* Table */
        .maintenance-table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 10px;
        }
        .maintenance-table th {
            background: var(--black);
            color: var(--gold);
            padding: 12px;
            text-align: left;
            border-bottom: 2px solid var(--gold);
        }
        .maintenance-table td {
            padding: 12px;
            border-bottom: 1px solid var(--black-lighter);
            color: var(--gray-light);
        }
        .maintenance-table tr:hover td { background: rgba(212, 175, 55, 0.05); }
        
        /* Users Table */
        .users-table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 10px;
        }
        .users-table th {
            background: var(--black);
            color: var(--gold);
            padding: 12px;
            text-align: left;
            border-bottom: 2px solid var(--gold);
        }
        .users-table td {
            padding: 12px;
            border-bottom: 1px solid var(--black-lighter);
            color: var(--gray-light);
        }
        .users-table tr:hover td { background: rgba(212, 175, 55, 0.05); }
        
        /* Footer */
        footer {
            background: var(--black-light);
            border-top: 1px solid var(--black-lighter);
            padding: 20px 0;
            margin-top: 40px;
            text-align: center;
            color: var(--gray);
            font-size: 14px;
        }
        footer span { color: var(--gold); }
        
        /* Utilities */
        .text-gold { color: var(--gold); }
        .text-gray { color: var(--gray-light); }
        .text-center { text-align: center; }
        .text-right { text-align: right; }
        .mt-10 { margin-top: 10px; }
        .mt-20 { margin-top: 20px; }
        .mb-10 { margin-bottom: 10px; }
        .mb-20 { margin-bottom: 20px; }
        .flex { display: flex; }
        .flex-between { display: flex; justify-content: space-between; align-items: center; }
        .gap-10 { gap: 10px; }
        .w-auto { width: auto; }
        
        /* Delete confirmation modal */
        .modal-overlay {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.8);
            z-index: 2000;
            align-items: center;
            justify-content: center;
        }
        .modal-overlay.active { display: flex; }
        .modal-box {
            background: var(--black-light);
            border: 2px solid #ff4444;
            border-radius: 15px;
            padding: 30px;
            max-width: 500px;
            width: 90%;
            animation: fadeInUp 0.3s ease;
        }
        .modal-box h3 { color: #ff4444; margin-bottom: 15px; }
        .modal-box p { color: var(--gray-light); margin-bottom: 20px; }
        .modal-actions { display: flex; gap: 10px; }
        .modal-actions .btn { width: auto; flex: 1; }
        
        /* Responsive */
        @media (max-width: 768px) {
            .navbar-menu {
                display: none;
                width: 100%;
                flex-direction: column;
                padding-top: 15px;
                gap: 10px;
            }
            .navbar-menu.open { display: flex; }
            .nav-toggle { display: block; }
            .auth-box { padding: 30px 20px; }
            .unit-grid { grid-template-columns: 1fr; }
            .stats-grid { grid-template-columns: repeat(2, 1fr); }
            .page-header { flex-direction: column; align-items: stretch; }
            .filter-bar { flex-direction: column; }
            .filter-bar input, .filter-bar select, .filter-bar .btn { width: 100%; }
            .detail-grid { grid-template-columns: 1fr; }
            .maintenance-table, .users-table { font-size: 13px; }
            .maintenance-table th, .maintenance-table td,
            .users-table th, .users-table td { padding: 8px; }
        }
        @media (max-width: 480px) {
            .stats-grid { grid-template-columns: 1fr; }
            .auth-logo h1 { font-size: 22px; }
            .unit-card .unit-actions a { flex: 1; text-align: center; min-width: 80px; }
        }
    </style>
</head>
<body>
<?php if (isLoggedIn() && !$is_public): ?>
<nav class="navbar">
    <div class="container">
        <a href="?page=dashboard" class="navbar-brand">
            <span>RailMainTainWeb</span>
        </a>
        <button class="nav-toggle" onclick="document.querySelector('.navbar-menu').classList.toggle('open')">☰</button>
        <div class="navbar-menu">
            <a href="?page=dashboard" class="<?= ($_GET['page'] ?? '') == 'dashboard' ? 'active' : '' ?>">Dashboard</a>
            <a href="?page=units" class="<?= ($_GET['page'] ?? '') == 'units' ? 'active' : '' ?>">Daftar Unit</a>
            <?php if (hasRole('admin') || hasRole('manager')): ?>
            <a href="?page=add_unit" class="<?= ($_GET['page'] ?? '') == 'add_unit' ? 'active' : '' ?>">Tambah Unit</a>
            <?php endif; ?>
            <?php if (hasRole('admin')): ?>
            <a href="?page=users" class="<?= ($_GET['page'] ?? '') == 'users' ? 'active' : '' ?>">👥 Users</a>
            <?php endif; ?>
            <span class="user-info">👤 <?= htmlspecialchars($_SESSION['name'] ?? 'User') ?></span>
            <span class="user-info" style="color:var(--gray);font-weight:normal;font-size:13px;">(<?= ucfirst($_SESSION['role'] ?? '') ?>)</span>
            <a href="?page=logout" class="btn-logout" style="text-decoration:none;padding:6px 18px;border:1px solid #ff4444;color:#ff4444;border-radius:6px;">Logout</a>
        </div>
    </div>
</nav>
<?php endif; ?>
<main>
<?php
}

// ============================================
// VIEW - FOOTER
// ============================================
function renderFooter() {
    ?>
</main>
<footer>
    <div class="container">
        &copy; <?= date('Y') ?> RailMainTainWeb - Sistem Perawatan Aset KAI<br>
        <span>Powered by PT Kereta Api Indonesia</span>
    </div>
</footer>
<script>
document.addEventListener('DOMContentLoaded', function() {
    // Auto-hide alerts
    setTimeout(function() {
        document.querySelectorAll('.alert').forEach(function(el) {
            setTimeout(function() {
                el.style.opacity = '0';
                setTimeout(function() { el.remove(); }, 300);
            }, 5000);
        });
    }, 100);
    
    // Filter units
    window.filterUnits = function() {
        var search = (document.getElementById('searchInput')?.value || '').toLowerCase();
        var type = document.getElementById('filterType')?.value || '';
        var status = document.getElementById('filterStatus')?.value || '';
        document.querySelectorAll('.unit-card').forEach(function(card) {
            var num = (card.dataset.number || '').toLowerCase();
            var ct = card.dataset.type || '';
            var cs = card.dataset.status || '';
            var show = true;
            if (search && !num.includes(search)) show = false;
            if (type && ct !== type) show = false;
            if (status && cs !== status) show = false;
            card.style.display = show ? 'block' : 'none';
        });
    };
    
    // Confirm delete function
    window.confirmDelete = function(url, name, type) {
        type = type || 'unit';
        var msg = 'Apakah Anda yakin ingin menghapus ' + type + ' "' + name + '"?\n\nData yang dihapus tidak dapat dikembalikan!';
        if (type == 'maintenance') {
            msg = 'Apakah Anda yakin ingin menghapus data maintenance untuk unit "' + name + '"?\n\nData yang dihapus tidak dapat dikembalikan!';
        }
        if (type == 'user') {
            msg = 'Apakah Anda yakin ingin menghapus user "' + name + '"?\n\nData yang dihapus tidak dapat dikembalikan!';
        }
        if (type == 'all_maintenance') {
            msg = 'Apakah Anda yakin ingin menghapus SEMUA data maintenance untuk unit "' + name + '"?\n\nData yang dihapus tidak dapat dikembalikan!';
        }
        if (confirm(msg)) {
            window.location.href = url;
        }
    };
});
</script>
</body>
</html>
<?php
}

// ============================================
// VIEW - LOGIN
// ============================================
function renderLogin($error = '') {
    renderHeader('Login');
    ?>
<div class="auth-container">
    <div class="auth-box">
        <div class="auth-logo">
            <h1>RailMainTainWeb</h1>
            <p>Sistem Perawatan Aset KAI</p>
        </div>
        <h2>Masuk</h2>
        <?php if ($error): ?><div class="alert alert-danger"><?= $error ?></div><?php endif; ?>
        <form method="POST" action="?page=login">
            <div class="form-group">
                <label>Email</label>
                <input type="email" name="email" placeholder="Masukkan email" required>
            </div>
            <div class="form-group">
                <label>Kata Sandi</label>
                <input type="password" name="password" placeholder="Masukkan kata sandi" required>
            </div>
            <button type="submit" class="btn">Masuk</button>
        </form>
        <div class="auth-link">
            Belum punya akun? <a href="?page=register">Daftar Disini</a>
        </div>
    </div>
</div>
<?php renderFooter();
}

// ============================================
// VIEW - REGISTER
// ============================================
function renderRegister($error = '', $success = '') {
    renderHeader('Daftar');
    ?>
<div class="auth-container">
    <div class="auth-box">
        <div class="auth-logo">
            <h1>RailMainTainWeb</h1>
            <p>Daftar Akun Baru</p>
        </div>
        <h2>Pendaftaran</h2>
        <?php if ($error): ?><div class="alert alert-danger"><?= $error ?></div><?php endif; ?>
        <?php if ($success): ?><div class="alert alert-success"><?= $success ?></div><?php endif; ?>
        <form method="POST" action="?page=register">
            <div class="form-group">
                <label>Nama Lengkap</label>
                <input type="text" name="name" placeholder="Masukkan nama lengkap" required>
            </div>
            <div class="form-group">
                <label>Email</label>
                <input type="email" name="email" placeholder="Masukkan email" required>
            </div>
            <div class="form-group">
                <label>Password (min 6 karakter)</label>
                <input type="password" name="password" placeholder="Minimal 6 karakter" required>
            </div>
            <div class="form-group">
                <label>Konfirmasi Password</label>
                <input type="password" name="confirm_password" placeholder="Ulangi password" required>
            </div>
            <div class="form-group">
                <label>Role</label>
                <select name="role" required>
                    <option value="">Pilih Role</option>
                    <option value="teknisi">Teknisi</option>
                    <option value="supervisor">Supervisor</option>
                    <option value="manager">Manager</option>
                    <option value="kepala_depo">Kepala Depo</option>
                </select>
            </div>
            <div class="form-group">
                <label>Departemen (Opsional)</label>
                <input type="text" name="departemen" placeholder="Contoh: Maintenance">
            </div>
            <div class="form-group">
                <label>No. Telepon (Opsional)</label>
                <input type="text" name="no_telepon" placeholder="Contoh: 081234567890">
            </div>
            <button type="submit" class="btn">Daftar</button>
        </form>
        <div class="auth-link">
            Sudah punya akun? <a href="?page=login">Masuk Disini</a>
        </div>
    </div>
</div>
<?php renderFooter();
}

// ============================================
// VIEW - DASHBOARD
// ============================================
function renderDashboard() {
    renderHeader('Dashboard');
    $stats = getUnitStats();
    $units = getAllUnits();
    $units = array_slice($units, 0, 5);
    $maintenance = getMaintenanceRecords();
    $maintenance = array_slice($maintenance, 0, 5);
    ?>
<div class="container">
    <div class="dashboard-header">
        <h1>Dashboard RailMainTainWeb</h1>
        <p>Selamat datang, <?= htmlspecialchars($_SESSION['name']) ?>! (<?= ucfirst($_SESSION['role']) ?>)</p>
    </div>
    
    <?php if (isset($_SESSION['message'])): ?>
    <div class="alert alert-<?= $_SESSION['message_type'] ?? 'success' ?>"><?= $_SESSION['message'] ?></div>
    <?php unset($_SESSION['message'], $_SESSION['message_type']); ?>
    <?php endif; ?>
    
    <div class="stats-grid">
        <div class="stat-card">
            <div class="number"><?= $stats['total'] ?? 0 ?></div>
            <div class="label">Total Unit</div>
        </div>
        <div class="stat-card" style="border-color:#44ff44;">
            <div class="number" style="color:#44ff44;"><?= $stats['layak'] ?? 0 ?></div>
            <div class="label"><span class="status-dot active"></span>Layak Operasi</div>
        </div>
        <div class="stat-card" style="border-color:#ffaa00;">
            <div class="number" style="color:#ffaa00;"><?= $stats['perawatan'] ?? 0 ?></div>
            <div class="label"><span class="status-dot maintenance"></span>Sedang Perawatan</div>
        </div>
        <div class="stat-card" style="border-color:#ff4444;">
            <div class="number" style="color:#ff4444;"><?= $stats['rusak'] ?? 0 ?></div>
            <div class="label"><span class="status-dot damaged"></span>Rusak</div>
        </div>
    </div>

    <h3 class="text-gold mt-20 mb-10">Unit Terbaru</h3>
    <div class="unit-grid">
        <?php foreach ($units as $unit): ?>
        <div class="unit-card">
            <div class="unit-header">
                <div>
                    <div class="unit-number"><?= htmlspecialchars($unit['Unit_number']) ?></div>
                    <div class="unit-type"><?= htmlspecialchars(str_replace('_', ' ', $unit['Type'])) ?></div>
                </div>
                <span class="unit-status status-<?= $unit['Status'] ?>">
                    <?= $unit['Status'] == 'active' ? 'Layak' : ($unit['Status'] == 'maintenance' ? 'Sedang Perawatan' : 'Rusak') ?>
                </span>
            </div>
            <div class="unit-info">Depo: <?= htmlspecialchars($unit['Nama_Depo'] ?? '-') ?></div>
            <div class="unit-info">Perawatan Terakhir: <?= $unit['Last_maintenance'] ? date('d M Y', strtotime($unit['Last_maintenance'])) : '-' ?></div>
            <div class="unit-actions">
                <a href="?page=detail&id=<?= $unit['Id'] ?>" class="btn-detail">Detail</a>
                <a href="?page=add_maintenance&id=<?= $unit['Id'] ?>" class="btn-maintenance">Catat Perawatan</a>
            </div>
        </div>
        <?php endforeach; ?>
        <?php if (empty($units)): ?><div class="text-gray text-center" style="padding:20px;">Belum ada unit</div><?php endif; ?>
    </div>
    <div class="text-center mt-20">
        <a href="?page=units" class="btn w-auto" style="padding:12px 40px;width:auto;">Lihat Semua Unit →</a>
    </div>

    <h3 class="text-gold mt-20 mb-10">🔧 Maintenance Terakhir</h3>
    <div style="background:var(--black-light);border-radius:12px;overflow:hidden;border:1px solid var(--black-lighter);">
        <table class="maintenance-table">
            <thead><tr><th>Unit</th><th>Tanggal</th><th>Jenis</th><th>Teknisi</th><th>Status</th></tr></thead>
            <tbody>
                <?php foreach ($maintenance as $m): ?>
                <tr>
                    <td><?= htmlspecialchars($m['Unit_number']) ?></td>
                    <td><?= date('d M Y', strtotime($m['Maintenance_date'])) ?></td>
                    <td><?= ucfirst($m['Maintenance_type']) ?></td>
                    <td><?= htmlspecialchars($m['Teknisi_Name'] ?? '-') ?></td>
                    <td><span style="color:<?= $m['Status_result'] == 'success' ? '#44ff44' : ($m['Status_result'] == 'pending' ? '#ffaa00' : '#ff4444') ?>"><?= ucfirst($m['Status_result']) ?></span></td>
                </tr>
                <?php endforeach; ?>
                <?php if (empty($maintenance)): ?><tr><td colspan="5" class="text-center text-gray" style="padding:20px;">Belum ada maintenance</td></tr><?php endif; ?>
            </tbody>
        </table>
    </div>
</div>
<?php renderFooter();
}

// ============================================
// VIEW - UNITS
// ============================================
function renderUnits() {
    renderHeader('Daftar Unit');
    $units = getAllUnits();
    $stats = getUnitStats();
    ?>
<div class="container">
    <div class="page-header">
        <h2>📋 Daftar Unit</h2>
        <div class="actions">
            <?php if (hasRole('admin') || hasRole('manager')): ?>
            <a href="?page=add_unit" class="btn w-auto" style="padding:10px 25px;width:auto;">+ Tambah Unit</a>
            <?php endif; ?>
            <a href="?page=add_maintenance" class="btn btn-secondary w-auto" style="padding:10px 25px;width:auto;">🔧 Catat Perawatan</a>
        </div>
    </div>

    <?php if (isset($_SESSION['message'])): ?>
    <div class="alert alert-<?= $_SESSION['message_type'] ?? 'success' ?>"><?= $_SESSION['message'] ?></div>
    <?php unset($_SESSION['message'], $_SESSION['message_type']); ?>
    <?php endif; ?>

    <div class="filter-bar">
        <input type="text" id="searchInput" placeholder="🔍 Cari unit..." onkeyup="filterUnits()">
        <select id="filterType" onchange="filterUnits()">
            <option value="">Semua Jenis</option>
            <option value="Lokomotif">Lokomotif</option>
            <option value="Kereta_Penumpang">Kereta Penumpang</option>
            <option value="Kereta_Barang">Kereta Barang</option>
            <option value="Kereta_Eksekutif">Kereta Eksekutif</option>
            <option value="Kereta_Bisnis">Kereta Bisnis</option>
            <option value="Kereta_Ekonomi">Kereta Ekonomi</option>
            <option value="KRL">KRL</option>
            <option value="KRD">KRD</option>
        </select>
        <select id="filterStatus" onchange="filterUnits()">
            <option value="">Semua Status</option>
            <option value="active">Layak</option>
            <option value="maintenance">Sedang Perawatan</option>
            <option value="damaged">Rusak</option>
        </select>
        <button class="btn w-auto" style="padding:10px 20px;width:auto;" onclick="filterUnits()">Filter</button>
    </div>

    <div style="display:flex;gap:20px;margin-bottom:20px;flex-wrap:wrap;padding:15px;background:var(--black-light);border-radius:10px;">
        <span>📊 Total: <strong class="text-gold"><?= $stats['total'] ?? 0 ?></strong></span>
        <span>🟢 Layak: <strong style="color:#44ff44;"><?= $stats['layak'] ?? 0 ?></strong></span>
        <span>🟡 Perawatan: <strong style="color:#ffaa00;"><?= $stats['perawatan'] ?? 0 ?></strong></span>
        <span>🔴 Rusak: <strong style="color:#ff4444;"><?= $stats['rusak'] ?? 0 ?></strong></span>
    </div>

    <div class="unit-grid">
        <?php foreach ($units as $unit): ?>
        <div class="unit-card" data-number="<?= strtolower($unit['Unit_number']) ?>" data-type="<?= $unit['Type'] ?>" data-status="<?= $unit['Status'] ?>">
            <div class="unit-header">
                <div>
                    <div class="unit-number"><?= htmlspecialchars($unit['Unit_number']) ?></div>
                    <div class="unit-type"><?= htmlspecialchars(str_replace('_', ' ', $unit['Type'])) ?></div>
                </div>
                <span class="unit-status status-<?= $unit['Status'] ?>">
                    <?= $unit['Status'] == 'active' ? 'Layak' : ($unit['Status'] == 'maintenance' ? 'Sedang Perawatan' : 'Rusak') ?>
                </span>
            </div>
            <div class="unit-info">🏢 Depo: <?= htmlspecialchars($unit['Nama_Depo'] ?? '-') ?></div>
            <div class="unit-info">🛠️ Perawatan Terakhir: <?= $unit['Last_maintenance'] ? date('d M Y', strtotime($unit['Last_maintenance'])) : '-' ?></div>
            <?php if ($unit['Next_maintenance']): ?>
            <div class="unit-info" style="color:var(--gold);">📅 Selanjutnya: <?= date('d M Y', strtotime($unit['Next_maintenance'])) ?></div>
            <?php endif; ?>
            <div class="unit-actions">
                <a href="?page=detail&id=<?= $unit['Id'] ?>" class="btn-detail">📄 Detail</a>
                <a href="?page=add_maintenance&id=<?= $unit['Id'] ?>" class="btn-maintenance">🔧 Catat Perawatan</a>
                <?php if (hasRole('admin') || hasRole('manager')): ?>
                <a href="?page=edit_unit&id=<?= $unit['Id'] ?>" class="btn-edit">✏️ Edit</a>
                <a href="javascript:void(0)" onclick="confirmDelete('?page=delete_unit&id=<?= $unit['Id'] ?>', '<?= addslashes($unit['Unit_number']) ?>', 'unit')" class="btn-delete">🗑️ Hapus</a>
                <?php endif; ?>
            </div>
        </div>
        <?php endforeach; ?>
        <?php if (empty($units)): ?>
        <div style="grid-column:1/-1;text-align:center;padding:40px;color:var(--gray);">
            <p style="font-size:18px;">Belum ada data unit</p>
            <a href="?page=add_unit" class="btn w-auto" style="margin-top:15px;width:auto;">+ Tambah Unit Pertama</a>
        </div>
        <?php endif; ?>
    </div>
</div>
<?php renderFooter();
}

// ============================================
// VIEW - DETAIL UNIT
// ============================================
function renderDetail($id) {
    renderHeader('Detail Unit');
    $unit = getUnitById($id);
    if (!$unit) { redirect('?page=units'); }
    $maintenance = getMaintenanceRecords($id);
    $count = count($maintenance);
    ?>
<div class="container">
    <div class="page-header">
        <h2>📄 Detail Unit</h2>
        <div class="actions">
            <a href="?page=units" class="btn btn-secondary w-auto" style="padding:10px 25px;width:auto;">← Kembali</a>
            <a href="?page=add_maintenance&id=<?= $id ?>" class="btn w-auto" style="padding:10px 25px;width:auto;">🔧 Catat Perawatan</a>
        </div>
    </div>

    <?php if (isset($_SESSION['message'])): ?>
    <div class="alert alert-<?= $_SESSION['message_type'] ?? 'success' ?>"><?= $_SESSION['message'] ?></div>
    <?php unset($_SESSION['message'], $_SESSION['message_type']); ?>
    <?php endif; ?>

    <div class="detail-card">
        <h3>🚂 Informasi Unit</h3>
        <div class="detail-grid">
            <div class="detail-item"><div class="label">Nomor Unit</div><div class="value"><?= htmlspecialchars($unit['Unit_number']) ?></div></div>
            <div class="detail-item"><div class="label">Jenis</div><div class="value"><?= htmlspecialchars(str_replace('_', ' ', $unit['Type'])) ?></div></div>
            <div class="detail-item">
                <div class="label">Status</div>
                <div class="value"><span class="unit-status status-<?= $unit['Status'] ?>"><?= $unit['Status'] == 'active' ? 'Layak' : ($unit['Status'] == 'maintenance' ? 'Sedang Perawatan' : 'Rusak') ?></span></div>
            </div>
            <div class="detail-item"><div class="label">Depo</div><div class="value"><?= htmlspecialchars($unit['Nama_Depo'] ?? '-') ?></div></div>
            <div class="detail-item"><div class="label">Perawatan Terakhir</div><div class="value"><?= $unit['Last_maintenance'] ? date('d M Y', strtotime($unit['Last_maintenance'])) : '-' ?></div></div>
            <div class="detail-item"><div class="label">Perawatan Selanjutnya</div><div class="value" style="color:var(--gold);"><?= $unit['Next_maintenance'] ? date('d M Y', strtotime($unit['Next_maintenance'])) : '-' ?></div></div>
            <div class="detail-item"><div class="label">Total KM</div><div class="value"><?= number_format($unit['Total_km'] ?? 0) ?> km</div></div>
            <div class="detail-item"><div class="label">Tahun Pembuatan</div><div class="value"><?= $unit['Tahun_Pembuatan'] ?? '-' ?></div></div>
            <?php if ($unit['Pabrikan']): ?>
            <div class="detail-item"><div class="label">Pabrikan</div><div class="value"><?= htmlspecialchars($unit['Pabrikan']) ?></div></div>
            <?php endif; ?>
        </div>
    </div>

    <div class="detail-card">
        <div class="flex-between">
            <h3>🔧 Riwayat Maintenance (<?= $count ?>)</h3>
            <?php if ($count > 0 && (hasRole('admin') || hasRole('manager'))): ?>
            <a href="javascript:void(0)" onclick="confirmDelete('?page=delete_all_maintenance&id=<?= $id ?>', '<?= addslashes($unit['Unit_number']) ?>', 'all_maintenance')" class="btn-danger-sm" style="text-decoration:none;">🗑️ Hapus Semua</a>
            <?php endif; ?>
        </div>
        <?php if (!empty($maintenance)): ?>
        <table class="maintenance-table">
            <thead><tr><th>Tanggal</th><th>Jenis</th><th>Level</th><th>Teknisi</th><th>Biaya</th><th>Status</th><th>Aksi</th></tr></thead>
            <tbody>
                <?php foreach ($maintenance as $m): ?>
                <tr>
                    <td><?= date('d M Y', strtotime($m['Maintenance_date'])) ?></td>
                    <td><?= ucfirst($m['Maintenance_type']) ?></td>
                    <td><?= ucfirst($m['Maintenance_Level']) ?></td>
                    <td><?= htmlspecialchars($m['Teknisi_Name'] ?? '-') ?></td>
                    <td>Rp <?= number_format($m['Biaya'] ?? 0, 0, ',', '.') ?></td>
                    <td><span style="color:<?= $m['Status_result'] == 'success' ? '#44ff44' : ($m['Status_result'] == 'pending' ? '#ffaa00' : '#ff4444') ?>"><?= ucfirst($m['Status_result']) ?></span></td>
                    <td>
                        <?php if (hasRole('admin') || hasRole('manager')): ?>
                        <a href="javascript:void(0)" onclick="confirmDelete('?page=delete_maintenance&id=<?= $m['Id'] ?>', '<?= addslashes($unit['Unit_number']) ?>', 'maintenance')" style="color:#ff4444;text-decoration:none;font-size:13px;">🗑️ Hapus</a>
                        <?php else: ?>
                        -
                        <?php endif; ?>
                    </td>
                </tr>
                <?php endforeach; ?>
            </tbody>
        </table>
        <?php else: ?>
        <p class="text-gray text-center" style="padding:20px;">Belum ada riwayat maintenance</p>
        <?php endif; ?>
    </div>
</div>
<?php renderFooter();
}

// ============================================
// VIEW - ADD UNIT
// ============================================
function renderAddUnit($error = '', $success = '') {
    if (!hasRole('admin') && !hasRole('manager')) { redirect('?page=dashboard'); }
    renderHeader('Tambah Unit');
    $depos = getAllDepo();
    ?>
<div class="container">
    <div class="page-header">
        <h2>➕ Tambah Unit Baru</h2>
        <div class="actions"><a href="?page=units" class="btn btn-secondary w-auto" style="padding:10px 25px;width:auto;">← Kembali</a></div>
    </div>
    <?php if ($error): ?><div class="alert alert-danger"><?= $error ?></div><?php endif; ?>
    <?php if ($success): ?><div class="alert alert-success"><?= $success ?></div><?php endif; ?>
    <div class="detail-card">
        <form method="POST" action="?page=add_unit">
            <div class="form-group">
                <label>Nomor Unit *</label>
                <input type="text" name="unit_number" placeholder="Contoh: KAI-1001" required>
            </div>
            <div class="form-group">
                <label>Jenis Unit *</label>
                <select name="type" required>
                    <option value="">Pilih Jenis</option>
                    <option value="Lokomotif">Lokomotif</option>
                    <option value="Kereta_Penumpang">Kereta Penumpang</option>
                    <option value="Kereta_Barang">Kereta Barang</option>
                    <option value="Kereta_Eksekutif">Kereta Eksekutif</option>
                    <option value="Kereta_Bisnis">Kereta Bisnis</option>
                    <option value="Kereta_Ekonomi">Kereta Ekonomi</option>
                    <option value="KRL">KRL</option>
                    <option value="KRD">KRD</option>
                </select>
            </div>
            <div class="form-group">
                <label>Depo *</label>
                <select name="depo_id" required>
                    <option value="">Pilih Depo</option>
                    <?php foreach ($depos as $d): ?>
                    <option value="<?= $d['Id'] ?>"><?= htmlspecialchars($d['Nama_Depo']) ?> (<?= htmlspecialchars($d['Kode_Depo']) ?>)</option>
                    <?php endforeach; ?>
                </select>
            </div>
            <div class="form-group">
                <label>Tahun Pembuatan</label>
                <input type="number" name="tahun_pembuatan" min="1900" max="<?= date('Y') ?>" placeholder="YYYY">
            </div>
            <div class="form-group">
                <label>Pabrikan</label>
                <input type="text" name="pabrikan" placeholder="Contoh: GE, INKA">
            </div>
            <div class="form-group">
                <label>Status *</label>
                <select name="status" required>
                    <option value="active">Layak</option>
                    <option value="maintenance">Sedang Perawatan</option>
                    <option value="damaged">Rusak</option>
                    <option value="inactive">Tidak Aktif</option>
                </select>
            </div>
            <button type="submit" class="btn">💾 Simpan Unit</button>
        </form>
    </div>
</div>
<?php renderFooter();
}

// ============================================
// VIEW - EDIT UNIT
// ============================================
function renderEditUnit($id, $error = '', $success = '') {
    if (!hasRole('admin') && !hasRole('manager')) { redirect('?page=dashboard'); }
    $unit = getUnitById($id);
    if (!$unit) { redirect('?page=units'); }
    renderHeader('Edit Unit');
    $depos = getAllDepo();
    ?>
<div class="container">
    <div class="page-header">
        <h2>✏️ Edit Unit</h2>
        <div class="actions"><a href="?page=units" class="btn btn-secondary w-auto" style="padding:10px 25px;width:auto;">← Kembali</a></div>
    </div>
    <?php if ($error): ?><div class="alert alert-danger"><?= $error ?></div><?php endif; ?>
    <?php if ($success): ?><div class="alert alert-success"><?= $success ?></div><?php endif; ?>
    <div class="detail-card">
        <form method="POST" action="?page=edit_unit&id=<?= $id ?>">
            <div class="form-group">
                <label>Nomor Unit *</label>
                <input type="text" name="unit_number" required value="<?= htmlspecialchars($unit['Unit_number']) ?>">
            </div>
            <div class="form-group">
                <label>Jenis Unit *</label>
                <select name="type" required>
                    <option value="Lokomotif" <?= $unit['Type'] == 'Lokomotif' ? 'selected' : '' ?>>Lokomotif</option>
                    <option value="Kereta_Penumpang" <?= $unit['Type'] == 'Kereta_Penumpang' ? 'selected' : '' ?>>Kereta Penumpang</option>
                    <option value="Kereta_Barang" <?= $unit['Type'] == 'Kereta_Barang' ? 'selected' : '' ?>>Kereta Barang</option>
                    <option value="Kereta_Eksekutif" <?= $unit['Type'] == 'Kereta_Eksekutif' ? 'selected' : '' ?>>Kereta Eksekutif</option>
                    <option value="Kereta_Bisnis" <?= $unit['Type'] == 'Kereta_Bisnis' ? 'selected' : '' ?>>Kereta Bisnis</option>
                    <option value="Kereta_Ekonomi" <?= $unit['Type'] == 'Kereta_Ekonomi' ? 'selected' : '' ?>>Kereta Ekonomi</option>
                    <option value="KRL" <?= $unit['Type'] == 'KRL' ? 'selected' : '' ?>>KRL</option>
                    <option value="KRD" <?= $unit['Type'] == 'KRD' ? 'selected' : '' ?>>KRD</option>
                </select>
            </div>
            <div class="form-group">
                <label>Depo *</label>
                <select name="depo_id" required>
                    <?php foreach ($depos as $d): ?>
                    <option value="<?= $d['Id'] ?>" <?= $unit['Depo_Id'] == $d['Id'] ? 'selected' : '' ?>>
                        <?= htmlspecialchars($d['Nama_Depo']) ?> (<?= htmlspecialchars($d['Kode_Depo']) ?>)
                    </option>
                    <?php endforeach; ?>
                </select>
            </div>
            <div class="form-group">
                <label>Tahun Pembuatan</label>
                <input type="number" name="tahun_pembuatan" min="1900" max="<?= date('Y') ?>" value="<?= htmlspecialchars($unit['Tahun_Pembuatan']) ?>">
            </div>
            <div class="form-group">
                <label>Pabrikan</label>
                <input type="text" name="pabrikan" value="<?= htmlspecialchars($unit['Pabrikan']) ?>">
            </div>
            <div class="form-group">
                <label>Status *</label>
                <select name="status" required>
                    <option value="active" <?= $unit['Status'] == 'active' ? 'selected' : '' ?>>Layak</option>
                    <option value="maintenance" <?= $unit['Status'] == 'maintenance' ? 'selected' : '' ?>>Sedang Perawatan</option>
                    <option value="damaged" <?= $unit['Status'] == 'damaged' ? 'selected' : '' ?>>Rusak</option>
                    <option value="inactive" <?= $unit['Status'] == 'inactive' ? 'selected' : '' ?>>Tidak Aktif</option>
                </select>
            </div>
            <button type="submit" class="btn">💾 Update Unit</button>
        </form>
    </div>
</div>
<?php renderFooter();
}

// ============================================
// VIEW - ADD MAINTENANCE
// ============================================
function renderAddMaintenance($unit_id = 0, $error = '', $success = '') {
    renderHeader('Catat Maintenance');
    $unit = $unit_id ? getUnitById($unit_id) : null;
    $units = getAllUnits();
    $teknisi = getAllTeknisi();
    ?>
<div class="container">
    <div class="page-header">
        <h2>🔧 Catat Perawatan Baru</h2>
        <div class="actions"><a href="?page=units" class="btn btn-secondary w-auto" style="padding:10px 25px;width:auto;">← Kembali</a></div>
    </div>
    <?php if ($error): ?><div class="alert alert-danger"><?= $error ?></div><?php endif; ?>
    <?php if ($success): ?><div class="alert alert-success"><?= $success ?></div><?php endif; ?>
    <div class="detail-card">
        <form method="POST" action="?page=add_maintenance">
            <?php if ($unit): ?>
            <div class="form-group">
                <label>Unit</label>
                <input type="text" value="<?= htmlspecialchars($unit['Unit_number']) ?> - <?= htmlspecialchars($unit['Type']) ?>" readonly style="background:var(--black-lighter);">
                <input type="hidden" name="unit_id" value="<?= $unit['Id'] ?>">
            </div>
            <?php else: ?>
            <div class="form-group">
                <label>Pilih Unit *</label>
                <select name="unit_id" required>
                    <option value="">Pilih Unit</option>
                    <?php foreach ($units as $u): ?>
                    <option value="<?= $u['Id'] ?>"><?= htmlspecialchars($u['Unit_number']) ?> - <?= htmlspecialchars($u['Type']) ?></option>
                    <?php endforeach; ?>
                </select>
            </div>
            <?php endif; ?>
            
            <div class="form-group">
                <label>Teknisi *</label>
                <select name="teknisi_id" required>
                    <option value="">Pilih Teknisi</option>
                    <?php foreach ($teknisi as $t): ?>
                    <option value="<?= $t['Id'] ?>"><?= htmlspecialchars($t['Name']) ?></option>
                    <?php endforeach; ?>
                </select>
            </div>
            <div class="form-group">
                <label>Tanggal Maintenance *</label>
                <input type="date" name="maintenance_date" required value="<?= date('Y-m-d') ?>">
            </div>
            <div class="form-group">
                <label>Jenis Maintenance *</label>
                <select name="maintenance_type" required>
                    <option value="">Pilih Jenis</option>
                    <option value="preventive">Preventive</option>
                    <option value="corrective">Corrective</option>
                    <option value="predictive">Predictive</option>
                    <option value="emergency">Emergency</option>
                    <option value="overhaul">Overhaul</option>
                </select>
            </div>
            <div class="form-group">
                <label>Level Maintenance *</label>
                <select name="maintenance_level" required>
                    <option value="">Pilih Level</option>
                    <option value="ringan">Ringan</option>
                    <option value="sedang">Sedang</option>
                    <option value="berat">Berat</option>
                    <option value="overhaul_total">Overhaul Total</option>
                </select>
            </div>
            <div class="form-group">
                <label>Kerusakan Ditemukan</label>
                <textarea name="damage_founds" placeholder="Deskripsikan kerusakan yang ditemukan"></textarea>
            </div>
            <div class="form-group">
                <label>Suku Cadang Diganti</label>
                <textarea name="parts_replaced" placeholder="Suku cadang yang diganti"></textarea>
            </div>
            <div class="form-group">
                <label>Biaya (Rp)</label>
                <input type="text" name="biaya" placeholder="Contoh: 2500000">
            </div>
            <div class="form-group">
                <label>Catatan</label>
                <textarea name="notes" placeholder="Catatan tambahan"></textarea>
            </div>
            <div class="form-group">
                <label>Status Hasil *</label>
                <select name="status_result" required>
                    <option value="">Pilih Status</option>
                    <option value="success">Selesai - Berhasil</option>
                    <option value="partial">Selesai - Sebagian</option>
                    <option value="failed">Gagal</option>
                    <option value="pending">Pending</option>
                    <option value="in_progress">Dalam Proses</option>
                </select>
            </div>
            <button type="submit" class="btn">💾 Simpan Maintenance</button>
        </form>
    </div>
</div>
<?php renderFooter();
}

// ============================================
// VIEW - USERS (Admin only)
// ============================================
function renderUsers() {
    if (!hasRole('admin')) { redirect('?page=dashboard'); }
    renderHeader('Manajemen User');
    $users = getAllUsers();
    ?>
<div class="container">
    <div class="page-header">
        <h2>👥 Manajemen User</h2>
        <div class="actions">
            <a href="?page=register" class="btn w-auto" style="padding:10px 25px;width:auto;">+ Tambah User</a>
        </div>
    </div>

    <?php if (isset($_SESSION['message'])): ?>
    <div class="alert alert-<?= $_SESSION['message_type'] ?? 'success' ?>"><?= $_SESSION['message'] ?></div>
    <?php unset($_SESSION['message'], $_SESSION['message_type']); ?>
    <?php endif; ?>

    <div class="detail-card">
        <table class="users-table">
            <thead>
                <tr>
                    <th>ID</th>
                    <th>Nama</th>
                    <th>Email</th>
                    <th>Role</th>
                    <th>Departemen</th>
                    <th>Status</th>
                    <th>Aksi</th>
                </tr>
            </thead>
            <tbody>
                <?php foreach ($users as $user): ?>
                <tr>
                    <td><?= $user['Id'] ?></td>
                    <td><?= htmlspecialchars($user['Name']) ?></td>
                    <td><?= htmlspecialchars($user['Email']) ?></td>
                    <td><span style="color:var(--gold);"><?= ucfirst($user['Role']) ?></span></td>
                    <td><?= htmlspecialchars($user['Departemen'] ?? '-') ?></td>
                    <td>
                        <span style="color:<?= $user['Is_active'] ? '#44ff44' : '#ff4444' ?>">
                            <?= $user['Is_active'] ? 'Aktif' : 'Nonaktif' ?>
                        </span>
                    </td>
                    <td>
                        <?php if ($user['Id'] != $_SESSION['user_id']): ?>
                        <a href="javascript:void(0)" onclick="confirmDelete('?page=delete_user&id=<?= $user['Id'] ?>', '<?= addslashes($user['Name']) ?>', 'user')" style="color:#ff4444;text-decoration:none;">🗑️ Hapus</a>
                        <?php else: ?>
                        <span style="color:var(--gray);">Akun sendiri</span>
                        <?php endif; ?>
                    </td>
                </tr>
                <?php endforeach; ?>
                <?php if (empty($users)): ?>
                <tr><td colspan="7" class="text-center text-gray" style="padding:20px;">Belum ada user</td></tr>
                <?php endif; ?>
            </tbody>
        </table>
    </div>
</div>
<?php renderFooter();
}

// ============================================
// ROUTING EXECUTION
// ============================================
$page = isset($_GET['page']) ? $_GET['page'] : 'home';

// Auth check untuk halaman yang membutuhkan login
$public_pages = ['login', 'register'];
if (!in_array($page, $public_pages) && !isLoggedIn()) {
    redirect('?page=login');
}

// Routing
switch ($page) {
    case 'login':
        renderLogin($error ?? '');
        break;
        
    case 'register':
        renderRegister($error ?? '', $success ?? '');
        break;
        
    case 'logout':
        // Sudah ditangani di atas
        break;
        
    case 'dashboard':
        renderDashboard();
        break;
        
    case 'units':
        renderUnits();
        break;
        
    case 'detail':
        renderDetail($id ?? 0);
        break;
        
    case 'add_unit':
        renderAddUnit($error ?? '', $success ?? '');
        break;
        
    case 'edit_unit':
        renderEditUnit($id ?? 0, $error ?? '', $success ?? '');
        break;
        
    case 'add_maintenance':
        $unit_id = isset($_GET['id']) ? intval($_GET['id']) : 0;
        renderAddMaintenance($unit_id, $error ?? '', $success ?? '');
        break;
        
    case 'users':
        renderUsers();
        break;
        
    case 'delete_unit':
    case 'delete_maintenance':
    case 'delete_user':
    case 'delete_all_maintenance':
        // Sudah ditangani di atas
        break;
        
    default:
        if (isLoggedIn()) {
            renderDashboard();
        } else {
            renderLogin('');
        }
        break;
}
