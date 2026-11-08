# Калькулятор зарплаты на руки в Италии

> От брутто к нетто в Италии в 2026 году: взносы сотрудника, подоходный налог IRPEF с вычетами, региональные и муниципальные надбавки для Милана, Рима, Турина, Болоньи и Неаполя.


<section> <span class="eyebrow">Калькулятор зарплаты на руки</span> <h1 id="net">Сколько сотрудник получит на руки в Италии</h1> <p class="lede">Укажите годовую зарплату брутто и город. Расчёт по правилам 2026 года: взносы сотрудника, подоходный налог с вычетами для работников по найму и местные надбавки. Полезно, когда кандидат из Алматы или Ташкента сравнивает итальянский оффер со своей текущей зарплатой.</p> </section> <section> <div class="grid"> <form class="panel" id="nf" style="display:grid;gap:16px"> <label for="g">Годовая зарплата брутто, RAL (EUR)<input id="g" type="number" min="10000" step="1000" value="50000"></label> <label for="city">Город<select id="city"><option value="Milan">Милан</option><option value="Rome">Рим</option><option value="Turin">Турин</option><option value="Bologna">Болонья</option><option value="Naples">Неаполь</option></select></label> <label for="inst">Количество выплат<select id="inst"><option value="13">13 выплат</option><option value="14">14 выплат</option><option value="12">12 (13-я/14-я равными долями)</option></select></label> </form> <div class="panel" style="min-width:0"><div class="tbl"><table id="nt"><thead><tr><th>Статья</th><th class="num">В год</th></tr></thead><tbody></tbody></table></div> <p id="nm" style="margin-bottom:0;font-weight:600"></p></div> </div>  </section> <section class="prose"> <h2 id="how">Как считается</h2> <ol> <li>Сотрудник платит 9,19% социальных взносов с брутто плюс 1% с части свыше €56 224 в год.</li> <li>С остатка берётся подоходный налог IRPEF: 23% до €28 000, 33% от €28 000 до €50 000, 43% свыше.</li> <li>Вычеты для работников по найму уменьшают налог. Они максимальны при низких зарплатах и обнуляются на €50 000. Между €20 000 и €40 000 действует дополнительный вычет до €1 000.</li> <li>Добавляются региональная и муниципальная надбавки, если доход не ниже порога освобождения в городе.</li> </ol> <p>Ничего из этого не меняет то, что платите вы как работодатель: см. <a href="calculator.html">калькулятор стоимости для работодателя</a>.</p> </section> <script>(function(){const N = {"status":"secondary","source":"data/params/2026.yaml (irpef, inps, addizionali); TUIR art. 11 and 13; L. 207/2024; L. 199/2025","employee_inps":0.0919,"extra_1pct_above":56224,"irpef":[[28000,0.23],[50000,0.33],[null,0.43]],"cities":{"Milan":{"region":"Ломбардия","regional":[[null,0.0123]],"municipal":0.008,"exempt":23000},"Rome":{"region":"Лацио","regional":[[28000,0.0173],[null,0.0333]],"municipal":0.009,"exempt":14000},"Turin":{"region":"Пьемонт","regional":[[null,0.0162]],"municipal":0.008,"municipal_note":"0.8% to 1.2% by band; 0.8% used","exempt":11790},"Bologna":{"region":"Эмилия-Романья","regional":[[null,0.0133]],"municipal":0.008,"exempt":15000},"Naples":{"region":"Кампания","regional":[[null,0.0173]],"municipal":0.01,"exempt":12000}}};

const eur = n => (n < 0 ? '−€' : '€') + Math.round(Math.abs(n)).toLocaleString('en-US');
function bands(x, b){ let t = 0, lo = 0; for (const [hi, r] of b){ const top = hi ?? Infinity; if (x > lo) t += (Math.min(x, top) - lo) * r; lo = top; } return t; }
function credit(r){ if (r <= 15000) return 1955; if (r <= 28000) return 1910 + 1190 * (28000 - r) / 13000; if (r <= 50000) return 1910 * (50000 - r) / 22000 + (r > 25000 && r <= 35000 ? 65 : 0); return 0; }
function extra(r){ if (r > 20000 && r <= 32000) return 1000; if (r > 32000 && r < 40000) return 1000 * (40000 - r) / 8000; return 0; }
function calc(){
  const g = +document.getElementById('g').value || 0, c = N.cities[document.getElementById('city').value], n = +document.getElementById('inst').value;
  const inps = g * N.employee_inps + Math.max(0, g - N.extra_1pct_above) * 0.01;
  const tx = g - inps;
  const irpef = Math.max(0, bands(tx, N.irpef) - credit(tx) - extra(tx));
  const reg = bands(tx, c.regional), mun = tx > c.exempt ? tx * c.municipal : 0;
  const net = g - inps - irpef - reg - mun;
  const rows = [['Зарплата брутто', g], ['Социальные взносы сотрудника', -inps], ['Подоходный налог (IRPEF) после вычетов', -irpef], [`Региональная надбавка (${c.region})`, -reg], ['Муниципальная надбавка', -mun]];
  document.querySelector('#nt tbody').innerHTML = rows.map(([k, v]) => `<tr><td>${k}</td><td class="num">${eur(v)}</td></tr>`).join('') + `<tr><td><b>На руки</b></td><td class="num"><b>${eur(net)}</b></td></tr>`;
  document.getElementById('nm').textContent = `Около ${eur(net / n)} на руки в каждой из ${n} выплат (${(net / g * 100).toFixed(0)}% от брутто).`;
}
document.getElementById('nf').addEventListener('input', calc); document.getElementById('nf').addEventListener('submit', e => e.preventDefault()); calc();
})();</script>


---
Source: https://outstaffingitaly.com/net-pay-calculator.html
