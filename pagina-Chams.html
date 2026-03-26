<?php
session_start();
date_default_timezone_set('America/Managua');
$page = isset($_GET['page']) ? $_GET['page'] : 'dashboard';
// Configuración de base de datos SQLite (todo en un archivo)
$db_file = 'database.sqlite';

// Crear conexión
try {
    $pdo = new PDO('sqlite:' . $db_file);
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
    
    // Crear tablas si no existen
    $pdo->exec("
        CREATE TABLE IF NOT EXISTS users (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            username TEXT UNIQUE NOT NULL,
            email TEXT UNIQUE NOT NULL,
            password TEXT NOT NULL,
            balance REAL DEFAULT 0,
            is_banned INTEGER DEFAULT 0,
            is_admin INTEGER DEFAULT 0,
            created_at DATETIME DEFAULT CURRENT_TIMESTAMP
        );
        
        CREATE TABLE IF NOT EXISTS products (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            name TEXT NOT NULL,
            description TEXT,
            image_url TEXT,
            price_1d REAL DEFAULT 0,
            price_7d REAL DEFAULT 0,
            price_15d REAL DEFAULT 0,
            price_30d REAL DEFAULT 0,
            active INTEGER DEFAULT 1
        );
        
        CREATE TABLE IF NOT EXISTS keys_stock (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            product_id INTEGER,
            key_value TEXT NOT NULL,
            duration_days INTEGER NOT NULL,
            is_sold INTEGER DEFAULT 0,
            sold_to INTEGER,
            sold_at DATETIME,
            created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
            FOREIGN KEY (product_id) REFERENCES products(id)
        );
        
        CREATE TABLE IF NOT EXISTS purchases (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            user_id INTEGER,
            product_id INTEGER,
            key_id INTEGER,
            price_paid REAL,
            purchased_at DATETIME DEFAULT CURRENT_TIMESTAMP,
            FOREIGN KEY (user_id) REFERENCES users(id),
            FOREIGN KEY (product_id) REFERENCES products(id),
            FOREIGN KEY (key_id) REFERENCES keys_stock(id)
        );
    ");
    
    // Insertar admin por defecto si no existe
    $stmt = $pdo->prepare("SELECT id FROM users WHERE username = ?");
    $stmt->execute(['lito']);
    if (!$stmt->fetch()) {
        $stmt = $pdo->prepare("INSERT INTO users (username, email, password, is_admin, balance) VALUES (?, ?, ?, 1, 999999)");
        $stmt->execute(['lito', 'admin@admin.com', password_hash('07', PASSWORD_DEFAULT)]);
    }
    
    // Insertar productos por defecto si no existen
    $stmt = $pdo->query("SELECT COUNT(*) FROM products");
    if ($stmt->fetchColumn() == 0) {
        $products = [
            ['Pato Team', 'Herramienta premium para gaming', 'https://via.placeholder.com/400x300/FF6B6B/FFFFFF?text=Pato+Team', 3.5, 4.5, 8.5, 10],
            ['HG Cheast', 'Mejora tu experiencia de juego', 'https://via.placeholder.com/400x300/4ECDC4/FFFFFF?text=HG+Cheast', 3.5, 4.5, 8.5, 10],
            ['Drip Client', 'Cliente modificado exclusivo', 'https://via.placeholder.com/400x300/45B7D1/FFFFFF?text=Drip+Client', 3.5, 4.5, 8.5, 10],
            ['Flourite (sin GBox)', 'Versión estándar de Flourite', 'https://via.placeholder.com/400x300/96CEB4/FFFFFF?text=Flourite', 3.5, 4.5, 8.5, 10],
            ['Flourite (con GBox)', 'Versión completa con GBox', 'https://via.placeholder.com/400x300/FFEAA7/FFFFFF?text=Flourite+GBox', 3.5, 4.5, 8.5, 10],
            ['Cuban Modz Store', 'Tienda de modificaciones', 'https://via.placeholder.com/400x300/DDA0DD/FFFFFF?text=Cuban+Modz', 3.5, 4.5, 8.5, 10],
            ['Cuban Rage PC', 'Rage para PC exclusivo', 'https://via.placeholder.com/400x300/FF7675/FFFFFF?text=Cuban+Rage', 3.5, 4.5, 8.5, 10],
            ['BR Mods PC', 'Modificaciones BR para PC', 'https://via.placeholder.com/400x300/74B9FF/FFFFFF?text=BR+Mods', 3.5, 4.5, 8.5, 10],
            ['Strick BR', 'Herramienta Strick exclusiva', 'https://via.placeholder.com/400x300/A29BFE/FFFFFF?text=Strick+BR', 3.5, 4.5, 8.5, 10],
            ['Cuban 8BP', 'Especial para 8 Ball Pool', 'https://via.placeholder.com/400x300/FD79A8/FFFFFF?text=Cuban+8BP', 3.5, 4.5, 8.5, 10],
            ['Seguidores TikTok (1,000)', '1,000 seguidores reales', 'https://via.placeholder.com/400x300/00B894/FFFFFF?text=TikTok+1K', 5, 5, 5, 5],
            ['Likes All Redes', 'Likes para todas las redes', 'https://via.placeholder.com/400x300/E17055/FFFFFF?text=Likes', 5, 5, 5, 5],
            ['Holograma VIP', 'Servicio VIP Holograma + Seguidores + Likes', 'https://via.placeholder.com/400x300/6C5CE7/FFFFFF?text=Holograma+VIP', 5, 5, 5, 5]
        ];
        
        $stmt = $pdo->prepare("INSERT INTO products (name, description, image_url, price_1d, price_7d, price_15d, price_30d) VALUES (?, ?, ?, ?, ?, ?, ?)");
        foreach ($products as $p) {
            $stmt->execute($p);
        }
    }
} catch (PDOException $e) {
    die("Error de base de datos: " . $e->getMessage());
}

// Funciones auxiliares
function isMobile() {
    return preg_match('/Mobile|Android|iPhone|iPad|iPod/i', $_SERVER['HTTP_USER_AGENT']);
}

function redirect($url) {
    header("Location: $url");
    exit;
}

function csrf_token() {
    if (!isset($_SESSION['csrf_token'])) {
        $_SESSION['csrf_token'] = bin2hex(random_bytes(32));
    }
    return $_SESSION['csrf_token'];
}

// Manejar acciones POST
$error = '';
$success = '';

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $action = $_POST['action'] ?? '';
    
    switch ($action) {
        case 'register':
            $username = trim($_POST['username'] ?? '');
            $email = trim($_POST['email'] ?? '');
            $password = $_POST['password'] ?? '';
            
            if (strlen($username) < 3 || strlen($password) < 6) {
                $error = 'Usuario mínimo 3 caracteres, contraseña 6';
            } else {
                try {
                    $stmt = $pdo->prepare("INSERT INTO users (username, email, password) VALUES (?, ?, ?)");
                    $stmt->execute([$username, $email, password_hash($password, PASSWORD_DEFAULT)]);
                    redirect("?page=login&registro=exitoso");
                } catch (PDOException $e) {
                    $error = 'Usuario o email ya existe';
                }
            }
            break;
            
        case 'login':
            $username = $_POST['username'] ?? '';
            $password = $_POST['password'] ?? '';
            
            $stmt = $pdo->prepare("SELECT * FROM users WHERE username = ?");
            $stmt->execute([$username]);
            $user = $stmt->fetch();
            
            if ($user && password_verify($password, $user['password'])) {
                if ($user['is_banned']) {
                    $error = 'Tu cuenta ha sido suspendida';
                } else {
                    $_SESSION['user_id'] = $user['id'];
                    $_SESSION['is_admin'] = $user['is_admin'];
                    redirect('?page=dashboard');
                }
            } else {
                $error = 'Credenciales incorrectas';
            }
            break;
            
        case 'logout':
            session_destroy();
            redirect('/');
            break;
            
        case 'add_balance':
            if (!$_SESSION['is_admin']) break;
            $user_id = $_POST['user_id'];
            $amount = floatval($_POST['amount']);
            $pdo->prepare("UPDATE users SET balance = balance + ? WHERE id = ?")->execute([$amount, $user_id]);
            $success = "Saldo agregado: $$amount";
            break;
            
        case 'add_key':
            if (!$_SESSION['is_admin']) break;
            $product_id = $_POST['product_id'];
            $key_value = trim($_POST['key_value']);
            $duration = intval($_POST['duration_days']);
            $pdo->prepare("INSERT INTO keys_stock (product_id, key_value, duration_days) VALUES (?, ?, ?)")
                ->execute([$product_id, $key_value, $duration]);
            $success = 'Key agregada al stock';
            break;
            
        case 'ban_user':
            if (!$_SESSION['is_admin']) break;
            $user_id = $_POST['user_id'];
            $status = $_POST['ban_status'] == '1' ? 1 : 0;
            $pdo->prepare("UPDATE users SET is_banned = ? WHERE id = ?")->execute([$status, $user_id]);
            $success = $status ? 'Usuario baneado' : 'Usuario desbaneado';
            break;
            
        case 'buy_key':
            $user_id = $_SESSION['user_id'];
            $product_id = $_POST['product_id'];
            $duration = intval($_POST['duration']);
            
            // Obtener precio según duración
            $stmt = $pdo->prepare("SELECT * FROM products WHERE id = ?");
            $stmt->execute([$product_id]);
            $product = $stmt->fetch();
            
            $price = match($duration) {
                1 => $product['price_1d'],
                7 => $product['price_7d'],
                15 => $product['price_15d'],
                30 => $product['price_30d'],
                default => 0
            };
            
            // Verificar saldo
            $stmt = $pdo->prepare("SELECT balance FROM users WHERE id = ?");
            $stmt->execute([$user_id]);
            $balance = $stmt->fetchColumn();
            
            if ($balance < $price) {
                $error = 'Saldo insuficiente';
            } else {
                // Buscar key disponible
                $stmt = $pdo->prepare("SELECT * FROM keys_stock WHERE product_id = ? AND duration_days = ? AND is_sold = 0 LIMIT 1");
                $stmt->execute([$product_id, $duration]);
                $key = $stmt->fetch();
                
                if (!$key) {
                    $error = 'No hay keys disponibles para esta duración';
                } else {
                    // Realizar compra
                    $pdo->beginTransaction();
                    $pdo->prepare("UPDATE users SET balance = balance - ? WHERE id = ?")->execute([$price, $user_id]);
                    $pdo->prepare("UPDATE keys_stock SET is_sold = 1, sold_to = ?, sold_at = CURRENT_TIMESTAMP WHERE id = ?")
                        ->execute([$user_id, $key['id']]);
                    $pdo->prepare("INSERT INTO purchases (user_id, product_id, key_id, price_paid) VALUES (?, ?, ?, ?)")
                        ->execute([$user_id, $product_id, $key['id'], $price]);
                    $pdo->commit();
                    
                    $_SESSION['last_purchase'] = $key['key_value'];
                    $success = '¡Compra exitosa! Tu key está lista';
                }
            }
            break;
    }
}

