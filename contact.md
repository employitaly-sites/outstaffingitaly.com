# Получить расчёт

> Запросите расчёт стоимости сотрудника в Италии. Три коротких шага, ответ в течение одного рабочего дня.


<section> <span class="eyebrow">Расчёт стоимости</span> <h1 id="contact">Кого вы хотите нанять в Италии?</h1> <p class="lede">Три коротких шага. В течение одного рабочего дня пришлём полную стоимость и схему договора. Можно писать на русском, английском или итальянском.</p> </section> <section> <form id="lead" class="panel" style="display:grid;gap:18px;max-width:640px" novalidate> <p id="stepinfo" class="tag" style="justify-self:start">Шаг 1 из 3</p> <fieldset data-step="1" style="border:0;padding:0;margin:0;display:grid;gap:14px"> <label for="email">Рабочий email<input id="email" type="email" autocomplete="email" required></label> <label for="company">Компания<input id="company" autocomplete="organization"></label> </fieldset> <fieldset data-step="2" hidden style="border:0;padding:0;margin:0;display:grid;gap:14px"> <label for="service">Что вам нужно?<select id="service"><option>Employer of Record (аутстаффинг)</option><option>Расчёт зарплаты (есть итальянское юрлицо)</option><option>Рабочая виза / Голубая карта ЕС</option><option>Подбор персонала</option></select></label> <label for="count">Сколько человек в Италии?<input id="count" type="number" min="1" value="1"></label> <label for="city">Город или регион<input id="city" placeholder="Милан, Рим, удалённо…"></label> <label for="salary">Планируемая годовая зарплата брутто (EUR)<input id="salary" type="number" step="1000"></label> </fieldset> <fieldset data-step="3" hidden style="border:0;padding:0;margin:0;display:grid;gap:14px"> <label for="name">Ваше имя<input id="name" autocomplete="name"></label> <label for="start">Желаемая дата выхода<input id="start" type="date"></label> <label for="notes">Что ещё нам важно знать (гражданство кандидата, нужна ли виза)<textarea id="notes" rows="4"></textarea></label> <label style="display:flex;gap:10px;align-items:flex-start;font-weight:400"><input id="consent" type="checkbox" style="width:auto;margin-top:5px">Я согласен(на), что Outstaffing Italy обрабатывает мои данные, чтобы ответить на запрос (политика конфиденциальности).</label> </fieldset> <p id="msg" role="status" style="margin:0;color:var(--accent)"></p> <div class="cta"><button type="button" class="btn ghost" id="back" hidden>Назад</button><button type="submit" class="btn" id="next">Далее</button></div> </form> </section> <script>
// Il passo 1 salva subito l'email (lead abbandonato recuperabile dal nurturing). Endpoint del worker: /api/lead.
const f = document.getElementById('lead'); let step = 1;
const sets = [...f.querySelectorAll('fieldset')];
const data = () => Object.fromEntries([...f.querySelectorAll('input,select,textarea')].map(e => [e.id, e.type === 'checkbox' ? e.checked : e.value]));
async function save(final){ try { await fetch('/api/lead', {method:'POST', headers:{'Content-Type':'application/json'}, body: JSON.stringify({...data(), step, final, brand:'outstaffingitaly'})}); } catch(e){} }
function show(){ sets.forEach(s => s.hidden = +s.dataset.step !== step);
  document.getElementById('stepinfo').textContent = `Шаг ${step} из 3`;
  document.getElementById('back').hidden = step === 1;
  document.getElementById('next').textContent = step === 3 ? 'Отправить запрос' : 'Далее'; }
// Ripresa dal link nelle email: ?resume=<token> precompila i campi e salta al passo successivo.
(async () => { const tok = new URLSearchParams(location.search).get('resume'); if (!tok) return;
  try { const r = await fetch('/api/lead/' + encodeURIComponent(tok)); if (!r.ok) return; const d = await r.json();
    for (const [k, v] of Object.entries(d)) { const el = document.getElementById(k); if (el && v != null) el.value = v; }
    step = d.step || 2; show(); } catch(e){} })();
document.getElementById('back').onclick = () => { step--; show(); };
f.addEventListener('submit', e => { e.preventDefault(); const m = document.getElementById('msg'); m.textContent = '';
  if (step === 1 && !document.getElementById('email').checkValidity()) { m.textContent = 'Укажите корректный рабочий email, чтобы продолжить.'; return; }
  if (step === 3 && !document.getElementById('consent').checked) { m.textContent = 'Отметьте согласие, чтобы мы могли ответить.'; return; }
  save(step === 3);
  if (step < 3) { step++; show(); } else { f.querySelectorAll('fieldset,.cta').forEach(x => x.hidden = true); m.textContent = 'Спасибо. Ответим в течение одного рабочего дня.'; } });
</script>

<script>(function(){var sj=new URLSearchParams(location.search).get('subject');var n=document.getElementById('notes');if(sj&&n&&!n.value)n.value='['+sj.slice(0,40)+'] ';})();</script>


---
Source: https://outstaffingitaly.com/contact.html
