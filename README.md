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

    header { text-align: center; margin-bottom: 48px; }

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

    @keyframes shine {
        to { background-position: 200% center; }
    }

    .subtitle {
        color: var(--text-dim);
        font-size: 0.95rem;
        margin-top: 8px;
    }

    /* ====== STEPS ====== */
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

    .step-badge {
        font-size: 0.75rem;
        padding: 4px 12px;
        border-radius: 100px;
        background: rgba(247,179,43,0.15);
        border: 1px solid rgba(247,179,43,0.3);
        color: var(--gold);
        font-weight: 600;
        white-space: nowrap;
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
    .btn-back:hover {
        border-color: var(--gold);
        color: var(--gold);
    }

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

    .species-name {
        font-weight: 700;
        font-size: 1rem;
        position: relative;
    }

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

    /* ====== FORM ====== */
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

    .form-input-wrap {
        position: relative;
    }

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
    .form-input::-webkit-inner-spin-button {
        -webkit-appearance: none;
        margin: 0;
    }
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
    .verdict.profit {
        background: rgba(74,222,128,0.15);
        color: var(--success);
        border: 1px solid rgba(74,222,128,0.35);
    }
    .verdict.loss {
        background: rgba(255,85,119,0.15);
        color: var(--danger);
        border: 1px solid rgba(255,85,119,0.35);
    }
    .verdict.neutral {
        background: rgba(247,179,43,0.15);
        color: var(--gold);
        border: 1px solid rgba(247,179,43,0.35);
    }

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

    .hero-result.loss::before {
        background: radial-gradient(circle, rgba(255,85,119,0.12) 0%, transparent 60%);
    }

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

    .hero-result.loss .hero-value {
        background: linear-gradient(135deg, #ff5577, #ff8866);
        -webkit-background-clip: text;
        background-clip: text;
    }

    .hero-unit {
        font-size: 1rem;
        color: var(--text-dim);
        margin-top: 8px;
        position: relative;
        font-weight: 600;
    }

    .result-grid {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 12px;
        margin-bottom: 20px;
    }

    .result-card {
        background: var(--card);
        border: 1px solid var(--border);
        border-radius: 14px;
        padding: 18px 20px;
        transition: all 0.25s;
    }

    .result-card:hover {
        border-color: rgba(247,179,43,0.4);
        transform: translateY(-2px);
    }

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

    .result-sub {
        font-size: 0.75rem;
        color: var(--text-dim);
        margin-top: 3px;
        font-weight: 500;
    }

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
    }

    .btn-reset:hover {
        border-color: var(--gold);
        color: var(--gold);
        background: rgba(247,179,43,0.05);
    }

    .hidden { display: none !important; }

    /* ====== RESPONSIVE ====== */
    @media (max-width: 640px) {
        body { padding: 24px 14px 60px; }
        h1 { font-size: 1.7rem; }
        .step { padding: 20px; border-radius: 16px; }
        .species-grid { grid-template-columns: repeat(2, 1fr); }
        .species-grid .species-card:last-child { grid-column: span 2; }
        .rarity-grid { grid-template-columns: repeat(2, 1fr); }
        .hero-value { font-size: 2.2rem; }
        .result-grid { grid-template-columns: 1fr; }
        .form-row { grid-template-columns: 1fr; }
    }
</style>
</head>
<body>
<div class="container">
    <header>
        <h1>🐝 Calculadora SolanaBee</h1>
        <p class="subtitle">Descubra o lucro real da sua abelha em segundos</p>
    </header>

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

        <button class="btn-calc" onclick="calcular()">Calcular Lucro</button>
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

        <button class="btn-reset" onclick="novaAnalise()">🔄 Nova análise</button>
    </section>
</div>

<script>
    // ===== MULTIPLICADORES POR RARIDADE =====
    const MULTIPLICADORES = {
        'Comum': 1.2,
        'Incomum': 1.9,
        'Raro': 4.0,
        'Épico': 7.0,
        'Lendário': 14.8,
        'Místico': 39.0
    };

    // ===== ESTADO =====
    const state = {
        especie: null,
        raridade: null
    };

    // ===== NAVEGAÇÃO =====
    function mostrar(id) {
        const el = document.getElementById(id);
        el.classList.remove('hidden');
        // Pequeno delay para garantir que o display já mudou
        setTimeout(() => {
            el.scrollIntoView({ behavior: 'smooth', block: 'start' });
        }, 60);
    }

    function esconder(id) {
        document.getElementById(id).classList.add('hidden');
    }

    function voltar(step) {
        if (step === 'species') {
            esconder('step-rarity');
            esconder('step-form');
            esconder('step-results');
            document.querySelectorAll('.rarity-card').forEach(c => c.classList.remove('selected'));
            state.raridade = null;
        } else if (step === 'rarity') {
            esconder('step-form');
            esconder('step-results');
            limparFormulario();
        }
    }

    function limparFormulario() {
        ['preco', 'dias', 'horas', 'renovacoes', 'custo-renov', 'producao']
            .forEach(id => document.getElementById(id).value = '');
    }

    // ===== SELEÇÃO DE ESPÉCIE =====
    document.querySelectorAll('.species-card').forEach(card => {
        card.addEventListener('click', () => {
            const especie = card.dataset.species;
            if (state.especie === especie) return;

            state.especie = especie;
            state.raridade = null;

            document.querySelectorAll('.species-card').forEach(c => c.classList.remove('selected'));
            card.classList.add('selected');

            // Resetar passos seguintes
            document.querySelectorAll('.rarity-card').forEach(c => c.classList.remove('selected'));
            limparFormulario();
            esconder('step-form');
            esconder('step-results');

            mostrar('step-rarity');
        });
    });

    // ===== SELEÇÃO DE RARIDADE =====
    document.querySelectorAll('.rarity-card').forEach(card => {
        card.addEventListener('click', () => {
            const raridade = card.dataset.rarity;
            if (state.raridade === raridade) return;

            state.raridade = raridade;

            document.querySelectorAll('.rarity-card').forEach(c => c.classList.remove('selected'));
            card.classList.add('selected');

            limparFormulario();
            esconder('step-results');
            mostrar('step-form');
        });
    });

    // ===== ANIMAÇÃO DE NÚMERO =====
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

    // ===== CÁLCULO =====
    function calcular() {
        const preco = parseFloat(document.getElementById('preco').value) || 0;
        const dias = parseFloat(document.getElementById('dias').value) || 0;
        const horas = parseFloat(document.getElementById('horas').value) || 0;
        const renovacoes = parseFloat(document.getElementById('renovacoes').value) || 0;
        const custoRenov = parseFloat(document.getElementById('custo-renov').value) || 0;
        const producaoBase = parseFloat(document.getElementById('producao').value) || 0;

        // Validação básica
        if (producaoBase <= 0) {
            alert('⚠️ Informe quanto a abelha farma por dia para calcular o lucro.');
            return;
        }

        // Pega o multiplicador da raridade escolhida
        const multiplicador = MULTIPLICADORES[state.raridade] || 1;
        
        // Produção diária já com o multiplicador aplicado
        const producaoDiaria = producaoBase * multiplicador;

        // Vida antes das renovações (em dias, incluindo fração de horas)
        const vidaPreRenov = dias + horas / 24;
        // Vida total: vida restante + 1 dia por cada renovação
        const vidaTotalDias = vidaPreRenov + renovacoes;

        // Produção total (toda a vida)
        const producaoTotal = vidaTotalDias * producaoDiaria;

        // Custos
        const custoRenovTotal = renovacoes * custoRenov;
        const investimentoTotal = preco + custoRenovTotal;

        // Resultados
        const lucroLiquido = producaoTotal - investimentoTotal;
        const lucroPorDia = vidaTotalDias > 0 ? lucroLiquido / vidaTotalDias : 0;
        const roi = preco > 0 ? (lucroLiquido / preco) * 100 : null;

        // Break-even (quando a produção acumulada cobre o investimento)
        const breakEvenDias = producaoDiaria > 0 ? investimentoTotal / producaoDiaria : null;

        // ===== PREENCHER RESULTADOS =====
        const hero = document.getElementById('hero');
        const heroValue = document.getElementById('hero-value');
        const verdict = document.getElementById('verdict');

        hero.classList.remove('loss');
        if (lucroLiquido >= 0) {
            heroValue.style.background = 'linear-gradient(135deg, #f7b32b 0%, #ffd155 100%)';
            heroValue.style.webkitBackgroundClip = 'text';
            heroValue.style.backgroundClip = 'text';
            verdict.className = 'verdict profit';
            verdict.textContent = '✅ Investimento lucrativo';
        } else {
            hero.classList.add('loss');
            heroValue.style.background = 'linear-gradient(135deg, #ff5577, #ff8866)';
            heroValue.style.webkitBackgroundClip = 'text';
            heroValue.style.backgroundClip = 'text';
            verdict.className = 'verdict loss';
            verdict.textContent = '❌ Investimento no prejuízo';
        }

        heroValue.dataset.valorAtual = '0';
        animarValor(heroValue, lucroLiquido);

        // Cards
        const fmt = (v) => v.toFixed(2) + ' NEC';

        document.getElementById('r-producao').textContent = fmt(producaoTotal);
        document.getElementById('r-producao-sub').textContent = `Base ${producaoBase.toFixed(2)} × ${multiplicador}x`;
        
        document.getElementById('r-investimento').textContent = fmt(investimentoTotal);
        document.getElementById('r-custo').textContent = fmt(custoRenovTotal);
        document.getElementById('r-custo-sub').textContent =
            renovacoes === 0 ? 'Sem renovações' :
            renovacoes === 1 ? '1 renovação' : `${renovacoes} renovações`;

        const lucroDiaEl = document.getElementById('r-lucro-dia');
        lucroDiaEl.textContent = fmt(lucroPorDia);
        lucroDiaEl.className = 'result-value ' + (lucroPorDia >= 0 ? 'gold' : 'red');

        // Vida total
        const diasInt = Math.floor(vidaTotalDias);
        const horasInt = Math.round((vidaTotalDias - diasInt) * 24);
        document.getElementById('r-vida').textContent =
            horasInt > 0 ? `${diasInt}d ${horasInt}h` : `${diasInt} dias`;
        document.getElementById('r-vida-sub').textContent =
            vidaPreRenov > 0
                ? `${vidaPreRenov.toFixed(1)} dias livres + ${renovacoes} renov.`
                : `${renovacoes} renovações`;

        // ROI
        const roiEl = document.getElementById('r-roi');
        const roiSub = document.getElementById('r-roi-sub');
        if (roi === null) {
            roiEl.textContent = 'N/A';
            roiEl.className = 'result-value';
            roiSub.textContent = 'Preço pago foi 0';
        } else {
            roiEl.textContent = (roi >= 0 ? '+' : '') + roi.toFixed(1) + '%';
            roiEl.className = 'result-value ' + (roi >= 0 ? 'green' : 'red');
            roiSub.textContent = breakEvenDias !== null && breakEvenDias <= vidaTotalDias
                ? `Paga-se em ~${breakEvenDias.toFixed(1)} dias`
                : 'Não retorna o investimento';
        }

        mostrar('step-results');
    }

    // ===== RESET =====
    function novaAnalise() {
        state.especie = null;
        state.raridade = null;
        document.querySelectorAll('.species-card, .rarity-card').forEach(c => c.classList.remove('selected'));
        limparFormulario();
        esconder('step-rarity');
        esconder('step-form');
        esconder('step-results');
        window.scrollTo({ top: 0, behavior: 'smooth' });
    }
</script>
</body>
</html>