// Determinar página actual
$page = $_GET['page'] ?? 'home';
if (isset($_SESSION['user_id'])) {
    if ($_SESSION['is_admin'] && $page === 'admin') {
        // Admin panel
    } elseif ($page === 'dashboard') {
        // User dashboard
    } else {
        $page = $_SESSION['is_admin'] ? 'admin' : 'dashboard';
    }
} else {
    if (!in_array($page, ['home', 'login', 'register'])) {
        $page = 'home';
    }
}

// Obtener datos para el dashboard
$user = null;
$products = [];
$my_keys = [];
$all_users = [];
$all_keys = [];

if (isset($_SESSION['user_id'])) {
    $stmt = $pdo->prepare("SELECT * FROM users WHERE id = ?");
    $stmt->execute([$_SESSION['user_id']]);
    $user = $stmt->fetch();
    
    if ($_SESSION['is_admin']) {
        $all_users = $pdo->query("SELECT * FROM users ORDER BY created_at DESC")->fetchAll();
        $all_keys = $pdo->query("
            SELECT k.*, p.name as product_name, u.username as buyer_name 
            FROM keys_stock k 
            LEFT JOIN products p ON k.product_id = p.id 
            LEFT JOIN users u ON k.sold_to = u.id 
            ORDER BY k.created_at DESC
        ")->fetchAll();
    } else {
        $my_keys = $pdo->prepare("
            SELECT k.key_value, k.duration_days, p.name as product_name, pur.purchased_at 
            FROM purchases pur
            JOIN keys_stock k ON pur.key_id = k.id
            JOIN products p ON pur.product_id = p.id
            WHERE pur.user_id = ?
            ORDER BY pur.purchased_at DESC
        ");
        $my_keys->execute([$_SESSION['user_id']]);
        $my_keys = $my_keys->fetchAll();
    }
    
    $products = $pdo->query("SELECT * FROM products WHERE active = 1")->fetchAll();
}
?>
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>🔥 KeyStore Chams - Panel Premium</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700;900&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root {
            --primary: #6C5CE7;
            --secondary: #00CEC9;
            --accent: #FD79A8;
            --dark: #2D3436;
            --darker: #1E272E;
            --glass: rgba(255,255,255,0.05);
            --gradient-1: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            --gradient-2: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
            --gradient-3: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Poppins', sans-serif;
            -webkit-tap-highlight-color: transparent;
        }
        
        body {
            background: var(--darker);
            color: white;
            overflow-x: hidden;
            min-height: 100vh;
        }
        
        <?php if (!isMobile() && $page !== 'home'): ?>
        /* Anti-PC: Solo móvil permitido para funcionalidad */
        .mobile-only-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: var(--darker);
            z-index: 999999;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 2rem;
            text-align: center;
        }
        .mobile-only-overlay i {
            font-size: 4rem;
            color: var(--accent);
            margin-bottom: 1rem;
            animation: pulse 2s infinite;
        }
        .mobile-only-overlay h1 {
            font-size: 1.5rem;
            margin-bottom: 1rem;
            background: var(--gradient-2);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        <?php endif; ?>
        
        /* Anti-screenshot */
        .no-screenshot {
            position: relative;
        }
        .no-screenshot::after {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: repeating-linear-gradient(
                0deg,
                transparent,
                transparent 2px,
                rgba(0,0,0,0.03) 2px,
                rgba(0,0,0,0.03) 4px
            );
            pointer-events: none;
            animation: scan 8s linear infinite;
        }
        
        @keyframes scan {
            0% { transform: translateY(-100%); }
            100% { transform: translateY(100%); }
        }
        
        /* Animaciones */
        @keyframes fadeInUp {
            from { opacity: 0; transform: translateY(30px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        @keyframes float {
            0%, 100% { transform: translateY(0px); }
            50% { transform: translateY(-20px); }
        }
        
        @keyframes glow {
            0%, 100% { box-shadow: 0 0 20px rgba(108, 92, 231, 0.5); }
            50% { box-shadow: 0 0 40px rgba(108, 92, 231, 0.8), 0 0 60px rgba(0, 206, 201, 0.4); }
        }
        
        @keyframes pulse {
            0%, 100% { transform: scale(1); opacity: 1; }
            50% { transform: scale(1.1); opacity: 0.8; }
        }
        
        @keyframes slideIn {
            from { transform: translateX(-100%); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }
        
        @keyframes rotate {
            from { transform: rotate(0deg); }
            to { transform: rotate(360deg); }
        }
        
        .animate-fade-in { animation: fadeInUp 0.6s ease-out; }
        .animate-float { animation: float 6s ease-in-out infinite; }
        .animate-glow { animation: glow 3s ease-in-out infinite; }
        .animate-slide-in { animation: slideIn 0.5s ease-out; }
        
        /* Fondo animado */
        .bg-animation {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
            overflow: hidden;
        }
        
        .bg-animation::before {
            content: '';
            position: absolute;
            width: 150%;
            height: 150%;
            background: radial-gradient(circle, rgba(108,92,231,0.15) 0%, transparent 70%);
            top: -50%;
            left: -50%;
            animation: rotate 30s linear infinite;
        }
        
        .particles {
            position: absolute;
            width: 100%;
            height: 100%;
        }
        
        .particle {
            position: absolute;
            width: 4px;
            height: 4px;
            background: var(--secondary);
            border-radius: 50%;
            opacity: 0.5;
            animation: float 10s infinite;
        }
        
        /* Navegación */
        .navbar {
            background: rgba(30, 39, 46, 0.95);
            backdrop-filter: blur(20px);
            padding: 1rem 2rem;
            position: fixed;
            width: 100%;
            top: 0;
            z-index: 1000;
            border-bottom: 1px solid rgba(255,255,255,0.1);
            transition: all 0.3s;
        }
        
        .navbar-content {
            max-width: 1400px;
            margin: 0 auto;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .logo {
            font-size: 1.5rem;
            font-weight: 900;
            background: var(--gradient-1);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }
        
        .nav-links {
            display: flex;
            gap: 2rem;
            align-items: center;
        }
        
        .nav-links a {
            color: white;
            text-decoration: none;
            font-weight: 500;
            transition: all 0.3s;
            position: relative;
        }
        
        .nav-links a::after {
            content: '';
            position: absolute;
            bottom: -5px;
            left: 0;
            width: 0;
            height: 2px;
            background: var(--secondary);
            transition: width 0.3s;
        }
        
        .nav-links a:hover::after {
            width: 100%;
        }
        
        .balance-badge {
            background: var(--gradient-3);
            padding: 0.5rem 1rem;
            border-radius: 50px;
            font-weight: 700;
            box-shadow: 0 4px 15px rgba(79, 172, 254, 0.4);
        }
        
        /* Contenedor principal */
        .container {
            max-width: 1400px;
            margin: 0 auto;
            padding: 6rem 2rem 2rem;
        }
        
        /* Hero Section */
        .hero {
            min-height: 90vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            text-align: center;
            padding: 2rem;
        }
        
        .hero h1 {
            font-size: clamp(2.5rem, 8vw, 5rem);
            font-weight: 900;
            margin-bottom: 1rem;
            background: linear-gradient(135deg, #fff 0%, var(--secondary) 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            animation: fadeInUp 1s ease-out;
        }
        
        .hero p {
            font-size: 1.2rem;
            color: rgba(255,255,255,0.7);
            margin-bottom: 2rem;
            max-width: 600px;
            animation: fadeInUp 1s ease-out 0.2s both;
        }
        
        .btn {
            display: inline-flex;
            align-items: center;
            gap: 0.5rem;
            padding: 1rem 2rem;
            border-radius: 50px;
            font-weight: 700;
            text-decoration: none;
            border: none;
            cursor: pointer;
            transition: all 0.3s;
            font-size: 1rem;
            position: relative;
            overflow: hidden;
        }
        
        .btn-primary {
            background: var(--gradient-1);
            color: white;
            box-shadow: 0 10px 30px rgba(102, 126, 234, 0.4);
        }
        
        .btn-secondary {
            background: transparent;
            color: white;
            border: 2px solid rgba(255,255,255,0.3);
        }
        
        .btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 20px 40px rgba(0,0,0,0.3);
        }
        
        .btn-primary::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.4), transparent);
            transition: left 0.5s;
        }
        
        .btn-primary:hover::before {
            left: 100%;
        }
        
        .hero-buttons {
            display: flex;
            gap: 1rem;
            flex-wrap: wrap;
            justify-content: center;
            animation: fadeInUp 1s ease-out 0.4s both;
        }
        
        /* Tarjetas de productos */
        .section-title {
            font-size: 2rem;
            margin-bottom: 2rem;
            text-align: center;
            position: relative;
            display: inline-block;
            left: 50%;
            transform: translateX(-50%);
        }
        
        .section-title::after {
            content: '';
            position: absolute;
            bottom: -10px;
            left: 50%;
            transform: translateX(-50%);
            width: 60px;
            height: 4px;
            background: var(--gradient-2);
            border-radius: 2px;
        }
        
        .products-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
            gap: 2rem;
            margin-top: 3rem;
        }
        
        .product-card {
            background: var(--glass);
            backdrop-filter: blur(20px);
            border-radius: 20px;
            overflow: hidden;
            border: 1px solid rgba(255,255,255,0.1);
            transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            position: relative;
        }
        
        .product-card:hover {
            transform: translateY(-10px) scale(1.02);
            box-shadow: 0 30px 60px rgba(0,0,0,0.4);
            border-color: rgba(108, 92, 231, 0.5);
        }
        
        .product-image {
            width: 100%;
            height: 200px;
            object-fit: cover;
            position: relative;
        }
        
        .product-image img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
        
        .product-badge {
            position: absolute;
            top: 1rem;
            right: 1rem;
            background: var(--gradient-2);
            padding: 0.3rem 0.8rem;
            border-radius: 20px;
            font-size: 0.8rem;
            font-weight: 700;
        }
        
        .product-info {
            padding: 1.5rem;
        }
        
        .product-name {
            font-size: 1.3rem;
            font-weight: 700;
            margin-bottom: 0.5rem;
        }
        
        .product-desc {
            color: rgba(255,255,255,0.6);
            font-size: 0.9rem;
            margin-bottom: 1rem;
        }
        
        .price-selector {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 0.5rem;
            margin-bottom: 1rem;
        }
        
        .price-option {
            background: rgba(255,255,255,0.05);
            border: 2px solid transparent;
            padding: 0.8rem;
            border-radius: 12px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s;
        }
        
        .price-option:hover, .price-option.selected {
            border-color: var(--secondary);
            background: rgba(0, 206, 201, 0.1);
        }
        
        .price-option .days {
            font-size: 0.8rem;
            color: rgba(255,255,255,0.6);
        }
        
        .price-option .price {
            font-size: 1.1rem;
            font-weight: 700;
            color: var(--secondary);
        }
        
        .buy-btn {
            width: 100%;
            padding: 1rem;
            background: var(--gradient-1);
            border: none;
            border-radius: 12px;
            color: white;
            font-weight: 700;
            cursor: pointer;
            transition: all 0.3s;
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 0.5rem;
        }
        
        .buy-btn:hover {
            transform: scale(1.05);
            box-shadow: 0 10px 30px rgba(102, 126, 234, 0.4);
        }
        
        .buy-btn:disabled {
            background: #636e72;
            cursor: not-allowed;
            transform: none;
        }
        
        /* Modal de compra */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.9);
            z-index: 2000;
            justify-content: center;
            align-items: center;
            padding: 2rem;
            backdrop-filter: blur(10px);
        }
        
        .modal.active {
            display: flex;
            animation: fadeInUp 0.3s ease-out;
        }
        
        .modal-content {
            background: var(--darker);
            border: 1px solid rgba(255,255,255,0.1);
            border-radius: 20px;
            padding: 2rem;
            max-width: 500px;
            width: 100%;
            text-align: center;
            position: relative;
            box-shadow: 0 25px 50px rgba(0,0,0,0.5);
        }
        
        .modal-close {
            position: absolute;
            top: 1rem;
            right: 1rem;
            background: none;
            border: none;
            color: white;
            font-size: 1.5rem;
            cursor: pointer;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s;
        }
        
        .modal-close:hover {
            background: rgba(255,255,255,0.1);
        }
        
        .key-display {
            background: rgba(0,0,0,0.5);
            border: 2px dashed var(--secondary);
            padding: 1.5rem;
            border-radius: 12px;
            margin: 1.5rem 0;
            font-family: 'Courier New', monospace;
            font-size: 1.1rem;
            word-break: break-all;
            position: relative;
            user-select: all;
        }
        
        .copy-btn {
            background: var(--secondary);
            color: var(--dark);
            border: none;
            padding: 0.8rem 1.5rem;
            border-radius: 8px;
            font-weight: 700;
            cursor: pointer;
            transition: all 0.3s;
            display: inline-flex;
            align-items: center;
            gap: 0.5rem;
        }
        
        .copy-btn:hover {
            transform: scale(1.05);
            box-shadow: 0 5px 20px rgba(0, 206, 201, 0.4);
        }
        
        .copy-btn.copied {
            background: #00b894;
            color: white;
        }
        
        /* Panel Admin */
        .admin-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
            gap: 2rem;
            margin-top: 2rem;
        }
        
        .admin-card {
            background: var(--glass);
            backdrop-filter: blur(20px);
            border-radius: 20px;
            padding: 2rem;
            border: 1px solid rgba(255,255,255,0.1);
        }
        
        .admin-card h3 {
            margin-bottom: 1.5rem;
            color: var(--secondary);
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }
        
        .form-group {
            margin-bottom: 1.5rem;
        }
        
        .form-group label {
            display: block;
            margin-bottom: 0.5rem;
            font-weight: 500;
            color: rgba(255,255,255,0.8);
        }
        
        .form-control {
            width: 100%;
            padding: 1rem;
            background: rgba(255,255,255,0.05);
            border: 2px solid rgba(255,255,255,0.1);
            border-radius: 12px;
            color: white;
            font-size: 1rem;
            transition: all 0.3s;
        }
        
        .form-control:focus {
            outline: none;
            border-color: var(--secondary);
            background: rgba(255,255,255,0.08);
        }
        
        select.form-control option {
            background: var(--dark);
        }
        
        .table-container {
            overflow-x: auto;
            margin-top: 1rem;
        }
        
        table {
            width: 100%;
            border-collapse: collapse;
        }
        
        th, td {
            padding: 1rem;
            text-align: left;
            border-bottom: 1px solid rgba(255,255,255,0.1);
        }
        
        th {
            color: var(--secondary);
            font-weight: 600;
        }
        
        tr:hover {
            background: rgba(255,255,255,0.03);
        }
        
        .status-badge {
            padding: 0.3rem 0.8rem;
            border-radius: 20px;
            font-size: 0.8rem;
            font-weight: 600;
        }
        
        .status-active {
            background: rgba(0, 184, 148, 0.2);
            color: #00b894;
        }
        
        .status-banned {
            background: rgba(255, 118, 117, 0.2);
            color: #ff7675;
        }
        
        .action-btns {
            display: flex;
            gap: 0.5rem;
        }
        
        .btn-small {
            padding: 0.4rem 0.8rem;
            border-radius: 6px;
            font-size: 0.8rem;
            border: none;
            cursor: pointer;
            transition: all 0.3s;
        }
        
        .btn-ban {
            background: #ff7675;
            color: white;
        }
        
        .btn-unban {
            background: #00b894;
            color: white;
        }
        
        /* Login/Register Forms */
        .auth-container {
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 2rem;
        }
        
        .auth-box {
            background: var(--glass);
            backdrop-filter: blur(20px);
            border-radius: 30px;
            padding: 3rem;
            width: 100%;
            max-width: 450px;
            border: 1px solid rgba(255,255,255,0.1);
            box-shadow: 0 25px 50px rgba(0,0,0,0.5);
        }
        
        .auth-box h2 {
            text-align: center;
            margin-bottom: 2rem;
            font-size: 2rem;
        }
        
        .auth-links {
            text-align: center;
            margin-top: 1.5rem;
            color: rgba(255,255,255,0.6);
        }
        
        .auth-links a {
            color: var(--secondary);
            text-decoration: none;
            font-weight: 600;
        }
        
        /* Alertas */
        .alert {
            padding: 1rem;
            border-radius: 12px;
            margin-bottom: 1rem;
            display: flex;
            align-items: center;
            gap: 0.5rem;
            animation: slideIn 0.3s ease-out;
        }
        
        .alert-error {
            background: rgba(255, 118, 117, 0.2);
            border: 1px solid #ff7675;
            color: #ff7675;
        }
        
        .alert-success {
            background: rgba(0, 184, 148, 0.2);
            border: 1px solid #00b894;
            color: #00b894;
        }
        
        /* Mis Keys */
        .keys-list {
            display: flex;
            flex-direction: column;
            gap: 1rem;
            margin-top: 2rem;
        }
        
        .key-item {
            background: var(--glass);
            border-radius: 15px;
            padding: 1.5rem;
            border: 1px solid rgba(255,255,255,0.1);
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 1rem;
            transition: all 0.3s;
        }
        
        .key-item:hover {
            border-color: var(--secondary);
            transform: translateX(10px);
        }
        
        .key-info h4 {
            color: var(--secondary);
            margin-bottom: 0.3rem;
        }
        
        .key-info p {
            font-size: 0.9rem;
            color: rgba(255,255,255,0.6);
        }
        
        .key-value-box {
            background: rgba(0,0,0,0.3);
            padding: 0.8rem 1.2rem;
            border-radius: 8px;
            font-family: 'Courier New', monospace;
            font-size: 0.9rem;
            border: 1px dashed rgba(255,255,255,0.2);
        }
        
        /* WhatsApp Float */
        .whatsapp-float {
            position: fixed;
            bottom: 2rem;
            right: 2rem;
            background: #25D366;
            color: white;
            width: 60px;
            height: 60px;
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 2rem;
            box-shadow: 0 10px 30px rgba(37, 211, 102, 0.4);
            cursor: pointer;
            transition: all 0.3s;
            z-index: 999;
            text-decoration: none;
        }
        
        .whatsapp-float:hover {
            transform: scale(1.1) rotate(10deg);
            box-shadow: 0 15px 40px rgba(37, 211, 102, 0.6);
        }
        
        .whatsapp-tooltip {
            position: absolute;
            right: 70px;
            background: white;
            color: #333;
            padding: 0.5rem 1rem;
            border-radius: 8px;
            font-size: 0.9rem;
            white-space: nowrap;
            opacity: 0;
            pointer-events: none;
            transition: all 0.3s;
            font-weight: 600;
        }
        
        .whatsapp-float:hover .whatsapp-tooltip {
            opacity: 1;
            transform: translateX(-10px);
        }
        
        /* Responsive */
        @media (max-width: 768px) {
            .navbar {
                padding: 1rem;
            }
            
            .nav-links {
                gap: 1rem;
                font-size: 0.9rem;
            }
            
            .container {
                padding: 5rem 1rem 1rem;
            }
            
            .products-grid {
                grid-template-columns: 1fr;
            }
            
            .admin-grid {
                grid-template-columns: 1fr;
            }
            
            .auth-box {
                padding: 2rem;
            }
        }
        
        /* Loading Animation */
        .loader {
            display: inline-block;
            width: 20px;
            height: 20px;
            border: 3px solid rgba(255,255,255,.3);
            border-radius: 50%;
            border-top-color: white;
            animation: rotate 1s ease-in-out infinite;
        }
        
        /* Efecto de brillo */
        .shine {
            position: relative;
            overflow: hidden;
        }
        
        .shine::after {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: linear-gradient(
                to right,
                transparent 0%,
                rgba(255,255,255,0.1) 50%,
                transparent 100%
            );
            transform: rotate(30deg);
            transition: all 0.6s;
        }
        
        .shine:hover::after {
            left: 100%;
        }
    </style>
