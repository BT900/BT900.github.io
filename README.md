<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Peach State Fabrication — SecureComm</title>
<link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=DM+Sans:wght@300;400;500;600&family=DM+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
  :root {
    --orange: #E8641A;
    --orange-light: #F5A05A;
    --orange-dark: #B84D0E;
    --steel: #1C2B3A;
    --steel-mid: #2D4155;
    --steel-light: #3E5670;
    --slate: #8FA3B4;
    --bg: #111A22;
    --surface: #182433;
    --surface2: #1E2E3F;
    --surface3: #243548;
    --border: rgba(255,255,255,0.07);
    --border2: rgba(255,255,255,0.12);
    --text: #EDF2F6;
    --text2: #8FA3B4;
    --text3: #556B7E;
    --green: #2ECC8A;
    --green-bg: rgba(46,204,138,0.12);
    --red: #E84040;
    --red-bg: rgba(232,64,64,0.12);
    --amber: #F0A020;
    --amber-bg: rgba(240,160,32,0.12);
    --radius: 10px;
    --radius-sm: 6px;
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body { font-family: 'DM Sans', sans-serif; background: var(--bg); color: var(--text); min-height: 100vh; overflow: hidden; }

  /* ── SCREEN SYSTEM ── */
  .screen { display: none; width: 100vw; height: 100vh; }
  .screen.active { display: flex; }

  /* ── LOGIN SCREEN ── */
  #login-screen {
    background: var(--bg);
    align-items: center;
    justify-content: center;
    position: relative;
    overflow: hidden;
  }
  .login-bg {
    position: absolute; inset: 0; pointer-events: none;
    background:
      radial-gradient(ellipse 60% 50% at 70% 50%, rgba(232,100,26,0.08) 0%, transparent 70%),
      radial-gradient(ellipse 40% 60% at 20% 20%, rgba(30,46,63,0.9) 0%, transparent 60%);
  }
  .login-grid {
    position: absolute; inset: 0; pointer-events: none;
    background-image: linear-gradient(var(--border) 1px, transparent 1px), linear-gradient(90deg, var(--border) 1px, transparent 1px);
    background-size: 48px 48px;
    opacity: 0.4;
  }
  .login-card {
    position: relative; z-index: 2;
    background: var(--surface);
    border: 1px solid var(--border2);
    border-radius: 16px;
    padding: 48px 40px;
    width: 400px;
    box-shadow: 0 32px 80px rgba(0,0,0,0.5);
    animation: fadeUp 0.5s ease both;
  }
  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(20px); }
    to { opacity: 1; transform: translateY(0); }
  }
  .login-logo {
    display: flex; align-items: center; gap: 12px;
    margin-bottom: 32px;
  }
  .login-logo-icon {
    width: 44px; height: 44px;
    background: var(--orange);
    border-radius: 10px;
    display: flex; align-items: center; justify-content: center;
    font-size: 22px;
  }
  .login-logo-text { }
  .login-logo-text h1 {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 22px;
    letter-spacing: 1px;
    color: var(--text);
    line-height: 1;
  }
  .login-logo-text span {
    font-size: 11px;
    color: var(--orange);
    text-transform: uppercase;
    letter-spacing: 2px;
    font-weight: 500;
  }
  .login-headline {
    font-size: 24px; font-weight: 500;
    margin-bottom: 6px;
  }
  .login-sub {
    font-size: 13px; color: var(--text2);
    margin-bottom: 28px;
  }
  .field { margin-bottom: 16px; }
  .field label {
    display: block; font-size: 12px; font-weight: 500;
    color: var(--text2); margin-bottom: 6px;
    text-transform: uppercase; letter-spacing: 0.8px;
  }
  .field input, .field select {
    width: 100%;
    background: var(--surface2);
    border: 1px solid var(--border2);
    border-radius: var(--radius-sm);
    padding: 10px 14px;
    font-family: 'DM Sans', sans-serif;
    font-size: 14px;
    color: var(--text);
    outline: none;
    transition: border-color 0.2s;
  }
  .field input:focus, .field select:focus {
    border-color: var(--orange);
  }
  .field select option { background: var(--surface2); }
  .btn-primary {
    width: 100%;
    background: var(--orange);
    color: #fff;
    border: none;
    border-radius: var(--radius-sm);
    padding: 12px;
    font-family: 'DM Sans', sans-serif;
    font-size: 15px; font-weight: 600;
    cursor: pointer;
    transition: background 0.2s, transform 0.1s;
    margin-top: 8px;
  }
  .btn-primary:hover { background: var(--orange-dark); }
  .btn-primary:active { transform: scale(0.98); }
  .login-error {
    font-size: 12px; color: var(--red);
    margin-top: 10px; text-align: center;
    min-height: 16px;
  }
  .demo-hint {
    margin-top: 20px;
    padding: 12px;
    background: var(--surface2);
    border-radius: var(--radius-sm);
    border: 1px solid var(--border);
    font-size: 11px;
    color: var(--text3);
  }
  .demo-hint b { color: var(--text2); }

  /* ── MAIN APP ── */
  #app-screen { flex-direction: column; }

  .topbar {
    height: 52px;
    background: var(--surface);
    border-bottom: 1px solid var(--border);
    display: flex; align-items: center;
    padding: 0 20px;
    gap: 16px;
    flex-shrink: 0;
    z-index: 10;
  }
  .topbar-logo {
    display: flex; align-items: center; gap: 8px;
    font-family: 'Bebas Neue', sans-serif;
    font-size: 18px; letter-spacing: 1px;
    color: var(--text);
  }
  .topbar-logo-dot {
    width: 8px; height: 8px; border-radius: 50%;
    background: var(--orange); flex-shrink: 0;
  }
  .topbar-divider { width: 1px; height: 24px; background: var(--border); }
  .topbar-org { font-size: 13px; color: var(--text2); }
  .topbar-spacer { flex: 1; }
  .topbar-user {
    display: flex; align-items: center; gap: 8px;
  }
  .user-avatar {
    width: 30px; height: 30px; border-radius: 50%;
    background: var(--orange);
    display: flex; align-items: center; justify-content: center;
    font-size: 11px; font-weight: 600; color: #fff;
  }
  .user-info { font-size: 12px; }
  .user-name { color: var(--text); font-weight: 500; }
  .user-role { color: var(--orange); font-size: 10px; text-transform: uppercase; letter-spacing: 0.8px; }
  .btn-logout {
    background: transparent; border: 1px solid var(--border2);
    border-radius: var(--radius-sm); padding: 5px 12px;
    font-size: 12px; color: var(--text2); cursor: pointer;
    transition: all 0.2s;
  }
  .btn-logout:hover { border-color: var(--red); color: var(--red); }

  .app-body {
    display: flex; flex: 1; overflow: hidden;
  }

  /* ── SIDEBAR ── */
  .sidebar {
    width: 220px; flex-shrink: 0;
    background: var(--surface);
    border-right: 1px solid var(--border);
    display: flex; flex-direction: column;
    overflow-y: auto;
  }
  .sidebar-section { padding: 16px 12px 8px; }
  .sidebar-label {
    font-size: 10px; font-weight: 600;
    text-transform: uppercase; letter-spacing: 1.2px;
    color: var(--text3); padding: 0 8px;
    margin-bottom: 6px;
  }
  .channel-item {
    display: flex; align-items: center; gap: 8px;
    padding: 8px 10px;
    border-radius: var(--radius-sm);
    cursor: pointer;
    transition: background 0.15s;
    font-size: 13px;
    color: var(--text2);
    position: relative;
  }
  .channel-item:hover { background: var(--surface2); color: var(--text); }
  .channel-item.active { background: rgba(232,100,26,0.12); color: var(--text); }
  .channel-item.active .ch-icon { color: var(--orange); }
  .ch-icon { font-size: 14px; flex-shrink: 0; }
  .ch-name { flex: 1; }
  .ch-badge {
    background: var(--orange); color: #fff;
    font-size: 10px; font-weight: 600;
    border-radius: 10px; padding: 1px 6px;
  }
  .sidebar-divider { height: 1px; background: var(--border); margin: 8px 12px; }
  .online-dot { width: 7px; height: 7px; border-radius: 50%; background: var(--green); flex-shrink: 0; }
  .offline-dot { width: 7px; height: 7px; border-radius: 50%; background: var(--text3); flex-shrink: 0; }

  /* ── MAIN PANEL ── */
  .main-panel {
    flex: 1; display: flex; flex-direction: column; overflow: hidden;
  }

  /* ── CHANNEL HEADER ── */
  .channel-header {
    height: 52px; padding: 0 20px;
    border-bottom: 1px solid var(--border);
    display: flex; align-items: center; gap: 12px;
    flex-shrink: 0;
  }
  .ch-header-icon { font-size: 18px; }
  .ch-header-name { font-size: 15px; font-weight: 500; }
  .ch-header-desc { font-size: 12px; color: var(--text2); flex: 1; }
  .ch-header-members { font-size: 12px; color: var(--text2); }

  /* ── MESSAGES ── */
  .messages-area {
    flex: 1; overflow-y: auto;
    padding: 20px;
    display: flex; flex-direction: column; gap: 2px;
  }
  .messages-area::-webkit-scrollbar { width: 4px; }
  .messages-area::-webkit-scrollbar-track { background: transparent; }
  .messages-area::-webkit-scrollbar-thumb { background: var(--border2); border-radius: 4px; }

  .msg-group { margin-bottom: 16px; }
  .msg-header {
    display: flex; align-items: baseline; gap: 8px;
    margin-bottom: 4px;
  }
  .msg-author {
    font-size: 13px; font-weight: 600; color: var(--text);
  }
  .msg-role-tag {
    font-size: 10px; padding: 1px 6px;
    border-radius: 4px; font-weight: 500;
    text-transform: uppercase; letter-spacing: 0.5px;
  }
  .role-owner { background: rgba(232,100,26,0.2); color: var(--orange); }
  .role-manager { background: rgba(46,204,138,0.15); color: var(--green); }
  .role-sales { background: rgba(52,152,219,0.15); color: #5DADE2; }
  .role-shop { background: rgba(240,160,32,0.15); color: var(--amber); }
  .msg-time { font-size: 11px; color: var(--text3); }
  .msg-bubble {
    font-size: 13.5px; color: var(--text2);
    line-height: 1.6;
    padding-left: 2px;
  }
  .msg-file-attach {
    display: inline-flex; align-items: center; gap: 8px;
    margin-top: 6px;
    background: var(--surface2);
    border: 1px solid var(--border2);
    border-radius: var(--radius-sm);
    padding: 8px 12px;
    font-size: 12px; color: var(--text2);
    cursor: pointer;
    transition: border-color 0.2s;
  }
  .msg-file-attach:hover { border-color: var(--orange); color: var(--text); }
  .file-icon { font-size: 18px; }
  .file-meta { }
  .file-name { font-size: 12px; font-weight: 500; color: var(--text); }
  .file-size { font-size: 11px; color: var(--text3); }
  .day-divider {
    text-align: center; font-size: 11px; color: var(--text3);
    margin: 16px 0;
    display: flex; align-items: center; gap: 10px;
  }
  .day-divider::before, .day-divider::after {
    content: ''; flex: 1; height: 1px; background: var(--border);
  }

  /* ── COMPOSE ── */
  .compose-area {
    padding: 12px 20px 16px;
    border-top: 1px solid var(--border);
    flex-shrink: 0;
  }
  .compose-box {
    background: var(--surface2);
    border: 1px solid var(--border2);
    border-radius: var(--radius);
    overflow: hidden;
    transition: border-color 0.2s;
  }
  .compose-box:focus-within { border-color: var(--orange); }
  .compose-input {
    width: 100%; padding: 12px 16px;
    background: transparent; border: none;
    font-family: 'DM Sans', sans-serif;
    font-size: 14px; color: var(--text);
    outline: none; resize: none;
    min-height: 42px; max-height: 120px;
  }
  .compose-input::placeholder { color: var(--text3); }
  .compose-actions {
    display: flex; align-items: center; gap: 8px;
    padding: 8px 12px;
    border-top: 1px solid var(--border);
  }
  .compose-btn {
    background: transparent; border: 1px solid var(--border2);
    border-radius: var(--radius-sm); padding: 5px 10px;
    font-size: 12px; color: var(--text2); cursor: pointer;
    display: flex; align-items: center; gap: 4px;
    transition: all 0.15s;
  }
  .compose-btn:hover { border-color: var(--border2); background: var(--surface3); color: var(--text); }
  .compose-spacer { flex: 1; }
  .btn-send {
    background: var(--orange); color: #fff;
    border: none; border-radius: var(--radius-sm);
    padding: 6px 16px; font-size: 13px; font-weight: 600;
    font-family: 'DM Sans', sans-serif;
    cursor: pointer; display: flex; align-items: center; gap: 6px;
    transition: background 0.15s;
  }
  .btn-send:hover { background: var(--orange-dark); }
  .btn-send:disabled { opacity: 0.4; cursor: not-allowed; }

  /* ── RIGHT PANEL ── */
  .right-panel {
    width: 240px; flex-shrink: 0;
    background: var(--surface);
    border-left: 1px solid var(--border);
    overflow-y: auto;
    padding: 16px;
  }
  .rp-section { margin-bottom: 24px; }
  .rp-title {
    font-size: 11px; font-weight: 600; color: var(--text3);
    text-transform: uppercase; letter-spacing: 1px;
    margin-bottom: 12px;
  }
  .rp-stat {
    background: var(--surface2);
    border-radius: var(--radius-sm);
    padding: 10px 12px;
    margin-bottom: 8px;
    display: flex; justify-content: space-between; align-items: center;
  }
  .rp-stat-label { font-size: 12px; color: var(--text2); }
  .rp-stat-value { font-size: 15px; font-weight: 600; color: var(--text); }
  .rp-member {
    display: flex; align-items: center; gap: 8px;
    padding: 6px 0;
  }
  .rp-member-avatar {
    width: 28px; height: 28px; border-radius: 50%;
    display: flex; align-items: center; justify-content: center;
    font-size: 10px; font-weight: 600; flex-shrink: 0;
  }
  .rp-member-info { }
  .rp-member-name { font-size: 12px; font-weight: 500; color: var(--text); }
  .rp-member-role { font-size: 10px; color: var(--text3); }
  .rp-file {
    display: flex; align-items: center; gap: 8px;
    padding: 8px;
    background: var(--surface2);
    border-radius: var(--radius-sm);
    margin-bottom: 6px;
    cursor: pointer;
    transition: background 0.15s;
  }
  .rp-file:hover { background: var(--surface3); }
  .rp-file-icon { font-size: 20px; }
  .rp-file-info { flex: 1; min-width: 0; }
  .rp-file-name { font-size: 11px; font-weight: 500; color: var(--text); white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
  .rp-file-date { font-size: 10px; color: var(--text3); }

  /* ── FILE UPLOAD MODAL ── */
  .modal-overlay {
    display: none;
    position: fixed; inset: 0;
    background: rgba(0,0,0,0.7);
    z-index: 100;
    align-items: center; justify-content: center;
  }
  .modal-overlay.open { display: flex; }
  .modal {
    background: var(--surface);
    border: 1px solid var(--border2);
    border-radius: 14px;
    padding: 28px;
    width: 420px;
    box-shadow: 0 24px 60px rgba(0,0,0,0.6);
    animation: fadeUp 0.2s ease both;
  }
  .modal-header {
    display: flex; align-items: center; justify-content: space-between;
    margin-bottom: 20px;
  }
  .modal-title { font-size: 16px; font-weight: 600; }
  .modal-close {
    background: transparent; border: none;
    color: var(--text3); font-size: 20px; cursor: pointer;
    line-height: 1;
  }
  .modal-close:hover { color: var(--text); }
  .drop-zone {
    border: 2px dashed var(--border2);
    border-radius: var(--radius);
    padding: 32px;
    text-align: center;
    cursor: pointer;
    transition: all 0.2s;
    margin-bottom: 16px;
  }
  .drop-zone:hover, .drop-zone.dragover {
    border-color: var(--orange);
    background: rgba(232,100,26,0.05);
  }
  .drop-icon { font-size: 32px; margin-bottom: 8px; }
  .drop-text { font-size: 14px; color: var(--text2); margin-bottom: 4px; }
  .drop-sub { font-size: 11px; color: var(--text3); }
  .modal-actions { display: flex; gap: 8px; justify-content: flex-end; }
  .btn-ghost {
    background: transparent; border: 1px solid var(--border2);
    border-radius: var(--radius-sm); padding: 8px 16px;
    font-size: 13px; color: var(--text2); cursor: pointer;
    font-family: 'DM Sans', sans-serif;
    transition: all 0.15s;
  }
  .btn-ghost:hover { color: var(--text); background: var(--surface2); }

  /* ── NOTIFICATION TOAST ── */
  .toast {
    position: fixed; bottom: 24px; right: 24px;
    background: var(--surface);
    border: 1px solid var(--border2);
    border-left: 3px solid var(--green);
    border-radius: var(--radius-sm);
    padding: 12px 16px;
    font-size: 13px; color: var(--text);
    box-shadow: 0 8px 24px rgba(0,0,0,0.4);
    z-index: 200;
    opacity: 0; transform: translateX(20px);
    transition: all 0.3s;
    pointer-events: none;
  }
  .toast.show { opacity: 1; transform: translateX(0); }
  .toast-title { font-weight: 600; margin-bottom: 2px; }
  .toast-body { color: var(--text2); font-size: 12px; }

  /* ── AUDIT LOG TAB ── */
  .log-entry {
    display: flex; gap: 10px;
    padding: 10px 0;
    border-bottom: 1px solid var(--border);
    font-size: 12px;
  }
  .log-time { color: var(--text3); flex-shrink: 0; font-family: 'DM Mono', monospace; font-size: 11px; }
  .log-icon { flex-shrink: 0; }
  .log-text { color: var(--text2); }
  .log-text b { color: var(--text); }

  /* Scrollbar */
  .sidebar::-webkit-scrollbar,
  .right-panel::-webkit-scrollbar { width: 3px; }
  .sidebar::-webkit-scrollbar-thumb,
  .right-panel::-webkit-scrollbar-thumb { background: var(--border2); }
</style>
</head>
<body>

<!-- ═══════════════════════════════════════════════════
     LOGIN SCREEN
═══════════════════════════════════════════════════ -->
<div id="login-screen" class="screen active">
  <div class="login-bg"></div>
  <div class="login-grid"></div>
  <div class="login-card">
    <div class="login-logo">
      <div class="login-logo-icon">🍑</div>
      <div class="login-logo-text">
        <h1>Peach State Fab</h1>
        <span>SecureComm v1.0</span>
      </div>
    </div>
    <p class="login-headline">Sign in to continue</p>
    <p class="login-sub">Internal use only — authorized personnel</p>
    <div class="field">
      <label>Email address</label>
      <input type="email" id="login-email" placeholder="you@peachstatefab.com" />
    </div>
    <div class="field">
      <label>Password</label>
      <input type="password" id="login-password" placeholder="••••••••" />
    </div>
    <div class="field">
      <label>Sign in as (demo)</label>
      <select id="login-role">
        <option value="owner">Owner / Operations Manager</option>
        <option value="manager">Production Manager</option>
        <option value="sales">Sales / Admin</option>
        <option value="shop">Shop Floor Employee</option>
      </select>
    </div>
    <button class="btn-primary" onclick="doLogin()">Sign In →</button>
    <p class="login-error" id="login-error"></p>
    <div class="demo-hint">
      <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:8px">
        <span style="font-size:11px;color:var(--text2);font-weight:600;text-transform:uppercase;letter-spacing:0.8px">Admin Account</span>
        <span style="font-size:10px;background:rgba(232,100,26,0.18);color:var(--orange);padding:2px 8px;border-radius:4px;font-weight:600">PRE-SET</span>
      </div>
      <div style="display:flex;gap:6px;margin-bottom:10px">
        <div style="flex:1;background:var(--surface3);border-radius:6px;padding:7px 10px">
          <div style="font-size:10px;color:var(--text3);margin-bottom:2px">Email</div>
          <div style="font-size:12px;color:var(--text);font-family:'DM Mono',monospace">admin@peachstatefab.com</div>
        </div>
        <div style="flex:1;background:var(--surface3);border-radius:6px;padding:7px 10px">
          <div style="font-size:10px;color:var(--text3);margin-bottom:2px">Password</div>
          <div style="font-size:12px;color:var(--text);font-family:'DM Mono',monospace">Admin@PSF1</div>
        </div>
      </div>
      <button onclick="fillAdmin()" style="width:100%;background:rgba(232,100,26,0.12);border:1px solid rgba(232,100,26,0.3);border-radius:6px;padding:8px;font-size:12px;color:var(--orange);font-family:'DM Sans',sans-serif;cursor:pointer;font-weight:600;transition:background 0.2s" onmouseover="this.style.background='rgba(232,100,26,0.22)'" onmouseout="this.style.background='rgba(232,100,26,0.12)'">
        ⚡ Fill Admin Credentials
      </button>
      <div style="margin-top:10px;padding-top:10px;border-top:1px solid var(--border);font-size:11px;color:var(--text3)">
        Or use any email &amp; password (min 4 chars) and select a role above.
      </div>
    </div>
  </div>
</div>

<!-- ═══════════════════════════════════════════════════
     MAIN APP
═══════════════════════════════════════════════════ -->
<div id="app-screen" class="screen">

  <!-- TOP BAR -->
  <div class="topbar">
    <div class="topbar-logo">
      <div class="topbar-logo-dot"></div>
      PEACH STATE FAB
    </div>
    <div class="topbar-divider"></div>
    <div class="topbar-org">SecureComm Platform</div>
    <div class="topbar-spacer"></div>
    <div class="topbar-user">
      <div class="user-avatar" id="topbar-avatar"></div>
      <div class="user-info">
        <div class="user-name" id="topbar-name"></div>
        <div class="user-role" id="topbar-role"></div>
      </div>
    </div>
    <button class="btn-logout" onclick="doLogout()">Sign Out</button>
  </div>

  <!-- APP BODY -->
  <div class="app-body">

    <!-- SIDEBAR -->
    <div class="sidebar">
      <div class="sidebar-section">
        <div class="sidebar-label">Channels</div>
        <div class="channel-item active" id="ch-general" onclick="switchChannel('general')">
          <span class="ch-icon">#</span>
          <span class="ch-name">general</span>
          <span class="ch-badge" id="badge-general" style="display:none">2</span>
        </div>
        <div class="channel-item" id="ch-production" onclick="switchChannel('production')">
          <span class="ch-icon">#</span>
          <span class="ch-name">production</span>
        </div>
        <div class="channel-item" id="ch-sales" onclick="switchChannel('sales')" data-roles="owner,manager,sales">
          <span class="ch-icon">#</span>
          <span class="ch-name">sales-orders</span>
          <span class="ch-badge" id="badge-sales" style="display:none">1</span>
        </div>
        <div class="channel-item" id="ch-management" onclick="switchChannel('management')" data-roles="owner,manager">
          <span class="ch-icon">🔒</span>
          <span class="ch-name">management</span>
        </div>
        <div class="channel-item" id="ch-docs" onclick="switchChannel('docs')">
          <span class="ch-icon">📁</span>
          <span class="ch-name">documents</span>
        </div>
        <div class="channel-item" id="ch-audit" onclick="switchChannel('audit')" data-roles="owner,manager">
          <span class="ch-icon">🛡</span>
          <span class="ch-name">audit-log</span>
        </div>
      </div>

      <div class="sidebar-divider"></div>

      <div class="sidebar-section">
        <div class="sidebar-label">Online Now</div>
        <div class="channel-item">
          <div class="online-dot"></div>
          <span class="ch-name" style="font-size:12px">Marcus Webb</span>
        </div>
        <div class="channel-item">
          <div class="online-dot"></div>
          <span class="ch-name" style="font-size:12px">Deja Robinson</span>
        </div>
        <div class="channel-item">
          <div class="online-dot"></div>
          <span class="ch-name" style="font-size:12px">Tyler Owens</span>
        </div>
        <div class="channel-item">
          <div class="offline-dot"></div>
          <span class="ch-name" style="font-size:12px;color:var(--text3)">Keisha Morris</span>
        </div>
        <div class="channel-item">
          <div class="offline-dot"></div>
          <span class="ch-name" style="font-size:12px;color:var(--text3)">Ray Simmons</span>
        </div>
      </div>
    </div>

    <!-- MAIN MESSAGES PANEL -->
    <div class="main-panel" id="main-panel">

      <!-- Channel Header -->
      <div class="channel-header">
        <span class="ch-header-icon" id="ch-hdr-icon">#</span>
        <span class="ch-header-name" id="ch-hdr-name">general</span>
        <span class="ch-header-desc" id="ch-hdr-desc">Company-wide updates and announcements</span>
        <span class="ch-header-members">👥 12 members</span>
      </div>

      <!-- Messages -->
      <div class="messages-area" id="messages-area"></div>

      <!-- Compose -->
      <div class="compose-area" id="compose-area">
        <div class="compose-box">
          <textarea class="compose-input" id="compose-input"
            placeholder="Send a message to #general…" rows="1"
            onkeydown="handleCompose(event)"></textarea>
          <div class="compose-actions">
            <button class="compose-btn" onclick="openUploadModal()">📎 Attach</button>
            <button class="compose-btn" onclick="insertEmoji()">😊</button>
            <div class="compose-spacer"></div>
            <span style="font-size:11px;color:var(--text3)">Enter to send</span>
            <button class="btn-send" id="btn-send" onclick="sendMessage()">
              Send ↑
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- RIGHT PANEL -->
    <div class="right-panel" id="right-panel">
      <div class="rp-section">
        <div class="rp-title">Channel Stats</div>
        <div class="rp-stat"><span class="rp-stat-label">Messages today</span><span class="rp-stat-value" id="stat-msgs">0</span></div>
        <div class="rp-stat"><span class="rp-stat-label">Files shared</span><span class="rp-stat-value" id="stat-files">0</span></div>
        <div class="rp-stat"><span class="rp-stat-label">Active members</span><span class="rp-stat-value">3</span></div>
      </div>
      <div class="rp-section" id="rp-files-section">
        <div class="rp-title">Recent Files</div>
        <div id="rp-files-list">
          <div class="rp-file">
            <div class="rp-file-icon">📄</div>
            <div class="rp-file-info">
              <div class="rp-file-name">Job_Spec_WRH-044.pdf</div>
              <div class="rp-file-date">Today, 9:12 AM</div>
            </div>
          </div>
          <div class="rp-file">
            <div class="rp-file-icon">🖼</div>
            <div class="rp-file-info">
              <div class="rp-file-name">steel_beam_drawing.png</div>
              <div class="rp-file-date">Yesterday</div>
            </div>
          </div>
        </div>
      </div>
      <div class="rp-section">
        <div class="rp-title">Channel Members</div>
        <div id="rp-members"></div>
      </div>
    </div>

  </div><!-- app-body -->
</div><!-- app-screen -->

<!-- ═══ FILE UPLOAD MODAL ═══ -->
<div class="modal-overlay" id="upload-modal">
  <div class="modal">
    <div class="modal-header">
      <span class="modal-title">📎 Attach a File</span>
      <button class="modal-close" onclick="closeUploadModal()">✕</button>
    </div>
    <div class="drop-zone" id="drop-zone"
      onclick="document.getElementById('file-input').click()"
      ondragover="event.preventDefault();this.classList.add('dragover')"
      ondragleave="this.classList.remove('dragover')"
      ondrop="handleDrop(event)">
      <div class="drop-icon">📁</div>
      <div class="drop-text">Drop file here or click to browse</div>
      <div class="drop-sub">PDF, PNG, JPG, DWG — max 25 MB</div>
    </div>
    <input type="file" id="file-input" style="display:none" onchange="handleFileSelect(event)">
    <div id="upload-file-info" style="font-size:13px;color:var(--text2);margin-bottom:16px;display:none"></div>
    <div class="modal-actions">
      <button class="btn-ghost" onclick="closeUploadModal()">Cancel</button>
      <button class="btn-primary" style="width:auto;padding:8px 20px;margin-top:0" id="btn-upload" onclick="confirmUpload()">Upload &amp; Share</button>
    </div>
  </div>
</div>

<!-- ═══ TOAST ═══ -->
<div class="toast" id="toast">
  <div class="toast-title" id="toast-title"></div>
  <div class="toast-body" id="toast-body"></div>
</div>

<script>
// ── DATA ──────────────────────────────────────────────────────
const USERS = {
  owner:   { name: 'Alex Carter',    initials: 'AC', role: 'Owner',            roleTag: 'role-owner',   color: '#E8641A' },
  manager: { name: 'Marcus Webb',    initials: 'MW', role: 'Production Mgr',   roleTag: 'role-manager', color: '#2ECC8A' },
  sales:   { name: 'Deja Robinson',  initials: 'DR', role: 'Sales / Admin',    roleTag: 'role-sales',   color: '#5DADE2' },
  shop:    { name: 'Tyler Owens',    initials: 'TO', role: 'Shop Floor',       roleTag: 'role-shop',    color: '#F0A020' }
};

const CHANNEL_META = {
  general:    { icon: '#', name: 'general',      desc: 'Company-wide updates and announcements',          roles: null },
  production: { icon: '#', name: 'production',   desc: 'Job updates, schedules, and shop floor comms',    roles: null },
  sales:      { icon: '#', name: 'sales-orders', desc: 'Customer orders and sales documentation',         roles: ['owner','manager','sales'] },
  management: { icon: '🔒', name: 'management',  desc: 'Management only — restricted access',             roles: ['owner','manager'] },
  docs:       { icon: '📁', name: 'documents',   desc: 'Shared drawings, specs, and project files',       roles: null },
  audit:      { icon: '🛡', name: 'audit-log',   desc: 'System activity log — admin view',                roles: ['owner','manager'] }
};

const SEED_MSGS = {
  general: [
    { user: 'manager', text: 'Good morning team. All stations verified and ready for the WRH-044 job today.', time: '8:04 AM', day: 'Today' },
    { user: 'sales',   text: 'Reminder: warehouse client site visit is confirmed for Thursday 2PM. Please have sample welds ready.', time: '8:31 AM' },
    { user: 'owner',   text: 'Q2 safety walkthrough scheduled for Friday morning. All department heads please attend.', time: '9:15 AM' },
    { user: 'shop',    text: 'Bay 3 plasma cutter serviced and back online. 👍', time: '10:02 AM' },
    { user: 'manager', text: 'Structural steel delivery arriving at 2:30 PM. Need two people at the dock.', time: '11:47 AM', file: { name: 'Delivery_Manifest_0501.pdf', size: '142 KB' } },
  ],
  production: [
    { user: 'manager', text: 'WRH-044: steel beams cut to spec. Moving to welding phase this afternoon.', time: '7:55 AM', day: 'Today' },
    { user: 'shop',    text: 'Welding on bays 1 and 2. ETA for first assembly is 3 PM.', time: '9:30 AM' },
    { user: 'manager', text: 'Updated job spec attached below — client revised load rating requirements.', time: '10:18 AM', file: { name: 'WRH-044_Rev2_Spec.pdf', size: '385 KB' } },
    { user: 'shop',    text: 'First assembly complete. Quality check passed. Moving to coating.', time: '3:12 PM' },
  ],
  sales: [
    { user: 'sales',   text: 'New RFQ from Cornerstone Construction for 40-ft I-beam assemblies. Sending to Marcus for feasibility.', time: '8:50 AM', day: 'Today' },
    { user: 'manager', text: 'Feasibility looks good. 2-week lead time if we get PO by end of week.', time: '9:20 AM' },
    { user: 'owner',   text: 'Go ahead and quote at standard rates. Let me know if they push back.', time: '10:05 AM', file: { name: 'Cornerstone_RFQ_0501.pdf', size: '210 KB' } },
  ],
  management: [
    { user: 'owner',   text: 'Q1 revenue up 14% YoY. Good work. Targeting 10% margin improvement in Q2 through workflow optimization.', time: '8:00 AM', day: 'Today' },
    { user: 'manager', text: 'Main bottleneck is still the coating line. Recommending we bring in a second unit by June.', time: '8:45 AM' },
    { user: 'owner',   text: 'Agreed. Get me a quote by end of week and we'll review capex.', time: '9:00 AM' },
  ],
  docs: [
    { user: 'sales',   text: 'Uploading all active client drawings for WRH and Cornerstone projects:', time: '8:15 AM', day: 'Today', file: { name: 'Client_Drawings_Bundle.zip', size: '2.4 MB' } },
    { user: 'manager', text: 'Updated assembly instructions for structural beam series:', time: '9:45 AM', file: { name: 'Beam_Assembly_SOP_v3.pdf', size: '780 KB' } },
  ],
  audit: []
};

const AUDIT_LOG = [
  { time: '11:52', icon: '🔑', text: '<b>Tyler Owens</b> logged in from 192.168.1.44' },
  { time: '11:47', icon: '📤', text: '<b>Marcus Webb</b> uploaded <b>Delivery_Manifest_0501.pdf</b> in #production' },
  { time: '10:18', icon: '📤', text: '<b>Marcus Webb</b> uploaded <b>WRH-044_Rev2_Spec.pdf</b> in #production' },
  { time: '10:05', icon: '📤', text: '<b>Alex Carter</b> uploaded <b>Cornerstone_RFQ_0501.pdf</b> in #sales-orders' },
  { time: '09:15', icon: '💬', text: '<b>Alex Carter</b> posted in #general' },
  { time: '08:50', icon: '💬', text: '<b>Deja Robinson</b> posted in #sales-orders' },
  { time: '08:31', icon: '💬', text: '<b>Deja Robinson</b> posted in #general' },
  { time: '08:15', icon: '📤', text: '<b>Deja Robinson</b> uploaded <b>Client_Drawings_Bundle.zip</b> in #documents' },
  { time: '08:04', icon: '🔑', text: '<b>Marcus Webb</b> logged in from 192.168.1.22' },
  { time: '08:00', icon: '🔑', text: '<b>Alex Carter</b> logged in from 192.168.1.10' },
];

const MEMBERS = {
  general:    ['owner','manager','sales','shop'],
  production: ['manager','shop'],
  sales:      ['owner','manager','sales'],
  management: ['owner','manager'],
  docs:       ['owner','manager','sales','shop'],
  audit:      ['owner','manager'],
};

// ── STATE ──────────────────────────────────────────────────────
let currentUser = null;
let currentChannel = 'general';
let msgCounts = { general: 0, production: 0, sales: 0, management: 0, docs: 0 };
let fileCounts = { general: 1, production: 1, sales: 1, management: 0, docs: 2 };
let selectedFile = null;

// ── LOGIN ──────────────────────────────────────────────────────
const ADMIN_ACCOUNT = { email: 'admin@peachstatefab.com', password: 'Admin@PSF1', role: 'owner' };

function fillAdmin() {
  document.getElementById('login-email').value = ADMIN_ACCOUNT.email;
  document.getElementById('login-password').value = ADMIN_ACCOUNT.password;
  document.getElementById('login-role').value = ADMIN_ACCOUNT.role;
  document.getElementById('login-error').textContent = '';
}

function doLogin() {
  const email = document.getElementById('login-email').value.trim();
  const pass  = document.getElementById('login-password').value;
  const role  = document.getElementById('login-role').value;
  const err   = document.getElementById('login-error');
  if (!email || !email.includes('@')) { err.textContent = 'Please enter a valid email address.'; return; }
  if (pass.length < 4) { err.textContent = 'Password must be at least 4 characters.'; return; }
  const isAdmin = email === ADMIN_ACCOUNT.email && pass === ADMIN_ACCOUNT.password;
  if (email === ADMIN_ACCOUNT.email && !isAdmin) { err.textContent = 'Incorrect password for admin account.'; return; }
  err.textContent = '';
  currentUser = isAdmin ? ADMIN_ACCOUNT.role : role;
  const u = USERS[role];
  document.getElementById('topbar-avatar').textContent = u.initials;
  document.getElementById('topbar-avatar').style.background = u.color;
  document.getElementById('topbar-name').textContent = u.name;
  document.getElementById('topbar-role').textContent = u.role;
  document.getElementById('login-screen').classList.remove('active');
  document.getElementById('app-screen').classList.add('active');
  applyRoleAccess();
  switchChannel('general');
  addAuditEntry('🔑', `<b>${u.name}</b> logged in${isAdmin ? ' <span style="color:var(--orange)">[ADMIN]</span>' : ''}`);
  setTimeout(() => showToast(isAdmin ? 'Admin Access Granted' : 'Signed in', isAdmin ? `Welcome, ${u.name.split(' ')[0]}. Full access enabled.` : `Welcome back, ${u.name.split(' ')[0]}!`), 400);
}

function doLogout() {
  document.getElementById('app-screen').classList.remove('active');
  document.getElementById('login-screen').classList.add('active');
  document.getElementById('login-email').value = '';
  document.getElementById('login-password').value = '';
  currentUser = null;
}

// ── ROLE ACCESS ──────────────────────────────────────────────────
function applyRoleAccess() {
  ['sales','management','audit'].forEach(ch => {
    const el = document.getElementById('ch-' + ch);
    const meta = CHANNEL_META[ch];
    if (meta.roles && !meta.roles.includes(currentUser)) {
      el.style.opacity = '0.35';
      el.style.pointerEvents = 'none';
    } else {
      el.style.opacity = '';
      el.style.pointerEvents = '';
    }
  });
}

// ── CHANNEL SWITCHING ─────────────────────────────────────────
function switchChannel(ch) {
  const meta = CHANNEL_META[ch];
  if (meta.roles && !meta.roles.includes(currentUser)) {
    showToast('Access Denied', 'You do not have permission to view this channel.', true);
    return;
  }
  document.querySelectorAll('.channel-item').forEach(el => el.classList.remove('active'));
  document.getElementById('ch-' + ch).classList.add('active');
  currentChannel = ch;
  document.getElementById('ch-hdr-icon').textContent = meta.icon;
  document.getElementById('ch-hdr-name').textContent = meta.name;
  document.getElementById('ch-hdr-desc').textContent = meta.desc;
  document.getElementById('compose-input').placeholder = `Send a message to #${meta.name}…`;
  renderMessages();
  renderRightPanel();
  clearBadge(ch);
}

// ── RENDER MESSAGES ───────────────────────────────────────────
function renderMessages() {
  const area = document.getElementById('messages-area');
  const composeArea = document.getElementById('compose-area');
  area.innerHTML = '';

  if (currentChannel === 'audit') {
    composeArea.style.display = 'none';
    const log = [...AUDIT_LOG].reverse();
    log.forEach(entry => {
      const div = document.createElement('div');
      div.className = 'log-entry';
      div.innerHTML = `<span class="log-time">${entry.time}</span><span class="log-icon">${entry.icon}</span><span class="log-text">${entry.text}</span>`;
      area.appendChild(div);
    });
    area.scrollTop = area.scrollHeight;
    return;
  }
  composeArea.style.display = '';

  const msgs = SEED_MSGS[currentChannel] || [];
  let lastDay = '';
  msgs.forEach(msg => {
    if (msg.day && msg.day !== lastDay) {
      lastDay = msg.day;
      const div = document.createElement('div');
      div.className = 'day-divider';
      div.textContent = msg.day;
      area.appendChild(div);
    }
    area.appendChild(buildMsgEl(msg));
  });
  area.scrollTop = area.scrollHeight;
  updateStats();
}

function buildMsgEl(msg) {
  const u = USERS[msg.user];
  const div = document.createElement('div');
  div.className = 'msg-group';
  let fileHtml = '';
  if (msg.file) {
    fileHtml = `<div class="msg-file-attach">
      <span class="file-icon">📄</span>
      <div class="file-meta">
        <div class="file-name">${msg.file.name}</div>
        <div class="file-size">${msg.file.size}</div>
      </div>
    </div>`;
  }
  div.innerHTML = `
    <div class="msg-header">
      <span class="msg-author">${u.name}</span>
      <span class="msg-role-tag ${u.roleTag}">${u.role}</span>
      <span class="msg-time">${msg.time}</span>
    </div>
    <div class="msg-bubble">${msg.text}${fileHtml}</div>
  `;
  return div;
}

// ── SEND MESSAGE ──────────────────────────────────────────────
function handleCompose(e) {
  if (e.key === 'Enter' && !e.shiftKey) { e.preventDefault(); sendMessage(); }
}

function sendMessage() {
  const input = document.getElementById('compose-input');
  const text = input.value.trim();
  if (!text) return;
  const now = new Date();
  const time = now.toLocaleTimeString('en-US', { hour: 'numeric', minute: '2-digit' });
  const msg = { user: currentUser, text, time };
  if (!SEED_MSGS[currentChannel]) SEED_MSGS[currentChannel] = [];
  SEED_MSGS[currentChannel].push(msg);
  msgCounts[currentChannel] = (msgCounts[currentChannel] || 0) + 1;
  input.value = '';
  const area = document.getElementById('messages-area');
  area.appendChild(buildMsgEl(msg));
  area.scrollTop = area.scrollHeight;
  updateStats();
  addAuditEntry('💬', `<b>${USERS[currentUser].name}</b> posted in #${CHANNEL_META[currentChannel].name}`);
}

function insertEmoji() {
  const emojis = ['👍','✅','🔥','⚠️','📋','🔧','🏗️'];
  const e = emojis[Math.floor(Math.random() * emojis.length)];
  const inp = document.getElementById('compose-input');
  inp.value += e;
  inp.focus();
}

// ── FILE UPLOAD ───────────────────────────────────────────────
function openUploadModal() {
  document.getElementById('upload-modal').classList.add('open');
  selectedFile = null;
  document.getElementById('upload-file-info').style.display = 'none';
}
function closeUploadModal() {
  document.getElementById('upload-modal').classList.remove('open');
  document.getElementById('drop-zone').classList.remove('dragover');
}

function handleFileSelect(e) {
  const f = e.target.files[0];
  if (f) previewFile(f);
}
function handleDrop(e) {
  e.preventDefault();
  document.getElementById('drop-zone').classList.remove('dragover');
  const f = e.dataTransfer.files[0];
  if (f) previewFile(f);
}
function previewFile(f) {
  selectedFile = f;
  const info = document.getElementById('upload-file-info');
  info.style.display = 'block';
  info.innerHTML = `📄 <b>${f.name}</b> — ${formatBytes(f.size)}`;
}
function formatBytes(b) {
  if (b < 1024) return b + ' B';
  if (b < 1048576) return (b/1024).toFixed(1) + ' KB';
  return (b/1048576).toFixed(1) + ' MB';
}

function confirmUpload() {
  if (!selectedFile) { showToast('No file selected', 'Please choose a file first.', true); return; }
  closeUploadModal();
  const now = new Date();
  const time = now.toLocaleTimeString('en-US', { hour: 'numeric', minute: '2-digit' });
  const msg = {
    user: currentUser,
    text: `Shared a file:`,
    time,
    file: { name: selectedFile.name, size: formatBytes(selectedFile.size) }
  };
  if (!SEED_MSGS[currentChannel]) SEED_MSGS[currentChannel] = [];
  SEED_MSGS[currentChannel].push(msg);
  fileCounts[currentChannel] = (fileCounts[currentChannel] || 0) + 1;
  const area = document.getElementById('messages-area');
  area.appendChild(buildMsgEl(msg));
  area.scrollTop = area.scrollHeight;
  updateStats();
  addRpFile(selectedFile.name, time);
  addAuditEntry('📤', `<b>${USERS[currentUser].name}</b> uploaded <b>${selectedFile.name}</b> in #${CHANNEL_META[currentChannel].name}`);
  showToast('File shared', `${selectedFile.name} uploaded successfully.`);
  selectedFile = null;
}

// ── RIGHT PANEL ───────────────────────────────────────────────
function renderRightPanel() {
  const members = MEMBERS[currentChannel] || [];
  const el = document.getElementById('rp-members');
  el.innerHTML = members.map(r => {
    const u = USERS[r];
    return `<div class="rp-member">
      <div class="rp-member-avatar" style="background:${u.color}22;color:${u.color}">${u.initials}</div>
      <div class="rp-member-info">
        <div class="rp-member-name">${u.name}</div>
        <div class="rp-member-role">${u.role}</div>
      </div>
    </div>`;
  }).join('');
}

function addRpFile(name, time) {
  const list = document.getElementById('rp-files-list');
  const ext = name.split('.').pop().toLowerCase();
  const icon = ext === 'pdf' ? '📄' : (ext === 'png' || ext === 'jpg' ? '🖼' : '📁');
  const div = document.createElement('div');
  div.className = 'rp-file';
  div.innerHTML = `<div class="rp-file-icon">${icon}</div><div class="rp-file-info"><div class="rp-file-name">${name}</div><div class="rp-file-date">Today, ${time}</div></div>`;
  list.prepend(div);
}

function updateStats() {
  const msgs = SEED_MSGS[currentChannel] || [];
  document.getElementById('stat-msgs').textContent = msgs.length;
  document.getElementById('stat-files').textContent = fileCounts[currentChannel] || 0;
}

// ── AUDIT LOG ─────────────────────────────────────────────────
function addAuditEntry(icon, text) {
  const now = new Date();
  const time = now.toLocaleTimeString('en-US', { hour: '2-digit', minute: '2-digit', hour12: false });
  AUDIT_LOG.unshift({ time, icon, text });
  if (currentChannel === 'audit') renderMessages();
}

// ── TOAST ─────────────────────────────────────────────────────
function showToast(title, body, isError = false) {
  const t = document.getElementById('toast');
  document.getElementById('toast-title').textContent = title;
  document.getElementById('toast-body').textContent = body;
  t.style.borderLeftColor = isError ? 'var(--red)' : 'var(--green)';
  t.classList.add('show');
  clearTimeout(t._timer);
  t._timer = setTimeout(() => t.classList.remove('show'), 3000);
}

function clearBadge(ch) {
  const b = document.getElementById('badge-' + ch);
  if (b) b.style.display = 'none';
}

// ── INIT ──────────────────────────────────────────────────────
document.getElementById('login-password').addEventListener('keydown', e => {
  if (e.key === 'Enter') doLogin();
});
</script>
</body>
</html>
