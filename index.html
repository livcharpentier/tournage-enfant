// ─── CONSTANTES ───────────────────────────────────────────────────
const LIMITES = { '<3': 120, '3-5': 180, '6-11': 240, '12-15': 360, '16-17': 480 };
const TYPE_LABEL = {
  arrivee: 'Arrivée', hmc: 'HMC/Prép.', tournage: 'Tournage',
  pause: 'Pause', repas: 'Repas', attente: 'Attente',
  cours: 'Cours', cascade: 'Cascade', sante: 'Santé', fin: 'Fin journée'
};
const TYPE_COLOR = {
  arrivee: ['#d1fae5', '#065f46'], hmc: ['#dbeafe', '#1d4ed8'],
  tournage: ['#a7f3d0', '#065f46'], pause: ['#f3f4f6', '#1f2937'],
  repas: ['#fef9c3', '#713f12'], attente: ['#fef3c7', '#92400e'],
  cours: ['#ede9fe', '#5b21b6'], cascade: ['#fee2e2', '#991b1b'],
  sante: ['#fce7f3', '#9d174d'], fin: ['#fce7f3', '#9d174d']
};
const WORK_TYPES = ['tournage', 'hmc', 'cascade', 'cours'];

// ─── STATE ────────────────────────────────────────────────────────
let state = {
  films: [],
  enfants: [],
  logs: [],       // { id, filmId, enfantId, type, heure, fin, date }
  activeFilmId: null,
  contacts: { scripte: '', prod: '' }
};

let selectedEnfants = [];  // [] = tous
let recognition = null;
let isListening = false;
let nextId = 1;

// ─── PERSISTENCE ─────────────────────────────────────────────────
function save() {
  try { localStorage.setItem('tournage_v2', JSON.stringify({ state, nextId, selectedEnfants })); } catch(e) {}
}

function load() {
  try {
    const raw = localStorage.getItem('tournage_v2');
    if (raw) {
      const d = JSON.parse(raw);
      state = d.state || state;
      nextId = d.nextId || 1;
      selectedEnfants = d.selectedEnfants || [];
    }
  } catch(e) {}
  if (state.commentaireJour) {
    const el = document.getElementById('commentaire-jour');
    if (el) el.value = state.commentaireJour;
  }
  // restore contacts inputs
  if (state.contacts) {
    const sc = document.getElementById('contact-scripte');
    const pr = document.getElementById('contact-prod');
    if (sc) sc.value = state.contacts.scripte || '';
    if (pr) pr.value = state.contacts.prod || '';
    const ws = document.getElementById('wa-scripte');
    const wp = document.getElementById('wa-prod');
    if (ws) ws.value = state.contacts.scripte || '';
    if (wp) wp.value = state.contacts.prod || '';
  }
}

function saveCommentaire() {
  const el = document.getElementById('commentaire-jour');
  if (el) { state.commentaireJour = el.value; save(); }
}

function saveContacts() {
  const sc = document.getElementById('contact-scripte');
  const pr = document.getElementById('contact-prod');
  state.contacts = { scripte: sc ? sc.value : '', prod: pr ? pr.value : '' };
  const ws = document.getElementById('wa-scripte');
  const wp = document.getElementById('wa-prod');
  if (ws) ws.value = state.contacts.scripte;
  if (wp) wp.value = state.contacts.prod;
  save();
}

// ─── HELPERS ──────────────────────────────────────────────────────
function toMin(t) { const [h, m] = t.split(':').map(Number); return h * 60 + m; }
function fmt(min) {
  if (!min || min <= 0) return '—';
  const h = Math.floor(min / 60), m = min % 60;
  return h > 0 ? (m > 0 ? `${h}h${String(m).padStart(2,'0')}` : `${h}h`) : `${m}min`;
}
function nowHM() {
  const n = new Date();
  return `${String(n.getHours()).padStart(2,'0')}:${String(n.getMinutes()).padStart(2,'0')}`;
}
function todayStr() { return new Date().toISOString().slice(0, 10); }
function initials(nom) { return nom.split(' ').map(w => w[0]).join('').toUpperCase().slice(0, 2); }
function activeFilm() { return state.films.find(f => f.id === state.activeFilmId); }
function activeEnfants() { return state.enfants.filter(e => e.filmId === state.activeFilmId); }
function todayLogs() { return state.logs.filter(l => l.filmId === state.activeFilmId && l.date === todayStr()); }