</head>
<body>
    <div class="bg-animation">
        <div class="particles" id="particles"></div>
    </div>
    
    <?php if (!isMobile() && $page !== 'home'): ?>
    <div class="mobile-only-overlay">
        <i class="fas fa-mobile-alt"></i>
        <h1>📱 Acceso Exclusivo para Móviles</h1>
        <p>Esta sección solo está disponible en dispositivos móviles por seguridad. Por favor, accede desde tu teléfono.</p>
        <a href="?page=home" class="btn btn-primary" style="margin-top: 2rem;">
            <i class="fas fa-home"></i> Volver al Inicio
        </a>
    </div>
    <?php endif; ?>
    
    <?php if ($page !== 'home' && $page !== 'login' && $page !== 'register'): ?>
    <nav class="navbar">
        <div class="navbar-content">
            <div class="logo">
                <i class="fas fa-key"></i>
                KeyStore Pro
            </div>
            <div class="nav-links">
                <?php if ($_SESSION['is_admin']): ?>
                    <a href="?page=admin"><i class="fas fa-crown"></i> Admin</a>
                <?php else: ?>
                    <a href="?page=dashboard"><i class="fas fa-store"></i> Tienda</a>
                    <a href="?page=mykeys"><i class="fas fa-key"></i> Mis Keys</a>
                <?php endif; ?>
                <span class="balance-badge">
                    <i class="fas fa-wallet"></i> $<?php echo number_format($user['balance'], 2); ?>
                </span>
                <form method="POST" style="display: inline;">
                    <input type="hidden" name="action" value="logout">
                    <button type="submit" style="background: none; border: none; color: white; cursor: pointer;">
                        <i class="fas fa-sign-out-alt"></i>
                    </button>
                </form>
            </div>
        </div>
    </nav>
    <?php endif; ?>
    
    <?php if ($page === 'home'): ?>
    <section class="hero">
        <div class="animate-float" style="font-size: 5rem; margin-bottom: 1rem;">
            🔥
        </div>
        <h1>KeyStore Pro</h1>
        <p>Tu marketplace premium de keys y servicios digitales. Seguridad máxima, entrega instantánea.</p>
        <div class="hero-buttons">
            <a href="?page=login" class="btn btn-primary shine">
                <i class="fas fa-sign-in-alt"></i> Iniciar Sesión
            </a>
            <a href="?page=register" class="btn btn-secondary">
                <i class="fas fa-user-plus"></i> Crear Cuenta
            </a>
        </div>
        
        <div style="margin-top: 4rem; display: grid; grid-template-columns: repeat(3, 1fr); gap: 2rem; text-align: center;">
            <div class="animate-fade-in" style="background: var(--glass); padding: 2rem; border-radius: 20px;">
                <i class="fas fa-bolt" style="font-size: 2rem; color: var(--accent); margin-bottom: 1rem;"></i>
                <h3>Entrega Instantánea</h3>
                <p style="color: rgba(255,255,255,0.6); font-size: 0.9rem;">Obtén tu key inmediatamente después de comprar</p>
            </div>
            <div class="animate-fade-in" style="background: var(--glass); padding: 2rem; border-radius: 20px; animation-delay: 0.2s;">
                <i class="fas fa-shield-alt" style="font-size: 2rem; color: var(--secondary); margin-bottom: 1rem;"></i>
                <h3>100% Seguro</h3>
                <p style="color: rgba(255,255,255,0.6); font-size: 0.9rem;">Protección anti-screenshot y anti-fraudas</p>
            </div>
            <div class="animate-fade-in" style="background: var(--glass); padding: 2rem; border-radius: 20px; animation-delay: 0.4s;">
                <i class="fas fa-headset" style="font-size: 2rem; color: var(--primary); margin-bottom: 1rem;"></i>
                <h3>Soporte 24/7</h3>
                <p style="color: rgba(255,255,255,0.6); font-size: 0.9rem;">Contacto directo vía WhatsApp</p>
            </div>
        </div>
    </section>
    
    <?php elseif ($page === 'login'): ?>
    <div class="auth-container">
        <div class="auth-box animate-fade-in">
            <div style="text-align: center; margin-bottom: 2rem;">
                <i class="fas fa-key" style="font-size: 3rem; color: var(--primary);"></i>
            </div>
            <h2>Bienvenido de Vuelta</h2>
            
            <?php if ($error): ?>
                <div class="alert alert-error">
                    <i class="fas fa-exclamation-circle"></i> <?php echo $error; ?>
                </div>
            <?php endif; ?>
            
            <?php if ($success): ?>
                <div class="alert alert-success">
                    <i class="fas fa-check-circle"></i> <?php echo $success; ?>
                </div>
            <?php endif; ?>
            
            <?php if (isset($_GET['registro']) && $_GET['registro'] == 'exitoso'): ?>
    <div id="success-alert" style="background: rgba(0, 255, 204, 0.1); color: #00ffcc; padding: 12px; border-radius: 10px; border: 1px solid #00ffcc; text-align: center; margin-bottom: 20px; font-weight: bold; transition: opacity 1s ease;">
        <i class="fas fa-check-circle"></i> ¡Cuenta creada exitosamente! 🎉
    </div>

    <script>
        // Espera 4 segundos (4000ms) y luego desaparece
        setTimeout(function() {
            var alert = document.getElementById('success-alert');
            if (alert) {
                alert.style.opacity = '0'; // Empieza a desvanecerse
                setTimeout(function() {
                    alert.style.display = 'none'; // Se elimina por completo
                }, 1000); // Tiempo que tarda la animación de desvanecer
            }
        }, 4000); 
    </script>
