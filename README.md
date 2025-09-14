## Hi there 👋
/* README - FreeFire Sensi Pack & Xit Painel

Descrição

Este repositório é um modelo front-end (React single-file) para um "pack de sensi" (coleção de configurações de sensibilidade) e um "Xit Painel" (painel visual para ver / copiar / compartilhar sensis) focado em Free Fire. NÃO contém cheats, hacks ou qualquer modificação do jogo — apenas GUI para organizar e distribuir configurações que jogadores possam aplicar manualmente no jogo.

O que tem aqui

App.jsx: componente React que mostra:

lista de packs de sensibilidade (JSON)

painel "Xit" com preview, copiar para área de transferência, exportar JSON e instruções

busca e filtros simples



Como usar

1. Copie este arquivo para um projeto criado com create-react-app ou outro bundler que suporte JSX.


2. Instale dependências (se usar CRA, já vem com React + ReactDOM).


3. Rode npm start.



Licença

Use livremente — atribuição não exigida. */

import React, { useState } from "react";

const SAMPLE_PACKS = [ { id: "sensi-ghost", name: "Pack Sensi 👻 - Ghost", description: "Sensibilidade equilibrada para mira rápida e controle. Ideal para jogadores agressivos.", settings: { cameraCamera: 100, cameraADS: 90, cameraScope: 85, gyro: true, dpi: 480 }, author: "XitCrew", tags: ["rápido", "gyro"] }, { id: "sensi-smooth", name: "Pack Smooth 🔥 - Xit", description: "Sensibilidade suave para controle estável. Boa para jogadores que priorizam precisão.", settings: { cameraCamera: 80, cameraADS: 70, cameraScope: 65, gyro: false, dpi: 420 }, author: "XitCrew", tags: ["suave", "controle"] }, { id: "sensi-sniper", name: "Pack Sniper 🎯", description: "Configuração focada para tiros de longa distância e scopes.", settings: { cameraCamera: 60, cameraADS: 40, cameraScope: 30, gyro: false, dpi: 600 }, author: "SniperHub", tags: ["scope", "precisão"] } ];

