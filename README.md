<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>🐝 Calculadora SolanaBee</title>
<style>
    :root {
        --bg: #0a0812;
        --card: #14101f;
        --card-hover: #1a1528;
        --border: #2a2340;
        --gold: #f7b32b;
        --gold-bright: #ffd155;
        --text: #f0edf8;
        --text-dim: #8a83a8;
        --success: #4ade80;
        --danger: #ff5577;
        --info: #60a5fa;
    }

    * { box-sizing: border-box; margin: 0; padding: 0; }

    body {
        font-family: 'Inter', system-ui, -apple-system, sans-serif;
        background: var(--bg);
        color: var(--text);
        min-height: 100vh;
        padding: 40px 20px 80px;
        background-image:
            radial-gradient(circle at 15% 0%, rgba(247,179,43,0.06) 0%, transparent 45%),
            radial-gradient(circle at 85% 100%, rgba(160,100,255,0.06) 0%, transparent 45%);
        line-height: 1.5;
    }

    .container { max-width: 820px; margin: 0 auto; }

    header { text-align: center; margin-bottom: 30px; }

    h1 {
        font-size: 2.4rem;
        font-weight: 900;
        letter-spacing: -1px;
        background: linear-gradient(135deg, #f7b32b 0%, #ffd155 50%, #f7b32b 100%);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        background-clip: text;
        background-size: 200% auto;
        animation: shine 4s linear infinite;
    }

    @keyframes shine { to { background-position: 200% center; } }

    .subtitle { color: var(--text-dim); font-size: 0.95rem; margin-top: 8px; }

    /* ====== TABS ====== */
    .tabs {
        display: flex;
        background: var(--card);
        border: 1px solid var(--border);
        border-radius: 100px;
        padding: 6px;
        margin-bottom: 30px;
        gap: 6px;
    }

    .tab-btn {
        flex: 1;
        background: transparent;
        border: none;
        color: var(--text-dim);
        padding: 14px 20px;
        border-radius: 100px;
        font-size: 0.95rem;
        font-weight: 700;
        cursor: pointer;
        transition: all 0.3s;
        font-family: inherit;
        white-space: nowrap;
    }

    .tab-btn:hover { color: var(--text); }

    .tab-btn.active {
        background: var(--gold);
        color: #0a0812;
        box-shadow: 0 4px 15px rgba(247,179,43,0.3);
    }

    /* ====== STEPS / CARDS ====== */
    .step {
        background: var(--card);
        border: 1px solid var(--border);
        border-radius: 20px;
        padding: 28px;
        margin-bottom: 20px;
        animation: slideIn 0.5s cubic-bezier(0.4, 0, 0.2, 1);
        position: relative;
    }

    @keyframes slideIn {
        from { opacity: 0; transform: translateY(20px); }
        to { opacity: 1; transform: translateY(0); }
    }

    .step-header {
        display: flex;
        align-items: center;
        gap: 14px;
        margin-bottom: 22px;
    }

    .step-number {
        width: 32px;
        height: 32px;
        border-radius: 50%;
        background: linear-gradient(135deg, var(--gold), #ffa500);
        color: #0a0812;
        font-weight: 900;
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 0.95rem;
        flex-shrink: 0;
        box-shadow: 0 4px 15px rgba(247,179,43,0.3);
    }

    .step-title {
        font-size: 1.15rem;
        font-weight: 700;
        letter-spacing: -0.3px;
        flex: 1;
    }

    .btn-back {
        background: transparent;
        border: 1px solid var(--border);
        color: var(--text-dim);
        padding: 6px 14px;
        border-radius: 8px;
        font-size: 0.8rem;
        cursor: pointer;
        transition: all 0.2s;
        font-family: inherit;
        font-weight: 600;
    }
    .btn-back:hover { border-color: var(--gold); color: var(--gold); }

    /* ====== SPECIES ====== */
    .species-grid {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 14px;
    }

    .species-card {
        background: var(--card-hover);
        border: 2px solid var(--border);
        border-radius: 16px;
        padding: 24px 14px;
        text-align: center;
        cursor: pointer;
        transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        position: relative;
        overflow: hidden;
        user-select: none;
    }

    .species-card::before {
        content: '';
        position: absolute;
        inset: 0;
        background: radial-gradient(circle at 50% 0%, rgba(247,179,43,0.15), transparent 70%);
        opacity: 0;
        transition: opacity 0.3s;
    }

    .species-card:hover {
        border-color: var(--gold);
        transform: translateY(-4px);
        box-shadow: 0 15px 35px rgba(247,179,43,0.2);
    }
    .species-card:hover::before { opacity: 1; }
    .species-card:hover .species-emoji { transform: scale(1.15) rotate(-6deg); }

    .species-card.selected {
        border-color: var(--gold);
        background: linear-gradient(160deg, #1e1708, #1a1528);
        box-shadow: 0 0 0 3px rgba(247,179,43,0.2), 0 15px 35px rgba(247,179,43,0.25);
    }

    .species-card.selected::after {
        content: '✓';
        position: absolute;
        top: 10px;
        right: 10px;
        width: 22px;
        height: 22px;
        background: var(--gold);
        color: #0a0812;
        border-radius: 50%;
        font-size: 0.85rem;
        font-weight: 900;
        display: flex;
        align-items: center;
        justify-content: center;
        animation: pop 0.3s ease;
    }

    @keyframes pop {
        0% { transform: scale(0); }
        80% { transform: scale(1.2); }
        100% { transform: scale(1); }
    }

    .species-emoji {
        font-size: 2.8rem;
        display: block;
        margin-bottom: 10px;
        transition: transform 0.4s cubic-bezier(0.4, 0, 0.2, 1);
        position: relative;
    }

    .species-name { font-weight: 700; font-size: 1rem; position: relative; }

    /* ====== RARITY ====== */
    .rarity-grid {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 12px;
    }

    .rarity-card {
        background: var(--card-hover);
        border: 2px solid var(--border);
        border-radius: 14px;
        padding: 18px 12px;
        text-align: center;
        cursor: pointer;
        transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        font-weight: 700;
        font-size: 0.9rem;
        position: relative;
        overflow: hidden;
        user-select: none;
    }

    .rarity-card::before {
        content: '';
        position: absolute;
        inset: 0;
        background: linear-gradient(160deg, var(--r-glow), transparent 60%);
        opacity: 0.3;
        transition: opacity 0.3s;
    }

    [data-rarity="Comum"]    { --r-color: #9ca3af; --r-glow: rgba(156,163,175,0.4); }
    [data-rarity="Incomum"]  { --r-color: #4ade80; --r-glow: rgba(74,222,128,0.4); }
    [data-rarity="Raro"]     { --r-color: #60a5fa; --r-glow: rgba(96,165,250,0.4); }
    [data-rarity="Épico"]    { --r-color: #a78bfa; --r-glow: rgba(167,139,250,0.4); }
    [data-rarity="Lendário"] { --r-color: #fbbf24; --r-glow: rgba(251,191,36,0.5); }
    [data-rarity="Místico"]  { --r-color: #f472b6; --r-glow: rgba(244,114,182,0.5); }

    .rarity-card:hover {
        border-color: var(--r-color);
        transform: translateY(-3px);
        box-shadow: 0 12px 30px var(--r-glow);
    }
    .rarity-card:hover::before { opacity: 1; }

    .rarity-card.selected {
        border-color: var(--r-color);
        background: var(--card-hover);
        box-shadow: 0 0 0 3px var(--r-glow), 0 12px 30px var(--r-glow);
    }
    .rarity-card.selected::before { opacity: 1; }
    .rarity-card.selected::after {
        content: '✓';
        position: absolute;
        top: 8px;
        right: 8px;
        width: 20px;
        height: 20px;
        background: var(--r-color);
        color: #0a0812;
        border-radius: 50%;
        font-size: 0.75rem;
        font-weight: 900;
        display: flex;
        align-items: center;
        justify-content: center;
        animation: pop 0.3s ease;
    }

    .rarity-card span { position: relative; }

    /* ====== FORMS ====== */
    .form-group { margin-bottom: 20px; }

    .form-label {
        display: block;
        font-size: 0.85rem;
        color: var(--text-dim);
        margin-bottom: 8px;
        font-weight: 600;
    }

    .form-label .req { color: var(--gold); }

    .form-row {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 14px;
    }

    .form-row-3 {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 12px;
    }

    .form-input-wrap { position: relative; }

    .form-input {
        width: 100%;
        background: var(--bg);
        border: 1.5px solid var(--border);
        border-radius: 12px;
        padding: 14px 16px;
        padding-right: 55px;
        color: var(--text);
        font-size: 1rem;
        font-weight: 700;
        transition: all 0.2s;
        font-family: inherit;
    }

    .form-input:focus {
        outline: none;
        border-color: var(--gold);
        box-shadow: 0 0 0 4px rgba(247,179,43,0.15);
    }

    .form-input::-webkit-outer-spin-button,
    .form-input::-webkit-inner-spin-button { -webkit-appearance: none; margin: 0; }
    .form-input[type=number] { -moz-appearance: textfield; }

    .unit {
        position: absolute;
        right: 16px;
        top: 50%;
        transform: translateY(-50%);
        color: var(--text-dim);
        font-size: 0.8rem;
        font-weight: 700;
        pointer-events: none;
    }

    .form-hint {
        font-size: 0.75rem;
        color: var(--text-dim);
        margin-top: 6px;
        opacity: 0.8;
    }

    /* ====== ALERTS / WARNINGS ====== */
    .alert-box {
        background: rgba(255,85,119,0.1);
        border: 1.5px solid rgba(255,85,119,0.4);
        border-radius: 14px;
        padding: 18px;
        margin-bottom: 24px;
        color: #ffb3c1;
        font-size: 0.9rem;
        line-height: 1.6;
    }
    .alert-box strong { color: var(--danger); display: block; margin-bottom: 6px; font-size: 1rem; }
    
    .info-box {
        background: rgba(96,165,250,0.1);
        border: 1.5px solid rgba(96,165,250,0.4);
        border-radius: 14px;
        padding: 18px;
        margin-bottom: 24px;
        color: #bfdbfe;
        font-size: 0.9rem;
        line-height: 1.6;
    }
    .info-box strong { color: var(--info); display: block; margin-bottom: 6px; font-size: 1rem; }

    /* ====== BUTTONS ====== */
    .btn-calc {
        width: 100%;
        background: linear-gradient(135deg, #f7b32b, #ff9500);
        color: #0a0812;
        border: none;
        padding: 18px;
        border-radius: 14px;
        font-size: 1rem;
        font-weight: 900;
        cursor: pointer;
        transition: all 0.3s;
        text-transform: uppercase;
        letter-spacing: 1.5px;
        margin-top: 8px;
        position: relative;
        overflow: hidden;
        font-family: inherit;
        box-shadow: 0 6px 20px rgba(247,179,43,0.3);
    }

    .btn-calc::after {
        content: '';
        position: absolute;
        top: 0;
        left: -100%;
        width: 100%;
        height: 100%;
        background: linear-gradient(90deg, transparent, rgba(255,255,255,0.5), transparent);
        transition: left 0.6s;
    }

    .btn-calc:hover {
        transform: translateY(-2px);
        box-shadow: 0 12px 30px rgba(247,179,43,0.5);
    }
    .btn-calc:hover::after { left: 100%; }
    .btn-calc:active { transform: translateY(0); }

    .btn-reset {
        width: 100%;
        background: transparent;
        border: 1.5px solid var(--border);
        color: var(--text);
        padding: 16px;
        border-radius: 14px;
        font-size: 0.95rem;
        font-weight: 700;
        cursor: pointer;
        transition: all 0.2s;
        font-family: inherit;
        margin-top: 10px;
    }

    .btn-reset:hover {
        border-color: var(--gold);
        color: var(--gold);
        background: rgba(247,179,43,0.05);
    }

    /* ====== RESULTS ====== */
    .verdict {
        display: inline-flex;
        align-items: center;
        gap: 10px;
        padding: 10px 20px;
        border-radius: 100px;
        font-weight: 800;
        font-size: 0.85rem;
        text-transform: uppercase;
        letter-spacing: 1px;
        margin-bottom: 20px;
    }
    .verdict.profit { background: rgba(74,222,128,0.15); color: var(--success); border: 1px solid rgba(74,222,128,0.35); }
    .verdict.loss { background: rgba(255,85,119,0.15); color: var(--danger); border: 1px solid rgba(255,85,119,0.35); }

    .hero-result {
        background: linear-gradient(160deg, #1e1708 0%, #1a1528 100%);
        border: 2px solid rgba(247,179,43,0.4);
        border-radius: 20px;
        padding: 32px;
        text-align: center;
        margin-bottom: 18px;
        position: relative;
        overflow: hidden;
    }

    .hero-result::before {
        content: '';
        position: absolute;
        top: -50%;
        left: -50%;
        width: 200%;
        height: 200%;
        background: radial-gradient(circle, rgba(247,179,43,0.12) 0%, transparent 60%);
        animation: pulse 4s ease-in-out infinite;
        pointer-events: none;
    }

    .hero-result.loss::before { background: radial-gradient(circle, rgba(255,85,119,0.12) 0%, transparent 60%); }

    @keyframes pulse {
        0%, 100% { transform: scale(1); opacity: 0.6; }
        50% { transform: scale(1.1); opacity: 1; }
    }

    .hero-label {
        font-size: 0.8rem;
        text-transform: uppercase;
        letter-spacing: 2.5px;
        color: var(--text-dim);
        margin-bottom: 10px;
        position: relative;
        font-weight: 700;
    }

    .hero-value {
        font-size: 3.2rem;
        font-weight: 900;
        letter-spacing: -1.5px;
        position: relative;
        line-height: 1;
        background: linear-gradient(135deg, #f7b32b 0%, #ffd155 100%);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        background-clip: text;
        font-variant-numeric: tabular-nums;
    }

    .hero-result.loss .hero-value { background: linear-gradient(135deg, #ff5577, #ff8866); -webkit-background-clip: text; background-clip: text; }

    .hero-unit { font-size: 1rem; color: var(--text-dim); margin-top: 8px; position: relative; font-weight: 600; }

    .result-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-bottom: 20px; }

    .result-card {
        background: var(--card);
        border: 1px solid var(--border);
        border-radius: 14px;
        padding: 18px 20px;
        transition: all 0.25s;
    }

    .result-card:hover { border-color: rgba(247,179,43,0.4); transform: translateY(-2px); }

    .result-label {
        font-size: 0.72rem;
        text-transform: uppercase;
        letter-spacing: 1.2px;
        color: var(--text-dim);
        margin-bottom: 6px;
        font-weight: 700;
    }

    .result-value {
        font-size: 1.35rem;
        font-weight: 800;
        letter-spacing: -0.5px;
        font-variant-numeric: tabular-nums;
    }

    .result-value.gold { color: var(--gold); }
    .result-value.green { color: var(--success); }
    .result-value.red { color: var(--danger); }
    .result-value.info { color: var(--info); }

    .result-sub { font-size: 0.75rem; color: var(--text-dim); margin-top: 3px; font-weight: 500; }

    .hidden { display: none !important; }

    /* ====== RESPONSIVE (MOBILE) ====== */
    @media (max-width: 640px) {
        body { padding: 20px 12px 60px; }
        h1 { font-size: 1.8rem; }
        .subtitle { font-size: 0.85rem; }
        
        .tabs { flex-direction: column; border-radius: 16px; padding: 8px; }
        .tab-btn { padding: 12px 16px; font-size: 0.9rem; border-radius: 10px; text-align: center; }
        
        .step { padding: 20px; border-radius: 16px; }
        .step-title { font-size: 1.05rem; }
        
        .species-grid { grid-template-columns: repeat(2, 1fr); }
        .species-grid .species-card:last-child { grid-column: span 2; }
        .species-card { padding: 18px 10px; }
        .species-emoji { font-size: 2.2rem; }
        .species-name { font-size: 0.9rem; }
        
        .rarity-grid { grid-template-columns: repeat(2, 1fr); }
        .rarity-card { padding: 14px 8px; font-size: 0.85rem; }
        
        .form-row { grid-template-columns: 1fr; gap: 12px; }
        .form-row-3 { grid-template-columns: 1fr; gap: 12px; }
        .form-input { padding: 16px 14px; padding-right: 50px; font-size: 1rem; }
        
        .hero-value { font-size: 2.4rem; }
        .hero-result { padding: 24px 16px; }
        
        .result-grid { grid-template-columns: 1fr; }
        
        .alert-box, .info-box { padding: 14px; font-size: 0.85rem; }
    }
</style>
</head>
<body>
<div class="container">
    <header>
        <h1>🐝 SolanaBee Tools</h1>
        <p class="subtitle">Ferramentas para maximizar seus ganhos</p>
    </header>

    <!-- ABAS DE NAVEGAÇÃO -->
    <div class="tabs">
        <button class="tab-btn active" onclick="switchTab('abelha')">🐝 Analisar Abelha</button>
        <button class="tab-btn" onclick="switchTab('roi')">📊 Cálculo de ROI / Saída</button>
    </div>

    <!-- ========================================== -->
    <!-- ABA 1: ANÁLISE DE ABELHA                   -->
    <!-- ========================================== -->
    <div id="tab-abelha">
        <!-- STEP 1: Espécie -->
        <section class="step" id="step-species">
            <div class="step-header">
                <div class="step-number">1</div>
                <div class="step-title">Qual é a espécie da sua abelha?</div>
            </div>
            <div class="species-grid">
                <div class="species-card" data-species="Europeia">
                    <span class="species-emoji">🐝</span>
                    <div class="species-name">Europeia</div>
                </div>
                <div class="species-card" data-species="Jataí">
                    <span class="species-emoji">🍯</span>
                    <div class="species-name">Jataí</div>
                </div>
                <div class="species-card" data-species="Mamangava">
                    <span class="species-emoji">🦋</span>
                    <div class="species-name">Mamangava</div>
                </div>
            </div>
        </section>

        <!-- STEP 2: Raridade -->
        <section class="step hidden" id="step-rarity">
            <div class="step-header">
                <div class="step-number">2</div>
                <div class="step-title">E qual é a raridade?</div>
                <button class="btn-back" onclick="voltar('species')">← Voltar</button>
            </div>
            <div class="rarity-grid">
                <div class="rarity-card" data-rarity="Comum"><span>Comum</span></div>
                <div class="rarity-card" data-rarity="Incomum"><span>Incomum</span></div>
                <div class="rarity-card" data-rarity="Raro"><span>Raro</span></div>
                <div class="rarity-card" data-rarity="Épico"><span>Épico</span></div>
                <div class="rarity-card" data-rarity="Lendário"><span>Lendário</span></div>
                <div class="rarity-card" data-rarity="Místico"><span>Místico</span></div>
            </div>
        </section>

        <!-- STEP 3: Dados -->
        <section class="step hidden" id="step-form">
            <div class="step-header">
                <div class="step-number">3</div>
                <div class="step-title">Dados da abelha</div>
                <button class="btn-back" onclick="voltar('rarity')">← Voltar</button>
            </div>

            <div class="form-group">
                <label class="form-label">Quanto você pagou por ela? <span class="req">*</span></label>
                <div class="form-input-wrap">
                    <input type="number" class="form-input" id="preco" placeholder="0.00" min="0" step="0.01">
                    <span class="unit">NEC</span>
                </div>
            </div>

            <div class="form-group">
                <label class="form-label">Vida restante antes da primeira renovação <span class="req">*</span></label>
                <div class="form-row">
                    <div class="form-input-wrap">
                        <input type="number" class="form-input" id="dias" placeholder="0" min="0" step="1">
                        <span class="unit">dias</span>
                    </div>
                    <div class="form-input-wrap">
                        <input type="number" class="form-input" id="horas" placeholder="0" min="0" max="23" step="1">
                        <span class="unit">horas</span>
                    </div>
                </div>
                <div class="form-hint">Pode colocar 0 em ambos se ela já está pronta para renovar</div>
            </div>

            <div class="form-row">
                <div class="form-group">
                    <label class="form-label">Renovações restantes <span class="req">*</span></label>
                    <div class="form-input-wrap">
                        <input type="number" class="form-input" id="renovacoes" placeholder="0" min="0" max="10" step="1">
                        <span class="unit">un.</span>
                    </div>
                </div>
                <div class="form-group">
                    <label class="form-label">Custo por renovação <span class="req">*</span></label>
                    <div class="form-input-wrap">
                        <input type="number" class="form-input" id="custo-renov" placeholder="0.00" min="0" step="0.01">
                        <span class="unit">NEC</span>
                    </div>
                </div>
            </div>
            <div class="form-hint" style="margin-top:-10px;margin-bottom:20px;">Cada renovação dura 24 horas (1 dia)</div>

            <div class="form-group">
                <label class="form-label">Quanto ela farma por dia (Base)? <span class="req">*</span></label>
                <div class="form-input-wrap">
                    <input type="number" class="form-input" id="producao" placeholder="0.00" min="0" step="0.01">
                    <span class="unit">NEC/dia</span>
                </div>
            </div>

            <button class="btn-calc" onclick="calcularAbelha()">Calcular Lucro</button>
        </section>

        <!-- STEP 4: Resultado -->
        <section class="step hidden" id="step-results">
            <div class="step-header">
                <div class="step-number">4</div>
                <div class="step-title">Resultado da análise</div>
            </div>

            <div id="verdict" class="verdict profit">✅ Investimento lucrativo</div>

            <div id="hero" class="hero-result">
                <div class="hero-label">Lucro Líquido</div>
                <div class="hero-value" id="hero-value">0.00</div>
                <div class="hero-unit">NEC</div>
            </div>

            <div class="result-grid">
                <div class="result-card">
                    <div class="result-label">Produção Total</div>
                    <div class="result-value green" id="r-producao">0.00 NEC</div>
                    <div class="result-sub" id="r-producao-sub">Durante toda a vida útil</div>
                </div>
                <div class="result-card">
                    <div class="result-label">Investimento Total</div>
                    <div class="result-value red" id="r-investimento">0.00 NEC</div>
                    <div class="result-sub">Compra + renovações</div>
                </div>
                <div class="result-card">
                    <div class="result-label">Custo de Renovações</div>
                    <div class="result-value" id="r-custo">0.00 NEC</div>
                    <div class="result-sub" id="r-custo-sub">0 renovações</div>
                </div>
                <div class="result-card">
                    <div class="result-label">Lucro por Dia</div>
                    <div class="result-value gold" id="r-lucro-dia">0.00 NEC</div>
                    <div class="result-sub">Média diária líquida</div>
                </div>
                <div class="result-card">
                    <div class="result-label">Vida Total Restante</div>
                    <div class="result-value" id="r-vida">0 dias</div>
                    <div class="result-sub" id="r-vida-sub">—</div>
                </div>
                <div class="result-card">
                    <div class="result-label">Retorno do Investimento</div>
                    <div class="result-value gold" id="r-roi">0%</div>
                    <div class="result-sub" id="r-roi-sub">Sobre o preço pago</div>
                </div>
            </div>

            <button class="btn-reset" onclick="novaAnaliseAbelha()">🔄 Nova análise</button>
        </section>
    </div>

    <!-- ========================================== -->
    <!-- ABA 2: CÁLCULO DE ROI / SAÍDA              -->
    <!-- ========================================== -->
    <div id="tab-roi" class="hidden">
        <section class="step" id="roi-form">
            <div class="step-header">
                <div class="step-number">$</div>
                <div class="step-title">Simulador de Saída</div>
            </div>

            <div class="info-box">
                <strong>Passo 1: Investimento Inicial</strong>
                Para calcularmos se você já recuperou o valor investido, informe quanto você colocou no jogo desde o início.
            </div>

            <div class="form-group">
                <label class="form-label">Quanto foi seu investimento inicial? <span class="req">*</span></label>
                <div class="form-input-wrap">
                    <input type="number" class="form-input" id="roi-investimento" placeholder="0.00" min="0" step="0.01">
                    <span class="unit">USD ($)</span>
                </div>
            </div>

            <div class="alert-box">
                <strong>⚠️ Atenção antes de continuar</strong>
                Diga para o jogador fazer o próximo passo se deseja realmente sair. <b>Antes de continuar, venda todos seus equipamentos, acessórios e casas…</b> Converta tudo para NEC antes de preencher os próximos campos.
            </div>

            <div class="form-group">
                <label class="form-label">Quantos tokens NEC você tem neste momento? <span class="req">*</span></label>
                <div class="form-input-wrap">
                    <input type="number" class="form-input" id="roi-nec" placeholder="0" min="0" step="1">
                    <span class="unit">NEC</span>
                </div>
            </div>

            <div class="form-group">
                <label class="form-label">Qual é o seu Farm Diário atual? (Opcional)</label>
                <div class="form-input-wrap">
                    <input type="number" class="form-input" id="roi-farm-diario" placeholder="0" min="0" step="1">
                    <span class="unit">NEC/dia</span>
                </div>
                <div class="form-hint">Soma de tudo o que você coleta por dia no jogo.</div>
            </div>

            <div class="form-group">
                <label class="form-label">Quanto você gasta em renovações por dia? (Opcional)</label>
                <div class="form-input-wrap">
                    <input type="number" class="form-input" id="roi-custo-renovacao" placeholder="0" min="0" step="1">
                    <span class="unit">NEC/dia</span>
                </div>
                <div class="form-hint">Custo diário para manter todas as suas abelhas vivas.</div>
            </div>

            <div style="margin: 30px 0 20px; border-top: 1px solid var(--border);"></div>
            <div class="step-title" style="margin-bottom: 20px;">Inventário de Abelhas</div>
            
            <div class="form-row-3">
                <div class="form-group">
                    <label class="form-label">Qtd Comum</label>
                    <div class="form-input-wrap">
                        <input type="number" class="form-input" id="roi-qtd-comum" placeholder="0" min="0" step="1">
                    </div>
                </div>
                <div class="form-group">
                    <label class="form-label">Preço Venda Comum</label>
                    <div class="form-input-wrap">
                        <input type="number" class="form-input" id="roi-prc-comum" placeholder="0" min="0" step="1">
                        <span class="unit">NEC</span>
                    </div>
                </div>
            </div>

            <div class="form-row-3">
                <div class="form-group">
                    <label class="form-label">Qtd Incomum</label>
                    <div class="form-input-wrap">
                        <input type="number" class="form-input" id="roi-qtd-incomum" placeholder="0" min="0" step="1">
                    </div>
                </div>
                <div class="form-group">
                    <label class="form-label">Preço Venda Incomum</label>
                    <div class="form-input-wrap">
                        <input type="number" class="form-input" id="roi-prc-incomum" placeholder="0" min="0" step="1">
                        <span class="unit">NEC</span>
                    </div>
                </div>
            </div>

            <div class="form-row-3">
                <div class="form-group">
                    <label class="form-label">Qtd Rara</label>
                    <div class="form-input-wrap">
                        <input type="number" class="form-input" id="roi-qtd-rara" placeholder="0" min="0" step="1">
                    </div>
                </div>
                <div class="form-group">
                    <label class="form-label">Preço Venda Rara</label>
                    <div class="form-input-wrap">
                        <input type="number" class="form-input" id="roi-prc-rara" placeholder="0" min="0" step="1">
                        <span class="unit">NEC</span>
                    </div>
                </div>
            </div>

            <div class="form-row-3">
                <div class="form-group">
                    <label class="form-label">Qtd Épica</label>
                    <div class="form-input-wrap">
                        <input type="number" class="form-input" id="roi-qtd-epica" placeholder="0" min="0" step="1">
                    </div>
                </div>
                <div class="form-group">
                    <label class="form-label">Preço Venda Épica</label>
                    <div class="form-input-wrap">
                        <input type="number" class="form-input" id="roi-prc-epica" placeholder="0" min="0" step="1">
                        <span class="unit">NEC</span>
                    </div>
                </div>
            </div>

            <div class="form-row-3">
                <div class="form-group">
                    <label class="form-label">Qtd Lendária</label>
                    <div class="form-input-wrap">
                        <input type="number" class="form-input" id="roi-qtd-lendaria" placeholder="0" min="0" step="1">
                    </div>
                </div>
                <div class="form-group">
                    <label class="form-label">Preço Venda Lendária</label>
                    <div class="form-input-wrap">
                        <input type="number" class="form-input" id="roi-prc-lendaria" placeholder="0" min="0" step="1">
                        <span class="unit">NEC</span>
                    </div>
                </div>
            </div>

            <div class="form-row-3">
                <div class="form-group">
                    <label class="form-label">Qtd Mítica</label>
                    <div class="form-input-wrap">
                        <input type="number" class="form-input" id="roi-qtd-mitica" placeholder="0" min="0" step="1">
                    </div>
                </div>
                <div class="form-group">
                    <label class="form-label">Preço Venda Mítica</label>
                    <div class="form-input-wrap">
                        <input type="number" class="form-input" id="roi-prc-mitica" placeholder="0" min="0" step="1">
                        <span class="unit">NEC</span>
                    </div>
                </div>
            </div>

            <button class="btn-calc" onclick="calcularROI()">Calcular ROI de Saída</button>
        </section>

        <!-- Resultado ROI -->
        <section class="step hidden" id="roi-results">
            <div class="step-header">
                <div class="step-number">📊</div>
                <div class="step-title">Seu resultado de ROI</div>
            </div>

            <div id="roi-verdict" class="verdict profit">✅ Você já está no lucro</div>

            <div id="roi-hero" class="hero-result">
                <div class="hero-label">Lucro Líquido (Estimado)</div>
                <div class="hero-value" id="roi-hero-value">$0.00</div>
                <div class="hero-unit">USD</div>
            </div>

            <div class="result-grid">
                <div class="result-card">
                    <div class="result-label">Investimento Inicial</div>
                    <div class="result-value red" id="roi-r-invest">$0.00</div>
                    <div class="result-sub">Total aportado</div>
                </div>
                <div class="result-card">
                    <div class="result-label">Patrimônio Atual</div>
                    <div class="result-value info" id="roi-r-patrimonio">$0.00</div>
                    <div class="result-sub">NEC + Valor das Abelhas</div>
                </div>
                <div class="result-card">
                    <div class="result-label">Valor em NEC</div>
                    <div class="result-value" id="roi-r-nec">0 NEC</div>
                    <div class="result-sub">Total convertido</div>
                </div>
                <div class="result-card">
                    <div class="result-label">Valor das Abelhas</div>
                    <div class="result-value" id="roi-r-abelhas">0 NEC</div>
                    <div class="result-sub">Inventário total</div>
                </div>
            </div>

            <!-- Caixa de recuperação diária -->
            <div id="roi-recovery-box" class="info-box hidden">
                <strong>💡 Estratégia de Recuperação</strong>
                <p id="roi-recovery-text"></p>
            </div>

            <button class="btn-reset" onclick="novaAnaliseROI()">🔄 Nova simulação</button>
        </section>
    </div>
</div>

<script>
    // ===== NAVEGAÇÃO DE ABAS =====
    function switchTab(tab) {
        document.querySelectorAll('.tab-btn').forEach(btn => btn.classList.remove('active'));
        document.querySelector(`.tab-btn[onclick="switchTab('${tab}')"]`).classList.add('active');
        
        if (tab === 'abelha') {
            document.getElementById('tab-abelha').classList.remove('hidden');
            document.getElementById('tab-roi').classList.add('hidden');
        } else {
            document.getElementById('tab-abelha').classList.add('hidden');
            document.getElementById('tab-roi').classList.remove('hidden');
        }
    }

    // ==========================================
    // CÓDIGO ABA 1: ANÁLISE DE ABELHA
    // ==========================================
    const MULTIPLICADORES = {
        'Comum': 1.2,
        'Incomum': 1.9,
        'Raro': 4.0,
        'Épico': 7.0,
        'Lendário': 14.8,
        'Místico': 39.0
    };

    const state = { especie: null, raridade: null };

    function mostrar(id) {
        const el = document.getElementById(id);
        el.classList.remove('hidden');
        setTimeout(() => el.scrollIntoView({ behavior: 'smooth', block: 'start' }), 60);
    }

    function esconder(id) { document.getElementById(id).classList.add('hidden'); }

    function voltar(step) {
        if (step === 'species') {
            esconder('step-rarity'); esconder('step-form'); esconder('step-results');
            document.querySelectorAll('.rarity-card').forEach(c => c.classList.remove('selected'));
            state.raridade = null;
        } else if (step === 'rarity') {
            esconder('step-form'); esconder('step-results');
            limparFormularioAbelha();
        }
    }

    function limparFormularioAbelha() {
        ['preco', 'dias', 'horas', 'renovacoes', 'custo-renov', 'producao']
            .forEach(id => document.getElementById(id).value = '');
    }

    document.querySelectorAll('.species-card').forEach(card => {
        card.addEventListener('click', () => {
            const especie = card.dataset.species;
            if (state.especie === especie) return;
            state.especie = especie; state.raridade = null;
            document.querySelectorAll('.species-card').forEach(c => c.classList.remove('selected'));
            card.classList.add('selected');
            document.querySelectorAll('.rarity-card').forEach(c => c.classList.remove('selected'));
            limparFormularioAbelha(); esconder('step-form'); esconder('step-results');
            mostrar('step-rarity');
        });
    });

    document.querySelectorAll('.rarity-card').forEach(card => {
        card.addEventListener('click', () => {
            const raridade = card.dataset.rarity;
            if (state.raridade === raridade) return;
            state.raridade = raridade;
            document.querySelectorAll('.rarity-card').forEach(c => c.classList.remove('selected'));
            card.classList.add('selected');
            limparFormularioAbelha(); esconder('step-results');
            mostrar('step-form');
        });
    });

    function animarValor(el, target, prefixo = '', sufixo = '', duracao = 900) {
        const textoAtual = (el.dataset.valorAtual || '0').toString();
        const start = parseFloat(textoAtual) || 0;
        const inicio = performance.now();
        el.dataset.valorAtual = target;
        function tick(now) {
            const elapsed = now - inicio;
            const progresso = Math.min(elapsed / duracao, 1);
            const eased = 1 - Math.pow(1 - progresso, 3);
            const valor = start + (target - start) * eased;
            el.textContent = prefixo + valor.toFixed(2) + sufixo;
            if (progresso < 1) requestAnimationFrame(tick);
        }
        requestAnimationFrame(tick);
    }

    function calcularAbelha() {
        const preco = parseFloat(document.getElementById('preco').value) || 0;
        const dias = parseFloat(document.getElementById('dias').value) || 0;
        const horas = parseFloat(document.getElementById('horas').value) || 0;
        const renovacoes = parseFloat(document.getElementById('renovacoes').value) || 0;
        const custoRenov = parseFloat(document.getElementById('custo-renov').value) || 0;
        const producaoBase = parseFloat(document.getElementById('producao').value) || 0;

        if (producaoBase <= 0) { alert('⚠️ Informe quanto a abelha farma por dia.'); return; }

        const multiplicador = MULTIPLICADORES[state.raridade] || 1;
        const producaoDiaria = producaoBase * multiplicador;
        const vidaPreRenov = dias + horas / 24;
        const vidaTotalDias = vidaPreRenov + renovacoes;
        const producaoTotal = vidaTotalDias * producaoDiaria;
        const custoRenovTotal = renovacoes * custoRenov;
        const investimentoTotal = preco + custoRenovTotal;
        const lucroLiquido = producaoTotal - investimentoTotal;
        const lucroPorDia = vidaTotalDias > 0 ? lucroLiquido / vidaTotalDias : 0;
        const roi = preco > 0 ? (lucroLiquido / preco) * 100 : null;
        const breakEvenDias = producaoDiaria > 0 ? investimentoTotal / producaoDiaria : null;

        const hero = document.getElementById('hero');
        const heroValue = document.getElementById('hero-value');
        const verdict = document.getElementById('verdict');

        hero.classList.remove('loss');
        if (lucroLiquido >= 0) {
            heroValue.style.background = 'linear-gradient(135deg, #f7b32b 0%, #ffd155 100%)';
            heroValue.style.webkitBackgroundClip = 'text'; heroValue.style.backgroundClip = 'text';
            verdict.className = 'verdict profit'; verdict.textContent = '✅ Investimento lucrativo';
        } else {
            hero.classList.add('loss');
            heroValue.style.background = 'linear-gradient(135deg, #ff5577, #ff8866)';
            heroValue.style.webkitBackgroundClip = 'text'; heroValue.style.backgroundClip = 'text';
            verdict.className = 'verdict loss'; verdict.textContent = '❌ Investimento no prejuízo';
        }

        heroValue.dataset.valorAtual = '0';
        animarValor(heroValue, lucroLiquido);

        const fmt = (v) => v.toFixed(2) + ' NEC';
        document.getElementById('r-producao').textContent = fmt(producaoTotal);
        document.getElementById('r-producao-sub').textContent = `Base ${producaoBase.toFixed(2)} × ${multiplicador}x`;
        document.getElementById('r-investimento').textContent = fmt(investimentoTotal);
        document.getElementById('r-custo').textContent = fmt(custoRenovTotal);
        document.getElementById('r-custo-sub').textContent = renovacoes === 0 ? 'Sem renovações' : renovacoes === 1 ? '1 renovação' : `${renovacoes} renovações`;

        const lucroDiaEl = document.getElementById('r-lucro-dia');
        lucroDiaEl.textContent = fmt(lucroPorDia);
        lucroDiaEl.className = 'result-value ' + (lucroPorDia >= 0 ? 'gold' : 'red');

        const diasInt = Math.floor(vidaTotalDias);
        const horasInt = Math.round((vidaTotalDias - diasInt) * 24);
        document.getElementById('r-vida').textContent = horasInt > 0 ? `${diasInt}d ${horasInt}h` : `${diasInt} dias`;
        document.getElementById('r-vida-sub').textContent = vidaPreRenov > 0 ? `${vidaPreRenov.toFixed(1)} dias livres + ${renovacoes} renov.` : `${renovacoes} renovações`;

        const roiEl = document.getElementById('r-roi');
        const roiSub = document.getElementById('r-roi-sub');
        if (roi === null) {
            roiEl.textContent = 'N/A'; roiEl.className = 'result-value'; roiSub.textContent = 'Preço pago foi 0';
        } else {
            roiEl.textContent = (roi >= 0 ? '+' : '') + roi.toFixed(1) + '%';
            roiEl.className = 'result-value ' + (roi >= 0 ? 'green' : 'red');
            roiSub.textContent = breakEvenDias !== null && breakEvenDias <= vidaTotalDias ? `Paga-se em ~${breakEvenDias.toFixed(1)} dias` : 'Não retorna o investimento';
        }
        mostrar('step-results');
    }

    function novaAnaliseAbelha() {
        state.especie = null; state.raridade = null;
        document.querySelectorAll('.species-card, .rarity-card').forEach(c => c.classList.remove('selected'));
        limparFormularioAbelha();
        esconder('step-rarity'); esconder('step-form'); esconder('step-results');
        window.scrollTo({ top: 0, behavior: 'smooth' });
    }

    // ==========================================
    // CÓDIGO ABA 2: CÁLCULO DE ROI / SAÍDA
    // ==========================================
    const COTACAO_NEC = 10000; // 10.000 NEC = 1 USD

    function calcularROI() {
        // Captura valores
        const investimentoUSD = parseFloat(document.getElementById('roi-investimento').value) || 0;
        const necAtual = parseFloat(document.getElementById('roi-nec').value) || 0;
        const farmDiarioNEC = parseFloat(document.getElementById('roi-farm-diario').value) || 0;
        const custoRenovacaoNEC = parseFloat(document.getElementById('roi-custo-renovacao').value) || 0;
        
        const qtdComum = parseFloat(document.getElementById('roi-qtd-comum').value) || 0;
        const prcComum = parseFloat(document.getElementById('roi-prc-comum').value) || 0;
        const qtdIncomum = parseFloat(document.getElementById('roi-qtd-incomum').value) || 0;
        const prcIncomum = parseFloat(document.getElementById('roi-prc-incomum').value) || 0;
        const qtdRara = parseFloat(document.getElementById('roi-qtd-rara').value) || 0;
        const prcRara = parseFloat(document.getElementById('roi-prc-rara').value) || 0;
        const qtdEpica = parseFloat(document.getElementById('roi-qtd-epica').value) || 0;
        const prcEpica = parseFloat(document.getElementById('roi-prc-epica').value) || 0;
        const qtdLendaria = parseFloat(document.getElementById('roi-qtd-lendaria').value) || 0;
        const prcLendaria = parseFloat(document.getElementById('roi-prc-lendaria').value) || 0;
        const qtdMitica = parseFloat(document.getElementById('roi-qtd-mitica').value) || 0;
        const prcMitica = parseFloat(document.getElementById('roi-prc-mitica').value) || 0;

        // Cálculos de patrimônio
        const valorComum = qtdComum * prcComum;
        const valorIncomum = qtdIncomum * prcIncomum;
        const valorRara = qtdRara * prcRara;
        const valorEpica = qtdEpica * prcEpica;
        const valorLendaria = qtdLendaria * prcLendaria;
        const valorMitica = qtdMitica * prcMitica;
        
        const totalAbelhasNEC = valorComum + valorIncomum + valorRara + valorEpica + valorLendaria + valorMitica;
        const patrimonioTotalNEC = necAtual + totalAbelhasNEC;
        
        // Conversão para USD
        const totalAbelhasUSD = totalAbelhasNEC / COTACAO_NEC;
        const patrimonioTotalUSD = patrimonioTotalNEC / COTACAO_NEC;
        
        const lucroLiquidoUSD = patrimonioTotalUSD - investimentoUSD;

        // Configuração do resultado
        const hero = document.getElementById('roi-hero');
        const heroValue = document.getElementById('roi-hero-value');
        const verdict = document.getElementById('roi-verdict');
        const recoveryBox = document.getElementById('roi-recovery-box');
        const recoveryText = document.getElementById('roi-recovery-text');

        hero.classList.remove('loss');
        if (lucroLiquidoUSD >= 0) {
            heroValue.style.background = 'linear-gradient(135deg, #4ade80 0%, #22c55e 100%)';
            heroValue.style.webkitBackgroundClip = 'text'; heroValue.style.backgroundClip = 'text';
            verdict.className = 'verdict profit'; verdict.textContent = '✅ Você já está no lucro';
            recoveryBox.classList.add('hidden');
        } else {
            hero.classList.add('loss');
            heroValue.style.background = 'linear-gradient(135deg, #ff5577, #ff8866)';
            heroValue.style.webkitBackgroundClip = 'text'; heroValue.style.backgroundClip = 'text';
            verdict.className = 'verdict loss'; verdict.textContent = '❌ Você ainda está no prejuízo';
            recoveryBox.classList.remove('hidden');

            // Lógica dos 3 ovos diários + Farm - Renovações
            const ganhoDiarioOvosNEC = prcComum + prcIncomum + prcRara;
            const ganhoBrutoDiarioNEC = ganhoDiarioOvosNEC + farmDiarioNEC;
            const ganhoLiquidoDiarioNEC = ganhoBrutoDiarioNEC - custoRenovacaoNEC;
            
            const faltaUSD = Math.abs(lucroLiquidoUSD);

            if (ganhoLiquidoDiarioNEC > 0) {
                const ganhoLiquidoDiarioUSD = ganhoLiquidoDiarioNEC / COTACAO_NEC;
                const diasNecessarios = Math.ceil(faltaUSD / ganhoLiquidoDiarioUSD);
                
                let detalhesGanho = `<strong>$${ganhoLiquidoDiarioUSD.toFixed(2)} líquidos por dia</strong> (${Math.round(ganhoLiquidoDiarioNEC).toLocaleString('pt-BR')} NEC).`;
                
                // Monta a explicação detalhada
                let partes = [];
                if (farmDiarioNEC > 0) partes.push(`$${(farmDiarioNEC/COTACAO_NEC).toFixed(2)} de farm`);
                if (ganhoDiarioOvosNEC > 0) partes.push(`$${(ganhoDiarioOvosNEC/COTACAO_NEC).toFixed(2)} dos ovos`);
                
                let textoDetalhe = "";
                if (partes.length > 0) {
                    textoDetalhe += `<br><span style="font-size:0.8rem; color:var(--text-dim);">Sendo ${partes.join(' + ')}`;
                    if (custoRenovacaoNEC > 0) {
                        textoDetalhe += ` - $${(custoRenovacaoNEC/COTACAO_NEC).toFixed(2)} de renovações`;
                    }
                    textoDetalhe += `</span>`;
                }

                recoveryText.innerHTML = `Faltam <strong>$${faltaUSD.toFixed(2)}</strong> para você zerar o prejuízo.<br><br>
                Juntando seu farm + os 3 ovos e descontando as renovações, você tem um lucro de ${detalhesGanho}${textoDetalhe}<br><br>
                Você precisará de aproximadamente <strong>${diasNecessarios} dias</strong> jogando sem investir mais nada para ficar positivo.`;
            } else {
                // Jogador está no negativo
                recoveryText.innerHTML = `Faltam <strong>$${faltaUSD.toFixed(2)}</strong> para você zerar o prejuízo.<br><br>
                <span style="color:var(--danger); font-weight:700;">⚠️ Atenção: O custo das suas renovações diárias está maior ou igual ao que você ganha por dia!</span><br>
                Do jeito que está, você está perdendo <strong>$${Math.abs(ganhoLiquidoDiarioNEC/COTACAO_NEC).toFixed(2)} por dia</strong> e <b>nunca vai recuperar o investimento</b>. Você precisa reduzir as renovações, vender abelhas ou aumentar o seu farm para virar o jogo.`;
            }
        }

        heroValue.dataset.valorAtual = '0';
        animarValor(heroValue, lucroLiquidoUSD, '$', '');

        // Preencher cards menores
        const fmtUSD = (v) => '$' + v.toFixed(2);
        const fmtNEC = (v) => Math.round(v).toLocaleString('pt-BR') + ' NEC';

        document.getElementById('roi-r-invest').textContent = fmtUSD(investimentoUSD);
        document.getElementById('roi-r-patrimonio').textContent = fmtUSD(patrimonioTotalUSD);
        document.getElementById('roi-r-nec').textContent = fmtNEC(necAtual);
        document.getElementById('roi-r-abelhas').textContent = fmtNEC(totalAbelhasNEC);

        mostrar('roi-results');
    }

    function novaAnaliseROI() {
        ['roi-investimento', 'roi-nec', 'roi-farm-diario', 'roi-custo-renovacao', 'roi-qtd-comum', 'roi-prc-comum', 'roi-qtd-incomum', 'roi-prc-incomum', 'roi-qtd-rara', 'roi-prc-rara', 'roi-qtd-epica', 'roi-prc-epica', 'roi-qtd-lendaria', 'roi-prc-lendaria', 'roi-qtd-mitica', 'roi-prc-mitica']
            .forEach(id => document.getElementById(id).value = '');
        esconder('roi-results');
        window.scrollTo({ top: 0, behavior: 'smooth' });
    }
</script>
</body>
</html>