// ─── CLOCK ────────────────────────────────────────────────────────
function tickClock() {
  const n = new Date();
  const hm = `${String(n.getHours()).padStart(2,'0')}:${String(n.getMinutes()).padStart(2,'0')}`;
  const hms = hm + ':' + String(n.getSeconds()).padStart(2,'0');
  const el = document.getElementById('live-clock');
  if (el) el.textContent = hm;
  const days = ['Dimanche','Lundi','Mardi','Mercredi','Jeudi','Vendredi','Samedi'];
  const months = ['janvier','février','mars','avril','mai','juin','juillet','août','septembre','octobre','novembre','décembre'];
  const nd = new Date();
  const ds = `${days[nd.getDay()]} ${nd.getDate()} ${months[nd.getMonth()]} ${nd.getFullYear()}`;
  const ld = document.getElementById('live-date');
  if (ld) ld.textContent = ds;
  const lt = document.getElementById('live-date-top');
  if (lt) lt.textContent = hm + '\n' + ds;
}
setInterval(tickClock, 1000);
tickClock();

// ─── NAVIGATION ───────────────────────────────────────────────────
function showPage(name, btn) {
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
  document.getElementById('page-' + name).classList.add('active');
  if (btn) btn.classList.add('active');
  if (name === 'planning') renderPlanning();
  if (name === 'resume') renderResume();
  if (name === 'films') renderFilmsPage();
  if (name === 'live') { renderChips(); renderStatusBar(); renderLog(); updateFilmDisplay(); }
}

function openModal(id) { document.getElementById(id).classList.add('open'); }
function closeModal(id) { document.getElementById(id).classList.remove('open'); }

// ─── FILMS ────────────────────────────────────────────────────────
function ajouterFilm() {
  const nom = document.getElementById('new-film-nom').value.trim();
  const real = document.getElementById('new-film-real').value.trim();
  if (!nom) { alert('Entrez un titre de film.'); return; }
  const film = { id: nextId++, nom, real };
  state.films.push(film);
  state.activeFilmId = film.id;
  selectedEnfants = [];
  document.getElementById('new-film-nom').value = '';
  document.getElementById('new-film-real').value = '';
  closeModal('modal-film');
  save();
  renderFilmsPage();
  renderChips();
  updateFilmDisplay();
}

function setActiveFilm(id) {
  state.activeFilmId = id;
  selectedEnfants = [];
  save();
  renderFilmsPage();
  renderChips();
  updateFilmDisplay();
  renderStatusBar();
  renderLog();
}

function supprimerFilm(id) {
  if (!confirm('Supprimer ce film et toutes ses données ?')) return;
  state.films = state.films.filter(f => f.id !== id);
  state.enfants = state.enfants.filter(e => e.filmId !== id);
  state.logs = state.logs.filter(l => l.filmId !== id);
  if (state.activeFilmId === id) state.activeFilmId = state.films[0]?.id || null;
  save();
  renderFilmsPage();
  renderChips();
  updateFilmDisplay();
}

function updateFilmDisplay() {
  const f = activeFilm();
  const name = f ? f.nom : 'Aucun film sélectionné';
  ['film-name-live', 'planning-film-name', 'resume-film-name'].forEach(id => {
    const el = document.getElementById(id);
    if (el) el.textContent = name;
  });
}