export default function App() { const [packs, setPacks] = useState(SAMPLE_PACKS); const [query, setQuery] = useState(""); const [selected, setSelected] = useState(packs[0]); const [showJSON, setShowJSON] = useState(false);

function handleSearch(e) { const q = e.target.value.toLowerCase(); setQuery(q); const filtered = SAMPLE_PACKS.filter(p => ( p.name.toLowerCase().includes(q) || p.description.toLowerCase().includes(q) || p.tags.join(" ").toLowerCase().includes(q) )); setPacks(filtered); if (filtered.length) setSelected(filtered[0]); else setSelected(null); }

function copyToClipboard(text) { navigator.clipboard.writeText(text).then(() => { alert("Copiado para a área de transferência!"); }).catch(() => { alert("Falha ao copiar. Permita acesso à área de transferência."); }); }

function exportJSON(pack) { const filename = ${pack.id}-sensi.json; const blob = new Blob([JSON.stringify(pack, null, 2)], { type: "application/json" }); const url = URL.createObjectURL(blob); const a = document.createElement("a"); a.href = url; a.download = filename; a.click(); URL.revokeObjectURL(url); }

function handleSelect(packId) { const p = packs.find(x => x.id === packId) || null; setSelected(p); setShowJSON(false); }

function handleNewPack() { const id = custom-${Date.now()}; const newPack = { id, name: Meu Pack - ${new Date().toLocaleDateString()}, description: "Pack criado pelo usuário", settings: { cameraCamera: 85, cameraADS: 75, cameraScope: 70, gyro: false, dpi: 480 }, author: "Você", tags: ["custom"] }; setPacks([newPack, ...packs]); setSelected(newPack); }

return ( <div className="min-h-screen p-6 bg-gradient-to-b from-gray-50 to-gray-100 font-sans"> <div className="max-w-4xl mx-auto"> <header className="mb-6"> <h1 className="text-3xl font-extrabold">FreeFire • Pack de Sensi & Xit Painel</h1> <p className="text-sm text-gray-600 mt-1">Colete, visualize e compartilhe sensi para Free Fire — interface somente para configuração e export.</p> </header>

<div className="flex gap-6">
      <aside className="w-1/3 bg-white p-4 rounded-2xl shadow-sm">
        <div className="mb-3">
          <input
            value={query}
            onChange={handleSearch}
            placeholder="Pesquisar packs, tags..."
            className="w-full p-2 rounded-md border"
          />
        </div>
        <div className="space-y-2 max-h-72 overflow-auto">
          {packs.length === 0 && (<div className="text-sm text-gray-500">Nenhum pack encontrado.</div>)}
          {packs.map(pack => (
            <button key={pack.id} onClick={() => handleSelect(pack.id)} className={`text-left w-full p-2 rounded-lg hover:bg-gray-50 ${selected && selected.id === pack.id ? 'bg-gray-100' : ''}`}>
              <div className="flex items-center justify-between">
                <div>
                  <div className="font-medium">{pack.name}</div>
                  <div className="text-xs text-gray-500">{pack.author} · {pack.tags.join(', ')}</div>
                </div>
                <div className="text-xs text-gray-400">→</div>
              </div>
            </button>
          ))}
        </div>

        <div className="mt-4 flex gap-2">
          <button onClick={handleNewPack} className="flex-1 p-2 rounded-lg bg-black text-white">Novo Pack</button>
          <button onClick={() => { setPacks(SAMPLE_PACKS); setQuery(''); }} className="p-2 rounded-lg border">Reset</button>
        </div>
      </aside>

      <main className="flex-1 bg-white p-6 rounded-2xl shadow-sm">
        {!selected && (<div className="text-gray-500">Selecione um pack à esquerda.</div>)}

        {selected && (
          <div>
            <div className="flex items-start justify-between">
              <div>
                <h2 className="text-2xl font-bold">{selected.name}</h2>
                <p className="text-sm text-gray-600 mt-1">{selected.description}</p>
                <div className="text-xs text-gray-500 mt-2">Autor: {selected.author} • Tags: {selected.tags.join(', ')}</div>
              </div>

              <div className="text-right">
                <div className="mb-2 text-sm">Preview</div>
                <div className="p-3 rounded-lg border bg-gray-50 text-xs text-gray-700 w-56">
                  <div>Camera: {selected.settings.cameraCamera}</div>
                  <div>ADS: {selected.settings.cameraADS}</div>
                  <div>Scope: {selected.settings.cameraScope}</div>
                  <div>Gyro: {selected.settings.gyro ? 'On' : 'Off'}</div>
                  <div>DPI: {selected.settings.dpi}</div>
                </div>
              </div>
            </div>

            <div className="mt-6 grid grid-cols-3 gap-3">
              <button onClick={() => copyToClipboard(JSON.stringify(selected.settings))} className="col-span-1 p-3 rounded-lg border">Copiar Sensi (JSON)</button>
              <button onClick={() => exportJSON(selected)} className="col-span-1 p-3 rounded-lg border">Exportar JSON</button>
              <button onClick={() => { setShowJSON(!showJSON); }} className="col-span-1 p-3 rounded-lg border">{showJSON ? 'Fechar JSON' : 'Ver JSON'}</button>
            </div>

            {showJSON && (
              <pre className="mt-4 p-4 bg-black text-white rounded-lg overflow-auto text-sm">{JSON.stringify(selected, null, 2)}</pre>
            )}

            <section className="mt-6">
              <h3 className="font-semibold">Instruções rápidas</h3>
              <ol className="mt-2 list-decimal list-inside text-sm text-gray-600">
                <li>Abra Free Fire → Configurações → Sensibilidade.</li>
                <li>Insira manualmente os valores do pack (use o botão "Copiar Sensi (JSON)" como referência).</li>
                <li>Teste em treino e ajuste conforme sua preferência.</li>
              </ol>
            </section>

            <section className="mt-6">
              <h3 className="font-semibold">Compartilhar</h3>
              <p className="text-sm text-gray-600 mt-1">Use o botão de exportar para enviar o JSON para amigos ou publicar no GitHub.</p>
            </section>
          </div>
        )}
      </main>
    </div>

    <footer className="mt-8 text-center text-xs text-gray-500">
      Modelo de repositório — personalize, adicione mais packs e publique no GitHub.
    </footer>
  </div>
</div>

); }


<!--
**kaioxzz7/kaioxzz7** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->
