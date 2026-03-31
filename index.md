<!DOCTYPE html>
<html lang="fr" dir="ltr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MOUL LMLI7 - Fast Food Marocain</title>
    <meta name="description" content="MOUL LMLI7 - Le vrai goût du fast-food marocain. Tacos, Sandwiches, Burgers, Paninis et plus.">

    <!-- Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Poppins:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <!-- Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">

    <style>
        /* ============================================
           CSS VARIABLES & RESET
           ============================================ */
        :root {
            --orange: #FF6B00;
            --orange-dark: #D95A00;
            --orange-light: #FF8C38;
            --orange-glow: rgba(255, 107, 0, 0.35);
            --black: #0A0A0A;
            --black-light: #141414;
            --black-card: #1A1A1A;
            --black-hover: #222222;
            --white: #FFFFFF;
            --gray: #999999;
            --gray-light: #CCCCCC;
            --gray-dark: #555555;
            --danger: #FF4444;
            --success: #22C55E;
            --radius: 14px;
            --radius-sm: 8px;
            --shadow: 0 8px 32px rgba(0,0,0,0.4);
            --shadow-sm: 0 2px 12px rgba(0,0,0,0.3);
            --transition: 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            --font-display: 'Bebas Neue', sans-serif;
            --font-body: 'Poppins', sans-serif;
        }

        *, *::before, *::after {
            margin: 0; padding: 0; box-sizing: border-box;
        }

        html {
            scroll-behavior: smooth;
            scroll-padding-top: 80px;
        }

        body {
            font-family: var(--font-body);
            background: var(--black);
            color: var(--white);
            line-height: 1.6;
            overflow-x: hidden;
            -webkit-font-smoothing: antialiased;
        }

        body.no-scroll { overflow: hidden; }

        img { max-width: 100%; display: block; }
        a { text-decoration: none; color: inherit; }
        button { cursor: pointer; border: none; background: none; font-family: inherit; }
        input, textarea, select { font-family: inherit; }

        .container {
            width: 100%;
            max-width: 1200px;
            margin: 0 auto;
            padding: 0 20px;
        }

        /* ============================================
           LOADING SCREEN
           ============================================ */
        #loader {
            position: fixed; inset: 0; z-index: 10000;
            background: var(--black);
            display: flex; flex-direction: column;
            align-items: center; justify-content: center;
            transition: opacity 0.5s ease, visibility 0.5s ease;
        }
        #loader.hidden { opacity: 0; visibility: hidden; pointer-events: none; }

        .loader-brand {
            font-family: var(--font-display);
            font-size: clamp(2.5rem, 8vw, 4.5rem);
            color: var(--orange);
            letter-spacing: 4px;
            margin-bottom: 30px;
            animation: loaderPulse 1.2s ease-in-out infinite;
        }

        .loader-spinner {
            width: 50px; height: 50px;
            border: 3px solid var(--black-card);
            border-top-color: var(--orange);
            border-radius: 50%;
            animation: spin 0.8s linear infinite;
        }

        @keyframes spin { to { transform: rotate(360deg); } }
        @keyframes loaderPulse {
            0%, 100% { opacity: 1; transform: scale(1); }
            50% { opacity: 0.7; transform: scale(1.05); }
        }

        /* ============================================
           HEADER
           ============================================ */
        #header {
            position: fixed; top: 0; left: 0; right: 0; z-index: 1000;
            background: rgba(10, 10, 10, 0.85);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            border-bottom: 1px solid rgba(255,255,255,0.06);
            transition: var(--transition);
        }
        #header.scrolled {
            background: rgba(10, 10, 10, 0.95);
            box-shadow: 0 4px 30px rgba(0,0,0,0.5);
        }

        .header-inner {
            display: flex; align-items: center; justify-content: space-between;
            height: 72px;
        }

        .logo {
            font-family: var(--font-display);
            font-size: 1.8rem;
            color: var(--orange);
            letter-spacing: 3px;
            white-space: nowrap;
        }

        .nav-links { display: flex; gap: 8px; }
        .nav-links a {
            padding: 8px 18px;
            border-radius: 50px;
            font-size: 0.88rem;
            font-weight: 500;
            color: var(--gray-light);
            transition: var(--transition);
        }
        .nav-links a:hover,
        .nav-links a.active {
            color: var(--white);
            background: rgba(255, 107, 0, 0.12);
        }
        .nav-links a.active { color: var(--orange); }

        .header-actions { display: flex; align-items: center; gap: 8px; }

        .cart-btn {
            position: relative;
            width: 44px; height: 44px;
            border-radius: 50%;
            display: flex; align-items: center; justify-content: center;
            color: var(--white);
            font-size: 1.15rem;
            transition: var(--transition);
            background: rgba(255,255,255,0.06);
        }
        .cart-btn:hover { background: var(--orange); }

        .cart-count {
            position: absolute; top: -4px; right: -4px;
            min-width: 20px; height: 20px;
            background: var(--orange);
            color: var(--white);
            font-size: 0.7rem; font-weight: 700;
            border-radius: 50px;
            display: flex; align-items: center; justify-content: center;
            padding: 0 5px;
            transform: scale(0);
            transition: transform 0.3s cubic-bezier(0.68, -0.55, 0.27, 1.55);
        }
        .cart-count.show { transform: scale(1); }

        .mobile-menu-btn {
            display: none;
            width: 44px; height: 44px;
            border-radius: 50%;
            align-items: center; justify-content: center;
            color: var(--white); font-size: 1.2rem;
            background: rgba(255,255,255,0.06);
            transition: var(--transition);
        }
        .mobile-menu-btn:hover { background: rgba(255,255,255,0.12); }

        /* Mobile Navigation */
        #mobileNav {
            position: fixed; inset: 0; z-index: 999;
            background: rgba(10,10,10,0.97);
            backdrop-filter: blur(20px);
            display: flex; flex-direction: column;
            align-items: center; justify-content: center; gap: 12px;
            opacity: 0; visibility: hidden;
            transition: var(--transition);
        }
        #mobileNav.open { opacity: 1; visibility: visible; }

        #mobileNav a {
            font-family: var(--font-display);
            font-size: 2.2rem;
            letter-spacing: 3px;
            color: var(--gray-light);
            transition: var(--transition);
            padding: 10px 30px;
        }
        #mobileNav a:hover { color: var(--orange); }

        .mobile-nav-close {
            position: absolute; top: 20px; right: 20px;
            width: 48px; height: 48px;
            border-radius: 50%;
            display: flex; align-items: center; justify-content: center;
            color: var(--white); font-size: 1.5rem;
            background: rgba(255,255,255,0.08);
        }

        /* ============================================
           HERO SECTION
           ============================================ */
        #hero {
            position: relative;
            min-height: 100vh;
            display: flex; align-items: center;
            overflow: hidden;
        }

        .hero-bg {
            position: absolute; inset: 0;
            background: url('https://cdn.imgchest.com/files/e667888606db.png') center/cover no-repeat;
        }
        .hero-bg::after {
            content: ''; position: absolute; inset: 0;
            background: linear-gradient(
                135deg,
                rgba(10,10,10,0.93) 0%,
                rgba(10,10,10,0.72) 50%,
                rgba(217,90,0,0.2) 100%
            );
        }

        .hero-content {
            position: relative; z-index: 2;
            padding: 120px 0 80px;
            max-width: 650px;
        }

        .hero-badge {
            display: inline-flex; align-items: center; gap: 8px;
            padding: 8px 20px;
            background: rgba(255,107,0,0.15);
            border: 1px solid rgba(255,107,0,0.3);
            border-radius: 50px;
            font-size: 0.82rem; font-weight: 500;
            color: var(--orange-light);
            margin-bottom: 24px;
            animation: fadeInUp 0.8s ease 0.2s both;
        }
        .hero-badge i { font-size: 0.7rem; }

        .hero-title {
            font-family: var(--font-display);
            font-size: clamp(3.5rem, 12vw, 7rem);
            line-height: 0.95;
            letter-spacing: 4px;
            color: var(--white);
            margin-bottom: 20px;
            animation: fadeInUp 0.8s ease 0.4s both;
        }
        .hero-title .highlight { color: var(--orange); }

        .hero-desc {
            font-size: 1.05rem;
            color: var(--gray-light);
            margin-bottom: 36px;
            max-width: 480px;
            animation: fadeInUp 0.8s ease 0.6s both;
        }

        .hero-actions {
            display: flex; gap: 14px; flex-wrap: wrap;
            animation: fadeInUp 0.8s ease 0.8s both;
        }

        .btn-primary {
            display: inline-flex; align-items: center; gap: 10px;
            padding: 14px 32px;
            background: var(--orange);
            color: var(--white);
            font-weight: 600; font-size: 0.95rem;
            border-radius: 50px;
            transition: var(--transition);
            box-shadow: 0 4px 20px var(--orange-glow);
        }
        .btn-primary:hover {
            background: var(--orange-dark);
            transform: translateY(-2px);
            box-shadow: 0 8px 30px var(--orange-glow);
        }

        .btn-outline {
            display: inline-flex; align-items: center; gap: 10px;
            padding: 14px 32px;
            background: transparent;
            color: var(--white);
            font-weight: 500; font-size: 0.95rem;
            border-radius: 50px;
            border: 1.5px solid rgba(255,255,255,0.2);
            transition: var(--transition);
        }
        .btn-outline:hover {
            border-color: var(--orange);
            color: var(--orange);
            background: rgba(255,107,0,0.06);
        }

        .hero-stats {
            display: flex; gap: 40px; margin-top: 60px;
            animation: fadeInUp 0.8s ease 1s both;
        }
        .hero-stat-num {
            font-family: var(--font-display);
            font-size: 2.2rem; color: var(--orange);
            letter-spacing: 1px;
        }
        .hero-stat-label {
            font-size: 0.78rem; color: var(--gray);
            text-transform: uppercase; letter-spacing: 1px;
        }

        /* Floating blobs */
        .hero-float {
            position: absolute;
            border-radius: 50%;
            filter: blur(80px);
            opacity: 0.12;
            pointer-events: none;
        }
        .hero-float-1 {
            width: 400px; height: 400px;
            background: var(--orange);
            top: 10%; right: -5%;
            animation: floatSlow 8s ease-in-out infinite;
        }
        .hero-float-2 {
            width: 300px; height: 300px;
            background: #FF4444;
            bottom: 10%; right: 20%;
            animation: floatSlow 10s ease-in-out infinite reverse;
        }

        @keyframes floatSlow {
            0%, 100% { transform: translate(0, 0); }
            50% { transform: translate(-30px, 20px); }
        }
        @keyframes fadeInUp {
            from { opacity: 0; transform: translateY(30px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* ============================================
           MENU SECTION
           ============================================ */
        #menu {
            padding: 100px 0 80px;
            position: relative;
        }
        #menu::before {
            content: ''; position: absolute; top: 0; left: 0; right: 0; height: 1px;
            background: linear-gradient(90deg, transparent, rgba(255,107,0,0.3), transparent);
        }

        .section-header { text-align: center; margin-bottom: 50px; }
        .section-label {
            display: inline-flex; align-items: center; gap: 8px;
            font-size: 0.82rem; font-weight: 600;
            color: var(--orange);
            text-transform: uppercase;
            letter-spacing: 3px;
            margin-bottom: 12px;
        }
        .section-label::before, .section-label::after {
            content: ''; width: 30px; height: 1px; background: var(--orange);
        }

        .section-title {
            font-family: var(--font-display);
            font-size: clamp(2.5rem, 6vw, 3.5rem);
            letter-spacing: 3px;
            color: var(--white);
        }

        /* Category Tabs */
        .cat-tabs {
            display: flex; gap: 8px;
            overflow-x: auto;
            padding-bottom: 10px;
            margin-bottom: 40px;
            scrollbar-width: none;
            -ms-overflow-style: none;
        }
        .cat-tabs::-webkit-scrollbar { display: none; }

        .cat-tab {
            flex-shrink: 0;
            padding: 10px 22px;
            border-radius: 50px;
            font-size: 0.85rem; font-weight: 500;
            color: var(--gray-light);
            background: rgba(255,255,255,0.05);
            border: 1px solid rgba(255,255,255,0.08);
            transition: var(--transition);
            white-space: nowrap;
        }
        .cat-tab:hover { color: var(--white); border-color: rgba(255,255,255,0.15); }
        .cat-tab.active {
            color: var(--white);
            background: var(--orange);
            border-color: var(--orange);
            box-shadow: 0 4px 15px var(--orange-glow);
        }

        /* Menu Grid */
        .menu-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
            gap: 20px;
        }

        /* Menu Card */
        .menu-card {
            background: var(--black-card);
            border-radius: var(--radius);
            overflow: hidden;
            border: 1px solid rgba(255,255,255,0.05);
            transition: var(--transition);
            opacity: 0; transform: translateY(20px);
        }
        .menu-card.visible {
            opacity: 1; transform: translateY(0);
        }
        .menu-card:hover {
            transform: translateY(-4px);
            border-color: rgba(255,107,0,0.2);
            box-shadow: 0 12px 40px rgba(0,0,0,0.4);
        }

        .card-img {
            position: relative;
            height: 190px;
            overflow: hidden;
        }
        .card-img img {
            width: 100%; height: 100%; object-fit: cover;
            transition: transform 0.5s ease;
        }
        .menu-card:hover .card-img img { transform: scale(1.08); }

        .card-img::after {
            content: ''; position: absolute; bottom: 0; left: 0; right: 0;
            height: 60%;
            background: linear-gradient(to top, var(--black-card), transparent);
        }

        .card-cat-badge {
            position: absolute; top: 12px; left: 12px; z-index: 2;
            padding: 4px 12px;
            background: rgba(10,10,10,0.75);
            backdrop-filter: blur(8px);
            border-radius: 50px;
            font-size: 0.72rem; font-weight: 600;
            color: var(--orange);
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        /* Badge frites sur la carte */
        .card-frites-badge {
            position: absolute; top: 12px; right: 12px; z-index: 2;
            padding: 4px 10px;
            background: rgba(255,193,7,0.9);
            border-radius: 50px;
            font-size: 0.68rem; font-weight: 700;
            color: #1a1a1a;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .card-body { padding: 16px 18px 18px; }

        .card-name {
            font-weight: 600; font-size: 1.02rem;
            color: var(--white);
            margin-bottom: 8px;
            line-height: 1.3;
        }

        .card-price {
            display: flex; align-items: baseline; gap: 8px;
            margin-bottom: 6px;
        }
        .price-tag {
            font-family: var(--font-display);
            font-size: 1.5rem;
            color: var(--orange);
            letter-spacing: 1px;
        }
        .price-tag .curr { font-family: var(--font-body); font-size: 0.8rem; color: var(--gray); font-weight: 400; }
        .price-sep { color: var(--gray-dark); font-size: 0.8rem; }
        .price-small { font-size: 0.78rem; color: var(--gray); }
        .price-small span { color: var(--gray-light); font-weight: 600; }

        .card-addons-hint {
            font-size: 0.72rem; color: var(--gray-dark);
            margin-bottom: 14px;
            display: flex; flex-wrap: wrap; gap: 6px;
        }
        .card-addons-hint span {
            padding: 2px 8px;
            background: rgba(255,255,255,0.04);
            border-radius: 4px;
            border: 1px solid rgba(255,255,255,0.06);
        }

        .card-order-btn {
            width: 100%;
            padding: 11px;
            background: rgba(255,107,0,0.1);
            border: 1px solid rgba(255,107,0,0.25);
            color: var(--orange);
            font-weight: 600; font-size: 0.88rem;
            border-radius: var(--radius-sm);
            display: flex; align-items: center; justify-content: center; gap: 8px;
            transition: var(--transition);
        }
        .card-order-btn:hover {
            background: var(--orange);
            color: var(--white);
            border-color: var(--orange);
            box-shadow: 0 4px 15px var(--orange-glow);
        }

        /* ============================================
           CONTACT SECTION
           ============================================ */
        #contact {
            padding: 100px 0 80px;
            background: var(--black-light);
            position: relative;
        }
        #contact::before {
            content: ''; position: absolute; top: 0; left: 0; right: 0; height: 1px;
            background: linear-gradient(90deg, transparent, rgba(255,107,0,0.3), transparent);
        }

        .contact-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 40px;
            align-items: start;
        }

        .contact-info-list { display: flex; flex-direction: column; gap: 20px; }

        .contact-item { display: flex; gap: 16px; align-items: flex-start; }
        .contact-icon {
            width: 48px; height: 48px; flex-shrink: 0;
            background: rgba(255,107,0,0.1);
            border: 1px solid rgba(255,107,0,0.2);
            border-radius: 12px;
            display: flex; align-items: center; justify-content: center;
            color: var(--orange); font-size: 1.1rem;
        }
        .contact-item-label {
            font-size: 0.78rem; color: var(--gray);
            text-transform: uppercase; letter-spacing: 1px;
            margin-bottom: 4px;
        }
        .contact-item-value { font-weight: 500; color: var(--white); font-size: 0.95rem; }
        .contact-item-value a { transition: var(--transition); }
        .contact-item-value a:hover { color: var(--orange); }

        .contact-socials { display: flex; gap: 12px; margin-top: 30px; }
        .social-link {
            width: 46px; height: 46px;
            background: rgba(255,255,255,0.06);
            border: 1px solid rgba(255,255,255,0.1);
            border-radius: 12px;
            display: flex; align-items: center; justify-content: center;
            color: var(--gray-light); font-size: 1.15rem;
            transition: var(--transition);
        }
        .social-link:hover {
            background: var(--orange); color: var(--white);
            border-color: var(--orange); transform: translateY(-3px);
        }

        .contact-map {
            background: var(--black-card);
            border: 1px solid rgba(255,255,255,0.06);
            border-radius: var(--radius);
            overflow: hidden;
            height: 360px;
            display: flex; align-items: center; justify-content: center;
            flex-direction: column; gap: 16px;
            color: var(--gray);
        }
        .contact-map i { font-size: 3rem; color: var(--orange); opacity: 0.5; }
        .contact-map p { font-size: 0.9rem; }

        /* ============================================
           FOOTER
           ============================================ */
        footer {
            padding: 40px 0;
            border-top: 1px solid rgba(255,255,255,0.06);
            text-align: center;
        }
        .footer-brand {
            font-family: var(--font-display);
            font-size: 1.5rem; color: var(--orange);
            letter-spacing: 3px; margin-bottom: 10px;
        }
        .footer-copy { font-size: 0.8rem; color: var(--gray-dark); }

        /* ============================================
           CART SIDEBAR
           ============================================ */
        .cart-overlay {
            position: fixed; inset: 0; z-index: 2000;
            background: rgba(0,0,0,0.6);
            opacity: 0; visibility: hidden;
            transition: var(--transition);
        }
        .cart-overlay.open { opacity: 1; visibility: visible; }

        .cart-sidebar {
            position: fixed; top: 0; right: 0; bottom: 0; z-index: 2001;
            width: 100%; max-width: 420px;
            background: var(--black-light);
            border-left: 1px solid rgba(255,255,255,0.08);
            transform: translateX(100%);
            transition: transform 0.4s cubic-bezier(0.4, 0, 0.2, 1);
            display: flex; flex-direction: column;
        }
        .cart-sidebar.open { transform: translateX(0); }

        .cart-header {
            display: flex; align-items: center; justify-content: space-between;
            padding: 20px 24px;
            border-bottom: 1px solid rgba(255,255,255,0.08);
        }
        .cart-header h3 {
            font-family: var(--font-display);
            font-size: 1.5rem; letter-spacing: 2px;
        }
        .cart-close-btn {
            width: 38px; height: 38px; border-radius: 50%;
            display: flex; align-items: center; justify-content: center;
            color: var(--gray-light); font-size: 1.1rem;
            background: rgba(255,255,255,0.06);
            transition: var(--transition);
        }
        .cart-close-btn:hover { background: rgba(255,255,255,0.12); color: var(--white); }

        .cart-items { flex: 1; overflow-y: auto; padding: 16px 24px; }

        .cart-empty {
            text-align: center; padding: 60px 20px; color: var(--gray);
        }
        .cart-empty i { font-size: 3rem; margin-bottom: 16px; opacity: 0.3; display: block; }
        .cart-empty p { font-size: 0.9rem; }

        .cart-item {
            display: flex; gap: 14px; align-items: center;
            padding: 14px 0;
            border-bottom: 1px solid rgba(255,255,255,0.05);
            animation: fadeInUp 0.3s ease;
        }
        .cart-item-img {
            width: 56px; height: 56px; border-radius: 10px;
            object-fit: cover; flex-shrink: 0;
        }
        .cart-item-info { flex: 1; min-width: 0; }
        .cart-item-name {
            font-weight: 600; font-size: 0.88rem;
            white-space: nowrap; overflow: hidden; text-overflow: ellipsis;
        }
        .cart-item-detail {
            font-size: 0.73rem; color: var(--orange); margin-top: 2px;
            font-weight: 500;
        }
        .cart-item-price {
            font-weight: 700; font-size: 0.9rem; color: var(--orange);
            white-space: nowrap;
        }

        .cart-item-actions {
            display: flex; flex-direction: column; align-items: center; gap: 4px;
        }
        .qty-controls {
            display: flex; align-items: center;
            background: rgba(255,255,255,0.06);
            border-radius: 8px; overflow: hidden;
        }
        .qty-btn {
            width: 30px; height: 30px;
            display: flex; align-items: center; justify-content: center;
            color: var(--gray-light); font-size: 0.85rem;
            transition: var(--transition);
        }
        .qty-btn:hover { background: rgba(255,255,255,0.1); color: var(--white); }
        .qty-val { width: 28px; text-align: center; font-weight: 600; font-size: 0.85rem; }
        .remove-btn {
            font-size: 0.7rem; color: var(--danger);
            transition: var(--transition); padding: 2px;
        }
        .remove-btn:hover { color: #ff6666; }

        .cart-footer {
            padding: 20px 24px;
            border-top: 1px solid rgba(255,255,255,0.08);
        }
        .cart-total-row {
            display: flex; align-items: center; justify-content: space-between;
            margin-bottom: 16px;
        }
        .cart-total-label { font-size: 0.9rem; color: var(--gray-light); }
        .cart-total-value {
            font-family: var(--font-display);
            font-size: 1.8rem; color: var(--orange);
            letter-spacing: 1px;
        }

        .checkout-btn {
            width: 100%; padding: 14px;
            background: var(--orange);
            color: var(--white); font-weight: 600; font-size: 0.95rem;
            border-radius: var(--radius-sm);
            display: flex; align-items: center; justify-content: center; gap: 10px;
            transition: var(--transition);
            box-shadow: 0 4px 15px var(--orange-glow);
        }
        .checkout-btn:hover { background: var(--orange-dark); }
        .checkout-btn:disabled { opacity: 0.4; cursor: not-allowed; box-shadow: none; }

        /* ============================================
           MODALS
           ============================================ */
        .modal-overlay {
            position: fixed; inset: 0; z-index: 3000;
            background: rgba(0,0,0,0.7);
            backdrop-filter: blur(6px);
            display: flex; align-items: center; justify-content: center;
            padding: 20px;
            opacity: 0; visibility: hidden;
            transition: var(--transition);
        }
        .modal-overlay.open { opacity: 1; visibility: visible; }

        .modal-box {
            background: var(--black-light);
            border: 1px solid rgba(255,255,255,0.08);
            border-radius: var(--radius);
            width: 100%; max-width: 460px;
            max-height: 90vh; overflow-y: auto;
            transform: scale(0.9) translateY(20px);
            transition: transform 0.35s cubic-bezier(0.4, 0, 0.2, 1);
        }
        .modal-overlay.open .modal-box { transform: scale(1) translateY(0); }

        .modal-close {
            position: absolute; top: 16px; right: 16px;
            width: 36px; height: 36px; border-radius: 50%;
            display: flex; align-items: center; justify-content: center;
            background: rgba(0,0,0,0.6); color: var(--white); font-size: 1.3rem;
            transition: var(--transition); z-index: 5;
        }
        .modal-close:hover { background: rgba(0,0,0,0.8); }

        .modal-img {
            width: 100%; height: 200px; object-fit: cover;
            border-radius: var(--radius) var(--radius) 0 0;
        }

        .modal-body { padding: 24px; position: relative; }

        .modal-item-name {
            font-family: var(--font-display);
            font-size: 1.6rem; letter-spacing: 1px;
            margin-bottom: 20px;
        }

        /* Option labels */
        .option-label {
            font-size: 0.78rem; font-weight: 600;
            color: var(--gray); text-transform: uppercase;
            letter-spacing: 1px; margin-bottom: 8px;
        }
        .option-label i { margin-right: 4px; color: var(--orange); font-size: 0.72rem; }

        /* Size Selector */
        .size-options { display: flex; gap: 10px; margin-bottom: 22px; }
        .size-opt {
            flex: 1; padding: 12px;
            background: rgba(255,255,255,0.04);
            border: 1.5px solid rgba(255,255,255,0.1);
            border-radius: var(--radius-sm);
            text-align: center; cursor: pointer;
            transition: var(--transition);
        }
        .size-opt:hover { border-color: rgba(255,255,255,0.2); }
        .size-opt.selected {
            border-color: var(--orange);
            background: rgba(255,107,0,0.1);
        }
        .size-opt-name { font-weight: 600; font-size: 0.9rem; margin-bottom: 2px; }
        .size-opt-price { font-size: 0.8rem; color: var(--orange); }

        /* Addon Checkbox */
        .addon-option {
            display: flex; align-items: center; gap: 12px;
            padding: 12px 16px;
            background: rgba(255,255,255,0.04);
            border: 1.5px solid rgba(255,255,255,0.08);
            border-radius: var(--radius-sm);
            margin-bottom: 10px;
            cursor: pointer;
            transition: var(--transition);
            user-select: none;
        }
        .addon-option:hover { border-color: rgba(255,255,255,0.15); }
        .addon-option.checked {
            border-color: var(--orange);
            background: rgba(255,107,0,0.08);
        }
        .addon-check {
            width: 24px; height: 24px; border-radius: 7px;
            border: 2px solid var(--gray-dark);
            display: flex; align-items: center; justify-content: center;
            transition: var(--transition); flex-shrink: 0;
            font-size: 0.7rem; color: transparent;
        }
        .addon-option.checked .addon-check {
            background: var(--orange); border-color: var(--orange);
            color: var(--white);
        }
        .addon-text { font-size: 0.9rem; font-weight: 500; flex: 1; }
        .addon-price { font-weight: 700; color: var(--orange); font-size: 0.9rem; white-space: nowrap; }

        /* Frites addon special styling */
        .addon-option.frites-addon {
            background: rgba(255,193,7,0.04);
            border-color: rgba(255,193,7,0.15);
        }
        .addon-option.frites-addon:hover { border-color: rgba(255,193,7,0.3); }
        .addon-option.frites-addon.checked {
            background: rgba(255,193,7,0.12);
            border-color: #FFC107;
        }
        .addon-option.frites-addon .addon-check {
            border-color: rgba(255,193,7,0.5);
        }
        .addon-option.frites-addon.checked .addon-check {
            background: #FFC107; border-color: #FFC107; color: #1a1a1a;
        }

        /* Quantity in Modal */
        .modal-qty-row {
            display: flex; align-items: center; justify-content: space-between;
            margin: 22px 0;
            padding: 14px 0;
            border-top: 1px solid rgba(255,255,255,0.06);
            border-bottom: 1px solid rgba(255,255,255,0.06);
        }
        .modal-qty-label { font-weight: 500; }
        .modal-qty-controls {
            display: flex; align-items: center;
            background: rgba(255,255,255,0.06);
            border-radius: 8px; overflow: hidden;
        }
        .modal-qty-btn {
            width: 40px; height: 40px;
            display: flex; align-items: center; justify-content: center;
            color: var(--gray-light); font-size: 1.1rem;
            transition: var(--transition);
        }
        .modal-qty-btn:hover { background: rgba(255,255,255,0.1); color: var(--white); }
        .modal-qty-val { width: 44px; text-align: center; font-weight: 700; font-size: 1.05rem; }

        .modal-total {
            display: flex; align-items: center; justify-content: space-between;
            margin-bottom: 20px;
        }
        .modal-total-label { font-size: 0.9rem; color: var(--gray-light); }
        .modal-total-val {
            font-family: var(--font-display);
            font-size: 2rem; color: var(--orange);
        }

        .add-to-cart-btn {
            width: 100%; padding: 14px;
            background: var(--orange);
            color: var(--white); font-weight: 600; font-size: 0.95rem;
            border-radius: var(--radius-sm);
            display: flex; align-items: center; justify-content: center; gap: 10px;
            transition: var(--transition);
            box-shadow: 0 4px 15px var(--orange-glow);
        }
        .add-to-cart-btn:hover { background: var(--orange-dark); }

        /* Checkout Form */
        .checkout-summary {
            max-height: 200px; overflow-y: auto;
            margin-bottom: 20px;
            padding-bottom: 16px;
            border-bottom: 1px solid rgba(255,255,255,0.06);
        }
        .checkout-summary-item {
            display: flex; justify-content: space-between; align-items: center;
            padding: 6px 0; font-size: 0.85rem; gap: 10px;
        }
        .checkout-summary-item .name { color: var(--gray-light); text-align: right; }
        .checkout-summary-item .price { font-weight: 600; color: var(--white); white-space: nowrap; }
        .checkout-summary-total {
            display: flex; justify-content: space-between;
            padding: 10px 0 0; margin-top: 8px;
            border-top: 1px solid rgba(255,255,255,0.08);
            font-weight: 700;
        }
        .checkout-summary-total .price {
            font-family: var(--font-display);
            font-size: 1.3rem; color: var(--orange);
        }

        .form-group { margin-bottom: 16px; }
        .form-input {
            width: 100%; padding: 13px 16px;
            background: rgba(255,255,255,0.05);
            border: 1.5px solid rgba(255,255,255,0.1);
            border-radius: var(--radius-sm);
            color: var(--white);
            font-size: 0.9rem;
            transition: var(--transition);
            outline: none;
        }
        .form-input::placeholder { color: var(--gray-dark); }
        .form-input:focus {
            border-color: var(--orange);
            background: rgba(255,107,0,0.04);
        }
        textarea.form-input { resize: vertical; min-height: 80px; }

        .form-error {
            font-size: 0.75rem; color: var(--danger);
            margin-top: 4px; display: none;
        }
        .form-error.show { display: block; }

        .submit-order-btn {
            width: 100%; padding: 14px;
            background: #25D366;
            color: var(--white); font-weight: 600; font-size: 0.95rem;
            border-radius: var(--radius-sm);
            display: flex; align-items: center; justify-content: center; gap: 10px;
            transition: var(--transition);
            margin-top: 8px;
        }
        .submit-order-btn:hover { background: #1EBE57; transform: translateY(-1px); }
        .submit-order-btn:disabled { opacity: 0.5; cursor: not-allowed; transform: none; }

        /* Success Modal */
        .success-content { text-align: center; padding: 40px 30px; }
        .success-check {
            width: 80px; height: 80px; margin: 0 auto 24px;
            background: rgba(34,197,94,0.15);
            border: 2px solid var(--success);
            border-radius: 50%;
            display: flex; align-items: center; justify-content: center;
            font-size: 2.2rem; color: var(--success);
            animation: successPop 0.5s cubic-bezier(0.68, -0.55, 0.27, 1.55);
        }
        @keyframes successPop { from { transform: scale(0); } to { transform: scale(1); } }

        .success-title {
            font-family: var(--font-display);
            font-size: 1.8rem; letter-spacing: 2px;
            margin-bottom: 12px;
        }
        .success-desc { color: var(--gray-light); font-size: 0.9rem; margin-bottom: 28px; line-height: 1.6; }
        .success-close-btn {
            padding: 12px 36px;
            background: rgba(255,255,255,0.08);
            border: 1px solid rgba(255,255,255,0.15);
            color: var(--white); font-weight: 500;
            border-radius: 50px; transition: var(--transition);
        }
        .success-close-btn:hover { background: rgba(255,255,255,0.15); }

        /* ============================================
           FLOATING BUTTONS
           ============================================ */
        .float-btn {
            position: fixed; z-index: 800;
            width: 54px; height: 54px;
            border-radius: 50%;
            display: flex; align-items: center; justify-content: center;
            color: var(--white); font-size: 1.4rem;
            box-shadow: 0 4px 20px rgba(0,0,0,0.4);
            transition: var(--transition);
        }
        .float-btn:hover { transform: translateY(-3px); }

        .float-whatsapp { bottom: 24px; right: 24px; background: #25D366; }
        .float-whatsapp:hover { background: #1EBE57; box-shadow: 0 6px 25px rgba(37,211,102,0.4); }

        .float-call { bottom: 90px; right: 24px; background: var(--orange); }
        .float-call:hover { background: var(--orange-dark); box-shadow: 0 6px 25px var(--orange-glow); }

        /* ============================================
           RESPONSIVE
           ============================================ */
        @media (max-width: 768px) {
            .nav-links { display: none; }
            .mobile-menu-btn { display: flex; }
            .hero-content { padding: 100px 0 60px; }
            .hero-stats { gap: 24px; flex-wrap: wrap; }
            .menu-grid { grid-template-columns: repeat(auto-fill, minmax(250px, 1fr)); gap: 16px; }
            .contact-grid { grid-template-columns: 1fr; }
            .contact-map { height: 260px; }
            .cart-sidebar { max-width: 100%; }
            .float-whatsapp { bottom: 20px; right: 20px; width: 50px; height: 50px; }
            .float-call { bottom: 82px; right: 20px; width: 50px; height: 50px; }
            .modal-box { max-width: 100%; }
        }

        @media (max-width: 400px) {
            .menu-grid { grid-template-columns: 1fr; }
            .hero-actions { flex-direction: column; }
            .hero-actions a { text-align: center; justify-content: center; }
        }

        /* Scrollbar */
        ::-webkit-scrollbar { width: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--gray-dark); border-radius: 3px; }
        ::-webkit-scrollbar-thumb:hover { background: var(--gray); }
        ::selection { background: var(--orange); color: var(--white); }
    </style>
</head>
<body>

    <!-- LOADING SCREEN -->
    <div id="loader">
        <div class="loader-brand">MOUL LMLI7</div>
        <div class="loader-spinner"></div>
    </div>

    <!-- HEADER -->
    <header id="header">
        <div class="container header-inner">
            <div class="logo">MOUL LMLI7</div>
            <nav class="nav-links">
                <a href="#home" class="active" data-section="home">Accueil</a>
                <a href="#menu" data-section="menu">Menu</a>
                <a href="#contact" data-section="contact">Contact</a>
            </nav>
            <div class="header-actions">
                <button class="cart-btn" id="cartToggle" aria-label="Panier">
                    <i class="fas fa-shopping-bag"></i>
                    <span class="cart-count" id="cartCount">0</span>
                </button>
                <button class="mobile-menu-btn" id="mobileMenuOpen" aria-label="Menu">
                    <i class="fas fa-bars"></i>
                </button>
            </div>
        </div>
    </header>

    <!-- Mobile Nav -->
    <div id="mobileNav">
        <button class="mobile-nav-close" id="mobileMenuClose" aria-label="Fermer">
            <i class="fas fa-times"></i>
        </button>
        <a href="#home" class="mobile-nav-link">Accueil</a>
        <a href="#menu" class="mobile-nav-link">Menu</a>
        <a href="#contact" class="mobile-nav-link">Contact</a>
    </div>

    <!-- HERO -->
    <section id="home" style="scroll-margin-top:72px;">
        <div id="hero">
            <div class="hero-bg"></div>
            <div class="hero-float hero-float-1"></div>
            <div class="hero-float hero-float-2"></div>
            <div class="container">
                <div class="hero-content">
                    <div class="hero-badge">
                        <i class="fas fa-fire"></i> Fast Food Marocain Authentique
                    </div>
                    <h1 class="hero-title">MOUL<br><span class="highlight">LMLI7</span></h1>
                    <p class="hero-desc">
                        Découvrez nos tacos, burgers, sandwiches et plats préparés avec passion. Le vrai goût qui fait revenir.
                    </p>
                    <div class="hero-actions">
                        <a href="#menu" class="btn-primary"><i class="fas fa-utensils"></i> Voir le Menu</a>
                        <a href="tel:+212620250719" class="btn-outline"><i class="fas fa-phone"></i> Appeler Maintenant</a>
                    </div>
                    <div class="hero-stats">
                        <div><div class="hero-stat-num">30+</div><div class="hero-stat-label">Plats</div></div>
                        <div><div class="hero-stat-num">4.8</div><div class="hero-stat-label">Note Clients</div></div>
                        <div><div class="hero-stat-num">30min</div><div class="hero-stat-label">Livraison</div></div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- MENU -->
    <section id="menu" style="scroll-margin-top:72px;">
        <div class="container">
            <div class="section-header">
                <div class="section-label">Nos Spécialités</div>
                <h2 class="section-title">NOTRE MENU</h2>
            </div>
            <div class="cat-tabs" id="catTabs">
                <button class="cat-tab active" data-cat="tacos"><i class="fas fa-pepper-hot" style="margin-right:4px"></i> Tacos</button>
                <button class="cat-tab" data-cat="sandwiches"><i class="fas fa-bread-slice" style="margin-right:4px"></i> Sandwiches</button>
                <button class="cat-tab" data-cat="plats"><i class="fas fa-plate-wheat" style="margin-right:4px"></i> Plats</button>
                <button class="cat-tab" data-cat="paninis"><i class="fas fa-hotdog" style="margin-right:4px"></i> Paninis</button>
                <button class="cat-tab" data-cat="burgers"><i class="fas fa-burger" style="margin-right:4px"></i> Burgers</button>
                <button class="cat-tab" data-cat="pasticcio"><i class="fas fa-pizza-slice" style="margin-right:4px"></i> Pasticcio</button>
            </div>
            <div class="menu-grid" id="menuGrid"></div>
        </div>
    </section>

    <!-- CONTACT -->
    <section id="contact" style="scroll-margin-top:72px;">
        <div class="container">
            <div class="section-header">
                <div class="section-label">Rendez-nous Visite</div>
                <h2 class="section-title">CONTACT</h2>
            </div>
            <div class="contact-grid">
                <div class="contact-info-list">
                    <div class="contact-item">
                        <div class="contact-icon"><i class="fas fa-map-marker-alt"></i></div>
                        <div>
                            <div class="contact-item-label">Adresse</div>
                            <div class="contact-item-value">Boulevard Mohammed V, Casablanca</div>
                        </div>
                    </div>
                    <div class="contact-item">
                        <div class="contact-icon"><i class="fas fa-phone"></i></div>
                        <div>
                            <div class="contact-item-label">Téléphone</div>
                            <div class="contact-item-value"><a href="tel:+212620250719">+212 6 20 25 07 19</a></div>
                        </div>
                    </div>
                    <div class="contact-item">
                        <div class="contact-icon"><i class="fab fa-whatsapp"></i></div>
                        <div>
                            <div class="contact-item-label">WhatsApp</div>
                            <div class="contact-item-value"><a href="https://wa.me/212620250719" target="_blank">+212 6 20 25 07 19</a></div>
                        </div>
                    </div>
                    <div class="contact-item">
                        <div class="contact-icon"><i class="fas fa-clock"></i></div>
                        <div>
                            <div class="contact-item-label">Horaires</div>
                            <div class="contact-item-value">Tous les jours : 11h00 - 00h00</div>
                        </div>
                    </div>
                    <div class="contact-socials">
                        <a href="#" class="social-link" aria-label="Instagram"><i class="fab fa-instagram"></i></a>
                        <a href="#" class="social-link" aria-label="Facebook"><i class="fab fa-facebook-f"></i></a>
                        <a href="#" class="social-link" aria-label="TikTok"><i class="fab fa-tiktok"></i></a>
                    </div>
                </div>
                <div class="contact-map">
                    <i class="fas fa-map-marked-alt"></i>
                    <p>Intégrez votre carte Google Maps ici</p>
                    <span style="font-size:0.75rem;color:var(--gray-dark);">Remplacez ce bloc par une iframe Google Maps</span>
                </div>
            </div>
        </div>
    </section>

    <!-- FOOTER -->
    <footer>
        <div class="container">
            <div class="footer-brand">MOUL LMLI7</div>
            <p class="footer-copy">&copy; 2025 MOUL LMLI7. Tous droits réservés.</p>
        </div>
    </footer>

    <!-- CART SIDEBAR -->
    <div class="cart-overlay" id="cartOverlay"></div>
    <div class="cart-sidebar" id="cartSidebar">
        <div class="cart-header">
            <h3>VOTRE PANIER</h3>
            <button class="cart-close-btn" id="cartClose" aria-label="Fermer"><i class="fas fa-times"></i></button>
        </div>
        <div class="cart-items" id="cartItems"></div>
        <div class="cart-footer">
            <div class="cart-total-row">
                <span class="cart-total-label">Total</span>
                <span class="cart-total-value" id="cartTotal">0 dh</span>
            </div>
            <button class="checkout-btn" id="checkoutBtn" disabled>
                <i class="fab fa-whatsapp"></i> Passer la Commande
            </button>
        </div>
    </div>

    <!-- ORDER ITEM MODAL -->
    <div class="modal-overlay" id="orderModal">
        <div class="modal-box">
            <button class="modal-close" id="orderModalClose" aria-label="Fermer">&times;</button>
            <img class="modal-img" id="orderModalImg" src="" alt="">
            <div class="modal-body" id="orderModalBody"></div>
        </div>
    </div>

    <!-- CHECKOUT MODAL -->
    <div class="modal-overlay" id="checkoutModal">
        <div class="modal-box">
            <button class="modal-close" id="checkoutModalClose" aria-label="Fermer">&times;</button>
            <div class="modal-body">
                <h3 class="modal-item-name" style="margin-bottom:8px;">RÉCAPITULATIF</h3>
                <div class="checkout-summary" id="checkoutSummary"></div>
                <form id="checkoutForm" novalidate>
                    <div class="form-group">
                        <input class="form-input" type="text" name="name" placeholder="Votre nom complet" required>
                        <div class="form-error" id="nameError">Veuillez entrer votre nom</div>
                    </div>
                    <div class="form-group">
                        <input class="form-input" type="tel" name="phone" placeholder="Numéro de téléphone (06XXXXXXXX)" required>
                        <div class="form-error" id="phoneError">Veuillez entrer un numéro valide (06XX ou 07XX)</div>
                    </div>
                    <div class="form-group">
                        <input class="form-input" type="text" name="address" placeholder="Adresse de livraison" required>
                        <div class="form-error" id="addressError">Veuillez entrer votre adresse</div>
                    </div>
                    <div class="form-group">
                        <textarea class="form-input" name="notes" placeholder="Notes ou instructions spéciales (optionnel)"></textarea>
                    </div>
                    <button type="submit" class="submit-order-btn" id="submitOrderBtn">
                        <i class="fab fa-whatsapp"></i> Envoyer via WhatsApp
                    </button>
                </form>
            </div>
        </div>
    </div>

    <!-- SUCCESS MODAL -->
    <div class="modal-overlay" id="successModal">
        <div class="modal-box">
            <div class="modal-body success-content">
                <div class="success-check"><i class="fas fa-check"></i></div>
                <h3 class="success-title">COMMANDE ENVOYÉE</h3>
                <p class="success-desc">Votre commande a été envoyée avec succès via WhatsApp. Vous recevrez une confirmation très bientôt.</p>
                <button class="success-close-btn" id="successClose">Fermer</button>
            </div>
        </div>
    </div>

    <!-- FLOATING BUTTONS -->
    <!-- ⚠️ REMPLACEZ +212620250719 par votre vrai numéro -->
    <a href="https://wa.me/212620250719" target="_blank" class="float-btn float-whatsapp" aria-label="WhatsApp"><i class="fab fa-whatsapp"></i></a>
    <a href="tel:+212620250719" class="float-btn float-call" aria-label="Appeler"><i class="fas fa-phone"></i></a>


    <script>
    /* ============================================================
       CONFIGURATION — Changez ces valeurs avec vos vraies infos
       ============================================================ */
    const CONFIG = {
        restaurantPhone: '212620250719',  // Numéro WhatsApp international (sans +)
        restaurantName: 'MOUL LMLI7',
        currency: 'dh'
    };

    /* ============================================================
       DONNÉES DU MENU — Structure complète avec suppléments
       Chaque catégorie a "frites" en option (+5dh).
       Tacos a en plus "gratin" (prix selon taille).
       Burgers a en plus "extra fromage" (+3dh).
       ============================================================ */
    const MENU = {
        tacos: {
            label: 'Tacos',
            hasSizes: true,
            sizeLabels: ['Large', 'XLarge'],
            addons: [
                { id: 'frites', name: 'Frites', price: 5, isFrites: true },
                { id: 'gratin', name: 'Gratin', prices: [5, 8], isFrites: false }
            ],
            items: [
                { id: 't1', name: 'Tacos Poulet', prices: [25, 35], img: 'https://picsum.photos/seed/tacospoulet7/400/300.jpg' },
                { id: 't2', name: 'Tacos Viande Hachée', prices: [30, 40], img: 'https://picsum.photos/seed/tacosviande7/400/300.jpg' },
                { id: 't3', name: 'Tacos Mixte', prices: [25, 35], img: 'https://picsum.photos/seed/tacosmixte7/400/300.jpg' },
                { id: 't4', name: 'Tacos Cordon Bleu', prices: [25, 35], img: 'https://picsum.photos/seed/tacoscordon7/400/300.jpg' },
                { id: 't5', name: 'Tacos Poulet Crinchy', prices: [25, 35], img: 'https://picsum.photos/seed/tacoscrinchy7/400/300.jpg' }
            ]
        },
        sandwiches: {
            label: 'Sandwiches',
            hasSizes: false,
            addons: [
                { id: 'frites', name: 'Frites', price: 5, isFrites: true }
            ],
            items: [
                { id: 's1', name: 'Chawarma Poulet', price: 20, img: 'https://picsum.photos/seed/chawarmap7/400/300.jpg' },
                { id: 's2', name: 'Chawarma Poulet Large', price: 30, img: 'https://picsum.photos/seed/chawarmapl7/400/300.jpg' },
                { id: 's3', name: 'Poulet Crinchy', price: 20, img: 'https://picsum.photos/seed/pouletcrinchys7/400/300.jpg' },
                { id: 's4', name: 'Sandwich Américain', price: 23, img: 'https://picsum.photos/seed/americains7/400/300.jpg' },
                { id: 's5', name: 'Sandwich Mixte', price: 20, img: 'https://picsum.photos/seed/mixtesand7/400/300.jpg' },
                { id: 's6', name: 'Sandwich Cordon Bleu', price: 23, img: 'https://picsum.photos/seed/cordonsand7/400/300.jpg' }
            ]
        },
        plats: {
            label: 'Plats',
            hasSizes: false,
            addons: [
                { id: 'frites', name: 'Frites', price: 5, isFrites: true }
            ],
            items: [
                { id: 'p1', name: 'Emincé de Poulet', price: 35, img: 'https://picsum.photos/seed/emincepoulet7/400/300.jpg' },
                { id: 'p2', name: 'Cordon Bleu', price: 35, img: 'https://picsum.photos/seed/cordonbleuplat7/400/300.jpg' },
                { id: 'p3', name: 'Poulet Crinchy', price: 35, img: 'https://picsum.photos/seed/pouletcrinchyp7/400/300.jpg' },
                { id: 'p4', name: 'Suprême de Poulet', price: 35, img: 'https://picsum.photos/seed/supremepoulet7/400/300.jpg' },
                { id: 'p5', name: 'Pâtes Poulet Champignon', price: 35, img: 'https://picsum.photos/seed/patespoulet7/400/300.jpg' },
                { id: 'p6', name: 'Plat Chawarma', price: 35, img: 'https://picsum.photos/seed/platchawarma7/400/300.jpg' }
            ]
        },
        paninis: {
            label: 'Paninis',
            hasSizes: false,
            addons: [
                { id: 'frites', name: 'Frites', price: 5, isFrites: true }
            ],
            items: [
                { id: 'pa1', name: 'Panini Poulet', price: 20, img: 'https://picsum.photos/seed/paninipoulet7/400/300.jpg' },
                { id: 'pa2', name: 'Panini Mixte', price: 20, img: 'https://picsum.photos/seed/paninimixte7/400/300.jpg' },
                { id: 'pa3', name: 'Panini Poulet Crinchy', price: 20, img: 'https://picsum.photos/seed/paninicrinchy7/400/300.jpg' },
                { id: 'pa4', name: 'Panini Viande Hachée', price: 25, img: 'https://picsum.photos/seed/paniniviande7/400/300.jpg' }
            ]
        },
        burgers: {
            label: 'Burgers',
            hasSizes: false,
            addons: [
                { id: 'frites', name: 'Frites', price: 5, isFrites: true },
                { id: 'extrafromage', name: 'Extra Fromage', price: 3, isFrites: false }
            ],
            items: [
                { id: 'b1', name: 'Cheese Burger', price: 30, img: 'https://picsum.photos/seed/cheeseburger7/400/300.jpg' },
                { id: 'b2', name: 'Double Cheese Burger', price: 40, img: 'https://picsum.photos/seed/doublecheese7/400/300.jpg' },
                { id: 'b3', name: 'Chicken Burger', price: 25, img: 'https://picsum.photos/seed/chickenburger7/400/300.jpg' },
                { id: 'b4', name: 'Egg Burger', price: 33, img: 'https://picsum.photos/seed/eggburger7/400/300.jpg' }
            ]
        },
        pasticcio: {
            label: 'Pasticcio',
            hasSizes: false,
            addons: [
                { id: 'frites', name: 'Frites', price: 5, isFrites: true }
            ],
            items: [
                { id: 'ps1', name: 'Pasticcio Poulet', price: 30, img: 'https://picsum.photos/seed/pasticiopoulet7/400/300.jpg' },
                { id: 'ps2', name: 'Pasticcio Mixte', price: 30, img: 'https://picsum.photos/seed/pasticiomixte7/400/300.jpg' },
                { id: 'ps3', name: 'Pasticcio Jambon de Dinde', price: 30, img: 'https://picsum.photos/seed/pasticciojambon7/400/300.jpg' }
            ]
        }
    };

    /* ============================================================
       ÉTAT GLOBAL
       ============================================================ */
    let cart = [];
    let currentCategory = 'tacos';

    // État du modal de commande d'un article
    let orderModalState = {
        item: null,
        category: null,
        selectedSize: 0,
        selectedAddons: [],
        quantity: 1
    };

    /* ============================================================
       RÉFÉRENCES DOM
       ============================================================ */
    const $ = sel => document.querySelector(sel);
    const $$ = sel => document.querySelectorAll(sel);

    /* ============================================================
       LOADING SCREEN
       ============================================================ */
    window.addEventListener('load', () => {
        setTimeout(() => $('#loader').classList.add('hidden'), 1200);
    });

    /* ============================================================
       HEADER SCROLL
       ============================================================ */
    const header = $('#header');
    const sectionIds = ['home', 'menu', 'contact'];

    window.addEventListener('scroll', () => {
        header.classList.toggle('scrolled', window.scrollY > 50);
        let current = 'home';
        for (const id of sectionIds) {
            const el = document.getElementById(id);
            if (el && window.scrollY >= el.offsetTop - 120) current = id;
        }
        $$('.nav-links a').forEach(a => a.classList.toggle('active', a.dataset.section === current));
    });

    /* ============================================================
       MOBILE MENU
       ============================================================ */
    const mobileNav = $('#mobileNav');

    $('#mobileMenuOpen').addEventListener('click', () => {
        mobileNav.classList.add('open');
        document.body.classList.add('no-scroll');
    });

    function closeMobileMenu() {
        mobileNav.classList.remove('open');
        document.body.classList.remove('no-scroll');
    }

    $('#mobileMenuClose').addEventListener('click', closeMobileMenu);
    $$('.mobile-nav-link').forEach(a => a.addEventListener('click', closeMobileMenu));

    /* ============================================================
       AFFICHAGE DU MENU
       ============================================================ */
    function renderMenu(category) {
        const cat = MENU[category];
        if (!cat) return;

        menuGrid.innerHTML = '';

        cat.items.forEach((item, index) => {
            const card = document.createElement('div');
            card.className = 'menu-card';
            card.style.transitionDelay = `${index * 0.06}s`;

            // Prix affiché sur la carte
            let priceHTML = '';
            if (cat.hasSizes) {
                priceHTML = `
                    <div class="price-tag">${item.prices[0]}<span class="curr"> ${CONFIG.currency}</span></div>
                    <span class="price-sep">/</span>
                    <div class="price-small"><span>${item.prices[1]}</span> ${CONFIG.currency}</div>
                `;
            } else {
                priceHTML = `<div class="price-tag">${item.price}<span class="curr"> ${CONFIG.currency}</span></div>`;
            }

            // Tailles si applicable
            let sizeBadge = '';
            if (cat.hasSizes) {
                sizeBadge = `<span style="opacity:0.6"> · ${cat.sizeLabels.join(' / ')}</span>`;
            }

            // Liste des suppléments dispo (hinte sur la carte)
            let addonsHint = '';
            if (cat.addons.length > 0) {
                const hints = cat.addons.map(a => {
                    if (cat.hasSizes && a.prices) {
                        return `+${a.prices[0]}/${a.prices[1]} ${a.name}`;
                    }
                    return `+${a.price} ${a.name}`;
                });
                addonsHint = `<div class="card-addons-hint">${hints.map(h => `<span>${h}</span>`).join('')}</div>`;
            }

            card.innerHTML = `
                <div class="card-img">
                    <img src="${item.img}" alt="${item.name}" loading="lazy">
                    <div class="card-cat-badge">${cat.label}</div>
                    <div class="card-frites-badge"><i class="fas fa-french-fries" style="margin-right:3px;font-size:0.6rem"></i> +Frites</div>
                </div>
                <div class="card-body">
                    <div class="card-name">${item.name}${sizeBadge}</div>
                    <div class="card-price">${priceHTML}</div>
                    ${addonsHint}
                    <button class="card-order-btn" data-id="${item.id}" data-cat="${category}">
                        <i class="fas fa-plus"></i> Commander
                    </button>
                </div>
            `;

            menuGrid.appendChild(card);

            // Animation d'entrée
            requestAnimationFrame(() => {
                setTimeout(() => card.classList.add('visible'), 30 + index * 60);
            });
        });

        // Événements boutons commander
        $$('.card-order-btn').forEach(btn => {
            btn.addEventListener('click', e => {
                openOrderModal(e.currentTarget.dataset.id, e.currentTarget.dataset.cat);
            });
        });
    }

    /* ============================================================
       ONGLETS DE CATÉGORIE
       ============================================================ */
    $('#catTabs').addEventListener('click', e => {
        const tab = e.target.closest('.cat-tab');
        if (!tab) return;
        $$('.cat-tab').forEach(t => t.classList.remove('active'));
        tab.classList.add('active');
        currentCategory = tab.dataset.cat;
        renderMenu(currentCategory);
    });

    /* ============================================================
       MODAL DE COMMANDE D'UN ARTICLE
       ============================================================ */
    function openOrderModal(itemId, category) {
        const cat = MENU[category];
        const item = cat.items.find(i => i.id === itemId);
        if (!item) return;

        // Reset de l'état
        orderModalState = {
            item, category,
            selectedSize: 0,
            selectedAddons: [],
            quantity: 1
        };

        $('#orderModalImg').src = item.img;
        $('#orderModalImg').alt = item.name;

        let html = `<h3 class="modal-item-name">${item.name}</h3>`;

        // Sélecteur de taille (Tacos uniquement)
        if (cat.hasSizes) {
            html += `
                <div class="option-label"><i class="fas fa-ruler-horizontal"></i> Taille</div>
                <div class="size-options" id="sizeOptions">
                    ${cat.sizeLabels.map((label, i) => `
                        <div class="size-opt ${i === 0 ? 'selected' : ''}" data-size="${i}">
                            <div class="size-opt-name">${label}</div>
                            <div class="size-opt-price">${item.prices[i]} ${CONFIG.currency}</div>
                        </div>
                    `).join('')}
                </div>
            `;
        }

        // Suppléments : d'abord les frites (mis en avant), puis les autres
        html += `<div class="option-label" style="margin-top:8px;"><i class="fas fa-plus-circle"></i> Suppléments</div>`;

        cat.addons.forEach(addon => {
            const addonPrice = cat.hasSizes && addon.prices
                ? addon.prices[orderModalState.selectedSize]
                : addon.price;

            const fritesClass = addon.isFrites ? ' frites-addon' : '';
            const fritesIcon = addon.isFrites ? '<i class="fas fa-french-fries" style="margin-right:6px;color:#FFC107"></i>' : '';

            html += `
                <div class="addon-option${fritesClass}" data-addon="${addon.id}" data-price="${addonPrice}">
                    <div class="addon-check"><i class="fas fa-check"></i></div>
                    <span class="addon-text">${fritesIcon}${addon.name}</span>
                    <span class="addon-price">+${addonPrice} ${CONFIG.currency}</span>
                </div>
            `;
        });

        // Quantité
        html += `
            <div class="modal-qty-row">
                <span class="modal-qty-label">Quantité</span>
                <div class="modal-qty-controls">
                    <button class="modal-qty-btn" id="modalQtyMinus">&minus;</button>
                    <span class="modal-qty-val" id="modalQtyVal">1</span>
                    <button class="modal-qty-btn" id="modalQtyPlus">+</button>
                </div>
            </div>
        `;

        // Total
        const basePrice = cat.hasSizes ? item.prices[0] : item.price;
        html += `
            <div class="modal-total">
                <span class="modal-total-label">Total</span>
                <span class="modal-total-val" id="modalTotalVal">${basePrice} ${CONFIG.currency}</span>
            </div>
        `;

        html += `<button class="add-to-cart-btn" id="addToCartBtn"><i class="fas fa-cart-plus"></i> Ajouter au Panier</button>`;

        $('#orderModalBody').innerHTML = html;
        attachOrderModalEvents(cat, item);
        openModal($('#orderModal'));
    }

    /* ============================================================
       ÉVÉNEMENTS DU MODAL DE COMMANDE
       ============================================================ */
    function attachOrderModalEvents(cat, item) {
        // Sélection de taille
        const sizeOpts = $$('#sizeOptions .size-opt');
        if (sizeOpts.length > 0) {
            sizeOpts.forEach(opt => {
                opt.addEventListener('click', () => {
                    sizeOpts.forEach(o => o.classList.remove('selected'));
                    opt.classList.add('selected');
                    orderModalState.selectedSize = parseInt(opt.dataset.size);

                    // Mettre à jour le prix du gratin si la taille change
                    cat.addons.forEach(addon => {
                        if (addon.prices) {
                            const el = $(`.addon-option[data-addon="${addon.id}"]`);
                            if (el) {
                                const newPrice = addon.prices[orderModalState.selectedSize];
                                el.dataset.price = newPrice;
                                el.querySelector('.addon-price').textContent = `+${newPrice} ${CONFIG.currency}`;

                                // Mettre à jour dans selectedAddons si déjà coché
                                const existing = orderModalState.selectedAddons.find(a => a.id === addon.id);
                                if (existing) existing.price = newPrice;
                            }
                        }
                    });

                    updateModalTotal(cat, item);
                });
            });
        }

        // Toggle des suppléments
        $$('.addon-option').forEach(el => {
            el.addEventListener('click', () => {
                el.classList.toggle('checked');

                const addonId = el.dataset.addon;
                const addonPrice = parseFloat(el.dataset.price);

                if (el.classList.contains('checked')) {
                    orderModalState.selectedAddons.push({ id: addonId, price: addonPrice });
                } else {
                    orderModalState.selectedAddons = orderModalState.selectedAddons.filter(a => a.id !== addonId);
                }

                updateModalTotal(cat, item);
            });
        });

        // Quantité +/-
        $('#modalQtyMinus').addEventListener('click', () => {
            if (orderModalState.quantity > 1) {
                orderModalState.quantity--;
                $('#modalQtyVal').textContent = orderModalState.quantity;
                updateModalTotal(cat, item);
            }
        });
        $('#modalQtyPlus').addEventListener('click', () => {
            if (orderModalState.quantity < 20) {
                orderModalState.quantity++;
                $('#modalQtyVal').textContent = orderModalState.quantity;
                updateModalTotal(cat, item);
            }
        });

        // Ajouter au panier
        $('#addToCartBtn').addEventListener('click', () => {
            addToCart(cat, item);
            closeModal($('#orderModal'));
        });
    }

    /* Calcule et affiche le total dans le modal */
    function updateModalTotal(cat, item) {
        const basePrice = cat.hasSizes ? item.prices[orderModalState.selectedSize] : item.price;
        const addonsTotal = orderModalState.selectedAddons.reduce((s, a) => s + a.price, 0);
        const total = (basePrice + addonsTotal) * orderModalState.quantity;
        $('#modalTotalVal').textContent = `${total} ${CONFIG.currency}`;
    }

    /* ============================================================
       GESTION DU PANIER
       ============================================================ */
    function addToCart(cat, item) {
        const basePrice = cat.hasSizes ? item.prices[orderModalState.selectedSize] : item.price;
        const addonsTotal = orderModalState.selectedAddons.reduce((s, a) => s + a.price, 0);
        const unitPrice = basePrice + addonsTotal;

        // Construire le nom descriptif avec les options choisies
        let name = item.name;
        let details = [];

        if (cat.hasSizes) {
            details.push(cat.sizeLabels[orderModalState.selectedSize]);
        }

        if (orderModalState.selectedAddons.length > 0) {
            orderModalState.selectedAddons.forEach(a => {
                const addonDef = cat.addons.find(ad => ad.id === a.id);
                if (addonDef) details.push(addonDef.name);
            });
        }

        const detail = details.join(' + ');

        cart.push({
            id: item.id + '_' + Date.now(),
            name,
            detail,
            unitPrice,
            quantity: orderModalState.quantity,
            img: item.img
        });

        updateCartUI();

        // Animation feedback sur le badge
        const badge = $('#cartCount');
        badge.style.transform = 'scale(1.5)';
        setTimeout(() => { badge.style.transform = ''; }, 250);
    }

    function removeFromCart(index) {
        cart.splice(index, 1);
        updateCartUI();
    }

    function changeQty(index, delta) {
        cart[index].quantity = Math.min(20, Math.max(1, cart[index].quantity + delta));
        updateCartUI();
    }

    function getCartTotal() {
        return cart.reduce((s, i) => s + i.unitPrice * i.quantity, 0);
    }

    function getCartCount() {
        return cart.reduce((s, i) => s + i.quantity, 0);
    }

    /* Met à jour toute l'interface du panier */
    function updateCartUI() {
        const count = getCartCount();
        const total = getCartTotal();

        $('#cartCount').textContent = count;
        $('#cartCount').classList.toggle('show', count > 0);
        $('#cartTotal').textContent = `${total} ${CONFIG.currency}`;
        $('#checkoutBtn').disabled = cart.length === 0;

        if (cart.length === 0) {
            $('#cartItems').innerHTML = `
                <div class="cart-empty">
                    <i class="fas fa-shopping-bag"></i>
                    <p>Votre panier est vide</p>
                </div>`;
        } else {
            $('#cartItems').innerHTML = cart.map((item, i) => `
                <div class="cart-item">
                    <img class="cart-item-img" src="${item.img}" alt="${item.name}">
                    <div class="cart-item-info">
                        <div class="cart-item-name">${item.name}</div>
                        ${item.detail ? `<div class="cart-item-detail">${item.detail}</div>` : ''}
                        <div class="cart-item-price">${item.unitPrice * item.quantity} ${CONFIG.currency}</div>
                    </div>
                    <div class="cart-item-actions">
                        <div class="qty-controls">
                            <button class="qty-btn" onclick="changeQty(${i},-1)">&minus;</button>
                            <span class="qty-val">${item.quantity}</span>
                            <button class="qty-btn" onclick="changeQty(${i},1)">+</button>
                        </div>
                        <button class="remove-btn" onclick="removeFromCart(${i})"><i class="fas fa-trash-alt"></i></button>
                    </div>
                </div>
            `).join('');
        }
    }

    /* ============================================================
       OUVRIR/FERMER LE PANIER LATÉRAL
       ============================================================ */
    function openCart() {
        $('#cartOverlay').classList.add('open');
        $('#cartSidebar').classList.add('open');
        document.body.classList.add('no-scroll');
    }
    function closeCart() {
        $('#cartOverlay').classList.remove('open');
        $('#cartSidebar').classList.remove('open');
        document.body.classList.remove('no-scroll');
    }

    $('#cartToggle').addEventListener('click', openCart);
    $('#cartClose').addEventListener('click', closeCart);
    $('#cartOverlay').addEventListener('click', closeCart);

    /* ============================================================
       MODAL DE CHECKOUT (récapitulatif + formulaire)
       ============================================================ */
    $('#checkoutBtn').addEventListener('click', () => {
        if (cart.length === 0) return;
        closeCart();
        renderCheckoutSummary();
        setTimeout(() => openModal($('#checkoutModal')), 300);
    });

    function renderCheckoutSummary() {
        let html = cart.map(item => `
            <div class="checkout-summary-item">
                <span class="name">${item.quantity}x ${item.name}${item.detail ? ` (${item.detail})` : ''}</span>
                <span class="price">${item.unitPrice * item.quantity} ${CONFIG.currency}</span>
            </div>
        `).join('');

        html += `
            <div class="checkout-summary-total">
                <span class="name">Total</span>
                <span class="price">${getCartTotal()} ${CONFIG.currency}</span>
            </div>`;

        $('#checkoutSummary').innerHTML = html;
    }

    /* ============================================================
       VALIDATION DU FORMULAIRE & ENVOI
       ============================================================ */
    $('#checkoutForm').addEventListener('submit', e => {
        e.preventDefault();
        let valid = true;

        const name = $('#checkoutForm').name.value.trim();
        const phone = $('#checkoutForm').phone.value.trim();
        const address = $('#checkoutForm').address.value.trim();
        const notes = $('#checkoutForm').notes.value.trim();

        // Reset erreurs
        $$('.form-error').forEach(el => el.classList.remove('show'));

        if (!name) { $('#nameError').classList.add('show'); valid = false; }
        if (!/^0[67]\d{8}$/.test(phone)) { $('#phoneError').classList.add('show'); valid = false; }
        if (!address) { $('#addressError').classList.add('show'); valid = false; }

        if (!valid) return;

        // Construire et envoyer le message WhatsApp
        const message = buildWhatsAppMessage(name, phone, address, notes);
        sendWhatsApp(message);

        // Loading puis succès
        const btn = $('#submitOrderBtn');
        btn.disabled = true;
        btn.innerHTML = '<i class="fas fa-spinner fa-spin"></i> Envoi en cours...';

        setTimeout(() => {
            closeModal($('#checkoutModal'));
            $('#checkoutForm').reset();
            btn.disabled = false;
            btn.innerHTML = '<i class="fab fa-whatsapp"></i> Envoyer via WhatsApp';

            cart = [];
            updateCartUI();

            setTimeout(() => openModal($('#successModal')), 300);
        }, 1500);
    });

    /* ============================================================
       CONSTRUCTION DU MESSAGE WHATSAPP
       ============================================================ */
    function buildWhatsAppMessage(name, phone, address, notes) {
        let msg = `🛒 *Nouvelle Commande - ${CONFIG.restaurantName}*\n\n`;
        msg += `👤 *Client :* ${name}\n`;
        msg += `📱 *Téléphone :* ${phone}\n`;
        msg += `📍 *Adresse :* ${address}\n`;
        if (notes) msg += `📝 *Notes :* ${notes}\n`;
        msg += `\n*📋 Détail de la commande :*\n`;

        cart.forEach((item, i) => {
            msg += `${i + 1}. ${item.quantity}x ${item.name}`;
            if (item.detail) msg += ` (${item.detail})`;
            msg += ` → ${item.unitPrice * item.quantity} ${CONFIG.currency}\n`;
        });

        msg += `\n💰 *Total : ${getCartTotal()} ${CONFIG.currency}*`;
        return msg;
    }

    function sendWhatsApp(message) {
        const url = `https://wa.me/${CONFIG.restaurantPhone}?text=${encodeURIComponent(message)}`;
        window.open(url, '_blank');
    }

    /* ============================================================
       UTILITAIRES MODAL
       ============================================================ */
    function openModal(modal) {
        modal.classList.add('open');
        document.body.classList.add('no-scroll');
    }

    function closeModal(modal) {
        modal.classList.remove('open');
        if (!$('.modal-overlay.open') && !$('#cartSidebar').classList.contains('open')) {
            document.body.classList.remove('no-scroll');
        }
    }

    // Fermer en cliquant l'overlay
    $$('.modal-overlay').forEach(overlay => {
        overlay.addEventListener('click', e => {
            if (e.target === overlay) closeModal(overlay);
        });
    });

    // Boutons fermer
    $('#orderModalClose').addEventListener('click', () => closeModal($('#orderModal')));
    $('#checkoutModalClose').addEventListener('click', () => closeModal($('#checkoutModal')));
    $('#successClose').addEventListener('click', () => closeModal($('#successModal')));

    // Touche Escape
    document.addEventListener('keydown', e => {
        if (e.key !== 'Escape') return;
        if ($('#successModal').classList.contains('open')) closeModal($('#successModal'));
        else if ($('#checkoutModal').classList.contains('open')) closeModal($('#checkoutModal'));
        else if ($('#orderModal').classList.contains('open')) closeModal($('#orderModal'));
        else if ($('#cartSidebar').classList.contains('open')) closeCart();
        else if (mobileNav.classList.contains('open')) closeMobileMenu();
    });

    /* ============================================================
       INITIALISATION
       ============================================================ */
    renderMenu('tacos');
    updateCartUI();
    </script>
</body>
</html>