function renderFilmsPage() {
  const fl = document.getElementById('films-list');
  if (!fl) return;
  if (!state.films.length) {
    fl.innerHTML = '<div class="empty"><div class="empty-icon">🎬</div>Aucun film. Créez votre premier projet.</div>';
  } else {
    fl.innerHTML = state.films.map(f => `
      <div class="film-card ${f.id === state.activeFilmId ? 'active-film' : ''}" onclick="setActiveFilm(${f.id})">
        <div class="film-card-icon">🎬</div>
        <div class="film-card-info">
          <div class="film-card-name">${f.nom}</div>
          <div class="film-card-sub">${f.real ? 'Réal. ' + f.real : 'Appuyez pour sélectionner'}${f.id === state.activeFilmId ? ' ✓ Actif' : ''}</div>
        </div>
        <button class="film-card-del" onclick="event.stopPropagation();supprimerFilm(${f.id})">🗑</button>
      </div>`).join('');
  }
  renderEnfantsList();
}

// ─── ENFANTS ──────────────────────────────────────────────────────
function ajouterEnfant() {
  if (!state.activeFilmId) { alert('Créez ou sélectionnez un film d\'abord.'); return; }
  const nom = document.getElementById('new-enf-nom').value.trim();
  const age = document.getElementById('new-enf-age').value;
  const couleur = document.getElementById('new-enf-color').value;
  if (!nom) { alert('Entrez un prénom.'); return; }
  state.enfants.push({ id: nextId++, filmId: state.activeFilmId, nom, age, couleur });
  document.getElementById('new-enf-nom').value = '';
  closeModal('modal-enfant');
  save();
  renderEnfantsList();
  renderChips();
}

function supprimerEnfant(id) {
  if (!confirm('Supprimer cet enfant ?')) return;
  state.enfants = state.enfants.filter(e => e.id !== id);
  state.logs = state.logs.filter(l => l.enfantId !== id);
  selectedEnfants = selectedEnfants.filter(x => x !== id);
  save();
  renderEnfantsList();
  renderChips();
  renderStatusBar();
  renderLog();
}

function renderEnfantsList() {
  const el = document.getElementById('enfants-list');
  if (!el) return;
  const enfs = activeEnfants();
  if (!enfs.length) {
    el.innerHTML = '<div class="empty"><div class="empty-icon">👦</div>Aucun enfant sur ce film.</div>';
    return;
  }
  el.innerHTML = enfs.map(e => `
    <div style="display:flex;align-items:center;gap:10px;padding:10px 0;border-bottom:1px solid var(--border)">
      <div style="width:34px;height:34px;border-radius:50%;background:${e.couleur}22;color:${e.couleur};font-size:12px;font-weight:700;display:flex;align-items:center;justify-content:center;flex-shrink:0">${initials(e.nom)}</div>
      <div style="flex:1">
        <div style="font-size:14px;font-weight:600">${e.nom}</div>
        <div style="font-size:11px;color:var(--text-muted)">${e.age} ans · max ${fmt(LIMITES[e.age])}/jour</div>
      </div>
      <button onclick="supprimerEnfant(${e.id})" style="background:none;border:none;font-size:18px;cursor:pointer;color:var(--text-muted)">✕</button>
    </div>`).join('');
}

// ─── CHIPS ────────────────────────────────────────────────────────
function renderChips() {
  const c = document.getElementById('enfant-chips');
  if (!c) return;
  const enfs = activeEnfants();
  if (!enfs.length) {
    c.innerHTML = '<div style="font-size:13px;color:var(--text-muted)">Ajoutez des enfants dans l\'onglet Films</div>';
    return;
  }
  let html = '';
  if (enfs.length > 1) {
    const allSel = selectedEnfants.length === 0;
    html += `<div class="chip all-chip ${allSel ? 'sel' : ''}" onclick="selectEnfant('all')">Tous</div>`;
  }
  enfs.forEach(e => {
    const sel = selectedEnfants.includes(e.id);
    html += `<div class="chip ${sel ? 'sel' : ''}" style="${sel ? 'background:' + e.couleur + ';border-color:' + e.couleur : ''}" onclick="selectEnfant(${e.id})">${e.nom.split(' ')[0]}</div>`;
  });
  c.innerHTML = html;
}

function selectEnfant(id) {
  if (id === 'all') { selectedEnfants = []; }
  else {
    if (selectedEnfants.includes(id)) selectedEnfants = selectedEnfants.filter(x => x !== id);
    else selectedEnfants.push(id);
  }
  renderChips();
  save();
}

