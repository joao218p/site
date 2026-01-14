<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Plataforma do João</title>
  <style>
    * { box-sizing: border-box; font-family: Arial, sans-serif; }
    body { background-color: #121212; color: #e0e0e0; margin: 0; padding: 20px; }
    h1 { text-align: center; margin-bottom: 30px; }
    .materia { background-color: #1e1e1e; border: 1px solid #333; border-radius: 8px; margin-bottom: 20px; padding: 10px; }
    .materia h2 { cursor: pointer; margin: 0; background-color: #292929; padding: 10px; border-radius: 5px; }
    .materia h2 button { margin-left: 10px; font-size: 13px; }
    .topico { background-color: #2c2c2c; border: 1px solid #444; border-radius: 5px; padding: 10px; margin: 10px 0; }
    .topico-header { display: flex; justify-content: space-between; align-items: center; }
    .topico-nome { font-weight: bold; flex: 1; cursor: text; }
    .botoes { display: flex; gap: 5px; }
    .botoes button { background-color: #333; color: #fff; border: none; padding: 5px 8px; cursor: pointer; border-radius: 3px; }
    .botoes button:hover { background-color: #555; }
    .dados { display: flex; gap: 10px; margin-top: 10px; }
    .acertos, .erros, .total, .percentual { font-size: 14px; }
    .acertos { color: #4caf50; }
    .erros { color: #f44336; }
    .total { color: #aaa; }
    .percentual { color: #2196f3; }
    .historico { background-color: #1a1a1a; margin-top: 10px; padding: 8px; border-radius: 4px; display: none; }
    .mostrar-historico { margin-top: 8px; cursor: pointer; font-size: 13px; color: #bbb; }
    .adicionar-topico { margin-top: 10px; display: flex; gap: 10px; }
    .adicionar-topico input { flex: 1; padding: 5px; border-radius: 3px; border: none; background: #2c2c2c; color: white; }
    .adicionar-topico button { background: #4caf50; border: none, padding: 6px 10px; color: white; border-radius: 3px; cursor: pointer; }
    .materia-total { margin-top: 10px; font-size: 13px; color: #ccc; }
    body.tema-claro { background-color: #f5f5f5; color: #222; }
    body.tema-claro .materia { background-color: #fff; border: 1px solid #ddd; }
    body.tema-claro .topico { background-color: #f3f3f3; border: 1px solid #ccc; }
    body.tema-claro .botoes button { background-color: #2196f3; }
    body.tema-claro .botoes button:hover { background-color: #1976d2; }
    body.tema-claro .acertos { color: #4caf50; }
    body.tema-claro .erros { color: #f44336; }
    body.tema-claro .percentual { color: #2196f3; }
    body.tema-claro .materia h2 { background-color: #f0f0f0; color: #222; }
    body.tema-claro .materia-total { color: #444; }
    body.tema-claro .adicionar-topico input { background: #fff; color: #222; border: 1px solid #ccc; }
    #modalAjuda { display: none; position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background: rgba(0, 0, 0, 0.7); z-index: 10; align-items: center; justify-content: center; }
    #modalAjuda .conteudo { background: #222; padding: 30px; border-radius: 10px; max-width: 400px; color: #fff; position: relative; }
    #modalAjuda button.fechar { position: absolute; top: 10px; right: 10px; background: none; border: none; color: #fff; font-size: 16px; cursor: pointer; }
    #modalAjuda h2 { margin-top: 0; }
    #modalAjuda ul { list-style: none; padding: 0; margin: 0; font-size: 15px; }
    #modalAjuda ul li { margin-bottom: 10px; }

    /* Melhora visual dos grupos de temas */
    .temas-materia-box {
      background: #23272b;
      border-radius: 8px;
      padding: 8px 10px 4px 10px;
      margin-bottom: 10px;
      box-shadow: 0 2px 8px 0 #0002;
    }
    body.tema-claro .temas-materia-box {
      background: #f7f7fa;
      box-shadow: 0 2px 8px 0 #0001;
    }

    /* Melhora visual dos itens */
    .temas-materia-box ul li {
      background: #2d3238;
      color: #e0e0e0;
    }
    body.tema-claro .temas-materia-box ul li {
      background: #fff;
      color: #222;
    }

    /* Sugestão de Revisão Inteligente */
    #analiseRevisao {
      background: #23272b !important;
      color: #e0e0e0 !important;
    }
    body.tema-claro #analiseRevisao {
      background: #e3f2fd !important;
      color: #222 !important;
    }
    #analiseRevisao table tr {
      transition: background 0.2s;
    }

    /* Linhas zebra para tabelas de revisão */
    #analiseRevisao table tr.zebra-odd {
      background: #23272b !important;
    }
    #analiseRevisao table tr.zebra-even {
      background: #181a1d !important;
    }
    body.tema-claro #analiseRevisao table tr.zebra-odd {
      background: #f7f7fa !important;
    }
    body.tema-claro #analiseRevisao table tr.zebra-even {
      background: #e3e3e9 !important;
    }

    /* Prioridades mais suaves */
    .tr-prioridade-alta {
      background: #5e2630 !important;
    }
    .tr-prioridade-media {
      background: #4a4a23 !important;
    }
    .tr-prioridade-baixa {
      background: #234a36 !important;
    }
    body.tema-claro .tr-prioridade-alta {
      background: #ffd6db !important;
    }
    body.tema-claro .tr-prioridade-media {
      background: #fff9c4 !important;
    }
    body.tema-claro .tr-prioridade-baixa {
      background: #d0f8e0 !important;
    }

    /* Classes para prioridades de revisão */
    .prioridade-extremo {
      background: #ffebee !important;
      color: #c62828 !important;
      border: 1px solid #c62828 !important;
    }
    body:not(.tema-claro) .prioridade-extremo {
      background: #3a2327 !important;
      color: #ff5252 !important;
      border: 1px solid #ff5252 !important;
    }
    .prioridade-mediano {
      background: #fffde7 !important;
      color: #bfa600 !important;
      border: 1px solid #bfa600 !important;
    }
    body:not(.tema-claro) .prioridade-mediano {
      background: #393a23 !important;
      color: #ffe082 !important;
      border: 1px solid #ffe082 !important;
    }
    .prioridade-facil {
      background: #e8f5e9 !important;
      color: #388e3c !important;
      border: 1px solid #388e3c !important;
    }
    body:not(.tema-claro) .prioridade-facil {
      background: #233a2a !important;
      color: #81c784 !important;
      border: 1px solid #81c784 !important;
    }

    @media print {
  body {
    background: #fff !important;
    color: #000 !important;
    padding: 0 !important;
  }
  #modalAjuda, #temas-simples input, #temas-simples select, #temas-simples button,
  .adicionar-topico, #btnExportar, #btnImportar, #btnResetar, #btnTema, #btnAjuda, #busca, #importarArquivo {
    display: none !important;
  }
  .materia, .temas-materia-box, #analiseRevisao {
    box-shadow: none !important;
    border: 1px solid #aaa !important;
    background: #fff !important;
    color: #000 !important;
    page-break-inside: avoid;
  }
  .topico, .materia-total, .dados, .historico, .mostrar-historico {
    background: #fff !important;
    color: #000 !important;
  }
  .prioridade-extremo, .prioridade-mediano, .prioridade-facil {
    color: #000 !important;
    border: 1px solid #888 !important;
    background: #eee !important;
  }
  table, th, td {
    border: 1px solid #888 !important;
  }
}

@media (max-width: 900px) {
  .materia, .temas-materia-box, #analiseRevisao {
    padding: 6px !important;
    font-size: 15px !important;
  }
  .topico-header, .dados {
    flex-direction: column !important;
    align-items: flex-start !important;
    gap: 4px !important;
  }
  .adicionar-topico {
    flex-direction: column !important;
    gap: 4px !important;
  }
  #temas-simples > div[style*="display:flex"] {
    flex-direction: column !important;
    gap: 10px !important;
  }
  #temas-simples > div > div {
    min-width: 0 !important;
    width: 100% !important;
  }
  #painel > div {
    margin-bottom: 16px !important;
  }
  table {
    font-size: 13px !important;
  }
}
@media (max-width: 600px) {
  body {
    padding: 4px !important;
    font-size: 14px !important;
  }
  h1 {
    font-size: 20px !important;
  }
  .materia h2 {
    font-size: 16px !important;
    padding: 7px !important;
  }
  .topico {
    padding: 6px !important;
    margin: 6px 0 !important;
  }
  .temas-materia-box {
    padding: 6px 4px 2px 4px !important;
  }
  #analiseRevisao {
    padding: 6px !important;
    font-size: 13px !important;
  }
  table, th, td {
    padding: 4px !important;
  }
  .adicionar-topico input {
    font-size: 14px !important;
  }
  .adicionar-topico button {
    padding: 4px 8px !important;
    font-size: 14px !important;
  }
  .botoes button {
    padding: 4px 6px !important;
    font-size: 14px !important;
  }
  .topico-header {
    flex-direction: column !important;
    align-items: flex-start !important;
  }
  .topico-nome {
    margin-bottom: 4px !important;
  }
  .dados {
    flex-direction: column !important;
    align-items: flex-start !important;
  }
  .historico {
    margin-top: 8px !important;
  }
  .mostrar-historico {
    margin-top: 4px !important;
  }
  .materia-total {
    margin-top: 8px !important;
  }
}

.lista-topicos {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 10px;
  align-items: start;
}
@media (max-width: 700px) {
  .lista-topicos {
    grid-template-columns: 1fr !important;
  }
}

/* Select compacto com abreviação (mostra 1ª letra) */
select.mini-select {
  width: 48px; /* menor */
  padding: 4px 8px;
  font-size: 13px;
  appearance: none;
  -webkit-appearance: none;
  -moz-appearance: none;
  color: transparent; /* esconde texto nativo (usamos overlay) */
  text-shadow: 0 0 0 rgba(0,0,0,0); /* garante compatibilidade */
  background-color: #333;
  border-radius: 6px;
  border: 1px solid #444;
  height: 30px;
}
select.mini-select:focus,
select.mini-select:active {
  /* ao focar/abrir voltamos a cor para que as opções fiquem visíveis */
  color: #fff !important;
  text-shadow: none !important;
}
body.tema-claro select.mini-select:focus,
body.tema-claro select.mini-select:active {
  color: #222 !important;
}

select.mini-select:focus { outline: 1px solid #2196f3; }

.select-abbr {
  position: absolute;
  left: 10px;
  top: 50%;
  transform: translateY(-50%);
  pointer-events: none; /* não bloqueia interação com o select */
  font-weight: bold;
  font-size: 13px;
  color: #fff;
  font-family: inherit;
}

/* Abas internas das matérias */
.abas {
  display:flex;
  gap:8px;
  align-items:center;
  margin:8px 0 12px 0;
  flex-wrap:wrap;
}
.aba-btn {
  background:#333;
  color:#fff;
  padding:6px 10px;
  border-radius:6px;
  cursor:pointer;
  font-size:13px;
  border:1px solid #444;
}
.aba-btn.active {
  background:#2196f3;
  color:#fff;
  border-color:#1976d2;
}
.aba-input {
  display:flex;
  gap:6px;
  align-items:center;
}
.aba-input input {
  padding:6px;
  border-radius:6px;
  border:1px solid #444;
  background:#2c2c2c;
  color:inherit;
  font-size:13px;
  width:120px;
}
  </style>
</head>
<body>
  <div style="text-align:left;font-size:20px;color:#888;margin-bottom:8px;font-family:monospace;font-weight:bold;letter-spacing:1px;">
    @joao_vt31
  </div>
  <h1>Plataforma do João</h1>
  <div style="display:flex;gap:10px;flex-wrap:wrap;justify-content:center;margin-bottom:20px;">
    <input id="busca" placeholder="Buscar tópico..." style="padding:6px;border-radius:4px;border:none;min-width:200px;">
    <button id="btnExportar">Exportar Backup</button>
    <input type="file" id="importarArquivo" style="display:none;">
    <button id="btnImportar">Importar Backup</button>
    <button id="btnResetar">Resetar Progresso</button>
    <button id="btnTema">Alternar Tema</button>
    <button id="btnAjuda">Ajuda</button>
  </div>
  <!-- MOVA O DIV DE REVISÃO PARA CIMA DO PAINEL DE TÓPICOS -->
  <div id="analiseRevisao"></div>
  <div id="painel"></div>
  <div id="modalAjuda">
    <div class="conteudo">
      <button class="fechar" onclick="document.getElementById('modalAjuda').style.display='none'">X</button>
      <h2>Como usar</h2>
      <ul>
        <li>Adicione tópicos em cada matéria.</li>
        <li>Clique nos botões para registrar acertos/erros.</li>
        <li>Adicione anotações e datas de estudo.</li>
        <li>Use busca, exporte/import backup, resete progresso e alterne o tema.</li>
      </ul>
      <div style="margin-top:18px;font-size:16px;color:#2196f3;font-weight:bold;text-align:center;">
        Qualquer dúvida ou sugestão de melhoria, só chamar: <span style="color:#4caf50;">@joao_vt31</span>
      </div>
    </div>
      <!-- Modal de Ativação (chave) -->
      <div id="modalAtivacao" style="display:none;position:fixed;top:0;left:0;width:100vw;height:100vh;background:rgba(0,0,0,0.85);z-index:99999;align-items:center;justify-content:center;">
        <div style="background:#222;padding:24px;border-radius:8px;color:#fff;max-width:420px;width:90%;text-align:center;">
          <h2 style="margin-top:0">Acesso à Plataforma</h2>
          <div style="margin-bottom:12px;color:#ddd;">Esta plataforma requer ativação. Insira a chave para continuar.</div>
          <input id="chave-ativacao-input" placeholder="Chave de ativação" style="width:100%;padding:8px;border-radius:6px;border:1px solid #444;background:#1a1a1a;color:#fff;margin-bottom:10px;">
          <div style="display:flex;gap:8px;justify-content:center;">
            <button id="btnAtivar" style="background:#4caf50;border:none;padding:8px 12px;border-radius:6px;color:#fff;cursor:pointer;font-weight:bold;">Ativar</button>
            <button id="btnSair" style="background:#f44336;border:none;padding:8px 12px;border-radius:6px;color:#fff;cursor:pointer;">Sair</button>
          </div>
          <div style="margin-top:10px;font-size:13px;color:#bbb;">Caso você seja o dono, publique/edite <code>/activation.json</code> no repositório para ativar/desativar remotamente.</div>
        </div>
      </div>
  </div>
  <div id="temas-simples" style="margin:40px auto 0;max-width:800px;">
    <h2 style="text-align:center;">Temas Vistos e Não Vistos (por matéria)</h2>
    <div style="display:flex;gap:10px;align-items:flex-end;justify-content:center;margin-bottom:10px;">
      <select id="materia-tema" style="padding:6px;border-radius:4px;border:1px solid #ccc;">
        <option value="">Escolha a matéria</option>
        <option value="Matemática">Matemática</option>
        <option value="Português">Português</option>
        <option value="Física">Física</option>
        <option value="Inglês">Inglês</option>
        <option value="Redação">Redação</option>
      </select>
      <input id="novo-tema" placeholder="Adicionar novo tema..." style="padding:6px;border-radius:4px;border:1px solid #ccc;min-width:180px;">
      <button id="btnAddTema">Adicionar</button>
    </div>
    <div style="display:flex;gap:20px;justify-content:center;">
      <div style="flex:1;min-width:180px;">
        <div style="text-align:center;font-weight:bold;color:#2196f3;margin-bottom:4px;">Vistos</div>
        <div id="temas-vistos-materias"></div>
      </div>
      <div style="flex:1;min-width:180px;">
        <div style="text-align:center;font-weight:bold;color:#f44336;margin-bottom:4px;">Não Vistos</div>
        <div id="temas-nao-vistos-materias"></div>
      </div>
    </div>
  </div>
  <div style="position:fixed;right:18px;bottom:14px;font-size:20px;color:#888;font-family:monospace;z-index:9999;pointer-events:none;font-weight:bold;letter-spacing:1px;">
    @joao_vt31
  </div>
  <script>
  const STORAGE_KEY = 'painelJoaoMateriasV1';

  // --- Sistema de Ativação ---
  // O arquivo /activation.json (no GitHub Pages) pode conter: { "enabled": true, "key": "suaChave" }
  // Se não existir, usa a chave padrão abaixo.
  const DEFAULT_ALLOWED_KEY = 'joao2024';
  let allowedKey = DEFAULT_ALLOWED_KEY;
  function showAtivacaoModal() {
    document.getElementById('modalAtivacao').style.display = 'flex';
  }
  function hideAtivacaoModal() {
    document.getElementById('modalAtivacao').style.display = 'none';
  }

  function showDisabledOverlay() {
    const overlay = document.createElement('div');
    overlay.style.position = 'fixed';
    overlay.style.left = '0';
    overlay.style.top = '0';
    overlay.style.width = '100vw';
    overlay.style.height = '100vh';
    overlay.style.background = 'rgba(0,0,0,0.9)';
    overlay.style.color = '#fff';
    overlay.style.display = 'flex';
    overlay.style.alignItems = 'center';
    overlay.style.justifyContent = 'center';
    overlay.style.zIndex = '999999';
    overlay.innerHTML = `<div style="max-width:600px;padding:20px;text-align:center;"><h2>Plataforma Desativada</h2><p>O dono da página desativou temporariamente o acesso. Tente novamente mais tarde.</p></div>`;
    document.body.appendChild(overlay);
  }

  async function checkActivationAndStart() {
    // Tenta buscar /activation.json no mesmo domínio (GitHub Pages)
    try {
      const resp = await fetch('/activation.json', { cache: 'no-store' });
      if (resp.ok) {
        const cfg = await resp.json();
        if (cfg && cfg.enabled === false) {
          showDisabledOverlay();
          return;
        }
        if (cfg && cfg.key) allowedKey = String(cfg.key);
      }
    } catch (e) {
      // não crítico: usa chave default
      allowedKey = DEFAULT_ALLOWED_KEY;
    }

    const local = localStorage.getItem('plataformaAtivada');
    if (local === allowedKey) {
      initApp();
      return;
    }

    // mostra modal de ativação
    showAtivacaoModal();
    document.getElementById('btnAtivar').onclick = () => {
      const v = document.getElementById('chave-ativacao-input').value.trim();
      if (v === allowedKey) {
        localStorage.setItem('plataformaAtivada', allowedKey);
        hideAtivacaoModal();
        initApp();
      } else {
        alert('Chave incorreta.');
      }
    };
    document.getElementById('btnSair').onclick = () => {
      // redireciona para página externa ou limpa a tela
      document.body.innerHTML = '<div style="padding:40px;text-align:center;color:#fff;"><h2>Acesso não ativado</h2><p>Você saiu da plataforma.</p></div>';
    };
  }

  function initApp() {
    // inicia a aplicação após ativação
    renderCalendarioEstudo();
    aplicarTema();
    renderizar();
    renderTemasSimples();
  }

  function aplicarTema() {
    if (localStorage.getItem('tema') === 'claro') {
      document.body.classList.add('tema-claro');
    } else {
      document.body.classList.remove('tema-claro');
    }
  }

  function carregarMaterias() {
    const salvo = localStorage.getItem(STORAGE_KEY);
    if (salvo) {
      try {
        let mats = JSON.parse(salvo);
        mats.forEach(m => { 
          if (m.vista === undefined) m.vista = false;
          if (!m.abas || !Array.isArray(m.abas) || m.abas.length === 0) m.abas = ['Geral'];
          m.topicos = (m.topicos || []).map(t => {
            if (t.aba === undefined || t.aba === null) t.aba = 'Geral';
            if (t.prioridadeManual === undefined) t.prioridadeManual = 'Média';
            return t;
          });
        });
        return mats;
      } catch {
        return [
          { nome: "Matemática", topicos: [], vista: false, abas: ['Geral'] },
          { nome: "Português", topicos: [], vista: false, abas: ['Geral'] },
          { nome: "Física", topicos: [], vista: false, abas: ['Geral'] },
          { nome: "Inglês", topicos: [], vista: false, abas: ['Geral'] },
          { nome: "Redação", topicos: [], vista: false, abas: ['Geral'] }
        ];
      }
    }
    return [
      { nome: "Matemática", topicos: [], vista: false, abas: ['Geral'] },
      { nome: "Português", topicos: [], vista: false, abas: ['Geral'] },
      { nome: "Física", topicos: [], vista: false, abas: ['Geral'] },
      { nome: "Inglês", topicos: [], vista: false, abas: ['Geral'] },
      { nome: "Redação", topicos: [], vista: false, abas: ['Geral'] }
    ];
  }

  function salvarMaterias() {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(materias));
  }

  let materias = carregarMaterias();
  const painel = document.getElementById('painel');
  let filtroBusca = '';

  let materiasAbertas = {}; // controla aberto/fechado das matérias
  let materiasAbaSelecionada = {}; // aba ativa por matéria

  // 1. TÓPICOS EM 3 COLUNAS (já está OK, só garantir responsividade)
  function criarMateria(materia) {
    const div = document.createElement('div');
    div.className = 'materia';

    const titulo = document.createElement('h2');
    titulo.textContent = materia.nome;
    titulo.onclick = () => {
      // Inverte o estado salvo ao clicar
      materiasAbertas[materia.nome] = !(materiasAbertas[materia.nome] !== false);
      renderizar();
    };
    div.appendChild(titulo);

    // Use o estado salvo para manter aberto/fechado
    const aberto = materiasAbertas[materia.nome] !== false;
    div.setAttribute('data-fechado', !aberto);

    // Abas da matéria
    const abasContainer = document.createElement('div');
    abasContainer.className = 'abas';
    abasContainer.style.display = aberto ? 'flex' : 'none';

    const selectedAba = materiasAbaSelecionada[materia.nome] || (materia.abas && materia.abas[0]) || 'Geral';
    materiasAbaSelecionada[materia.nome] = selectedAba;

    function renderAbas() {
      abasContainer.innerHTML = '';
      (materia.abas || ['Geral']).forEach(aba => {
        const btnWrapper = document.createElement('div');
        btnWrapper.style.display = 'flex';
        btnWrapper.style.alignItems = 'center';
        btnWrapper.style.gap = '4px';
        
        const btn = document.createElement('div');
        btn.className = 'aba-btn' + (aba === materiasAbaSelecionada[materia.nome] ? ' active' : '');
        btn.textContent = aba;
        btn.onclick = () => {
          materiasAbaSelecionada[materia.nome] = aba;
          renderizar();
        };
        btnWrapper.appendChild(btn);
        
        // Botão de exclusão apenas para abas que não são 'Geral'
        if (aba !== 'Geral') {
          const btnDel = document.createElement('button');
          btnDel.textContent = '✕';
          btnDel.style.background = 'none';
          btnDel.style.border = 'none';
          btnDel.style.color = '#ff6b6b';
          btnDel.style.cursor = 'pointer';
          btnDel.style.fontSize = '16px';
          btnDel.style.padding = '0 4px';
          btnDel.style.fontWeight = 'bold';
          btnDel.onclick = (e) => {
            e.stopPropagation();
            if (confirm(`Deseja excluir a aba "${aba}"? Todos os tópicos serão movidos para Geral.`)) {
              materia.topicos.forEach(t => {
                if (t.aba === aba) t.aba = 'Geral';
              });
              materia.abas = materia.abas.filter(a => a !== aba);
              materiasAbaSelecionada[materia.nome] = 'Geral';
              salvarMaterias();
              renderizar();
            }
          };
          btnWrapper.appendChild(btnDel);
        }
        
        abasContainer.appendChild(btnWrapper);
      });
      // input para nova aba
      const inputWrap = document.createElement('div');
      inputWrap.className = 'aba-input';
      const inp = document.createElement('input');
      inp.placeholder = 'Nova aba';
      const add = document.createElement('button');
      add.textContent = '+';
      add.style.padding = '6px 8px';
      add.onclick = () => {
        const nome = inp.value.trim();
        if (!nome) return;
        materia.abas = materia.abas || [];
        if (!materia.abas.includes(nome)) materia.abas.push(nome);
        materiasAbaSelecionada[materia.nome] = nome;
        salvarMaterias();
        renderizar();
      };
      inputWrap.appendChild(inp);
      inputWrap.appendChild(add);
      abasContainer.appendChild(inputWrap);
    }
    renderAbas();
    div.appendChild(abasContainer);

    // Tópicos em 2 colunas usando grid
    const listaTopicos = document.createElement('div');
    listaTopicos.className = 'lista-topicos';
    listaTopicos.style.display = aberto ? 'grid' : 'none';
    const filtro = filtroBusca ? filtroBusca.toLowerCase() : '';
    const visiveis = materia.topicos
      .filter((t) => {
        const abaAtiva = materiasAbaSelecionada[materia.nome] || (materia.abas && materia.abas[0]) || 'Geral';
        const matchesAba = (t.aba || 'Geral') === abaAtiva;
        const matchesBusca = !filtro || (t.nome && t.nome.toLowerCase().includes(filtro));
        return matchesAba && matchesBusca;
      });
    visiveis.forEach((t, i) => {
      listaTopicos.appendChild(criarTopico(t, materia, materia.topicos.indexOf(t)));
    });
    div.appendChild(listaTopicos);

    const inputTopico = document.createElement('input');
    inputTopico.placeholder = 'Novo tópico';

    const botaoAdicionar = document.createElement('button');
    botaoAdicionar.textContent = 'Adicionar';
    botaoAdicionar.onclick = () => {
      const nome = inputTopico.value.trim();
      if (!nome) return;
      const novaAba = materiasAbaSelecionada[materia.nome] || (materia.abas && materia.abas[0]) || 'Geral';
      const novoTopico = {
        nome: nome,
        acertos: 0,
        erros: 0,
        historico: [],
        anotacao: '',
        datas: [],
        prioridadeManual: 'Média',
        diasRevisao: null,
        aba: novaAba
      };
      materia.topicos.push(novoTopico);
      salvarMaterias();
      // NÃO ALTERE O ESTADO DE ABERTO/FECHADO AQUI!
      renderizar();
      inputTopico.value = '';
    };

    const boxAdicionar = document.createElement('div');
    boxAdicionar.className = 'adicionar-topico';
    boxAdicionar.appendChild(inputTopico);
    boxAdicionar.appendChild(botaoAdicionar);
    boxAdicionar.style.display = aberto ? 'flex' : 'none';
    div.appendChild(boxAdicionar);

    const totalMateria = document.createElement('div');
    totalMateria.className = 'materia-total';
    atualizarTotalMateria(materia, totalMateria);
    totalMateria.style.display = aberto ? 'block' : 'none';
    div.appendChild(totalMateria);

    painel.appendChild(div);
  }

  function atualizarVisibilidadeMateria(div, nomeMateria) {
    const aberto = materiasAbertas[nomeMateria] !== false;
    div.querySelectorAll('.lista-topicos, .adicionar-topico, .materia-total').forEach(el => {
      el.style.display = aberto ? 'block' : 'none';
    });
    div.setAttribute('data-fechado', !aberto);
  }

  // 2. CONFIRMAÇÃO PARA EXCLUIR TÓPICO
  function criarTopico(topico, materia, index) {
    const div = document.createElement('div');
    div.className = 'topico';

    const header = document.createElement('div');
    header.className = 'topico-header';

    const nome = document.createElement('div');
    nome.className = 'topico-nome';
    nome.textContent = topico.nome;
    nome.contentEditable = true;
    nome.onblur = () => {
      topico.nome = nome.textContent.trim();
      salvarMaterias();
    };

    const botoes = document.createElement('div');
    botoes.className = 'botoes';

    const addAcerto = criarBotao('+A', () => modificar(topico, 1, 0, div, materia));
    const addErro = criarBotao('+E', () => modificar(topico, 0, 1, div, materia));
    const rmvAcerto = criarBotao('-A', () => modificar(topico, -1, 0, div, materia));
    const rmvErro = criarBotao('-E', () => modificar(topico, 0, -1, div, materia));
    const btnCima = criarBotao('↑', () => moverTopico(materia, index, -1));
    const btnBaixo = criarBotao('↓', () => moverTopico(materia, index, 1));
    const apagar = criarBotao('🗑', () => {
      if (confirm('Tem certeza que deseja excluir este tópico?')) {
        materia.topicos.splice(index, 1);
        salvarMaterias();
        renderizar();
      }
    });
    const btnData = criarBotao('📅', () => {
      const hoje = new Date().toLocaleDateString('pt-BR');
      if (!topico.datas) topico.datas = [];
      if (!topico.datas.includes(hoje)) topico.datas.push(hoje);
      salvarMaterias();
      renderizar();
    });

    // Seletor de prioridade manual (agora com opção Personalizado)
    const selectPrioridade = document.createElement('select');
    selectPrioridade.className = 'mini-select';
    selectPrioridade.style.marginLeft = '8px';
    selectPrioridade.title = 'Prioridade de revisão';
    ['Alta', 'Média', 'Baixa', 'Personalizado'].forEach(opt => {
      const o = document.createElement('option');
      o.value = opt;
      o.textContent = opt;
      selectPrioridade.appendChild(o);
    });
    selectPrioridade.value = topico.prioridadeManual || 'Média';
    selectPrioridade.onchange = () => {
      topico.prioridadeManual = selectPrioridade.value;
      // Não removemos mais diasRevisao automaticamente: permite dias personalizados para qualquer prioridade.
      inputDias.value = topico.diasRevisao ? topico.diasRevisao : '';
      salvarMaterias();
      atualizarSelectAbbr(selectPrioridade); // atualiza abreviação
      renderizar();
    };

    // Input para dias de revisão customizados
    const inputDias = document.createElement('input');
    inputDias.type = 'number';
    inputDias.min = '1';
    inputDias.placeholder = 'Dias revisão';
    inputDias.title = 'Defina quantos dias para revisar (personalizado)';
    inputDias.style.width = '86px';
    inputDias.style.marginLeft = '6px';
    if (topico.diasRevisao) inputDias.value = topico.diasRevisao;
    inputDias.onchange = () => {
      const v = parseInt(inputDias.value, 10);
      if (isNaN(v) || v <= 0) {
        topico.diasRevisao = null;
      } else {
        topico.diasRevisao = v;
      }
      salvarMaterias();
      renderizar();
    };

    // Envolvo o select em um wrapper para posicionar a "abreviação"
    const wrapperSelect = document.createElement('div');
    wrapperSelect.style.position = 'relative';
    wrapperSelect.style.display = 'inline-block';
    wrapperSelect.appendChild(selectPrioridade);

    // select para escolher aba do tópico (muda conforme abas da matéria)
    const selectAbaTopico = document.createElement('select');
    selectAbaTopico.style.marginLeft = '6px';
    (materia.abas || ['Geral']).forEach(a => {
      const o = document.createElement('option');
      o.value = a;
      o.textContent = a;
      selectAbaTopico.appendChild(o);
    });
    selectAbaTopico.value = topico.aba || ((materia.abas && materia.abas[0]) || 'Geral');
    selectAbaTopico.onchange = () => {
      topico.aba = selectAbaTopico.value;
      salvarMaterias();
      renderizar();
    };
    // estiliza compacto quando possível
    selectAbaTopico.className = 'mini-select';
    selectAbaTopico.style.width = '86px';
    selectAbaTopico.title = 'Mover tópico para aba';

    // span que mostra a primeira letra (visível quando select fechado)
    const spanAbbr = document.createElement('span');
    spanAbbr.className = 'select-abbr';
    spanAbbr.textContent = (selectPrioridade.value || '').toString().trim()[0] || '';
    wrapperSelect.appendChild(spanAbbr);

    // função local para manter abreviação sincronizada
    function atualizarSelectAbbr(sel) {
      const s = sel.nextSibling; // nosso span
      if (s && s.className === 'select-abbr') {
        s.textContent = (sel.value || '').toString().trim()[0] || '';
        // ajustar cor conforme tema
        s.style.color = document.body.classList.contains('tema-claro') ? '#222' : '#fff';
      }
    }
    // inicializa cor/valor
    atualizarSelectAbbr(selectPrioridade);

    [addAcerto, addErro, rmvAcerto, rmvErro, btnCima, btnBaixo, apagar, btnData].forEach(b => botoes.appendChild(b));
    botoes.appendChild(wrapperSelect); // adiciona wrapper com select + abreviação
    botoes.appendChild(selectAbaTopico); // adiciona seletor de aba para cada tópico
    botoes.appendChild(inputDias);

    header.appendChild(nome);
    header.appendChild(botoes);
    div.appendChild(header);

    // Botão para mostrar/ocultar anotação
    const mostrarAnotacao = document.createElement('div');
    mostrarAnotacao.className = 'mostrar-historico';
    mostrarAnotacao.textContent = '📝 Anotação/Resumo';
    mostrarAnotacao.style.marginTop = '8px';
    mostrarAnotacao.style.display = 'block';
    mostrarAnotacao.style.cursor = 'pointer';

    const painelAnotacao = document.createElement('div');
    painelAnotacao.style.display = 'none';
    painelAnotacao.style.marginTop = '6px';

    const anotacao = document.createElement('textarea');
    anotacao.placeholder = 'Anotações ou resumo...';
    anotacao.style.width = '100%';
    anotacao.style.background = 'rgba(0,0,0,0.08)';
    anotacao.style.color = 'inherit';
    anotacao.value = topico.anotacao || '';
    anotacao.onchange = () => {
      topico.anotacao = anotacao.value;
      salvarMaterias();
    };
    painelAnotacao.appendChild(anotacao);

    mostrarAnotacao.onclick = () => {
      painelAnotacao.style.display = painelAnotacao.style.display === 'block' ? 'none' : 'block';
    };

    div.appendChild(mostrarAnotacao);
    div.appendChild(painelAnotacao);

    // Datas de estudo
    if (topico.datas && topico.datas.length) {
      const datasDiv = document.createElement('div');
      datasDiv.style.fontSize = '12px';
      datasDiv.style.marginTop = '4px';
      datasDiv.innerHTML = 'Estudado em: ' + topico.datas.join(', ');
      div.appendChild(datasDiv);
    }

    const dados = document.createElement('div');
    dados.className = 'dados';
    div.appendChild(dados);

    const historico = document.createElement('div');
    historico.className = 'historico';
    div.appendChild(historico);

    const mostrarHist = document.createElement('div');
    mostrarHist.className = 'mostrar-historico';
    mostrarHist.textContent = '📅 Histórico';
    mostrarHist.onclick = () => {
      historico.style.display = historico.style.display === 'block' ? 'none' : 'block';
    };
    div.appendChild(mostrarHist);

    atualizarTopico(topico, dados, historico);
    return div;
  }

  function criarBotao(texto, acao) {
    const btn = document.createElement('button');
    btn.textContent = texto;
    btn.onclick = acao;
    return btn;
  }

  function modificar(topico, acertoDelta, erroDelta, div, materia) {
    if (acertoDelta !== 0) topico.acertos += acertoDelta;
    if (erroDelta !== 0) topico.erros += erroDelta;
    if (topico.acertos < 0) topico.acertos = 0;
    if (topico.erros < 0) topico.erros = 0;

    const data = new Date().toLocaleDateString('pt-BR');
    const ultimo = topico.historico[topico.historico.length - 1];
    if (ultimo && ultimo.data === data) {
      ultimo.acertos += acertoDelta;
      ultimo.erros += erroDelta;
    } else {
      topico.historico.push({ data, acertos: acertoDelta, erros: erroDelta });
    }

    salvarMaterias();
    renderizar();
  }

  function atualizarTopico(topico, dados, historico) {
    dados.innerHTML = `
      <div class="acertos">Acertos: ${topico.acertos}</div>
      <div class="erros">Erros: ${topico.erros}</div>
      <div class="total">Total: ${topico.acertos + topico.erros}</div>
      <div class="percentual">
        % Acertos: ${(topico.acertos / (topico.acertos + topico.erros || 1) * 100).toFixed(1)}%
      </div>
    `;

    historico.innerHTML = topico.historico.map(entry => {
      const total = entry.acertos + entry.erros;
      const pct = total > 0 ? ((entry.acertos / total) * 100).toFixed(1) : '0.0';
      return `<div>${entry.data}: +${entry.acertos} ✅ +${entry.erros} ❌ — ${pct}%</div>`;
    }).join('');
  }

  function atualizarTotalMateria(materia, el) {
    const totalAcertos = materia.topicos.reduce((s, t) => s + t.acertos, 0);
    const totalErros = materia.topicos.reduce((s, t) => s + t.erros, 0);
    const total = totalAcertos + totalErros;
    const pct = total > 0 ? ((totalAcertos / total) * 100).toFixed(1) : '0.0';

    el.innerHTML = `
      Pontuação geral: <span class="acertos">${totalAcertos}</span> ✅ 
      <span class="erros">${totalErros}</span> ❌ 
      (<span class="percentual">${pct}%</span> de acerto)
    `;
  }

  function moverTopico(materia, index, direcao) {
    const novoIndex = index + direcao;
    if (novoIndex < 0 || novoIndex >= materia.topicos.length) return;
    const temp = materia.topicos[index];
    materia.topicos[index] = materia.topicos[novoIndex];
    materia.topicos[novoIndex] = temp;
    salvarMaterias();
    renderizar();
  }

  function renderizar() {
    painel.innerHTML = '';
    materias.forEach(m => criarMateria(m));
    analiseRevisao(); // Chama antes, pois agora está acima do painel
    aplicarTema();
  }

  // --- ALTERE O analiseRevisao PARA NÃO CRIAR O DIV, SÓ USAR O EXISTENTE ---
  function analiseRevisao() {
    let analiseDiv = document.getElementById('analiseRevisao');
    analiseDiv.style.margin = '30px 0';
    analiseDiv.style.padding = '15px';
    analiseDiv.style.background = 'rgba(33,150,243,0.08)';
    analiseDiv.style.borderRadius = '8px';
    analiseDiv.style.fontSize = '15px';

    // Inicializa estado de abas da revisão
    if (!window.abasRevisaoAbertas) window.abasRevisaoAbertas = {};
    if (!window.abaRevisaoAtivaPorMateria) window.abaRevisaoAtivaPorMateria = {};

    let html = `<h2 style="margin-top:0;text-align:center;">Sugestão de Revisão Inteligente</h2>`;

    let temTopicos = false;

    for (const materia of materias) {
      const materiaNome = materia.nome;
      
      // Agrupa tópicos dessa matéria por aba
      let topicosAgrupados = {};
      
      materia.topicos.forEach(t => {
        const abaTopico = t.aba || 'Geral';
        if (!topicosAgrupados[abaTopico]) {
          topicosAgrupados[abaTopico] = [];
        }
        
        const total = t.acertos + t.erros;
        const media = total > 0 ? (t.acertos / total) * 100 : 0;
        let prioridadePalavra = '';
        let prioridadeClass = '';
        if (media < 40) {
          prioridadePalavra = 'Extremo';
          prioridadeClass = 'prioridade-extremo';
        } else if (media < 80) {
          prioridadePalavra = 'Mediano';
          prioridadeClass = 'prioridade-mediano';
        } else {
          prioridadePalavra = 'Fácil';
          prioridadeClass = 'prioridade-facil';
        }
        const prioridadeManual = t.prioridadeManual || 'Média';
        let diasRevisao = (t.diasRevisao && Number(t.diasRevisao) > 0) ? Number(t.diasRevisao) : null;

        let ultimoEstudo = (t.datas && t.datas.length) ? t.datas[t.datas.length-1] : 'Nunca';
        let diasRestantes = '-';
        let diasDesdeUltima = '-';
        if (ultimoEstudo !== 'Nunca') {
          const [dia, mes, ano] = ultimoEstudo.split('/');
          const dataUltimo = new Date(+ano, +mes - 1, +dia);
          const hoje = new Date();
          const diff = Math.floor((hoje - dataUltimo) / (1000 * 60 * 60 * 24));
          diasDesdeUltima = diff;
          if (typeof diasRevisao === 'number') diasRestantes = Math.max(0, diasRevisao - diff);
        }

        topicosAgrupados[abaTopico].push({
          topico: t.nome,
          acertos: t.acertos,
          erros: t.erros,
          ultimoEstudo,
          prioridadePalavra,
          prioridadeClass,
          media: media.toFixed(1),
          prioridadeManual,
          diasRevisao,
          diasRestantes,
          diasDesdeUltima,
          aba: abaTopico
        });
      });

      // Ordena cada aba
      for (const aba in topicosAgrupados) {
        topicosAgrupados[aba].sort((a, b) => {
          if (parseFloat(a.media) !== parseFloat(b.media)) return parseFloat(a.media) - parseFloat(b.media);
          if (a.diasRestantes !== b.diasRestantes) return a.diasRestantes - b.diasRestantes;
          return 0;
        });
      }

      // Se não há tópicos nessa matéria, pula
      if (!Object.keys(topicosAgrupados).length) continue;
      temTopicos = true;

      const aberto = window.abasRevisaoAbertas[materiaNome] !== false;
      const abaAtiva = window.abaRevisaoAtivaPorMateria[materiaNome] || 'Geral';
      
      html += `
      <div style="margin-bottom:32px;">
        <div style="font-size:18px;font-weight:bold;margin-bottom:12px;text-align:left;cursor:pointer;" onclick="(function(){
          window.abasRevisaoAbertas['${materiaNome}'] = !(${aberto});
          window.renderizar && renderizar();
        })()">
          <span style="margin-right:8px;">${aberto ? '📂' : '📁'}</span>${materiaNome}
        </div>
        
        <!-- Abas para essa matéria -->
        <div style="display:${aberto ? 'flex' : 'none'};gap:8px;margin-bottom:12px;flex-wrap:wrap;">`;
      
      // Botões de abas (Geral + abas específicas)
      const abasMateria = ['Geral', ...Object.keys(topicosAgrupados).filter(a => a !== 'Geral').sort()];
      abasMateria.forEach(aba => {
        const ativo = abaAtiva === aba ? '#2196f3' : '#555';
        const corBorda = abaAtiva === aba ? '#1976d2' : '#444';
        const nomeEscapado = aba.replace(/'/g, "\\'");
        const materiaEscapada = materiaNome.replace(/'/g, "\\'");
        html += `<button onclick="window.abaRevisaoAtivaPorMateria['${materiaEscapada}']='${nomeEscapado}';window.renderizar && renderizar();" style="background:${ativo};color:white;border:1px solid ${corBorda};padding:6px 12px;border-radius:6px;cursor:pointer;font-weight:bold;font-size:13px;">
          ${aba}
        </button>`;
      });
      
      html += `</div>`;

      // Determina qual(is) aba(s) mostrar
      let listaParaMostrar = [];
      if (abaAtiva === 'Geral') {
        // Mostra todos os tópicos da matéria misturados e ordenados por porcentagem de acertos
        for (const aba in topicosAgrupados) {
          listaParaMostrar = listaParaMostrar.concat(topicosAgrupados[aba]);
        }
        // Ordena por porcentagem de acertos (menor primeiro = maior prioridade)
        listaParaMostrar.sort((a, b) => {
          if (parseFloat(a.media) !== parseFloat(b.media)) return parseFloat(a.media) - parseFloat(b.media);
          if (a.diasRestantes !== b.diasRestantes) return a.diasRestantes - b.diasRestantes;
          return 0;
        });
      } else {
        // Mostra apenas tópicos da aba selecionada (já vem ordenada)
        listaParaMostrar = topicosAgrupados[abaAtiva] || [];
      }

      // Tabela
      html += `<div style="overflow-x:auto;${aberto ? '' : 'display:none;'}">
        <table style="width:100%;border-collapse:collapse;margin-bottom:0;">
          <tr style="background:#2196f3;color:#fff;">
            <th style="padding:6px 4px;text-align:left;">Tópico</th>
            <th style="padding:6px 4px;text-align:center;">Acertos</th>
            <th style="padding:6px 4px;text-align:center;">Erros</th>
            <th style="padding:6px 4px;text-align:center;">% Acertos</th>
            <th style="padding:6px 4px;text-align:center;">Último Estudo</th>
            <th style="padding:6px 4px;text-align:center;">Dias desde última</th>
            <th style="padding:6px 4px;text-align:center;">Prioridade Manual</th>
            <th style="padding:6px 4px;text-align:center;">Revisar a cada</th>
            <th style="padding:6px 4px;text-align:center;">Dias p/ revisão</th>
            <th style="padding:6px 4px;text-align:center;">Aba</th>
          </tr>`;
      
      listaParaMostrar.forEach((item, idx) => {
        let zebra = idx % 2 === 0 ? 'zebra-even' : 'zebra-odd';
        let diasRestantesHtml = item.diasRestantes === 0
          ? '<span style="color:#c62828;font-weight:bold;">Revisar</span>'
          : item.diasRestantes;
        // CORES DIAS DESDE ÚLTIMA
        let cor = '#fff';
        if (item.diasDesdeUltima !== '-' && item.diasDesdeUltima >= 31) cor = '#c62828';
        else if (item.diasDesdeUltima !== '-' && item.diasDesdeUltima >= 16) cor = '#ffb300';
        let diasUltimaHtml = (item.diasDesdeUltima !== '-')
          ? `<span style="color:${cor};font-weight:bold;">${item.diasDesdeUltima}</span>`
          : item.diasDesdeUltima;
        html += `<tr class="${item.prioridadeClass} ${zebra}">
<td style="padding:5px 4px;text-align:left;">${item.topico}</td>
<td style="padding:5px 4px;text-align:center;">${item.acertos}</td>
<td style="padding:5px 4px;text-align:center;">${item.erros}</td>
<td style="padding:5px 4px;text-align:center;">${item.media}%</td>
<td style="padding:5px 4px;text-align:center;">${item.ultimoEstudo}</td>
<td style="padding:5px 4px;text-align:center;">${diasUltimaHtml}</td>
<td style="padding:5px 4px;text-align:center;font-weight:bold;">
  <span style="padding:3px 10px;border-radius:12px;display:inline-block;">
    ${item.prioridadeManual}${item.prioridadeManual === 'Personalizado' && item.diasRevisao ? ' ('+item.diasRevisao+'d)' : ''}
  </span>
</td>
<td style="padding:5px 4px;text-align:center;">${item.diasRevisao ? item.diasRevisao + ' dias' : '-'}</td>
<td style="padding:5px 4px;text-align:center;">${diasRestantesHtml}</td>
<td style="padding:5px 4px;text-align:center;font-weight:bold;">
  <span style="padding:4px 8px;border-radius:10px;display:inline-block;background:rgba(255,255,255,0.06);color:inherit;">
    ${item.aba}
  </span>
</td>
</tr>`;
      });
      
      html += `</table></div></div>`;
    }

    if (!temTopicos) {
      html += `<div style="text-align:center;">Nenhum tópico cadastrado ainda.</div>`;
    } else {
      html += `<div style="font-size:13px;margin-top:8px;text-align:center;">
      <span class="prioridade-extremo" style="padding:2px 8px;border-radius:8px;font-weight:bold;">Extremo</span> = prioridade máxima &nbsp; 
      <span class="prioridade-mediano" style="padding:2px 8px;border-radius:8px;font-weight:bold;">Mediano</span> = prioridade média &nbsp; 
      <span class="prioridade-facil" style="padding:2px 8px;border-radius:8px;font-weight:bold;">Fácil</span> = prioridade baixa
    </div>`;
    }

    analiseDiv.innerHTML = html;
    window.renderizar = renderizar;
  }

  // 4. CALENDÁRIO DE HORAS E QUESTÕES ESTUDADAS
  // Adiciona o calendário após o painel de temas
  const calendarioDiv = document.createElement('div');
  calendarioDiv.id = 'calendario-estudos';
  calendarioDiv.style.margin = '40px auto 0';
  calendarioDiv.style.maxWidth = '900px';
  document.body.insertBefore(calendarioDiv, document.getElementById('temas-simples'));

  let horasEstudo = JSON.parse(localStorage.getItem('horasEstudo2025')) || {};
  let questoesEstudo = JSON.parse(localStorage.getItem('questoesEstudo2025')) || {};

  function renderCalendarioEstudo() {
  const meses = [
    'Agosto', 'Setembro', 'Outubro', 'Novembro', 'Dezembro',
    'Janeiro', 'Fevereiro', 'Março', 'Abril', 'Maio', 'Junho', 'Julho', 'Agosto'
  ];
  let html = `<h2 style="text-align:center;font-size:2rem;margin-bottom:20px;">Calendário de Estudos (Agosto/2025 a Agosto/2026)</h2>`;
  
  // Botões para alternar visualização
  html += `<div style="text-align:center;margin-bottom:20px;display:flex;gap:10px;justify-content:center;">`;
  html += `<button id="btn-horas" onclick="window.modoCalendario='horas';renderCalendarioEstudo();" style="background:#2196f3;color:white;border:none;padding:8px 16px;border-radius:5px;cursor:pointer;font-weight:bold;">📊 Horas</button>`;
  html += `<button id="btn-questoes" onclick="window.modoCalendario='questoes';renderCalendarioEstudo();" style="background:#666;color:white;border:none;padding:8px 16px;border-radius:5px;cursor:pointer;font-weight:bold;">📝 Questões</button>`;
  html += `</div>`;
  
  if (!window.modoCalendario) window.modoCalendario = 'horas';
  const modoAtual = window.modoCalendario;
  const dadosAtual = modoAtual === 'horas' ? horasEstudo : questoesEstudo;
  
  let ano = 2025;
  html += `<div style="display:grid;grid-template-columns:repeat(3,1fr);gap:18px;">`;
  for (let m = 0; m < 13; m++) {
    let mes = (m + 7) % 12;
    if (mes === 0 && m > 0) ano++;
    let diasNoMes = new Date(ano, mes + 1, 0).getDate();

    // --- Estatísticas do mês ---
    let diasEstudados = 0, diasNaoEstudados = 0, totalDados = 0, recordeDados = 0, recordeDia = '';
    const unidade = modoAtual === 'horas' ? 'h' : 'q';
    const nomeUnidade = modoAtual === 'horas' ? 'horas' : 'questões';
    
    for (let d = 1; d <= diasNoMes; d++) {
      let dataStr = `${d.toString().padStart(2,'0')}/${(mes+1).toString().padStart(2,'0')}/${ano}`;
      let valor = dadosAtual[dataStr] || 0;
      if (valor > 0) {
        diasEstudados++;
        totalDados += valor;
        if (valor > recordeDados) {
          recordeDados = valor;
          recordeDia = dataStr;
        }
      } else {
        diasNaoEstudados++;
      }
    }
    let mediaDadosDia = diasNoMes > 0 ? (totalDados / diasNoMes).toFixed(2) : '0.00';

    html += `<div style="margin-bottom:14px;background:#181a1d;padding:16px 10px 12px 10px;border-radius:12px;box-shadow:0 2px 10px #0002;max-width:370px;margin-left:auto;margin-right:auto;">
      <div style="font-weight:bold;font-size:1.2rem;margin-bottom:7px;text-align:center;letter-spacing:1px;">
        ${meses[m]} <span style="color:#2196f3;font-size:1rem;">(${(mes+1).toString().padStart(2,'0')})</span> / ${ano}
      </div>
      <div style="display:grid;grid-template-columns:repeat(7,32px);gap:2px;margin-bottom:10px;">`;
    ['D','S','T','Q','Q','S','S'].forEach(dia => {
      html += `<div style="font-size:13px;text-align:center;color:#888;font-weight:bold;">${dia}</div>`;
    });
    let primeiroDia = new Date(ano, mes, 1).getDay();
    for (let i = 0; i < primeiroDia; i++) html += `<div></div>`;
    for (let d = 1; d <= diasNoMes; d++) {
      let dataStr = `${d.toString().padStart(2,'0')}/${(mes+1).toString().padStart(2,'0')}/${ano}`;
      let valor = dadosAtual[dataStr] || 0;
      let cor = valor > 0 ? '#388e3c' : '#c62828';
      const funcaoAdicionar = modoAtual === 'horas' ? 'adicionarHorasEstudo' : 'adicionarQuestoesEstudo';
      html += `<div title="${dataStr}" style="background:${cor};color:#fff;text-align:center;cursor:pointer;border-radius:7px;font-size:15px;position:relative;height:32px;display:flex;flex-direction:column;align-items:center;justify-content:center;font-weight:bold;" onclick="${funcaoAdicionar}('${dataStr}')">
        ${d}
        <span style="display:block;font-size:10px;font-weight:normal;">${valor > 0 ? valor + unidade : ''}</span>
      </div>`;
    }
    html += `</div>
      <div style="display:flex;flex-wrap:wrap;gap:7px;justify-content:center;margin-top:6px;">`;

    // Retângulos de destaque para as estatísticas
    html += `
      <div style="background:#23272b;border-radius:7px;padding:7px 10px;min-width:80px;display:flex;flex-direction:column;align-items:center;box-shadow:0 1px 4px #0001;">
        <div style="font-size:12px;color:#bbb;">Estudados</div>
        <div style="font-size:1.1rem;color:#4caf50;font-weight:bold;">${diasEstudados}</div>
      </div>
      <div style="background:#23272b;border-radius:7px;padding:7px 10px;min-width:80px;display:flex;flex-direction:column;align-items:center;box-shadow:0 1px 4px #0001;">
        <div style="font-size:12px;color:#bbb;">Não estudados</div>
        <div style="font-size:1.1rem;color:#f44336;font-weight:bold;">${diasNaoEstudados}</div>
      </div>
      <div style="background:#23272b;border-radius:7px;padding:7px 10px;min-width:80px;display:flex;flex-direction:column;align-items:center;box-shadow:0 1px 4px #0001;">
        <div style="font-size:12px;color:#bbb;">Média/dia</div>
        <div style="font-size:1.1rem;color:#2196f3;font-weight:bold;">${mediaDadosDia}${unidade}</div>
      </div>
      <div style="background:#23272b;border-radius:7px;padding:7px 10px;min-width:80px;display:flex;flex-direction:column;align-items:center;box-shadow:0 1px 4px #0001;">
        <div style="font-size:12px;color:#bbb;">Total mês</div>
        <div style="font-size:1.1rem;color:#ffd600;font-weight:bold;">${totalDados.toFixed(modoAtual === 'horas' ? 2 : 0)}${unidade}</div>
      </div>
      <div style="background:#23272b;border-radius:7px;padding:7px 10px;min-width:80px;display:flex;flex-direction:column;align-items:center;box-shadow:0 1px 4px #0001;">
        <div style="font-size:12px;color:#bbb;">Recorde</div>
        <div style="font-size:1.1rem;color:#ff9800;font-weight:bold;">${recordeDados > 0 ? recordeDados+unidade : '-'}</div>
        ${recordeDia ? `<div style="font-size:10px;color:#888;margin-top:1px;">${recordeDia}</div>` : ''}
      </div>
    `;

    html += `</div></div>`;
  }
  html += `</div>`;
  calendarioDiv.innerHTML = html;
}
  window.adicionarHorasEstudo = function(dataStr) {
    let atual = horasEstudo[dataStr] || 0;
    let val = prompt(`Horas estudadas em ${dataStr}:`, atual || '');
    if (val === null) return;
    val = parseFloat(val.replace(',','.'));
    if (isNaN(val) || val < 0) val = 0;
    horasEstudo[dataStr] = val;
    localStorage.setItem('horasEstudo2025', JSON.stringify(horasEstudo));
    renderCalendarioEstudo();
  };
  
  window.adicionarQuestoesEstudo = function(dataStr) {
    let atual = questoesEstudo[dataStr] || 0;
    let val = prompt(`Questões respondidas em ${dataStr}:`, atual || '');
    if (val === null) return;
    val = parseInt(val, 10);
    if (isNaN(val) || val < 0) val = 0;
    questoesEstudo[dataStr] = val;
    localStorage.setItem('questoesEstudo2025', JSON.stringify(questoesEstudo));
    renderCalendarioEstudo();
  };
  

  // --- Gerenciador simples de temas vistos/não vistos por matéria ---
const STORAGE_TEMAS = 'painelJoaoTemasSimplesV2';
function carregarTemasSimples() {
  const salvo = localStorage.getItem(STORAGE_TEMAS);
  if (salvo) {
    try { return JSON.parse(salvo); } catch {}
  }
  return [];
}
function salvarTemasSimples() {
  localStorage.setItem(STORAGE_TEMAS, JSON.stringify(temasSimples));
}
let temasSimples = carregarTemasSimples();

const materiasFixas = ["Matemática","Português","Física","Inglês","Redação"];

function renderTemasSimples() {
  const divVistos = document.getElementById('temas-vistos-materias');
  const divNaoVistos = document.getElementById('temas-nao-vistos-materias');
  divVistos.innerHTML = '';
  divNaoVistos.innerHTML = '';
  materiasFixas.forEach(materia => {
    // Vistos
    const vistos = temasSimples.filter(t => t.materia === materia && t.visto);
    if (vistos.length) {
      const boxV = document.createElement('div');
      boxV.className = 'temas-materia-box';
      boxV.innerHTML = `<div style="font-size:13px;font-weight:bold;margin-bottom:2px;">${materia}</div>`;
      const ulV = document.createElement('ul');
      ulV.style.listStyle = 'none';
      ulV.style.padding = '0';
      ulV.style.margin = '0';
      vistos.forEach((tema) => {
        const li = document.createElement('li');
        li.textContent = tema.nome;
        li.style.cursor = 'pointer';
        li.style.padding = '5px 8px';
        li.style.margin = '3px 0';
        li.style.background = document.body.classList.contains('tema-claro') ? '#eee' : '#23272b';
        li.style.color = document.body.classList.contains('tema-claro') ? '#222' : '#e0e0e0';
        li.style.borderRadius = '4px';
        li.style.display = 'flex';
        li.style.alignItems = 'center';
        li.style.justifyContent = 'space-between';
        li.title = 'Clique para mover de lado. Clique no X para excluir.';
        li.onclick = function(e) {
          if (e.target !== li) return;
          tema.visto = false;
          salvarTemasSimples();
          renderTemasSimples();
        };
        const btnDel = document.createElement('button');
        btnDel.textContent = '✖';
        btnDel.style.marginLeft = '10px';
        btnDel.style.background = 'none';
        btnDel.style.border = 'none';
        btnDel.style.color = '#f44336';
        btnDel.style.cursor = 'pointer';
        btnDel.onclick = function(e) {
          e.stopPropagation();
          temasSimples.splice(temasSimples.indexOf(tema),1);
          salvarTemasSimples();
          renderTemasSimples();
        };
        li.appendChild(btnDel);
        ulV.appendChild(li);
      });
      boxV.appendChild(ulV);
      divVistos.appendChild(boxV);
    }
    // Não vistos
    const naoVistos = temasSimples.filter(t => t.materia === materia && !t.visto);
    if (naoVistos.length) {
      const boxNV = document.createElement('div');
      boxNV.className = 'temas-materia-box';
      boxNV.innerHTML = `<div style="font-size:13px;font-weight:bold;margin-bottom:2px;">${materia}</div>`;
      const ulNV = document.createElement('ul');
      ulNV.style.listStyle = 'none';
      ulNV.style.padding = '0';
      ulNV.style.margin = '0';
      naoVistos.forEach((tema) => {
        const li = document.createElement('li');
        li.textContent = tema.nome;
        li.style.cursor = 'pointer';
        li.style.padding = '5px 8px';
        li.style.margin = '3px 0';
        li.style.background = document.body.classList.contains('tema-claro') ? '#eee' : '#23272b';
        li.style.color = document.body.classList.contains('tema-claro') ? '#222' : '#e0e0e0';
        li.style.borderRadius = '4px';
        li.style.display = 'flex';
        li.style.alignItems = 'center';
        li.style.justifyContent = 'space-between';
        li.title = 'Clique para mover de lado. Clique no X para excluir.';
        li.onclick = function(e) {
          if (e.target !== li) return;
          tema.visto = true;
          salvarTemasSimples();
          renderTemasSimples();
        };
        const btnDel = document.createElement('button');
        btnDel.textContent = '✖';
        btnDel.style.marginLeft = '10px';
        btnDel.style.background = 'none';
        btnDel.style.border = 'none';
        btnDel.style.color = '#f44336';
        btnDel.style.cursor = 'pointer';
        btnDel.onclick = function(e) {
          e.stopPropagation();
          temasSimples.splice(temasSimples.indexOf(tema),1);
          salvarTemasSimples();
          renderTemasSimples();
        };
        li.appendChild(btnDel);
        ulNV.appendChild(li);
      });
      boxNV.appendChild(ulNV);
      divNaoVistos.appendChild(boxNV);
    }
  });
}
document.getElementById('btnAddTema').onclick = function() {
  const nome = document.getElementById('novo-tema').value.trim();
  const materia = document.getElementById('materia-tema').value;
  if (!nome || !materia) return;
  temasSimples.push({nome, materia, visto:false});
  salvarTemasSimples();
  renderTemasSimples();
  document.getElementById('novo-tema').value = '';
  document.getElementById('materia-tema').value = '';
};
document.getElementById('novo-tema').addEventListener('keydown', function(e){
  if (e.key === 'Enter') document.getElementById('btnAddTema').click();
});

  // Atalhos de teclado: Enter para adicionar tópico, ESC fecha ajuda
  document.addEventListener('keydown', function(e) {
    if (e.key === 'Escape') document.getElementById('modalAjuda').style.display = 'none';
    if (e.key === 'Enter' && document.activeElement.placeholder === 'Novo tópico') {
      document.activeElement.nextSibling.click();
    }
  });

  // Não inicializa a app automaticamente: aguarda verificação de ativação
  checkActivationAndStart();
  </script>
</body>
</html>
