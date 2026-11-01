<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Painel Financeiro</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg-color: #ffffff;
            --surface-color: #ffffff;
            --primary: #059669;
            --primary-hover: #047857;
            --text-main: #0f172a;
            --text-muted: #64748b;
            --border-line: #f1f5f9;
            --row-even: #ffffff;
            --row-odd: #f8fafc;
            --row-selected: #cbd5e1;
        }

        * {
            box-sizing: border-box;
            font-family: 'Inter', sans-serif;
            margin: 0;
            padding: 0;
            user-select: none;
        }

        /* Garante tela cheia absoluta sem margens */
        html, body {
            width: 100vw;
            height: 100vh;
            background-color: var(--bg-color);
            color: var(--text-main);
            overflow: hidden;
            margin: 0;
            padding: 0;
        }

        .container {
            width: 100vw;
            height: 100vh;
            display: flex;
            flex-direction: column;
            margin: 0;
            padding: 0;
        }

        header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: var(--surface-color);
            padding: 10px 20px;
            border-bottom: 1px solid var(--border-line);
            width: 100%;
            flex-shrink: 0;
        }

        .header-controls {
            display: flex;
            align-items: center;
            justify-content: space-between;
            width: 100%;
        }

        .filters-bar {
            background: var(--surface-color);
            padding: 10px 20px;
            border-bottom: 1px solid var(--border-line);
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            gap: 10px;
            width: 100%;
            flex-shrink: 0;
        }

        .filter-group {
            display: flex;
            align-items: center;
            gap: 6px;
            background: #f8fafc;
            padding: 4px 8px;
            border-radius: 4px;
            border: 1px solid var(--border-line);
        }

        .filter-group select, .filter-group input {
            border: 1px solid var(--border-line);
            border-radius: 4px;
            padding: 4px 6px;
            font-size: 0.78rem;
            outline: none;
            color: var(--text-main);
            background: #ffffff;
        }

        .filter-group input[type="text"] {
            width: 180px;
        }

        .filter-group select {
            cursor: pointer;
        }

        .btn {
            background: var(--primary);
            color: white;
            border: none;
            padding: 6px 16px;
            border-radius: 4px;
            font-weight: 600;
            font-size: 0.78rem;
            cursor: pointer;
            transition: background 0.15s;
        }

        .btn:hover { background: var(--primary-hover); }

        .btn-outline {
            background: transparent;
            color: var(--text-muted);
            border: 1px solid var(--border-line);
        }

        .btn-sm {
            padding: 4px 10px;
            font-size: 0.75rem;
        }

        /* Ocupa toda a largura da tela preenchendo as bordas */
        .table-container {
            flex: 1;
            width: 100vw;
            background: var(--surface-color);
            overflow-y: auto;
            overflow-x: auto;
        }

        table {
            width: 100%;
            table-layout: fixed;
            border-collapse: collapse;
            font-size: 0.8rem;
        }

        col:nth-child(1) { width: 12%; }
        col:nth-child(2) { width: 18%; }
        col:nth-child(3) { width: 34%; }
        col:nth-child(4) { width: 12%; }
        col:nth-child(5) { width: 12%; }
        col:nth-child(6) { width: 12%; }

        th, td {
            padding: 10px 16px;
            text-align: center;
            border: none;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            vertical-align: middle;
        }

        thead th {
            background: #ffffff;
            font-weight: 600;
            color: var(--text-muted);
            border-bottom: 1px solid var(--border-line);
            text-transform: uppercase;
            font-size: 0.7rem;
            letter-spacing: 0.03em;
            position: sticky;
            top: 0;
            z-index: 10;
        }

        tbody tr:nth-child(even) { background-color: var(--row-even); }
        tbody tr:nth-child(odd) { background-color: var(--row-odd); }
        tbody tr { cursor: pointer; transition: background-color 0.1s ease; }
        tbody tr:hover { background-color: #e2e8f0; }
        tbody tr.linha-selecionada { background-color: var(--row-selected) !important; font-weight: 500; }

        .modal-overlay {
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(15, 23, 42, 0.3);
            backdrop-filter: blur(2px);
            display: none;
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }

        .modal {
            background: var(--surface-color);
            border-radius: 8px;
            padding: 20px;
            width: 100%;
            max-width: 480px;
            box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1);
            border: 1px solid var(--border-line);
        }

        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 12px;
        }

        .modal-header h2 { font-size: 0.95rem; font-weight: 700; }

        .form-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
        }

        .form-group {
            display: flex;
            flex-direction: column;
            gap: 2px;
        }

        .form-group.full-width { grid-column: span 2; }
        .form-group label { font-size: 0.68rem; font-weight: 600; color: var(--text-muted); text-transform: uppercase; }
        .form-group input, .form-group select {
            padding: 6px 8px;
            border-radius: 4px;
            border: 1px solid var(--border-line);
            outline: none;
            font-size: 0.78rem;
            background: #f8fafc;
            width: 100%;
        }

        .modal-footer { margin-top: 14px; display: flex; justify-content: flex-end; gap: 8px; }
        #statusNuvem { font-size: 0.72rem; color: var(--text-muted); font-weight: 500; }
    </style>