// ─── LOG ACTION ───────────────────────────────────────────────────
function logAction(type) {
  if (!state.activeFilmId) { alert('Sélectionnez un film d\'abord (onglet Films).'); return; }
  const enfs = activeEnfants();
  if (!enfs.length) { alert('Ajoutez des enfants dans l\'onglet Films.'); return; }
  const hm = nowHM();
  const targets = selectedEnfants.length > 0 ? selectedEnfants : enfs.map(e => e.id);

  targets.forEach(eid => {
    // fermer l'entrée précédente ouverte pour cet enfant aujourd'hui
    const prev = todayLogs()
      .filter(l => l.enfantId === eid && !l.fin)
      .sort((a, b) => b.heure.localeCompare(a.heure))[0];
    if (prev) prev.fin = hm;

    state.logs.push({
      id: nextId++,
      filmId: state.activeFilmId,
      enfantId: eid,
      type,
      heure: hm,
      fin: null,
      date: todayStr()
    });
  });

  // update last-time on button
  const ltEl = document.getElementById('lt-' + type);
  if (ltEl) ltEl.textContent = hm;

  save();
  renderLog();
  renderStatusBar();

  // vibration feedback
  if (navigator.vibrate) navigator.vibrate(30);
}

function delLog(id) {
  state.logs = state.logs.filter(l => l.id !== id);
  save();
  renderLog();
  renderStatusBar();
}

function clearLog() {
  if (!confirm('Effacer tout le journal d\'aujourd\'hui ?')) return;
  state.logs = state.logs.filter(l => !(l.filmId === state.activeFilmId && l.date === todayStr()));
  save();
  renderLog();
  renderStatusBar();
}

function clearDay() {
  if (!confirm('Effacer le journal du jour ?')) return;
  state.logs = state.logs.filter(l => !(l.filmId === state.activeFilmId && l.date === todayStr()));
  save();
  renderLog();
  renderStatusBar();
}

function renderLog() {
  const el = document.getElementById('log-list');
  const lc = document.getElementById('log-count');
  const logs = todayLogs().sort((a, b) => a.heure.localeCompare(b.heure) || a.id - b.id);
  if (lc) lc.textContent = `(${logs.length})`;
  if (!el) return;
  if (!logs.length) {
    el.innerHTML = '<div class="empty"><div class="empty-icon">📋</div>Appuyez sur une action pour commencer</div>';
    return;
  }
  const enfs = activeEnfants();
  el.innerHTML = logs.map(l => {
    const enf = enfs.find(e => e.id === l.enfantId);
    const [bg, tc] = TYPE_COLOR[l.type] || ['#f3f4f6', '#1f2937'];
    const dur = l.fin ? fmt(toMin(l.fin) - toMin(l.heure)) : 'en cours';
    return `<div class="log-item">
      <div class="log-time">${l.heure}</div>
      <div class="log-tag" style="background:${bg};color:${tc}">${TYPE_LABEL[l.type] || l.type}</div>
      <div class="log-who">${enf ? enf.nom.split(' ')[0] : '?'}${l.fin ? ' → ' + l.fin : ''}</div>
      <div class="log-dur">${dur}</div>
      <button class="log-del" onclick="delLog(${l.id})">✕</button>
    </div>`;
  }).join('');
}