<?php endif; ?>
            
            <form method="POST">
                <input type="hidden" name="action" value="login">
                <div class="form-group">
                    <label><i class="fas fa-user"></i> Usuario</label>
                    <input type="text" name="username" class="form-control" required placeholder="Tu usuario">
                </div>
                <div class="form-group">
                    <label><i class="fas fa-lock"></i> Contraseña</label>
                    <input type="password" name="password" class="form-control" required placeholder="••••••">
                </div>
                <button type="submit" class="btn btn-primary" style="width: 100%;">
                    <i class="fas fa-sign-in-alt"></i> Entrar
                </button>
            </form>
            <div class="auth-links">
                ¿No tienes cuenta? <a href="?page=register">Regístrate aquí</a>
            </div>
        </div>
    </div>
    
    <?php elseif ($page === 'register'): ?>
    <div class="auth-container">
        <div class="auth-box animate-fade-in">
            <div style="text-align: center; margin-bottom: 2rem;">
                <i class="fas fa-user-plus" style="font-size: 3rem; color: var(--secondary);"></i>
            </div>
            <h2>Crear Cuenta</h2>
            
            <?php if ($error): ?>
                <div class="alert alert-error">
                    <i class="fas fa-exclamation-circle"></i> <?php echo $error; ?>
                </div>
            <?php endif; ?>
       
            <form method="POST">
                <input type="hidden" name="action" value="register">
                <div class="form-group">
                    <label><i class="fas fa-user"></i> Usuario</label>
                    <input type="text" name="username" class="form-control" required placeholder="Elige un usuario">
                </div>
                <div class="form-group">
                    <label><i class="fas fa-envelope"></i> Email</label>
                    <input type="email" name="email" class="form-control" required placeholder="tu@email.com">
                </div>
                <div class="form-group">
                    <label><i class="fas fa-lock"></i> Contraseña</label>
                    <input type="password" name="password" class="form-control" required placeholder="Mínimo 6 caracteres">
                </div>
                <button type="submit" class="btn btn-primary" style="width: 100%;">
                    <i class="fas fa-user-check"></i> Crear Cuenta
                </button>
            </form>
            <div class="auth-links">
                ¿Ya tienes cuenta? <a href="?page=login">Inicia sesión</a>
            </div>
        </div>
    </div>
    
    <?php elseif ($page === 'dashboard'): ?>
    <div class="container no-screenshot">
        <h2 class="section-title animate-fade-in" style="margin-top: 60px; font-size: 1.8rem; text-shadow: 0 0 10px #00ffcc; white-space: nowrap;">🛒 Store Chams Ofc⚡️</h2>
        
        <?php if ($error): ?>
            <div class="alert alert-error">
                <i class="fas fa-exclamation-circle"></i> <?php echo $error; ?>
            </div>
        <?php endif; ?>
        
        <?php if ($success): ?>
            <div class="alert alert-success">
                <i class="fas fa-check-circle"></i> <?php echo $success; ?>
            </div>
        <?php endif; ?>
        
        <div class="products-grid">
            <?php foreach ($products as $i => $product): ?>
            <div class="product-card animate-fade-in" style="animation-delay: <?php echo $i * 0.1; ?>s">
                <div class="product-image">
                    <img src="<?php echo htmlspecialchars($product['image_url']); ?>" alt="<?php echo htmlspecialchars($product['name']); ?>">
                    <span class="product-badge">HOT</span>
                </div>
                <div class="product-info">
                    <h3 class="product-name"><?php echo htmlspecialchars($product['name']); ?></h3>
                    <p class="product-desc"><?php echo htmlspecialchars($product['description']); ?></p>
                    
                    <div class="price-selector">
                        <?php if ($product['price_1d'] > 0): ?>
                        <div class="price-option" onclick="selectPrice(this, <?php echo $product['id']; ?>, 1, <?php echo $product['price_1d']; ?>)">
                            <div class="days">1 Día</div>
                            <div class="price">$<?php echo $product['price_1d']; ?></div>
                        </div>
                        <?php endif; ?>
                        <?php if ($product['price_7d'] > 0): ?>
                        <div class="price-option" onclick="selectPrice(this, <?php echo $product['id']; ?>, 7, <?php echo $product['price_7d']; ?>)">
                            <div class="days">7 Días</div>
                            <div class="price">$<?php echo $product['price_7d']; ?></div>
                        </div>
                        <?php endif; ?>
                        <?php if ($product['price_15d'] > 0): ?>
                        <div class="price-option" onclick="selectPrice(this, <?php echo $product['id']; ?>, 15, <?php echo $product['price_15d']; ?>)">
                            <div class="days">15 Días</div>
                            <div class="price">$<?php echo $product['price_15d']; ?></div>
                        </div>
                        <?php endif; ?>
                        <?php if ($product['price_30d'] > 0): ?>
                        <div class="price-option" onclick="selectPrice(this, <?php echo $product['id']; ?>, 30, <?php echo $product['price_30d']; ?>)">
                            <div class="days">30 Días</div>
                            <div class="price">$<?php echo $product['price_30d']; ?></div>
                        </div>
                        <?php endif; ?>
                    </div>
                    
                    <form method="POST" id="form-<?php echo $product['id']; ?>">
                        <input type="hidden" name="action" value="buy_key">
                        <input type="hidden" name="product_id" value="<?php echo $product['id']; ?>">
                        <input type="hidden" name="duration" id="duration-<?php echo $product['id']; ?>" value="">
                        <button type="submit" class="buy-btn" id="btn-<?php echo $product['id']; ?>" disabled onclick="return confirmPurchase(<?php echo $product['id']; ?>)">
                            <i class="fas fa-shopping-cart"></i> Selecciona una duración
                        </button>
                    </form>
                </div>
            </div>
            <?php endforeach; ?>
        </div>
    </div>
    
    <!-- Modal de Key Comprada -->
    <div class="modal" id="keyModal">
        <div class="modal-content">
            <button class="modal-close" onclick="closeModal()">
                <i class="fas fa-times"></i>
            </button>
            <i class="fas fa-check-circle" style="font-size: 4rem; color: #00b894; margin-bottom: 1rem;"></i>
            <h2>¡Compra Exitosa! 🔥</h2>
            <p style="color: rgba(255,255,255,0.7); margin-bottom: 1rem;">Tu key está lista. Guárdala en un lugar seguro.</p>
            
            <div class="key-display" id="keyValue">
                <?php echo $_SESSION['last_purchase'] ?? ''; ?>
            </div>
            
            <button class="copy-btn" onclick="copyKey()">
                <i class="fas fa-copy"></i> Copiar Key
            </button>
        </div>
    </div>
    
    <a href="https://wa.me/TUNUMERO?text=Hola,%20quiero%20comprar%20saldo" class="whatsapp-float" target="_blank">
        <i class="fab fa-whatsapp"></i>
        <span class="whatsapp-tooltip">¿Necesitas saldo? Escríbenos</span>
    </a>
    
    <?php elseif ($page === 'mykeys'): ?>
    <div class="container no-screenshot" style="padding-top: 90px;">
        <h2 class="section-title animate-fade-in">🔑 Mis Keys Compradas</h2>
        
        <?php if (empty($my_keys)): ?>
            <div style="text-align: center; padding: 4rem 2rem; background: var(--glass); border-radius: 20px; margin-top: 2rem;">
                <i class="fas fa-key" style="font-size: 4rem; color: rgba(255,255,255,0.2); margin-bottom: 1rem;"></i>
                <h3>No tienes keys aún</h3>
                <p style="color: rgba(255,255,255,0.6);">Visita la tienda para comprar tus primeras keys</p>
                <a href="?page=dashboard" class="btn btn-primary" style="margin-top: 1rem;">
                    <i class="fas fa-store"></i> Ir a la Tienda
                </a>
            </div>
        <?php else: ?>
            <div class="keys-list">
                <?php foreach ($my_keys as $key): ?>
                <div class="key-item animate-fade-in">
                    <div class="key-info">
                        <h4><?php echo htmlspecialchars($key['product_name']); ?></h4>
                        <p>
                            <i class="fas fa-clock"></i> <?php echo $key['duration_days']; ?> días | 
                            <i class="fas fa-calendar"></i> <?php echo date('d/m/Y H:i', strtotime($key['purchased_at'])); ?>
                        </p>
                    </div>
                    <div class="key-value-box">
                        <?php echo htmlspecialchars($key['key_value']); ?>
                    </div>
                    <button class="btn-small btn-unban" onclick="copyToClipboard('<?php echo htmlspecialchars($key['key_value']); ?>')">
                        <i class="fas fa-copy"></i>
                    </button>
                </div>
                <?php endforeach; ?>
            </div>
        <?php endif; ?>
    </div>
    
    <?php elseif ($page === 'admin'): ?>
    <div class="container" style="padding-top: 80px;">
        <h2 class="section-title animate-fade-in">👑 Panel de Administración</h2>
        
        <?php if ($error): ?>
            <div class="alert alert-error">
                <i class="fas fa-exclamation-circle"></i> <?php echo $error; ?>
            </div>
        <?php endif; ?>
        
        <?php if ($success): ?>
            <div class="alert alert-success">
                <i class="fas fa-check-circle"></i> <?php echo $success; ?>
            </div>
        <?php endif; ?>
        
        <div class="admin-grid">
            <!-- Agregar Saldo -->
            <div class="admin-card animate-fade-in">
                <h3><i class="fas fa-wallet"></i> Gestión de Saldo</h3>
                <form method="POST">
                    <input type="hidden" name="action" value="add_balance">
                    <div class="form-group">
                        <label>Usuario</label>
                        <select name="user_id" class="form-control" required>
                            <option value="">Seleccionar usuario...</option>
                            <?php foreach ($all_users as $u): if (!$u['is_admin']): ?>
                            <option value="<?php echo $u['id']; ?>">
                                <?php echo htmlspecialchars($u['username']); ?> (Saldo: $<?php echo $u['balance']; ?>)
                            </option>
                            <?php endif; endforeach; ?>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>Monto a agregar ($)</label>
                        <input type="number" name="amount" step="0.01" min="0" class="form-control" required placeholder="0.00">
                    </div>
                    <button type="submit" class="btn btn-primary" style="width: 100%;">
                        <i class="fas fa-plus-circle"></i> Agregar Saldo
                    </button>
                </form>
            </div>
            
            <!-- Agregar Keys -->
            <div class="admin-card animate-fade-in" style="animation-delay: 0.1s">
                <h3><i class="fas fa-key"></i> Agregar Keys al Stock</h3>
                <form method="POST">
                    <input type="hidden" name="action" value="add_key">
                    <div class="form-group">
                        <label>Producto</label>
                        <select name="product_id" class="form-control" required>
                            <option value="">Seleccionar producto...</option>
                            <?php foreach ($products as $p): ?>
                            <option value="<?php echo $p['id']; ?>"><?php echo htmlspecialchars($p['name']); ?></option>
                            <?php endforeach; ?>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>Duración</label>
                        <select name="duration_days" class="form-control" required>
                            <option value="1">1 Día</option>
                            <option value="7">7 Días</option>
                            <option value="15">15 Días</option>
                            <option value="30">30 Días</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>Key</label>
                        <input type="text" name="key_value" class="form-control" required placeholder="XXXX-XXXX-XXXX">
                    </div>
                    <button type="submit" class="btn btn-primary" style="width: 100%;">
                        <i class="fas fa-plus"></i> Agregar al Stock
                    </button>
                </form>
            </div>
            
            <!-- Gestión de Usuarios -->
            <div class="admin-card animate-fade-in" style="animation-delay: 0.2s; grid-column: 1 / -1;">
                <h3><i class="fas fa-users"></i> Gestión de Usuarios</h3>
                <div class="table-container">
                    <table>
                        <thead>
                            <tr>
                                <th>ID</th>
                                <th>Usuario</th>
                                <th>Email</th>
                                <th>Saldo</th>
                                <th>Estado</th>
                                <th>Registro</th>
                                <th>Acciones</th>
                            </tr>
                        </thead>
                        <tbody>
                            <?php foreach ($all_users as $u): if (!$u['is_admin']): ?>
                            <tr>
                                <td>#<?php echo $u['id']; ?></td>
                                <td><?php echo htmlspecialchars($u['username']); ?></td>
                                <td><?php echo htmlspecialchars($u['email']); ?></td>
                                <td>$<?php echo number_format($u['balance'], 2); ?></td>
                                <td>
                                    <span class="status-badge <?php echo $u['is_banned'] ? 'status-banned' : 'status-active'; ?>">
                                        <?php echo $u['is_banned'] ? 'Baneado' : 'Activo'; ?>
                                    </span>
                                </td>
                                <td><?php echo date('d/m/Y', strtotime($u['created_at'])); ?></td>
                                <td>
                                    <form method="POST" style="display: inline;">
                                        <input type="hidden" name="action" value="ban_user">
                                        <input type="hidden" name="user_id" value="<?php echo $u['id']; ?>">
                                        <input type="hidden" name="ban_status" value="<?php echo $u['is_banned'] ? '0' : '1'; ?>">
                                        <button type="submit" class="btn-small <?php echo $u['is_banned'] ? 'btn-unban' : 'btn-ban'; ?>">
                                            <i class="fas fa-<?php echo $u['is_banned'] ? 'check' : 'ban'; ?>"></i>
                                            <?php echo $u['is_banned'] ? 'Desbanear' : 'Banear'; ?>
                                        </button>
                                    </form>
                                </td>
                            </tr>
                            <?php endif; endforeach; ?>
                        </tbody>
                    </table>
                </div>
            </div>
            
            <!-- Inventario de Keys -->
            <div class="admin-card animate-fade-in" style="animation-delay: 0.3s; grid-column: 1 / -1;">
                <h3><i class="fas fa-box"></i> Inventario de Keys</h3>
                <div class="table-container">
                    <table>
                        <thead>
                            <tr>
                                <th>ID</th>
                                <th>Producto</th>
                                <th>Key</th>
                                <th>Duración</th>
                                <th>Estado</th>
                                <th>Comprador</th>
                                <th>Fecha Venta</th>
                            </tr>
                        </thead>
                        <tbody>
                            <?php foreach ($all_keys as $k): ?>
                            <tr style="<?php echo $k['is_sold'] ? 'opacity: 0.6;' : ''; ?>">
                                <td>#<?php echo $k['id']; ?></td>
                                <td><?php echo htmlspecialchars($k['product_name']); ?></td>
                                <td style="font-family: monospace;"><?php echo substr($k['key_value'], 0, 10); ?>...</td>
                                <td><?php echo $k['duration_days']; ?> días</td>
                                <td>
                                    <span class="status-badge <?php echo $k['is_sold'] ? 'status-banned' : 'status-active'; ?>">
                                        <?php echo $k['is_sold'] ? 'Vendida' : 'Disponible'; ?>
                                    </span>
                                </td>
                                <td><?php echo $k['buyer_name'] ? htmlspecialchars($k['buyer_name']) : '-'; ?></td>
                                <td><?php echo $k['sold_at'] ? date('d/m/Y H:i', strtotime($k['sold_at'])) : '-'; ?></td>
                            </tr>
                            <?php endforeach; ?>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>
    <?php endif; ?>
    
    <script>
        // Generar partículas de fondo
        const particlesContainer = document.getElementById('particles');
        if (particlesContainer) {
            for (let i = 0; i < 50; i++) {
                const particle = document.createElement('div');
                particle.className = 'particle';
                particle.style.left = Math.random() * 100 + '%';
                particle.style.top = Math.random() * 100 + '%';
                particle.style.animationDelay = Math.random() * 10 + 's';
                particle.style.animationDuration = (Math.random() * 10 + 10) + 's';
                particlesContainer.appendChild(particle);
            }
        }
        
        // Selección de precio
        let selectedPrices = {};
        
        function selectPrice(element, productId, days, price) {
            // Remover selección anterior
            const container = element.parentElement;
            container.querySelectorAll('.price-option').forEach(el => el.classList.remove('selected'));
            
            // Seleccionar actual
            element.classList.add('selected');
            selectedPrices[productId] = {days, price};
            
            // Actualizar formulario
            document.getElementById('duration-' + productId).value = days;
            const btn = document.getElementById('btn-' + productId);
            btn.disabled = false;
            btn.innerHTML = `<i class="fas fa-shopping-cart"></i> Comprar ($${price})`;
        }
        
        function confirmPurchase(productId) {
            const selection = selectedPrices[productId];
            if (!selection) {
                alert('Selecciona una duración primero');
                return false;
            }
            return confirm(`¿Confirmas comprar esta key por $${selection.price}?`);
        }
        
        // Modal
        <?php if (isset($_SESSION['last_purchase'])): ?>
        document.addEventListener('DOMContentLoaded', function() {
            document.getElementById('keyModal').classList.add('active');
        });
        <?php unset($_SESSION['last_purchase']); endif; ?>
        
        function closeModal() {
            document.getElementById('keyModal').classList.remove('active');
        }
        
        function copyKey() {
            const keyText = document.getElementById('keyValue').textContent.trim();
            navigator.clipboard.writeText(keyText).then(function() {
                const btn = document.querySelector('.copy-btn');
                btn.classList.add('copied');
                btn.innerHTML = '<i class="fas fa-check"></i> ¡Copiado!';
                setTimeout(() => {
                    btn.classList.remove('copied');
                    btn.innerHTML = '<i class="fas fa-copy"></i> Copiar Key';
                }, 2000);
            });
        }
        
        function copyToClipboard(text) {
            navigator.clipboard.writeText(text).then(function() {
                alert('Key copiada al portapapeles');
            });
        }
        
        // Anti-screenshot adicional
        document.addEventListener('keydown', function(e) {
            // Bloquear Print Screen
            if (e.key === 'PrintScreen') {
                e.preventDefault();
                alert('Screenshots deshabilitados por seguridad');
            }
            // Bloquear Ctrl+S, Ctrl+P
            if ((e.ctrlKey || e.metaKey) && (e.key === 's' || e.key === 'p')) {
                e.preventDefault();
            }
        });
        
        // Prevenir clic derecho en áreas sensibles
        document.querySelectorAll('.no-screenshot').forEach(el => {
            el.addEventListener('contextmenu', e => e.preventDefault());
        });
        
        // Efecto de brillo en tarjetas
        document.querySelectorAll('.product-card').forEach(card => {
            card.addEventListener('mousemove', function(e) {
                const rect = this.getBoundingClientRect();
                const x = e.clientX - rect.left;
                const y = e.clientY - rect.top;
                
                this.style.setProperty('--mouse-x', `${x}px`);
                this.style.setProperty('--mouse-y', `${y}px`);
            });
        });
    </script>
</body>
</html>