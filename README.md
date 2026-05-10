<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Android Simulator - Arseniy Studio</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            font-family: 'Roboto', Arial, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: #000;
            height: 100vh;
            width: 100vw;
            overflow: hidden;
            position: fixed;
            font-size: 14px;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        /* Черная рамка телефона */
        .phone-frame {
            width: 400px;
            height: 810px;
            background: #000;
            border-radius: 40px;
            padding: 12px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.5);
            position: relative;
            border: 2px solid #333;
        }

        /* Вырез для камеры */
        .phone-notch {
            width: 180px;
            height: 24px;
            background: #000;
            position: absolute;
            top: 0;
            left: 50%;
            transform: translateX(-50%);
            border-bottom-left-radius: 16px;
            border-bottom-right-radius: 16px;
            z-index: 1001;
            display: flex;
            justify-content: center;
            align-items: flex-end;
            padding-bottom: 4px;
        }

        .front-camera {
            width: 16px;
            height: 16px;
            background: #222;
            border-radius: 8px;
        }

        /* Экран регистрации/входа */
        .auth-screen {
            width: 100%;
            height: 100%;
            background: #ffffff;
            border-radius: 32px;
            overflow: hidden;
            position: relative;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: space-between;
            padding: 60px 40px 60px 40px;
        }

        .auth-content {
            flex: 1;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            text-align: center;
            width: 100%;
        }

        .android-logo {
            width: 80px;
            height: 80px;
            margin-bottom: 30px;
            animation: float 3s ease-in-out infinite;
        }

        @keyframes float {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-10px); }
        }

        .android-logo svg {
            width: 100%;
            height: 100%;
            fill: #3DDC84;
        }

        .welcome-title {
            font-size: 28px;
            font-weight: 400;
            color: #202124;
            margin-bottom: 10px;
        }

        .auth-subtitle {
            font-size: 16px;
            color: #5F6368;
            margin-bottom: 30px;
        }

        /* Форма регистрации/входа */
        .auth-form {
            width: 100%;
            max-width: 280px;
            display: flex;
            flex-direction: column;
            gap: 16px;
            margin-bottom: 30px;
        }

        .form-group {
            text-align: left;
        }

        .form-label {
            display: block;
            font-size: 14px;
            color: #5F6368;
            margin-bottom: 6px;
            font-weight: 500;
        }

        .form-input {
            width: 100%;
            padding: 14px;
            border: 1px solid #dadce0;
            border-radius: 12px;
            font-size: 16px;
            transition: border 0.2s;
            background: #ffffff;
        }

        .form-input:focus {
            outline: none;
            border-color: #1a73e8;
            box-shadow: 0 0 0 2px rgba(26, 115, 232, 0.2);
        }

        .form-input.error {
            border-color: #d93025;
            background: #fce8e6;
        }

        .password-container {
            position: relative;
        }

        .toggle-password {
            position: absolute;
            right: 12px;
            top: 50%;
            transform: translateY(-50%);
            background: none;
            border: none;
            color: #5F6368;
            cursor: pointer;
            font-size: 18px;
        }

        .form-error {
            color: #d93025;
            font-size: 12px;
            margin-top: 4px;
            text-align: left;
            display: none;
        }

        /* Переключатель между регистрацией и входом */
        .auth-toggle {
            display: flex;
            gap: 20px;
            margin-bottom: 30px;
        }

        .auth-tab {
            background: none;
            border: none;
            font-size: 16px;
            font-weight: 500;
            color: #5F6368;
            cursor: pointer;
            padding: 8px 0;
            position: relative;
        }

        .auth-tab.active {
            color: #1a73e8;
        }

        .auth-tab.active::after {
            content: '';
            position: absolute;
            bottom: -4px;
            left: 0;
            right: 0;
            height: 3px;
            background: #1a73e8;
            border-radius: 3px;
        }

        /* Кнопки */
        .auth-btn {
            width: 100%;
            max-width: 280px;
            height: 48px;
            background: #1a73e8;
            color: white;
            border: none;
            border-radius: 24px;
            font-size: 16px;
            font-weight: 500;
            cursor: pointer;
            transition: background 0.2s;
            margin-top: 10px;
        }

        .auth-btn:hover {
            background: #0d62d9;
        }

        .auth-btn:active {
            background: #0a56c2;
        }

        .auth-btn:disabled {
            background: #ccc;
            cursor: not-allowed;
        }

        .quick-access-btn {
            width: 100%;
            max-width: 280px;
            height: 48px;
            background: #f8f9fa;
            color: #5F6368;
            border: 1px solid #dadce0;
            border-radius: 24px;
            font-size: 14px;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.2s;
            margin-top: 12px;
        }

        .quick-access-btn:hover {
            background: #f0f0f0;
        }

        /* Домашний экран */
        .home-screen {
            width: 100%;
            height: 100%;
            background: #ffffff;
            border-radius: 32px;
            overflow: hidden;
            position: relative;
            display: none;
            flex-direction: column;
        }

        .status-bar {
            height: 28px;
            background: #ffffff;
            padding: 0 24px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-size: 12px;
            font-weight: 500;
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            z-index: 1000;
            border-bottom: 1px solid #e0e0e0;
        }

        /* Сетка приложений */
        .app-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 12px;
            padding: 16px;
            margin-top: 44px;
            flex: 1;
            overflow-y: auto;
        }

        .app-icon {
            width: 72px;
            height: 92px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            gap: 8px;
            cursor: pointer;
            border-radius: 28px;
            transition: all 0.2s;
            background: none;
            border: none;
            outline: none;
        }

        .app-icon:hover {
            background: rgba(0, 0, 0, 0.05);
            transform: translateY(-2px);
        }

        .app-icon:active {
            transform: translateY(0);
        }

        .app-icon-image {
            width: 52px;
            height: 52px;
            border-radius: 26px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            font-weight: 500;
            color: white;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
        }

        .chrome-icon { background: #4285F4; }
        .phone-icon { background: #34A853; }
        .messages-icon { background: #1A73E8; }
        .camera-icon { background: #EA4335; }
        .gallery-icon { background: #FBBC04; }
        .contacts-icon { background: #9C27B0; }
        .calculator-icon { background: #5F6368; }
        .settings-icon { background: #5F6368; }
        .music-icon { background: #9C27B0; }
        .weather-icon { background: #00BCD4; }
        .notes-icon { background: #FF9800; }
        .clock-icon { background: #607D8B; }

        .app-label {
            font-size: 12px;
            color: #000;
            text-align: center;
            max-width: 72px;
            font-weight: 500;
        }

        /* Панель навигации */
        .navigation-bar {
            height: 68px;
            background: #ffffff;
            border-top: 1px solid #e0e0e0;
            display: flex;
            justify-content: space-around;
            align-items: center;
            padding-bottom: 8px;
            flex-shrink: 0;
        }

        .nav-btn {
            width: 44px;
            height: 44px;
            border-radius: 22px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            background: none;
            border: none;
            color: #666;
            font-size: 24px;
            transition: all 0.2s;
        }

        .nav-btn:hover {
            background: rgba(0, 0, 0, 0.05);
        }

        /* ==================== БРАУЗЕР ==================== */
        .browser-screen {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: #ffffff;
            display: none;
            flex-direction: column;
            border-radius: 32px;
            z-index: 1001;
        }

        .browser-header {
            height: 56px;
            padding: 0 16px;
            display: flex;
            align-items: center;
            gap: 12px;
            background: #ffffff;
            border-bottom: 1px solid #e0e0e0;
            flex-shrink: 0;
        }

        .browser-toolbar {
            display: flex;
            align-items: center;
            gap: 8px;
            flex: 1;
        }

        .browser-url-bar {
            flex: 1;
            height: 40px;
            background: #f8f9fa;
            border-radius: 20px;
            display: flex;
            align-items: center;
            padding: 0 16px;
            border: 1px solid #dadce0;
        }

        .browser-url-input {
            flex: 1;
            background: transparent;
            border: none;
            outline: none;
            font-size: 14px;
            color: #000;
            padding: 8px 0;
        }

        .browser-content {
            flex: 1;
            overflow: hidden;
            position: relative;
            background: #ffffff;
        }

        .browser-tabs {
            display: flex;
            gap: 8px;
            padding: 12px 16px;
            background: #f8f9fa;
            border-bottom: 1px solid #e0e0e0;
            overflow-x: auto;
        }

        .browser-tab {
            padding: 8px 16px;
            background: white;
            border-radius: 8px;
            border: 1px solid #e0e0e0;
            min-width: 120px;
            display: flex;
            align-items: center;
            gap: 8px;
            cursor: pointer;
            flex-shrink: 0;
        }

        .browser-tab.active {
            background: #4285F4;
            color: white;
            border-color: #4285F4;
        }

        .browser-tab-close {
            margin-left: auto;
            background: none;
            border: none;
            color: inherit;
            cursor: pointer;
            opacity: 0.7;
        }

        .browser-tab-close:hover {
            opacity: 1;
        }

        .browser-view {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: white;
            display: none;
        }

        .browser-view.active {
            display: block;
        }

        .browser-iframe {
            width: 100%;
            height: 100%;
            border: none;
            background: white;
        }

        .browser-loading {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            background: white;
        }

        .browser-error {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            display: none;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 40px;
            text-align: center;
            background: white;
        }

        .browser-bookmarks {
            position: absolute;
            top: 100%;
            left: 0;
            right: 0;
            background: white;
            border-radius: 0 0 12px 12px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.15);
            display: none;
            z-index: 1002;
            max-height: 300px;
            overflow-y: auto;
        }

        .browser-bookmarks.active {
            display: block;
        }

        .bookmark-item {
            padding: 12px 16px;
            display: flex;
            align-items: center;
            gap: 12px;
            cursor: pointer;
            border-bottom: 1px solid #f0f0f0;
        }

        .bookmark-item:hover {
            background: #f8f9fa;
        }

        .bookmark-icon {
            width: 24px;
            height: 24px;
            border-radius: 4px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-size: 12px;
        }

        .bookmark-info {
            flex: 1;
        }

        .bookmark-title {
            font-weight: 500;
            margin-bottom: 2px;
        }

        .bookmark-url {
            font-size: 12px;
            color: #666;
        }

        /* ==================== ДРУГИЕ ПРИЛОЖЕНИЯ ==================== */
        .app-screen {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: #ffffff;
            display: none;
            flex-direction: column;
            border-radius: 32px;
            z-index: 1001;
        }

        .app-header {
            height: 56px;
            padding: 0 16px;
            display: flex;
            align-items: center;
            gap: 16px;
            background: #ffffff;
            border-bottom: 1px solid #e0e0e0;
            flex-shrink: 0;
        }

        .app-title {
            font-size: 20px;
            font-weight: 500;
            flex: 1;
            color: #000;
        }

        .app-content {
            flex: 1;
            overflow-y: auto;
            padding: 16px;
            background: #ffffff;
        }

        /* Телефон */
        .dial-pad {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 16px;
            margin-top: 32px;
        }

        .dial-btn {
            height: 72px;
            border-radius: 36px;
            background: #f8f9fa;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            border: none;
            color: #000;
            border: 1px solid #e0e0e0;
            transition: all 0.2s;
        }

        .dial-btn:hover {
            background: #f0f0f0;
        }

        .dial-btn:active {
            background: #e8e8e8;
        }

        .dial-number {
            font-size: 32px;
            font-weight: 400;
        }

        .dial-letters {
            font-size: 12px;
            color: #666;
            margin-top: 4px;
        }

        /* Калькулятор */
        .calculator-display {
            background: #000;
            color: #fff;
            padding: 20px;
            border-radius: 12px;
            margin-bottom: 20px;
            text-align: right;
            font-family: monospace;
            min-height: 80px;
            display: flex;
            flex-direction: column;
            justify-content: flex-end;
        }

        .calculator-expression {
            font-size: 14px;
            color: #aaa;
            margin-bottom: 8px;
            min-height: 20px;
        }

        .calculator-result {
            font-size: 32px;
            font-weight: 300;
        }

        .calculator-buttons {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
        }

        .calc-btn {
            height: 60px;
            border-radius: 30px;
            border: none;
            font-size: 20px;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.2s;
        }

        .calc-btn.number {
            background: #f8f9fa;
            color: #000;
        }

        .calc-btn.operator {
            background: #f0f0f0;
            color: #1a73e8;
        }

        .calc-btn.equals {
            background: #1a73e8;
            color: white;
        }

        .calc-btn.clear {
            background: #ea4335;
            color: white;
        }

        .calc-btn:hover {
            opacity: 0.9;
        }

        /* Утилиты */
        .hidden {
            display: none !important;
        }

        .studio-credit {
            font-size: 14px;
            color: #5F6368;
            text-align: center;
            margin-top: 20px;
            line-height: 1.6;
        }
    </style>
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
</head>
<body>
    <!-- Черная рамка телефона -->
    <div class="phone-frame">
        <!-- Вырез для камеры -->
        <div class="phone-notch">
            <div class="front-camera"></div>
        </div>
        
        <!-- Экран регистрации/входа -->
        <div class="auth-screen" id="authScreen">
            <div class="auth-content">
                <div class="android-logo">
                    <svg viewBox="0 0 24 24">
                        <path d="M6,18c0,0.55,0.45,1,1,1h1v3.5c0,0.83,0.67,1.5,1.5,1.5s1.5-0.67,1.5-1.5V19h2v3.5c0,0.83,0.67,1.5,1.5,1.5s1.5-0.67,1.5-1.5V19h1c0.55,0,1-0.45,1-1V8H6V18z M3.5,8C2.67,8,2,8.67,2,9.5v7c0,0.83,0.67,1.5,1.5,1.5S5,17.33,5,16.5v-7C5,8.67,4.33,8,3.5,8z M20.5,8c-0.83,0-1.5,0.67-1.5,1.5v7c0,0.83,0.67,1.5,1.5,1.5s1.5-0.67,1.5-1.5v-7C22,8.67,21.33,8,20.5,8z M15.53,2.16l1.3-1.3c0.31-0.31,0.85-0.09,0.85,0.35v3.5c0,0.45-0.54,0.67-0.85,0.35l-1.3-1.3C15.23,3.29,14.77,3.29,15.53,2.16z M8.47,2.16L7.17,3.46c-0.31,0.31-0.85,0.09-0.85-0.35v-3.5c0-0.45,0.54-0.67,0.85-0.35l1.3,1.3C8.77,0.71,9.23,0.71,8.47,2.16z"/>
                    </svg>
                </div>
                
                <!-- Переключатель Регистрация/Вход -->
                <div class="auth-toggle">
                    <button class="auth-tab active" id="registerTab">Регистрация</button>
                    <button class="auth-tab" id="loginTab">Вход</button>
                </div>
                
                <h1 class="welcome-title" id="authTitle">Создайте аккаунт</h1>
                <div class="auth-subtitle" id="authSubtitle">Для использования Android Simulator</div>
                
                <!-- Форма регистрации -->
                <form class="auth-form" id="registerForm">
                    <div class="form-group">
                        <label class="form-label">Имя и фамилия</label>
                        <input type="text" class="form-input" id="registerName" 
                               placeholder="Иван Иванов" required>
                        <div class="form-error" id="nameError"></div>
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label">Электронная почта</label>
                        <input type="email" class="form-input" id="registerEmail" 
                               placeholder="example@mail.ru" required>
                        <div class="form-error" id="emailError"></div>
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label">Пароль</label>
                        <div class="password-container">
                            <input type="password" class="form-input" id="registerPassword" 
                                   placeholder="Не менее 6 символов" required minlength="6">
                            <button type="button" class="toggle-password" data-target="registerPassword">
                                <i class="far fa-eye"></i>
                            </button>
                        </div>
                        <div class="form-error" id="passwordError"></div>
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label">Подтвердите пароль</label>
                        <div class="password-container">
                            <input type="password" class="form-input" id="registerConfirmPassword" 
                                   placeholder="Повторите пароль" required>
                            <button type="button" class="toggle-password" data-target="registerConfirmPassword">
                                <i class="far fa-eye"></i>
                            </button>
                        </div>
                        <div class="form-error" id="confirmPasswordError"></div>
                    </div>
                    
                    <button type="submit" class="auth-btn" id="registerBtn">
                        Зарегистрироваться
                    </button>
                </form>
                
                <!-- Форма входа -->
                <form class="auth-form hidden" id="loginForm">
                    <div class="form-group">
                        <label class="form-label">Электронная почта</label>
                        <input type="email" class="form-input" id="loginEmail" 
                               placeholder="example@mail.ru" required>
                        <div class="form-error" id="loginEmailError"></div>
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label">Пароль</label>
                        <div class="password-container">
                            <input type="password" class="form-input" id="loginPassword" 
                                   placeholder="Введите ваш пароль" required>
                            <button type="button" class="toggle-password" data-target="loginPassword">
                                <i class="far fa-eye"></i>
                            </button>
                        </div>
                        <div class="form-error" id="loginPasswordError"></div>
                    </div>
                    
                    <button type="submit" class="auth-btn" id="loginSubmitBtn">
                        Войти в аккаунт
                    </button>
                    
                    <button type="button" class="quick-access-btn" id="quickLoginBtn">
                        Быстрый вход (тест)
                    </button>
                </form>
                
                <!-- Информация о пользователе после входа -->
                <div class="user-info" id="userInfo">
                    <div class="user-avatar" id="userAvatar">И</div>
                    <div class="user-details">
                        <div class="user-name" id="userName">Иван Иванов</div>
                        <div class="user-email" id="userEmail">user@example.com</div>
                    </div>
                </div>
                
                <!-- Сообщение об ошибке -->
                <div class="error-message" id="errorMessage">
                    Ошибка авторизации. Попробуйте еще раз.
                </div>
                
                <!-- Индикатор загрузки -->
                <div class="loading" id="loading">
                    <div class="spinner"></div>
                    <div id="loadingText">Выполняется вход...</div>
                </div>
                
                <div class="studio-credit">
                    Android Simulator<br>
                    <span style="font-size: 12px; color: #9AA0A6;">by Arseniy Studio</span>
                </div>
            </div>
            
            <div style="width: 100%; text-align: center; padding: 20px 0;">
                <button class="auth-btn" id="enterSimulatorBtn" disabled>
                    Войти в симулятор
                </button>
            </div>
        </div>

        <!-- Домашний экран -->
        <div class="home-screen" id="homeScreen">
            <!-- Статус бар -->
            <div class="status-bar">
                <div id="currentTime">19:30</div>
                <div id="homeUserName"></div>
                <div>📶 100% 🔋</div>
            </div>

            <!-- Сетка приложений -->
            <div class="app-grid">
                <button class="app-icon" data-app="browser">
                    <div class="app-icon-image chrome-icon">
                        <i class="fas fa-globe"></i>
                    </div>
                    <div class="app-label">Chrome</div>
                </button>
                <button class="app-icon" data-app="phone">
                    <div class="app-icon-image phone-icon">
                        <i class="fas fa-phone"></i>
                    </div>
                    <div class="app-label">Телефон</div>
                </button>
                <button class="app-icon" data-app="calculator">
                    <div class="app-icon-image calculator-icon">
                        <i class="fas fa-calculator"></i>
                    </div>
                    <div class="app-label">Калькулятор</div>
                </button>
                <button class="app-icon" data-app="settings">
                    <div class="app-icon-image settings-icon">
                        <i class="fas fa-cog"></i>
                    </div>
                    <div class="app-label">Настройки</div>
                </button>
            </div>

            <!-- Панель навигации -->
            <div class="navigation-bar">
                <button class="nav-btn" id="recentBtn">
                    <i class="fas fa-square"></i>
                </button>
                <button class="nav-btn active" id="homeBtn">
                    <i class="fas fa-home"></i>
                </button>
                <button class="nav-btn" id="backBtn">
                    <i class="fas fa-arrow-left"></i>
                </button>
            </div>
        </div>

        <!-- ==================== БРАУЗЕР ==================== -->
        <div id="browserApp" class="browser-screen">
            <div class="browser-header">
                <button class="nav-btn back-btn" data-app="browser">
                    <i class="fas fa-arrow-left"></i>
                </button>
                
                <div class="browser-toolbar">
                    <button class="nav-btn" id="browserBackBtn" title="Назад">
                        <i class="fas fa-chevron-left"></i>
                    </button>
                    <button class="nav-btn" id="browserForwardBtn" title="Вперед">
                        <i class="fas fa-chevron-right"></i>
                    </button>
                    <button class="nav-btn" id="browserRefreshBtn" title="Обновить">
                        <i class="fas fa-redo"></i>
                    </button>
                    
                    <div class="browser-url-bar">
                        <i class="fas fa-lock" style="color: #34A853; margin-right: 8px; font-size: 12px;"></i>
                        <input type="text" class="browser-url-input" id="browserUrlInput" 
                               placeholder="Введите адрес сайта" value="https://www.google.com">
                        <button class="nav-btn" id="browserGoBtn" style="margin-left: 8px;">
                            <i class="fas fa-arrow-right"></i>
                        </button>
                    </div>
                    
                    <button class="nav-btn" id="browserBookmarksBtn" title="Закладки">
                        <i class="fas fa-star"></i>
                    </button>
                    <button class="nav-btn" id="browserMenuBtn" title="Меню">
                        <i class="fas fa-ellipsis-v"></i>
                    </button>
                </div>
            </div>

            <div class="browser-tabs" id="browserTabs">
                <!-- Вкладки будут добавляться динамически -->
            </div>

            <div class="browser-content" id="browserContent">
                <!-- Контент вкладок -->
                <div class="browser-view active" id="tab-1">
                    <iframe class="browser-iframe" 
                            src="https://www.google.com" 
                            allow="geolocation; microphone; camera; midi; encrypted-media"
                            sandbox="allow-same-origin allow-scripts allow-popups allow-forms"
                            loading="lazy"></iframe>
                </div>
            </div>

            <!-- Закладки -->
            <div class="browser-bookmarks" id="browserBookmarks">
                <div class="bookmark-item" data-url="https://www.google.com">
                    <div class="bookmark-icon" style="background: #4285F4;">G</div>
                    <div class="bookmark-info">
                        <div class="bookmark-title">Google</div>
                        <div class="bookmark-url">www.google.com</div>
                    </div>
                </div>
                <div class="bookmark-item" data-url="https://www.youtube.com">
                    <div class="bookmark-icon" style="background: #FF0000;">YT</div>
                    <div class="bookmark-info">
                        <div class="bookmark-title">YouTube</div>
                        <div class="bookmark-url">www.youtube.com</div>
                    </div>
                </div>
                <div class="bookmark-item" data-url="https://ru.wikipedia.org">
                    <div class="bookmark-icon" style="background: #636466;">W</div>
                    <div class="bookmark-info">
                        <div class="bookmark-title">Wikipedia</div>
                        <div class="bookmark-url">ru.wikipedia.org</div>
                    </div>
                </div>
                <div class="bookmark-item" data-url="https://github.com">
                    <div class="bookmark-icon" style="background: #333;">GH</div>
                    <div class="bookmark-info">
                        <div class="bookmark-title">GitHub</div>
                        <div class="bookmark-url">github.com</div>
                    </div>
                </div>
                <div class="bookmark-item" data-url="https://www.weather.com">
                    <div class="bookmark-icon" style="background: #00BCD4;">W</div>
                    <div class="bookmark-info">
                        <div class="bookmark-title">Weather</div>
                        <div class="bookmark-url">www.weather.com</div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Приложение: Телефон -->
        <div id="phoneApp" class="app-screen">
            <div class="app-header">
                <button class="nav-btn back-btn" data-app="phone">
                    <i class="fas fa-arrow-left"></i>
                </button>
                <div class="app-title">Телефон</div>
            </div>
            <div class="app-content">
                <div style="text-align: center; margin: 40px 0;">
                    <div style="font-size: 48px; font-weight: 300; margin: 16px 0;" id="phoneNumberDisplay">+7</div>
                    <div style="color: #666; font-size: 16px;">Наберите номер</div>
                </div>
                <div class="dial-pad">
                    <button class="dial-btn" data-number="1"><div class="dial-number">1</div></button>
                    <button class="dial-btn" data-number="2"><div class="dial-number">2</div><div class="dial-letters">ABC</div></button>
                    <button class="dial-btn" data-number="3"><div class="dial-number">3</div><div class="dial-letters">DEF</div></button>
                    <button class="dial-btn" data-number="4"><div class="dial-number">4</div><div class="dial-letters">GHI</div></button>
                    <button class="dial-btn" data-number="5"><div class="dial-number">5</div><div class="dial-letters">JKL</div></button>
                    <button class="dial-btn" data-number="6"><div class="dial-number">6</div><div class="dial-letters">MNO</div></button>
                    <button class="dial-btn" data-number="7"><div class="dial-number">7</div><div class="dial-letters">PQRS</div></button>
                    <button class="dial-btn" data-number="8"><div class="dial-number">8</div><div class="dial-letters">TUV</div></button>
                    <button class="dial-btn" data-number="9"><div class="dial-number">9</div><div class="dial-letters">WXYZ</div></button>
                    <button class="dial-btn" data-number="*"><div class="dial-number">*</div></button>
                    <button class="dial-btn" data-number="0"><div class="dial-number">0</div><div class="dial-letters">+</div></button>
                    <button class="dial-btn" data-number="#"><div class="dial-number">#</div></button>
                </div>
                <div style="display: flex; gap: 32px; margin-top: 40px; justify-content: center;">
                    <button class="nav-btn" style="background: #f44336; color: white;" id="clearDialBtn">
                        <i class="fas fa-times"></i>
                    </button>
                    <button class="nav-btn" style="background: #4CAF50; color: white; width: 64px; height: 64px;" id="makeCallBtn">
                        <i class="fas fa-phone"></i>
                    </button>
                </div>
            </div>
        </div>

        <!-- Приложение: Калькулятор -->
        <div id="calculatorApp" class="app-screen">
            <div class="app-header">
                <button class="nav-btn back-btn" data-app="calculator">
                    <i class="fas fa-arrow-left"></i>
                </button>
                <div class="app-title">Калькулятор</div>
            </div>
            <div class="app-content">
                <div class="calculator-display">
                    <div class="calculator-expression" id="calcExpression"></div>
                    <div class="calculator-result" id="calcResult">0</div>
                </div>
                <div class="calculator-buttons">
                    <button class="calc-btn clear" data-action="clear">C</button>
                    <button class="calc-btn clear" data-action="clearEntry">CE</button>
                    <button class="calc-btn operator" data-action="backspace">⌫</button>
                    <button class="calc-btn operator" data-action="/">/</button>
                    
                    <button class="calc-btn number" data-number="7">7</button>
                    <button class="calc-btn number" data-number="8">8</button>
                    <button class="calc-btn number" data-number="9">9</button>
                    <button class="calc-btn operator" data-action="*">×</button>
                    
                    <button class="calc-btn number" data-number="4">4</button>
                    <button class="calc-btn number" data-number="5">5</button>
                    <button class="calc-btn number" data-number="6">6</button>
                    <button class="calc-btn operator" data-action="-">−</button>
                    
                    <button class="calc-btn number" data-number="1">1</button>
                    <button class="calc-btn number" data-number="2">2</button>
                    <button class="calc-btn number" data-number="3">3</button>
                    <button class="calc-btn operator" data-action="+">+</button>
                    
                    <button class="calc-btn number" data-number="0" style="grid-column: span 2;">0</button>
                    <button class="calc-btn number" data-action=".">.</button>
                    <button class="calc-btn equals" data-action="=">=</button>
                </div>
            </div>
        </div>

        <!-- Приложение: Настройки -->
        <div id="settingsApp" class="app-screen">
            <div class="app-header">
                <button class="nav-btn back-btn" data-app="settings">
                    <i class="fas fa-arrow-left"></i>
                </button>
                <div class="app-title">Настройки</div>
            </div>
            <div class="app-content">
                <div style="background: #f8f9fa; border-radius: 16px; overflow: hidden; border: 1px solid #e0e0e0;">
                    <div style="padding: 20px 16px; display: flex; align-items: center; gap: 16px; border-bottom: 1px solid #e0e0e0;">
                        <div style="width: 40px; height: 40px; border-radius: 20px; background: #e0e0e0; display: flex; align-items: center; justify-content: center;">
                            <i class="fas fa-wifi"></i>
                        </div>
                        <div style="flex: 1;">
                            <div style="font-weight: 500; margin-bottom: 4px;">Wi-Fi</div>
                            <div style="font-size: 14px; color: #666;">Подключено</div>
                        </div>
                        <div style="width: 52px; height: 32px; background: #34A853; border-radius: 16px; position: relative;">
                            <div style="position: absolute; top: 4px; right: 4px; width: 24px; height: 24px; background: #fff; border-radius: 12px;"></div>
                        </div>
                    </div>
                    <div style="padding: 20px 16px; display: flex; align-items: center; gap: 16px;">
                        <div style="width: 40px; height: 40px; border-radius: 20px; background: #e0e0e0; display: flex; align-items: center; justify-content: center;">
                            <i class="fas fa-bluetooth"></i>
                        </div>
                        <div style="flex: 1;">
                            <div style="font-weight: 500; margin-bottom: 4px;">Bluetooth</div>
                            <div style="font-size: 14px; color: #666;">Выключено</div>
                        </div>
                        <div style="width: 52px; height: 32px; background: #e0e0e0; border-radius: 16px; position: relative;">
                            <div style="position: absolute; top: 4px; left: 4px; width: 24px; height: 24px; background: #fff; border-radius: 12px;"></div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // ==================== ОСНОВНОЙ КОД ====================
        const USER_DB_KEY = 'android_simulator_users';
        const CURRENT_USER_KEY = 'android_simulator_current_user';
        
        let currentUser = null;
        let isLoggedIn = false;

        // ==================== ДАННЫЕ БРАУЗЕРА ====================
        const browser = {
            tabs: [
                {
                    id: 1,
                    title: "Google",
                    url: "https://www.google.com",
                    favicon: "G",
                    color: "#4285F4",
                    active: true,
                    loading: false
                }
            ],
            currentTabId: 1,
            history: [],
            bookmarks: [
                { title: "Google", url: "https://www.google.com", favicon: "G", color: "#4285F4" },
                { title: "YouTube", url: "https://www.youtube.com", favicon: "YT", color: "#FF0000" },
                { title: "Wikipedia", url: "https://ru.wikipedia.org", favicon: "W", color: "#636466" },
                { title: "GitHub", url: "https://github.com", favicon: "GH", color: "#333" },
                { title: "Weather", url: "https://www.weather.com", favicon: "W", color: "#00BCD4" }
            ]
        };

        // Телефон
        let phoneNumber = '+7';
        
        // Калькулятор
        let calculator = {
            currentInput: '0',
            previousInput: '',
            operator: '',
            expression: ''
        };

        // ==================== ИНИЦИАЛИЗАЦИЯ ====================
        document.addEventListener('DOMContentLoaded', function() {
            initializeApp();
            setupEventListeners();
            updateTime();
            checkAutoLogin();
            
            setInterval(updateTime, 60000);
        });

        function initializeApp() {
            if (!localStorage.getItem(USER_DB_KEY)) {
                localStorage.setItem(USER_DB_KEY, JSON.stringify([]));
            }
            
            const users = getUsers();
            if (users.length === 0) {
                const testUser = {
                    id: 'test_' + Date.now(),
                    name: 'Тестовый Пользователь',
                    email: 'test@example.com',
                    password: '123456',
                    createdAt: new Date().toISOString()
                };
                users.push(testUser);
                saveUsers(users);
            }
        }

        function getUsers() {
            const usersJson = localStorage.getItem(USER_DB_KEY);
            return usersJson ? JSON.parse(usersJson) : [];
        }

        function saveUsers(users) {
            localStorage.setItem(USER_DB_KEY, JSON.stringify(users));
        }

        function checkAutoLogin() {
            const savedUser = localStorage.getItem(CURRENT_USER_KEY);
            if (savedUser) {
                try {
                    currentUser = JSON.parse(savedUser);
                    showUserInfo();
                    document.getElementById('enterSimulatorBtn').disabled = false;
                    showNotification(`С возвращением, ${currentUser.name}!`);
                } catch (e) {
                    localStorage.removeItem(CURRENT_USER_KEY);
                }
            }
        }

        // ==================== РЕГИСТРАЦИЯ И ВХОД ====================
        function setupEventListeners() {
            // Регистрация/вход
            document.getElementById('registerTab').addEventListener('click', switchToRegister);
            document.getElementById('loginTab').addEventListener('click', switchToLogin);
            
            document.querySelectorAll('.toggle-password').forEach(btn => {
                btn.addEventListener('click', togglePasswordVisibility);
            });
            
            document.getElementById('registerForm').addEventListener('submit', function(e) {
                e.preventDefault();
                registerUser();
            });
            
            document.getElementById('loginForm').addEventListener('submit', function(e) {
                e.preventDefault();
                loginUser();
            });
            
            document.getElementById('quickLoginBtn').addEventListener('click', quickLogin);
            document.getElementById('enterSimulatorBtn').addEventListener('click', enterSimulator);
            
            // Навигация
            document.getElementById('backBtn').addEventListener('click', goBack);
            document.getElementById('homeBtn').addEventListener('click', goHome);
            
            // Приложения
            setupAppListeners();
            
            // Браузер
            setupBrowserListeners();
            
            // Телефон
            setupPhoneListeners();
            
            // Калькулятор
            setupCalculatorListeners();
        }

        function switchToRegister() {
            document.getElementById('registerTab').classList.add('active');
            document.getElementById('loginTab').classList.remove('active');
            document.getElementById('registerForm').classList.remove('hidden');
            document.getElementById('loginForm').classList.add('hidden');
            document.getElementById('authTitle').textContent = 'Создайте аккаунт';
            document.getElementById('authSubtitle').textContent = 'Для использования Android Simulator';
        }

        function switchToLogin() {
            document.getElementById('loginTab').classList.add('active');
            document.getElementById('registerTab').classList.remove('active');
            document.getElementById('loginForm').classList.remove('hidden');
            document.getElementById('registerForm').classList.add('hidden');
            document.getElementById('authTitle').textContent = 'Вход в аккаунт';
            document.getElementById('authSubtitle').textContent = 'Добро пожаловать обратно';
        }

        function togglePasswordVisibility(e) {
            const targetId = e.currentTarget.getAttribute('data-target');
            const input = document.getElementById(targetId);
            const icon = e.currentTarget.querySelector('i');
            
            if (input.type === 'password') {
                input.type = 'text';
                icon.classList.remove('fa-eye');
                icon.classList.add('fa-eye-slash');
            } else {
                input.type = 'password';
                icon.classList.remove('fa-eye-slash');
                icon.classList.add('fa-eye');
            }
        }

        function registerUser() {
            const name = document.getElementById('registerName').value.trim();
            const email = document.getElementById('registerEmail').value.trim();
            const password = document.getElementById('registerPassword').value;
            const confirmPassword = document.getElementById('registerConfirmPassword').value;
            
            if (!name || !email || !password || password !== confirmPassword) {
                showError('Пожалуйста, заполните все поля правильно');
                return;
            }
            
            showLoading(true, 'Регистрация...');
            
            setTimeout(() => {
                const newUser = {
                    id: 'user_' + Date.now(),
                    name: name,
                    email: email,
                    password: password,
                    createdAt: new Date().toISOString()
                };
                
                const users = getUsers();
                users.push(newUser);
                saveUsers(users);
                
                currentUser = newUser;
                localStorage.setItem(CURRENT_USER_KEY, JSON.stringify(newUser));
                
                showUserInfo();
                showLoading(false);
                document.getElementById('enterSimulatorBtn').disabled = false;
                
                showSuccess('Аккаунт успешно создан!');
                
                document.getElementById('registerForm').classList.add('hidden');
                document.getElementById('loginForm').classList.add('hidden');
                document.getElementById('authToggle').classList.add('hidden');
            }, 1500);
        }

        function loginUser() {
            const email = document.getElementById('loginEmail').value.trim();
            const password = document.getElementById('loginPassword').value;
            
            if (!email || !password) {
                showError('Введите email и пароль');
                return;
            }
            
            showLoading(true, 'Вход...');
            
            setTimeout(() => {
                const users = getUsers();
                const user = users.find(u => u.email === email && u.password === password);
                
                if (user) {
                    currentUser = user;
                    localStorage.setItem(CURRENT_USER_KEY, JSON.stringify(user));
                    
                    showUserInfo();
                    showLoading(false);
                    document.getElementById('enterSimulatorBtn').disabled = false;
                    
                    showSuccess('Вход выполнен успешно!');
                    
                    document.getElementById('loginForm').classList.add('hidden');
                    document.getElementById('registerForm').classList.add('hidden');
                    document.getElementById('authToggle').classList.add('hidden');
                } else {
                    showLoading(false);
                    showError('Неверный email или пароль');
                }
            }, 1000);
        }

        function quickLogin() {
            showLoading(true, 'Быстрый вход...');
            
            setTimeout(() => {
                const users = getUsers();
                if (users.length > 0) {
                    currentUser = users[0];
                    localStorage.setItem(CURRENT_USER_KEY, JSON.stringify(users[0]));
                    
                    showUserInfo();
                    showLoading(false);
                    document.getElementById('enterSimulatorBtn').disabled = false;
                    
                    document.getElementById('loginEmail').value = users[0].email;
                    document.getElementById('loginPassword').value = users[0].password;
                    
                    showSuccess('Тестовый вход выполнен!');
                    
                    document.getElementById('loginForm').classList.add('hidden');
                    document.getElementById('registerForm').classList.add('hidden');
                    document.getElementById('authToggle').classList.add('hidden');
                } else {
                    showLoading(false);
                    showError('Нет тестовых пользователей');
                }
            }, 500);
        }

        function showUserInfo() {
            if (!currentUser) return;
            
            const userInfo = document.getElementById('userInfo');
            const userAvatar = document.getElementById('userAvatar');
            const userName = document.getElementById('userName');
            const userEmail = document.getElementById('userEmail');
            
            const firstLetter = currentUser.name.charAt(0).toUpperCase();
            userAvatar.textContent = firstLetter;
            
            userName.textContent = currentUser.name;
            userEmail.textContent = currentUser.email;
            
            userInfo.style.display = 'flex';
            document.getElementById('enterSimulatorBtn').textContent = `Войти как ${currentUser.name}`;
        }

        function enterSimulator() {
            if (!currentUser) {
                showError('Сначала войдите в аккаунт');
                return;
            }
            
            showLoading(true, 'Загрузка симулятора...');
            
            setTimeout(() => {
                document.getElementById('authScreen').style.display = 'none';
                document.getElementById('homeScreen').style.display = 'flex';
                
                const name = currentUser.name;
                const shortName = name.length > 15 ? name.substring(0, 12) + '...' : name;
                document.getElementById('homeUserName').textContent = shortName;
                
                showLoading(false);
                isLoggedIn = true;
                
                showNotification(`Добро пожаловать, ${currentUser.name}!`);
            }, 2000);
        }

        // ==================== ПРИЛОЖЕНИЯ ====================
        function setupAppListeners() {
            document.querySelectorAll('.app-icon').forEach(icon => {
                icon.addEventListener('click', function() {
                    if (!isLoggedIn) {
                        showError('Сначала войдите в симулятор');
                        return;
                    }
                    
                    const app = this.getAttribute('data-app');
                    openApp(app);
                });
            });
            
            document.querySelectorAll('.back-btn').forEach(btn => {
                btn.addEventListener('click', function() {
                    const app = this.getAttribute('data-app');
                    closeApp(app);
                });
            });
        }

        function openApp(app) {
            document.getElementById('homeScreen').style.display = 'none';
            
            const appScreen = document.getElementById(`${app}App`);
            if (appScreen) {
                appScreen.style.display = 'flex';
                showNotification(`Открыто: ${getAppName(app)}`);
                
                if (app === 'browser') {
                    updateBrowserTabs();
                }
            }
        }

        function closeApp(app) {
            const appScreen = document.getElementById(`${app}App`);
            if (appScreen) {
                appScreen.style.display = 'none';
            }
            
            document.getElementById('homeScreen').style.display = 'flex';
        }

        function getAppName(app) {
            const appNames = {
                'browser': 'Chrome',
                'phone': 'Телефон',
                'calculator': 'Калькулятор',
                'settings': 'Настройки'
            };
            return appNames[app] || app;
        }

        // ==================== БРАУЗЕР ====================
        function setupBrowserListeners() {
            // Навигация
            document.getElementById('browserBackBtn').addEventListener('click', browserGoBack);
            document.getElementById('browserForwardBtn').addEventListener('click', browserGoForward);
            document.getElementById('browserRefreshBtn').addEventListener('click', browserRefresh);
            
            // URL бар
            document.getElementById('browserUrlInput').addEventListener('keypress', function(e) {
                if (e.key === 'Enter') {
                    navigateToUrl(this.value);
                }
            });
            
            document.getElementById('browserGoBtn').addEventListener('click', function() {
                const urlInput = document.getElementById('browserUrlInput');
                navigateToUrl(urlInput.value);
            });
            
            // Закладки
            document.getElementById('browserBookmarksBtn').addEventListener('click', toggleBookmarks);
            
            // Клики по закладкам
            document.querySelectorAll('.bookmark-item').forEach(bookmark => {
                bookmark.addEventListener('click', function() {
                    const url = this.getAttribute('data-url');
                    navigateToUrl(url);
                    hideBookmarks();
                });
            });
            
            // Клик вне закладок
            document.addEventListener('click', function(e) {
                if (!e.target.closest('#browserBookmarksBtn') && !e.target.closest('#browserBookmarks')) {
                    hideBookmarks();
                }
            });
        }

        function updateBrowserTabs() {
            const tabsContainer = document.getElementById('browserTabs');
            tabsContainer.innerHTML = '';
            
            browser.tabs.forEach(tab => {
                const tabElement = document.createElement('div');
                tabElement.className = `browser-tab ${tab.active ? 'active' : ''}`;
                tabElement.innerHTML = `
                    <div class="bookmark-icon" style="background: ${tab.color};">${tab.favicon}</div>
                    <span>${tab.title}</span>
                    <button class="browser-tab-close" data-tab-id="${tab.id}">
                        <i class="fas fa-times"></i>
                    </button>
                `;
                
                // Клик по вкладке
                tabElement.addEventListener('click', function(e) {
                    if (!e.target.closest('.browser-tab-close')) {
                        switchToTab(tab.id);
                    }
                });
                
                // Кнопка закрытия вкладки
                const closeBtn = tabElement.querySelector('.browser-tab-close');
                closeBtn.addEventListener('click', function(e) {
                    e.stopPropagation();
                    closeBrowserTab(tab.id);
                });
                
                tabsContainer.appendChild(tabElement);
            });
            
            // Кнопка новой вкладки
            const newTabBtn = document.createElement('button');
            newTabBtn.className = 'nav-btn';
            newTabBtn.innerHTML = '<i class="fas fa-plus"></i>';
            newTabBtn.title = 'Новая вкладка';
            newTabBtn.addEventListener('click', createNewTab);
            tabsContainer.appendChild(newTabBtn);
            
            // Обновляем iframe
            updateBrowserIframe();
        }

        function createNewTab() {
            const newTabId = Date.now();
            const newTab = {
                id: newTabId,
                title: "Новая вкладка",
                url: "about:blank",
                favicon: "N",
                color: "#666",
                active: true,
                loading: false
            };
            
            // Деактивируем все вкладки
            browser.tabs.forEach(tab => tab.active = false);
            
            // Добавляем новую вкладку
            browser.tabs.push(newTab);
            browser.currentTabId = newTabId;
            
            // Создаем iframe для новой вкладки
            const iframe = document.createElement('iframe');
            iframe.className = 'browser-iframe';
            iframe.id = `iframe-${newTabId}`;
            iframe.sandbox = "allow-same-origin allow-scripts allow-popups allow-forms";
            iframe.loading = "lazy";
            
            const viewContainer = document.createElement('div');
            viewContainer.className = 'browser-view';
            viewContainer.id = `tab-${newTabId}`;
            viewContainer.appendChild(iframe);
            
            document.getElementById('browserContent').appendChild(viewContainer);
            
            // Обновляем интерфейс
            updateBrowserTabs();
            updateBrowserUrl();
            
            showNotification('Создана новая вкладка');
        }

        function switchToTab(tabId) {
            browser.tabs.forEach(tab => {
                tab.active = tab.id === tabId;
            });
            
            browser.currentTabId = tabId;
            updateBrowserTabs();
            updateBrowserUrl();
        }

        function closeBrowserTab(tabId) {
            if (browser.tabs.length <= 1) {
                showError('Нельзя закрыть последнюю вкладку');
                return;
            }
            
            const tabIndex = browser.tabs.findIndex(tab => tab.id === tabId);
            browser.tabs.splice(tabIndex, 1);
            
            // Удаляем iframe
            const viewElement = document.getElementById(`tab-${tabId}`);
            if (viewElement) {
                viewElement.remove();
            }
            
            // Активируем предыдущую вкладку
            if (browser.currentTabId === tabId) {
                browser.currentTabId = browser.tabs[0].id;
                browser.tabs[0].active = true;
            }
            
            updateBrowserTabs();
            updateBrowserUrl();
            
            showNotification('Вкладка закрыта');
        }

        function updateBrowserIframe() {
            // Скрываем все вкладки
            document.querySelectorAll('.browser-view').forEach(view => {
                view.classList.remove('active');
            });
            
            // Показываем активную вкладку
            const activeView = document.getElementById(`tab-${browser.currentTabId}`);
            if (activeView) {
                activeView.classList.add('active');
                
                // Если iframe еще не создан, создаем его
                let iframe = activeView.querySelector('.browser-iframe');
                if (!iframe) {
                    iframe = document.createElement('iframe');
                    iframe.className = 'browser-iframe';
                    iframe.sandbox = "allow-same-origin allow-scripts allow-popups allow-forms";
                    iframe.loading = "lazy";
                    activeView.appendChild(iframe);
                }
            }
        }

        function updateBrowserUrl() {
            const currentTab = browser.tabs.find(tab => tab.id === browser.currentTabId);
            if (currentTab) {
                document.getElementById('browserUrlInput').value = currentTab.url;
            }
        }

        function navigateToUrl(url) {
            let fullUrl = url;
            
            // Добавляем https:// если нет протокола
            if (!url.startsWith('http://') && !url.startsWith('https://') && !url.startsWith('about:')) {
                fullUrl = 'https://' + url;
            }
            
            // Валидация URL
            try {
                new URL(fullUrl);
            } catch (e) {
                // Если URL невалидный, используем поиск Google
                fullUrl = `https://www.google.com/search?q=${encodeURIComponent(url)}`;
            }
            
            const currentTab = browser.tabs.find(tab => tab.id === browser.currentTabId);
            if (currentTab) {
                currentTab.url = fullUrl;
                currentTab.loading = true;
                
                // Обновляем iframe
                const iframe = document.getElementById(`iframe-${currentTab.id}`);
                if (iframe) {
                    iframe.src = fullUrl;
                    
                    // Обновляем заголовок при загрузке
                    iframe.onload = function() {
                        currentTab.loading = false;
                        try {
                            // Пытаемся получить заголовок страницы
                            const title = iframe.contentDocument?.title || new URL(fullUrl).hostname;
                            currentTab.title = title.length > 20 ? title.substring(0, 17) + '...' : title;
                            updateBrowserTabs();
                        } catch (e) {
                            // Заголовок из домена
                            const domain = new URL(fullUrl).hostname.replace('www.', '');
                            currentTab.title = domain;
                            updateBrowserTabs();
                        }
                    };
                    
                    iframe.onerror = function() {
                        currentTab.loading = false;
                        currentTab.title = "Ошибка загрузки";
                        updateBrowserTabs();
                        showError('Не удалось загрузить страницу');
                    };
                }
                
                updateBrowserUrl();
                showNotification(`Загрузка: ${fullUrl}`);
            }
        }

        function browserGoBack() {
            // В реальном браузере здесь была бы навигация по истории
            showNotification('Назад (история браузера)');
        }

        function browserGoForward() {
            showNotification('Вперед (история браузера)');
        }

        function browserRefresh() {
            const currentTab = browser.tabs.find(tab => tab.id === browser.currentTabId);
            if (currentTab) {
                const iframe = document.getElementById(`iframe-${currentTab.id}`);
                if (iframe) {
                    iframe.src = iframe.src;
                    showNotification('Обновление страницы...');
                }
            }
        }

        function toggleBookmarks() {
            const bookmarks = document.getElementById('browserBookmarks');
            bookmarks.classList.toggle('active');
        }

        function hideBookmarks() {
            const bookmarks = document.getElementById('browserBookmarks');
            bookmarks.classList.remove('active');
        }

        // ==================== ТЕЛЕФОН ====================
        function setupPhoneListeners() {
            document.querySelectorAll('.dial-btn[data-number]').forEach(btn => {
                btn.addEventListener('click', function() {
                    const number = this.getAttribute('data-number');
                    addPhoneNumber(number);
                });
            });
            
            document.getElementById('clearDialBtn').addEventListener('click', clearPhoneNumber);
            document.getElementById('makeCallBtn').addEventListener('click', makeCall);
        }

        function addPhoneNumber(number) {
            if (phoneNumber.length < 16) {
                phoneNumber += number;
                updatePhoneDisplay();
            }
        }

        function clearPhoneNumber() {
            if (phoneNumber.length > 2) {
                phoneNumber = phoneNumber.slice(0, -1);
                updatePhoneDisplay();
            }
        }

        function updatePhoneDisplay() {
            document.getElementById('phoneNumberDisplay').textContent = phoneNumber;
        }

        function makeCall() {
            if (phoneNumber.length > 2) {
                showNotification(`Звонок на номер: ${phoneNumber}`);
                setTimeout(() => {
                    showNotification(`Вызов завершен: ${phoneNumber}`);
                    phoneNumber = '+7';
                    updatePhoneDisplay();
                }, 3000);
            } else {
                showError('Введите номер для звонка');
            }
        }

        // ==================== КАЛЬКУЛЯТОР ====================
        function setupCalculatorListeners() {
            document.querySelectorAll('.calc-btn').forEach(btn => {
                btn.addEventListener('click', function() {
                    const action = this.getAttribute('data-action');
                    const number = this.getAttribute('data-number');
                    
                    if (number !== null) {
                        inputNumber(number);
                    } else if (action) {
                        handleCalculatorAction(action);
                    }
                });
            });
        }

        function inputNumber(num) {
            if (calculator.currentInput === '0' || calculator.currentInput === 'Error') {
                calculator.currentInput = num;
            } else {
                calculator.currentInput += num;
            }
            updateCalculatorDisplay();
        }

        function handleCalculatorAction(action) {
            switch(action) {
                case 'clear':
                    calculator.currentInput = '0';
                    calculator.previousInput = '';
                    calculator.operator = '';
                    calculator.expression = '';
                    break;
                    
                case 'clearEntry':
                    calculator.currentInput = '0';
                    break;
                    
                case 'backspace':
                    if (calculator.currentInput.length > 1) {
                        calculator.currentInput = calculator.currentInput.slice(0, -1);
                    } else {
                        calculator.currentInput = '0';
                    }
                    break;
                    
                case '+':
                case '-':
                case '*':
                case '/':
                    if (calculator.previousInput && calculator.operator) {
                        calculateResult();
                    }
                    calculator.previousInput = calculator.currentInput;
                    calculator.operator = action;
                    calculator.currentInput = '0';
                    calculator.expression = `${calculator.previousInput} ${getOperatorSymbol(action)}`;
                    break;
                    
                case '.':
                    if (!calculator.currentInput.includes('.')) {
                        calculator.currentInput += '.';
                    }
                    break;
                    
                case '=':
                    if (calculator.previousInput && calculator.operator) {
                        calculateResult();
                        calculator.previousInput = '';
                        calculator.operator = '';
                        calculator.expression = '';
                    }
                    break;
            }
            updateCalculatorDisplay();
        }

        function getOperatorSymbol(op) {
            const symbols = {
                '+': '+',
                '-': '−',
                '*': '×',
                '/': '÷'
            };
            return symbols[op] || op;
        }

        function calculateResult() {
            const prev = parseFloat(calculator.previousInput);
            const current = parseFloat(calculator.currentInput);
            
            if (isNaN(prev) || isNaN(current)) {
                calculator.currentInput = 'Error';
                return;
            }
            
            let result;
            switch(calculator.operator) {
                case '+':
                    result = prev + current;
                    break;
                case '-':
                    result = prev - current;
                    break;
                case '*':
                    result = prev * current;
                    break;
                case '/':
                    if (current === 0) {
                        calculator.currentInput = 'Error';
                        return;
                    }
                    result = prev / current;
                    break;
                default:
                    return;
            }
            
            calculator.currentInput = result.toString();
            calculator.expression = `${calculator.previousInput} ${getOperatorSymbol(calculator.operator)} ${current} =`;
        }

        function updateCalculatorDisplay() {
            document.getElementById('calcResult').textContent = calculator.currentInput;
            document.getElementById('calcExpression').textContent = calculator.expression;
        }

        // ==================== НАВИГАЦИЯ ====================
        function goBack() {
            if (!isLoggedIn) return;
            
            const openApps = document.querySelectorAll('.browser-screen[style*="display: flex"], .app-screen[style*="display: flex"]');
            if (openApps.length > 0) {
                const lastApp = openApps[openApps.length - 1];
                lastApp.style.display = 'none';
                document.getElementById('homeScreen').style.display = 'flex';
            } else {
                showNotification('Уже на главном экране');
            }
        }

        function goHome() {
            if (!isLoggedIn) return;
            
            document.querySelectorAll('.browser-screen, .app-screen').forEach(app => {
                app.style.display = 'none';
            });
            
            document.getElementById('homeScreen').style.display = 'flex';
            showNotification('Главный экран');
        }

        // ==================== УТИЛИТЫ ====================
        function updateTime() {
            const now = new Date();
            const timeString = now.getHours().toString().padStart(2, '0') + ':' + 
                             now.getMinutes().toString().padStart(2, '0');
            document.getElementById('currentTime').textContent = timeString;
        }

        function showNotification(text) {
            console.log('📢 ' + text);
            
            const notification = document.createElement('div');
            notification.style.cssText = `
                position: fixed;
                top: 20px;
                right: 20px;
                background: #333;
                color: white;
                padding: 12px 20px;
                border-radius: 8px;
                z-index: 10000;
                animation: slideInRight 0.3s ease;
                font-family: 'Roboto', sans-serif;
                max-width: 300px;
                word-wrap: break-word;
                box-shadow: 0 4px 12px rgba(0,0,0,0.15);
            `;
            notification.textContent = text;
            document.body.appendChild(notification);
            
            setTimeout(() => {
                notification.style.animation = 'slideOutRight 0.3s ease';
                setTimeout(() => notification.remove(), 300);
            }, 3000);
        }

        function showSuccess(message) {
            const successEl = document.createElement('div');
            successEl.style.cssText = `
                position: fixed;
                top: 20px;
                right: 20px;
                background: #34A853;
                color: white;
                padding: 12px 20px;
                border-radius: 8px;
                z-index: 10000;
                animation: slideInRight 0.3s ease;
                font-family: 'Roboto', sans-serif;
                max-width: 300px;
                word-wrap: break-word;
                box-shadow: 0 4px 12px rgba(0,0,0,0.15);
            `;
            successEl.textContent = '✅ ' + message;
            document.body.appendChild(successEl);
            
            setTimeout(() => {
                successEl.style.animation = 'slideOutRight 0.3s ease';
                setTimeout(() => successEl.remove(), 300);
            }, 3000);
        }

        function showError(message) {
            const errorEl = document.getElementById('errorMessage');
            errorEl.textContent = message;
            errorEl.style.display = 'block';
            showLoading(false);
            
            setTimeout(() => {
                errorEl.style.display = 'none';
            }, 5000);
        }

        function showLoading(show, text = 'Загрузка...') {
            const loadingEl = document.getElementById('loading');
            const loadingText = document.getElementById('loadingText');
            
            loadingText.textContent = text;
            loadingEl.style.display = show ? 'flex' : 'none';
        }

        // Стили для анимаций
        const style = document.createElement('style');
        style.textContent = `
            @keyframes slideInRight {
                from { transform: translateX(100%); opacity: 0; }
                to { transform: translateX(0); opacity: 1; }
            }
            @keyframes slideOutRight {
                from { transform: translateX(0); opacity: 1; }
                to { transform: translateX(100%); opacity: 0; }
            }
        `;
        document.head.appendChild(style);
    </script>
</body>
</html>