// ─── STATUS BAR ───────────────────────────────────────────────────
function renderStatusBar() {
  const sb = document.getElementById('status-bar');
  if (!sb) return;
  const enfs = activeEnfants();
  if (!enfs.length) { sb.innerHTML = ''; return; }
  sb.innerHTML = enfs.map(e => {
    const logs = todayLogs().filter(l => l.enfantId === e.id);
    const last = logs.sort((a, b) => b.heure.localeCompare(a.heure))[0];
    const lim = LIMITES[e.age];
    const travail = logs.filter(l => WORK_TYPES.includes(l.type) && l.fin)
      .reduce((s, l) => s + Math.max(0, toMin(l.fin) - toMin(l.heure)), 0);
    const pct = Math.round(travail / lim * 100);
    const warn = pct >= 90;
    const [bg] = last ? TYPE_COLOR[last.type] || ['#f3f4f6'] : ['#f3f4f6'];
    return `<div class="status-card ${warn ? 'status-warn' : ''}">
      <div class="status-name">${e.nom.split(' ')[0]}</div>
      <div class="status-state"><div class="dot" style="background:${bg}"></div>${last ? (TYPE_LABEL[last.type] || last.type) : 'Non arrivé'}</div>
      <div class="status-time">${last ? last.heure : '—'} · plateau ${pct}%</div>
    </div>`;
  }).join('');
}

// ─── VOICE ────────────────────────────────────────────────────────
function toggleMic() {
  if (!('webkitSpeechRecognition' in window) && !('SpeechRecognition' in window)) {
    alert('La dictée vocale nécessite Chrome ou Safari récent.');
    return;
  }
  if (isListening) { stopMic(); return; }
  const SR = window.SpeechRecognition || window.webkitSpeechRecognition;
  recognition = new SR();
  recognition.lang = 'fr-FR';
  recognition.interimResults = true;
  recognition.continuous = false;
  recognition.onstart = () => {
    isListening = true;
    document.getElementById('mic-btn').classList.add('on');
    document.getElementById('mic-hint').textContent = 'Écoute… parlez !';
  };
  recognition.onresult = (e) => {
    const t = Array.from(e.results).map(r => r[0].transcript).join('');
    document.getElementById('mic-txt').textContent = t;
    if (e.results[e.results.length - 1].isFinal) parseVoice(t.toLowerCase());
  };
  recognition.onerror = () => stopMic();
  recognition.onend = () => stopMic();
  recognition.start();
}

function stopMic() {
  isListening = false;
  if (recognition) try { recognition.stop(); } catch(e) {}
  const btn = document.getElementById('mic-btn');
  if (btn) btn.classList.remove('on');
  const hint = document.getElementById('mic-hint');
  if (hint) hint.textContent = 'Dicter : "tournage", "pause", "repas"…';
  setTimeout(() => { const t = document.getElementById('mic-txt'); if (t) t.textContent = ''; }, 3000);
}

function parseVoice(text) {
  const map = {
    'arrivée': 'arrivee', 'arrivee': 'arrivee', 'arrivé': 'arrivee',
    'hmc': 'hmc', 'maquillage': 'hmc', 'costume': 'hmc', 'préparation': 'hmc', 'prep': 'hmc',
    'tournage': 'tournage', 'action': 'tournage', 'on tourne': 'tournage',
    'pause': 'pause', 'coupure': 'pause',
    'repas': 'repas', 'déjeuner': 'repas', 'manger': 'repas', 'lunch': 'repas',
    'attente': 'attente',
    'cours': 'cours', 'école': 'cours', 'scolaire': 'cours',
    'cascade': 'cascade',
    'santé': 'sante', 'médecin': 'sante', 'docteur': 'sante',
    'fin': 'fin', 'départ': 'fin', 'terminé': 'fin', 'fini': 'fin'
  };
  for (const [k, v] of Object.entries(map)) {
    if (text.includes(k)) { logAction(v); return; }
  }
  const t = document.getElementById('mic-txt');
  if (t) t.textContent = `Pas compris : "${text}"`;
}