</head>
<body onload="carregarDados()">

    <div class="container">
        <header>
            <div class="header-controls">
                <span id="statusNuvem">Sincronizando...</span>
                <button class="btn" onclick="abrirModal()">+ Adicionar Item</button>
            </div>
        </header>

        <div class="filters-bar">
            <div class="filter-group">
                <input type="text" id="inputPesquisa" oninput="renderizarTabela()" placeholder="Pesquisar...">
            </div>
            <div class="filter-group">
                <select id="filtroCategoria" onchange="renderizarTabela()">
                    <option value="">Todas Categorias</option>
                </select>
            </div>
            <div class="filter-group">
                <select id="filtroTipo" onchange="renderizarTabela()">
                    <option value="">Todos Tipos</option>
                </select>
            </div>
            <div class="filter-group">
                <select id="filtroStatus" onchange="renderizarTabela()">
                    <option value="">Todos Status</option>
                </select>
            </div>
            <div class="filter-group">
                <input type="date" id="dataInicio" onchange="renderizarTabela()">
                <span>até</span>
                <input type="date" id="dataFim" onchange="renderizarTabela()">
            </div>
            <button class="btn-outline btn btn-sm" onclick="removerTodosFiltros()">✕ Limpar Filtros</button>
        </div>

        <div class="table-container">
            <table>
                <colgroup><col><col><col><col><col><col></colgroup>
                <thead id="tableHeader"></thead>
                <tbody id="tableBody"></tbody>
            </table>
        </div>
    </div>

    <!-- Modal -->
    <div class="modal-overlay" id="modalOverlay">
        <div class="modal">
            <div class="modal-header">
                <h2>Adicionar Novo Item</h2>
                <button class="btn-outline btn btn-sm" onclick="fecharModal()">✕</button>
            </div>
            <form id="dataForm" onsubmit="salvarRegistro(event)">
                <div class="form-grid">
                    <div class="form-group">
                        <label>Data</label>
                        <input type="date" id="fData" required>
                    </div>
                    <div class="form-group">
                        <label>Categoria</label>
                        <select id="fCategoria" required><option value="">Carregando...</option></select>
                    </div>
                    <div class="form-group full-width">
                        <label>Descritivo</label>
                        <input type="text" id="fDescritivo" placeholder="Digite a descrição..." required>
                    </div>
                    <div class="form-group">
                        <label>Valor (R$)</label>
                        <input type="text" id="fValor" placeholder="Ex: 150,00" required>
                    </div>
                    <div class="form-group">
                        <label>Tipo</label>
                        <select id="fTipo" required><option value="">Carregando...</option></select>
                    </div>
                    <div class="form-group full-width">
                        <label>Status</label>
                        <input type="text" id="fStatus" placeholder="Ex: Pago, Pendente..." required>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-outline" onclick="fecharModal()">Cancelar</button>
                    <button type="submit" class="btn">Salvar no Topo</button>
                </div>
            </form>
        </div>
    </div>

    <script>
        const WEB_APP_URL = 'https://script.google.com/macros/s/AKfycbzMSr4YxW90lYjK-2pAJQ5gYVOnVSn0yDsTWYvN0ulz_QjBQZMCWFhyXwYP7rX2u-Fp/exec';
        let dadosBrutos = [];
        let linhasSelecionadasIndices = new Set();
        let ultimoIndiceClicado = null;

        function parseDataParaDateObj(valor) {
            if (!valor) return null;
            const str = valor.toString().trim();
            if (str.includes('/')) {
                const p = str.split('/');
                if (p.length === 3) return new Date(p[2], p[1] - 1, p[0]);
            }
            if (str.includes('-')) {
                const p = str.split('T')[0].split('-');
                if (p.length === 3) return new Date(p[0], p[1] - 1, p[2]);
            }
            const dtNum = Number(str);
            if (!isNaN(dtNum) && dtNum > 30000) {
                return new Date((dtNum - (25567 + 2)) * 86400 * 1000);
            }
            return null;
        }

        function formatarDataExibicao(valor) {
            const dt = parseDataParaDateObj(valor);
            if (!dt || isNaN(dt.getTime())) return valor || '';
            const dia = String(dt.getDate()).padStart(2, '0');
            const mes = String(dt.getMonth() + 1).padStart(2, '0');
            const ano = dt.getFullYear();
            return `${dia}/${mes}/${ano}`;
        }

        async function carregarDados() {
            const statusEl = document.getElementById('statusNuvem');
            statusEl.innerText = "Sincronizando...";
            try {
                const resp = await fetch(WEB_APP_URL);
                dadosBrutos = await resp.json();
                if (!dadosBrutos || dadosBrutos.length === 0) {
                    document.getElementById('tableBody').innerHTML = `<tr><td colspan="100%">Planilha vazia.</td></tr>`;
                    return;
                }
                popularFiltrosCabecalho();
                preencherOpcoesFormularioModal();
                renderizarTabela();
                statusEl.innerText = "Online";
            } catch (err) {
                console.error("Erro:", err);
                statusEl.innerText = "Erro de conexão";
            }
        }

        function popularFiltrosCabecalho() {
            const categoriasSet = new Set(), tiposSet = new Set(), statusSet = new Set();
            let temStatusVazio = false;
            dadosBrutos.slice(1).forEach(row => {
                if (row[1]) categoriasSet.add(row[1].toString().trim());
                if (row[4]) tiposSet.add(row[4].toString().trim());
                const valStatus = row[5];
                if (valStatus !== null && valStatus !== undefined && valStatus.toString().trim() !== '') {
                    statusSet.add(valStatus.toString().trim());
                } else {
                    temStatusVazio = true;
                }
            });
            preencherSelectFiltro('filtroCategoria', categoriasSet, 'Todas Categorias');
            preencherSelectFiltro('filtroTipo', tiposSet, 'Todos Tipos');
            const selectStatus = document.getElementById('filtroStatus');
            const valorAtualStatus = selectStatus.value;
            selectStatus.innerHTML = `<option value="">Todos Status</option>`;
            if (temStatusVazio) {
                const optVazio = document.createElement('option');
                optVazio.value = "VAZIO_VAL";
                optVazio.innerText = "(Em branco)";
                selectStatus.appendChild(optVazio);
            }
            Array.from(statusSet).sort().forEach(item => {
                const opt = document.createElement('option');
                opt.value = item;
                opt.innerText = item;
                selectStatus.appendChild(opt);
            });
            selectStatus.value = valorAtualStatus;
        }

        function preencherSelectFiltro(id, conjunto, textoPadrao) {
            const select = document.getElementById(id);
            const valorAtual = select.value;
            select.innerHTML = `<option value="">${textoPadrao}</option>`;
            Array.from(conjunto).sort().forEach(item => {
                const opt = document.createElement('option');
                opt.value = item;
                opt.innerText = item;
                select.appendChild(opt);
            });
            select.value = valorAtual;
        }

        function removerTodosFiltros() {
            document.getElementById('inputPesquisa').value = '';
            document.getElementById('filtroCategoria').value = '';
            document.getElementById('filtroTipo').value = '';
            document.getElementById('filtroStatus').value = '';
            document.getElementById('dataInicio').value = '';
            document.getElementById('dataFim').value = '';
            renderizarTabela();
        }

        function preencherOpcoesFormularioModal() {
            const catSet = new Set(), tipoSet = new Set();
            dadosBrutos.slice(1).forEach(row => {
                if (row[1]) catSet.add(row[1].toString().trim());
                if (row[4]) tipoSet.add(row[4].toString().trim());
            });
            preencherSelectModal('fCategoria', catSet, 'Selecione...');
            preencherSelectModal('fTipo', tipoSet, 'Selecione...');
        }

        function preencherSelectModal(id, setOpcoes, textoPadrao) {
            const select = document.getElementById(id);
            select.innerHTML = `<option value="">${textoPadrao}</option>`;
            Array.from(setOpcoes).sort().forEach(op => {
                const opt = document.createElement('option');
                opt.value = op;
                opt.innerText = op;
                select.appendChild(opt);
            });
        }

        function renderizarTabela() {
            const tbody = document.getElementById('tableBody');
            const thead = document.getElementById('tableHeader');
            const cabecalhos = dadosBrutos[0];
            thead.innerHTML = `<tr>${cabecalhos.map(col => `<th>${col}</th>`).join('')}</tr>`;

            const textoPesquisa = document.getElementById('inputPesquisa').value.toLowerCase().trim();
            const filtroCat = document.getElementById('filtroCategoria').value;
            const filtroTip = document.getElementById('filtroTipo').value;
            const filtroSta = document.getElementById('filtroStatus').value;
            const inputInicio = document.getElementById('dataInicio').value;
            const inputFim = document.getElementById('dataFim').value;
            const dInicio = inputInicio ? new Date(inputInicio + "T00:00:00") : null;
            const dFim = inputFim ? new Date(inputFim + "T23:59:59") : null;

            let linhasFiltradas = dadosBrutos.slice(1).filter(row => {
                if (dInicio || dFim) {
                    const dtLinha = parseDataParaDateObj(row[0]);
                    if (dtLinha) {
                        if (dInicio && dtLinha < dInicio) return false;
                        if (dFim && dtLinha > dFim) return false;
                    } else { return false; }
                }
                if (filtroCat && row[1] && row[1].toString().trim() !== filtroCat) return false;
                if (filtroTip && row[4] && row[4].toString().trim() !== filtroTip) return false;
                if (filtroSta !== "") {
                    const valStatusRow = row[5] !== null && row[5] !== undefined ? row[5].toString().trim() : "";
                    if (filtroSta === "VAZIO_VAL") {
                        if (valStatusRow !== "") return false;
                    } else {
                        if (valStatusRow !== filtroSta) return false;
                    }
                }
                if (textoPesquisa) {
                    const linhaMatch = row.some(cell => {
                        if (cell === null || cell === undefined) return false;
                        return cell.toString().toLowerCase().includes(textoPesquisa);
                    });
                    if (!linhaMatch) return false;
                }
                return true;
            });

            tbody.innerHTML = linhasFiltradas.map((row, index) => {
                const trId = `row-${index}`;
                const isSelected = linhasSelecionadasIndices.has(index) ? 'linha-selecionada' : '';
                return `<tr id="${trId}" class="${isSelected}" onclick="selecionarLinha(${index}, event)">
                    ${row.map((cell, idx) => {
                        const val = (idx === 0) ? formatarDataExibicao(cell) : (cell !== null && cell !== undefined ? cell : '');
                        return `<td title="${val}">${val}</td>`;
                    }).join('')}
                </tr>`;
            }).join('');
        }

        function selecionarLinha(index, event) {
            const isCtrlPressed = event.ctrlKey || event.metaKey;
            const isShiftPressed = event.shiftKey;
            if (isShiftPressed && ultimoIndiceClicado !== null) {
                const inicio = Math.min(ultimoIndiceClicado, index);
                const fim = Math.max(ultimoIndiceClicado, index);
                if (!isCtrlPressed) { linhasSelecionadasIndices.clear(); }
                for (let i = inicio; i <= fim; i++) { linhasSelecionadasIndices.add(i); }
            } else if (isCtrlPressed) {
                if (linhasSelecionadasIndices.has(index)) { linhasSelecionadasIndices.delete(index); }
                else { linhasSelecionadasIndices.add(index); }
                ultimoIndiceClicado = index;
            } else {
                linhasSelecionadasIndices.clear();
                linhasSelecionadasIndices.add(index);
                ultimoIndiceClicado = index;
            }
            renderizarTabela();
        }

        function abrirModal() {
            document.getElementById('dataForm').reset();
            document.getElementById('modalOverlay').style.display = 'flex';
        }

        function fecharModal() {
            document.getElementById('modalOverlay').style.display = 'none';
        }

        async function salvarRegistro(event) {
            event.preventDefault();
            const dataInput = document.getElementById('fData').value;
            let dataFormatada = dataInput;
            if (dataInput && dataInput.includes('-')) {
                const p = dataInput.split('-');
                dataFormatada = `${p[2]}/${p[1]}/${p[0]}`;
            }
            const payload = {
                col1: dataFormatada,
                col2: document.getElementById('fCategoria').value,
                col3: document.getElementById('fDescritivo').value,
                col4: document.getElementById('fValor').value,
                col5: document.getElementById('fTipo').value,
                col6: document.getElementById('fStatus').value
            };
            document.getElementById('statusNuvem').innerText = "Salvando...";
            try {
                await fetch(WEB_APP_URL, {
                    method: 'POST',
                    headers: { 'Content-Type': 'text/plain;charset=utf-8' },
                    body: JSON.stringify(payload)
                });
                fecharModal();
                setTimeout(carregarDados, 1200);
            } catch (err) {
                console.error("Erro ao salvar:", err);
                alert("Erro ao salvar dados.");
            }
        }
    </script>
</body>
</html>