// ─── PLANNING ─────────────────────────────────────────────────────
function renderPlanning() {
  const sv = document.getElementById('gs')?.value || '06:00';
  const ev = document.getElementById('ge')?.value || '22:00';
  const sm = toMin(sv), em = toMin(ev), span = em - sm;
  if (span <= 0) return;

  const nowMin = new Date().getHours() * 60 + new Date().getMinutes();
  const nowPct = Math.min(100, Math.max(0, (nowMin - sm) / span * 100));

  const ticks = [];
  let h = Math.ceil(sm / 60);
  while (h * 60 <= em) { ticks.push(h); h++; }

  const enfs = activeEnfants();
  const logs = todayLogs();
  const rows = document.getElementById('gantt-rows');
  if (!rows) return;

  let html = `<div class="gantt-row"><div class="gantt-name-empty"></div><div class="gantt-ticks">${ticks.map(h => {
    const p = ((h * 60 - sm) / span * 100).toFixed(1);
    return `<span class="gantt-tick" style="left:${p}%">${String(h).padStart(2,'0')}h</span>`;
  }).join('')}</div></div>`;

  enfs.forEach(e => {
    const evs = logs.filter(l => l.enfantId === e.id).sort((a, b) => a.heure.localeCompare(b.heure));
    const lim = LIMITES[e.age];
    const limPct = Math.min(100, lim / span * 100).toFixed(1);

    const blocks = evs.map(l => {
      const ls = Math.max(0, toMin(l.heure) - sm);
      const le = l.fin ? toMin(l.fin) - sm : nowMin - sm;
      const left = (ls / span * 100).toFixed(1);
      const width = Math.max(0.5, ((le - ls) / span * 100)).toFixed(1);
      if (parseFloat(left) >= 100) return '';
      const [bg] = TYPE_COLOR[l.type] || ['#f3f4f6'];
      return `<div class="gantt-block" style="left:${left}%;width:${Math.min(100 - parseFloat(left), parseFloat(width))}%;background:${bg}" title="${TYPE_LABEL[l.type]} ${l.heure}${l.fin ? '→' + l.fin : ''}"></div>`;
    }).join('');

    html += `<div class="gantt-row">
      <div class="gantt-name" style="display:flex;align-items:center;gap:4px">
        <div style="width:20px;height:20px;border-radius:50%;background:${e.couleur}22;color:${e.couleur};font-size:9px;font-weight:700;display:flex;align-items:center;justify-content:center;flex-shrink:0">${initials(e.nom)}</div>
        ${e.nom.split(' ')[0]}
      </div>
      <div class="gantt-track">
        <div class="gantt-limit" style="width:${limPct}%"></div>
        ${blocks}
        <div class="gantt-now" style="left:${nowPct.toFixed(1)}%"></div>
      </div>
    </div>`;
  });

  rows.innerHTML = html || '<div class="empty">Aucun enfant</div>';

  // progress bars
  const pb = document.getElementById('prog-bars');
  if (pb) pb.innerHTML = enfs.map(e => {
    const lim = LIMITES[e.age];
    const travail = logs.filter(l => l.enfantId === e.id && WORK_TYPES.includes(l.type) && l.fin)
      .reduce((s, l) => s + Math.max(0, toMin(l.fin) - toMin(l.heure)), 0);
    const pct = Math.min(100, Math.round(travail / lim * 100));
    const col = pct >= 100 ? '#ef4444' : pct >= 80 ? '#f59e0b' : '#10b981';
    return `<div class="prog-row">
      <div class="prog-info">
        <div class="name">${e.nom.split(' ')[0]}</div>
        <div class="sub">${fmt(travail)} / ${fmt(lim)}</div>
      </div>
      <div class="prog-bar-wrap"><div class="prog-bar" style="width:${pct}%;background:${col}"></div></div>
      <div class="prog-pct" style="color:${col}">${pct}%</div>
    </div>`;
  }).join('') || '<div class="empty">Aucun enfant</div>';
}

// ─── RÉSUMÉ ───────────────────────────────────────────────────────
function buildSummary() {
  const film = activeFilm();
  const enfs = activeEnfants();
  const logs = todayLogs().sort((a, b) => a.heure.localeCompare(b.heure));
  const today = new Date();
  const days = ['Dimanche','Lundi','Mardi','Mercredi','Jeudi','Vendredi','Samedi'];
  const months = ['janvier','février','mars','avril','mai','juin','juillet','août','septembre','octobre','novembre','décembre'];
  const dateStr = `${days[today.getDay()]} ${today.getDate()} ${months[today.getMonth()]} ${today.getFullYear()}`;
  const commentaire = document.getElementById('commentaire-jour')?.value?.trim() || '';

  let txt = `🎬 ${film ? film.nom.toUpperCase() : 'TOURNAGE'}\n📅 ${dateStr}\n${'─'.repeat(30)}\n\n`;

  enfs.forEach(enf => {
    const evs = logs.filter(l => l.enfantId === enf.id);
    if (!evs.length) return;

    const dur = {};
    Object.keys(TYPE_LABEL).forEach(t => dur[t] = 0);
    evs.forEach(l => {
      if (l.fin) dur[l.type] = (dur[l.type] || 0) + Math.max(0, toMin(l.fin) - toMin(l.heure));
    });
    const travail = WORK_TYPES.reduce((s, t) => s + (dur[t] || 0), 0);
    const lim = LIMITES[enf.age];
    const pct = Math.round(travail / lim * 100);
    const statut = pct >= 100 ? '⚠️ DÉPASSEMENT' : pct >= 80 ? '⚠️ Proche limite' : '✅ OK';

    // heures clés
    const hArrivee = evs.find(l => l.type === 'arrivee');
    const hFin = [...evs].reverse().find(l => l.type === 'fin');
    const hRepas = evs.find(l => l.type === 'repas');

    txt += `👦 ${enf.nom}\n`;
    if (hArrivee) txt += `🟢 Arrivée       ${hArrivee.heure}\n`;
    if (dur.hmc)  txt += `✂️👗🎨 HMC/Prép.  ${fmt(dur.hmc)}\n`;
    if (dur.tournage) txt += `🎥 Tournage      ${fmt(dur.tournage)}\n`;
    if (dur.pause)    txt += `🧩 Pause         ${fmt(dur.pause)}\n`;
    if (hRepas)       txt += `🍽 Repas         ${hRepas.heure}${hRepas.fin ? ' → ' + hRepas.fin : ''}\n`;
    if (dur.attente)  txt += `☁️ Attente       ${fmt(dur.attente)}\n`;
    if (dur.cours)    txt += `📚 Cours         ${fmt(dur.cours)}\n`;
    if (dur.cascade)  txt += `🤸 Cascade       ${fmt(dur.cascade)}\n`;
    if (hFin)         txt += `🏁 Fin journée   ${hFin.heure}\n`;
    txt += `${statut} Plateau : ${fmt(travail)} / ${fmt(lim)}\n\n`;
  });

  if (commentaire) {
    txt += `${'─'.repeat(30)}\n📝 NOTE DU JOUR\n${commentaire}\n\n`;
  }

  txt += `${'─'.repeat(30)}\n🕐 Envoyé à ${nowHM()}`;
  return txt;
}

function renderResume() {
  const sp = document.getElementById('summary-txt');
  if (sp) sp.textContent = buildSummary();

  // contacts sync
  const ws = document.getElementById('wa-scripte');
  const wp = document.getElementById('wa-prod');
  if (ws && state.contacts.scripte) ws.value = state.contacts.scripte;
  if (wp && state.contacts.prod) wp.value = state.contacts.prod;

  // alerts + metrics
  const enfs = activeEnfants();
  const logs = todayLogs();
  const alertDiv = document.getElementById('resume-alerts');
  const metrics = document.getElementById('resume-metrics');
  const chrono = document.getElementById('chrono-enfants');

  let alerts = '';
  let totTournage = 0, present = new Set();
  logs.forEach(l => {
    if (l.type === 'tournage' && l.fin) { totTournage += Math.max(0, toMin(l.fin) - toMin(l.heure)); present.add(l.enfantId); }
  });

  if (metrics) metrics.innerHTML = `
    <div class="metric"><div class="metric-val">${present.size}</div><div class="metric-lbl">Présents</div></div>
    <div class="metric"><div class="metric-val">${fmt(totTournage) || '—'}</div><div class="metric-lbl">Tournage</div></div>
    <div class="metric"><div class="metric-val">${logs.length}</div><div class="metric-lbl">Entrées</div></div>
  `;

  enfs.forEach(e => {
    const lim = LIMITES[e.age];
    const travail = logs.filter(l => l.enfantId === e.id && WORK_TYPES.includes(l.type) && l.fin)
      .reduce((s, l) => s + Math.max(0, toMin(l.fin) - toMin(l.heure)), 0);
    const pct = Math.round(travail / lim * 100);
    if (pct >= 100) alerts += `<div class="alert alert-danger">⚠ ${e.nom} a dépassé la limite légale (${fmt(travail)} / ${fmt(lim)})</div>`;
    else if (pct >= 80) alerts += `<div class="alert alert-warn">⚠ ${e.nom} proche de la limite (${pct}%)</div>`;
  });
  if (!alerts && logs.length) alerts = '<div class="alert alert-ok">✅ Tous les enfants respectent les limites légales</div>';
  if (alertDiv) alertDiv.innerHTML = alerts;

  if (chrono) {
    chrono.innerHTML = enfs.map(e => {
      const evs = logs.filter(l => l.enfantId === e.id).sort((a, b) => a.heure.localeCompare(b.heure));
      if (!evs.length) return '';
      const items = evs.map(l => {
        const [bg, tc] = TYPE_COLOR[l.type] || ['#f3f4f6', '#1f2937'];
        return `<span class="chrono-item"><span class="chrono-item-time">${l.heure}</span> <span style="background:${bg};color:${tc};padding:1px 7px;border-radius:8px;font-size:11px;font-weight:600">${TYPE_LABEL[l.type]}</span>${l.fin ? '<span style="font-size:11px;color:var(--text-muted)">→' + l.fin + '</span>' : ''}</span>`;
      }).join(' ');
      return `<div class="chrono-block">
        <div class="chrono-name">
          <div style="width:22px;height:22px;border-radius:50%;background:${e.couleur}22;color:${e.couleur};font-size:10px;font-weight:700;display:flex;align-items:center;justify-content:center">${initials(e.nom)}</div>
          ${e.nom}
        </div>
        <div class="chrono-items">${items}</div>
      </div>`;
    }).join('') || '<div class="empty">Aucune donnée pour aujourd\'hui</div>';
  }
}

// ─── WHATSAPP / SMS ───────────────────────────────────────────────
function getNumber(role) {
  const fromResume = document.getElementById('wa-' + role)?.value;
  const fromContacts = state.contacts[role];
  const n = fromResume || fromContacts || '';
  if (!n) { alert(`Entrez le numéro ${role === 'scripte' ? 'de la scripte' : 'de la production'} dans le champ ci-dessous.`); }
  // save it
  if (n) { state.contacts[role] = n; save(); }
  return n;
}

function sendWhatsApp(role) {
  const n = getNumber(role);
  if (!n) return;
  const clean = n.replace(/\s/g, '').replace(/^00/, '+');
  const msg = encodeURIComponent(buildSummary());
  window.open(`https://wa.me/${clean}?text=${msg}`);
}

function sendSMS(role) {
  const n = getNumber(role);
  if (!n) return;
  const msg = encodeURIComponent(buildSummary());
  // iOS uses &, Android uses ?
  const sep = /iPhone|iPad|iPod/.test(navigator.userAgent) ? '&' : '?';
  window.open(`sms:${n}${sep}body=${msg}`);
}

function copyResume() {
  const txt = buildSummary();
  navigator.clipboard.writeText(txt)
    .then(() => alert('Résumé copié !'))
    .catch(() => {
      const ta = document.createElement('textarea');
      ta.value = txt; document.body.appendChild(ta); ta.select();
      document.execCommand('copy'); document.body.removeChild(ta);
      alert('Résumé copié !');
    });
}

// ─── INIT ─────────────────────────────────────────────────────────
load();
renderChips();
renderLog();
renderStatusBar();
updateFilmDisplay();

// refresh planning + status every minute
setInterval(() => {
  const activePage = document.querySelector('.page.active');
  if (activePage && activePage.id === 'page-planning') renderPlanning();
  renderStatusBar();
}, 60000);

// register service worker
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('sw.js').catch(() => {});
}
