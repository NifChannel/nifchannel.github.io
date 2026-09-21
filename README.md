<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no">
<title>波波攒</title>
<style>
:root{
  --bg:#0a0d14;
  --panel:#141922;
  --panel2:#1b2230;
  --border:#2a3342;
  --text:#e6edf3;
  --dim:#7d8899;
  --good:#3fb950;
  --bad:#f85149;
}
*{box-sizing:border-box;margin:0;padding:0;-webkit-tap-highlight-color:transparent;}
html,body{height:100%;}
body{
  font-family:-apple-system,BlinkMacSystemFont,"PingFang SC","Hiragino Sans GB","Microsoft YaHei",sans-serif;
  background:radial-gradient(1200px 800px at 50% -10%, #1b2540 0%, #0a0d14 60%);
  color:var(--text);
  overflow:hidden;
  user-select:none;
}
.screen{display:none;height:100vh;}
.screen.active{display:flex;}

/* ============ 菜单 ============ */
#menuScreen{align-items:center;justify-content:center;padding:20px;overflow-y:auto;}
.menu-box{width:100%;max-width:430px;text-align:center;padding:10px 0;}
.menu-box h1{
  font-size:46px;letter-spacing:14px;text-indent:14px;font-weight:800;
  background:linear-gradient(180deg,#ffffff,#6ea8fe 80%);
  -webkit-background-clip:text;background-clip:text;color:transparent;
  margin-bottom:8px;
}
.sub{color:var(--dim);font-size:13px;letter-spacing:4px;margin-bottom:30px;}
.modes{display:flex;flex-direction:column;gap:12px;margin-bottom:24px;}
.mode-btn{
  display:flex;flex-direction:column;gap:5px;align-items:flex-start;
  padding:16px 20px;border-radius:14px;cursor:pointer;
  background:linear-gradient(180deg,#1b2230,#141922);
  border:1px solid var(--border);
  color:var(--text);text-align:left;
  transition:.18s;
  font-family:inherit;
}
.mode-btn:hover{border-color:#3f6fd8;transform:translateY(-2px);box-shadow:0 8px 24px rgba(63,111,216,.18);}
.mode-btn:active{transform:translateY(0);}
.mode-btn b{font-size:17px;letter-spacing:2px;}
.mode-btn span{font-size:12px;color:var(--dim);}
.mode-btn.locked{opacity:.45;cursor:not-allowed;}
.mode-btn.locked:hover{transform:none;border-color:var(--border);box-shadow:none;}

.rules{text-align:left;background:#0f141d;border:1px solid var(--border);border-radius:12px;padding:0 14px;}
.rules summary{padding:12px 0;cursor:pointer;font-size:14px;color:var(--dim);list-style:none;}
.rules summary::-webkit-details-marker{display:none;}
.rules summary::before{content:"▸ ";transition:.2s;display:inline-block;}
.rules[open] summary::before{transform:rotate(90deg);}
.rules-body{font-size:12.5px;line-height:1.9;color:#a9b4c4;padding:0 0 14px;border-top:1px solid var(--border);margin-top:-1px;padding-top:12px;}
.rules-body b{color:#e6edf3;}
.rules-body .rt{color:#6ea8fe;font-weight:700;display:block;margin-top:8px;}
.rules-body .rt:first-child{margin-top:0;}

/* ============ 游戏 ============ */
#gameScreen{
  flex-direction:column;
  max-width:860px;margin:0 auto;padding:8px 10px 10px;
  gap:8px;position:relative;
}
.topbar{display:flex;align-items:center;justify-content:space-between;flex:0 0 auto;height:36px;}
.icon-btn{
  width:34px;height:34px;border-radius:10px;border:1px solid var(--border);
  background:var(--panel);color:var(--text);font-size:16px;cursor:pointer;
  display:grid;place-items:center;font-family:inherit;transition:.15s;
}
.icon-btn:hover{background:var(--panel2);}
.round-info{font-size:13px;color:var(--dim);letter-spacing:2px;}
.timer{font-size:15px;font-weight:700;color:#6ea8fe;min-width:56px;text-align:right;font-variant-numeric:tabular-nums;}
.timer.warn{color:#f85149;animation:pulse .5s infinite;}
@keyframes pulse{50%{opacity:.4;}}

.fighter{
  background:linear-gradient(180deg,#161d29,#11161f);
  border:1px solid var(--border);border-radius:14px;
  padding:10px 12px;flex:0 0 auto;
}
.fighter.me{border-color:#2c4a7d;}
.f-row{display:flex;align-items:center;justify-content:space-between;gap:8px;margin-bottom:8px;}
.f-name{font-size:14px;font-weight:700;letter-spacing:1px;}
.f-last{font-size:12px;color:#6ea8fe;flex:1;text-align:center;min-height:16px;}
.f-hp{font-size:12px;color:var(--dim);font-variant-numeric:tabular-nums;}
.hp-bar{height:9px;background:#0a0e15;border-radius:5px;overflow:hidden;margin-bottom:9px;}
.hp-fill{height:100%;width:100%;border-radius:5px;transition:width .35s ease,background .35s ease;}
.res-row{display:flex;gap:6px;flex-wrap:wrap;}
.res-chip{
  display:inline-flex;align-items:center;gap:5px;
  padding:2px 9px 2px 3px;border-radius:999px;
  background:#0d1219;border:1px solid #232c3a;
  font-size:12px;font-variant-numeric:tabular-nums;
  transition:.2s;
}
.res-chip i{
  font-style:normal;width:18px;height:18px;border-radius:50%;
  display:grid;place-items:center;font-size:11px;font-weight:800;color:#0a0d14;
}
.res-A i{background:linear-gradient(180deg,#c3ced9,#8d99a6);}
.res-B i{background:linear-gradient(180deg,#7cc4ff,#3d8bdd);}
.res-C i{background:linear-gradient(180deg,#79e08c,#3aa84e);}
.res-D i{background:linear-gradient(180deg,#dd8bf0,#a94ec7);}
.res-chip.empty{opacity:.32;}

.log{
  flex:1 1 auto;min-height:70px;overflow-y:auto;
  background:#0b0f16;border:1px solid var(--border);border-radius:12px;
  padding:8px 10px;font-size:12.5px;line-height:1.75;
  scroll-behavior:smooth;
}
.log::-webkit-scrollbar{width:5px;}
.log::-webkit-scrollbar-thumb{background:#2a3342;border-radius:3px;}
.log-line{margin-bottom:2px;color:#9aa6b8;}
.log-line.round{color:#5b6b85;text-align:center;font-size:11px;letter-spacing:2px;margin:8px 0 6px;}
.log-line.round:first-child{margin-top:0;}
.log-line.p{color:#7fc7ff;}
.log-line.e{color:#ffb0a6;}
.log-line.good{color:#5fdb74;}
.log-line.bad{color:#ff8078;}
.log-line b{color:#fff;}

.actions{
  flex:0 0 auto;max-height:42vh;overflow-y:auto;
  display:flex;flex-direction:column;gap:8px;padding-right:2px;
}
.actions::-webkit-scrollbar{width:5px;}
.actions::-webkit-scrollbar-thumb{background:#2a3342;border-radius:3px;}
.act-group-title{font-size:11px;color:#5b6b85;letter-spacing:3px;margin:2px 0 5px;}
.act-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(150px,1fr));gap:6px;}
.act-btn{
  display:flex;flex-direction:column;gap:3px;align-items:flex-start;
  padding:8px 10px;border-radius:10px;cursor:pointer;font-family:inherit;
  background:linear-gradient(180deg,#1a2130,#141a24);
  border:1px solid #263043;color:var(--text);
  transition:.14s;text-align:left;
}
.act-btn:hover:not(.disabled){border-color:#3f6fd8;background:linear-gradient(180deg,#1f2a3d,#161d2a);transform:translateY(-1px);}
.act-btn:active:not(.disabled){transform:translateY(0);}
.act-btn.disabled{opacity:.3;cursor:not-allowed;}
.act-top{display:flex;align-items:center;justify-content:space-between;width:100%;gap:6px;}
.act-top b{font-size:13.5px;letter-spacing:1px;}
.act-top em{font-style:normal;font-size:10.5px;color:#8fa3c0;background:#0d1219;padding:2px 6px;border-radius:6px;white-space:nowrap;}
.act-top em.gain{color:#5fdb74;}
.act-desc{font-size:10.5px;color:#6b7789;line-height:1.4;}
.act-btn.kind-attack .act-top b{color:#ffb0a6;}
.act-btn.kind-combo .act-top b{color:#dd8bf0;}
.act-btn.kind-gather .act-top b{color:#a9c6e8;}

/* ============ 结算浮层 ============ */
.overlay{
  position:absolute;inset:0;background:rgba(5,8,13,.82);
  backdrop-filter:blur(6px);
  display:none;align-items:center;justify-content:center;z-index:50;border-radius:14px;
}
.overlay.show{display:flex;animation:fade .3s;}
@keyframes fade{from{opacity:0;}to{opacity:1;}}
.overlay-box{text-align:center;padding:30px 26px;}
.overlay-box h2{font-size:34px;letter-spacing:6px;margin-bottom:10px;}
.overlay-box h2.win{color:#5fdb74;text-shadow:0 0 30px rgba(95,219,116,.4);}
.overlay-box h2.lose{color:#ff8078;text-shadow:0 0 30px rgba(255,128,120,.35);}
.overlay-box h2.draw{color:#e3b341;text-shadow:0 0 30px rgba(227,179,65,.35);}
.overlay-box p{color:var(--dim);font-size:13px;margin-bottom:22px;letter-spacing:1px;}
.overlay-btns{display:flex;gap:10px;justify-content:center;}
.btn{
  padding:10px 22px;border-radius:10px;border:1px solid var(--border);
  background:var(--panel);color:var(--text);font-size:13.5px;cursor:pointer;
  font-family:inherit;letter-spacing:1px;transition:.15s;
}
.btn:hover{background:var(--panel2);}
.btn.primary{background:linear-gradient(180deg,#2f5fbd,#24499b);border-color:#3f6fd8;}
.btn.primary:hover{background:linear-gradient(180deg,#3a6ed4,#2b56b3);}

@media (max-width:420px){
  .menu-box h1{font-size:36px;letter-spacing:10px;text-indent:10px;}
  .act-grid{grid-template-columns:repeat(2,1fr);}
  .actions{max-height:38vh;}
  .fighter{padding:8px 10px;}
}
</style>
</head>
<body>

<!-- ==================== 主菜单 ==================== -->
<div id="menuScreen" class="screen active">
  <div class="menu-box">
    <h1>波波攒</h1>
    <p class="sub">钢铁 · 聚气 · 盾 · 屏</p>

    <div class="modes">
      <button class="mode-btn" data-mode="normal">
        <b>普通模式</b><span>1V1 对战 AI · 不限时</span>
      </button>
      <button class="mode-btn" data-mode="realistic">
        <b>拟真模式</b><span>1V1 对战 AI · 每回合限时 3 秒</span>
      </button>
      <button class="mode-btn locked" data-mode="boss">
        <b>Boss 战</b><span>即将开放</span>
      </button>
    </div>

    <details class="rules">
      <summary>查看规则</summary>
      <div class="rules-body">
        <span class="rt">◆ 积攒（获得资源）</span>
        <b>锻铁</b> → +1 钢铁（钢）<br>
        <b>攒气</b> → +1 聚气（气）<br>
        <b>架盾</b> → +1 盾，且本回合抵挡所有刀系攻击<br>
        <b>屏障</b> → +1 屏，且本回合抵挡所有气系攻击

        <span class="rt">◆ 基础攻击</span>
        <b>小刀</b> 钢×1 → 刀系 1 伤害<br>
        <b>能量波</b> 气×1 → 气系 1 伤害<br>
        <b>长刀</b> 钢×2 → 刀系 2 伤害<br>
        <b>冲击波</b> 气×2 → 气系 2 伤害

        <span class="rt">◆ 进阶动作</span>
        <b>防护罩</b> 盾×1 屏×1 → 本回合免疫一切攻击<br>
        <b>气刃斩</b> 钢×1 气×1 → 1 伤害，只能被防护罩防御<br>
        <b>圣盾阵</b> 钢×1 盾×1 → 抵挡刀系，并造成 1.5 刀系伤害<br>
        <b>真空波</b> 气×1 屏×1 → 抵挡气系，并造成 1.5 气系伤害<br>
        <b>降龙十八掌</b> 钢×2 气×2 盾×2 屏×2 → 4 伤害，只能被防护罩防御<br>
        <b>毁天灭地</b> 钢×3 气×3 盾×3 屏×3 → 5 伤害，无法被任何手段防御

        <span class="rt">◆ 胜负</span>
        双方 HP 上限均为 5。<br>
        若同一回合双方 HP 同时归零，视为平局。
      </div>
    </details>
  </div>
</div>

<!-- ==================== 游戏界面 ==================== -->
<div id="gameScreen" class="screen">

  <div class="topbar">
    <button id="backBtn" class="icon-btn">←</button>
    <div class="round-info" id="roundLabel">回合 1</div>
    <div class="timer" id="timerLabel"></div>
  </div>

  <!-- 对手 -->
  <div class="fighter enemy">
    <div class="f-row">
      <span class="f-name">🤖 对手</span>
      <span class="f-last" id="eLast"></span>
      <span class="f-hp" id="eHpText">HP 5 / 5</span>
    </div>
    <div class="hp-bar"><div class="hp-fill" id="eHpFill"></div></div>
    <div class="res-row" id="eRes"></div>
  </div>

  <!-- 战斗日志 -->
  <div class="log" id="log"></div>

  <!-- 玩家 -->
  <div class="fighter me">
    <div class="f-row">
      <span class="f-name">🧑 你</span>
      <span class="f-last" id="pLast"></span>
      <span class="f-hp" id="pHpText">HP 5 / 5</span>
    </div>
    <div class="hp-bar"><div class="hp-fill" id="pHpFill"></div></div>
    <div class="res-row" id="pRes"></div>
  </div>

  <!-- 动作面板 -->
  <div class="actions" id="actions"></div>

  <!-- 结算浮层 -->
  <div class="overlay" id="overlay">
    <div class="overlay-box">
      <h2 id="overlayTitle"></h2>
      <p id="overlaySub"></p>
      <div class="overlay-btns">
        <button id="againBtn" class="btn primary">再来一局</button>
        <button id="menuBtn" class="btn">返回主菜单</button>
      </div>
    </div>
  </div>

</div>

<script>
/* =========================================================
   波波攒 —— 核心数据
   ========================================================= */

const RES_META = {
  A: { name: '钢铁', short: '钢' },
  B: { name: '聚气', short: '气' },
  C: { name: '盾',   short: '盾' },
  D: { name: '屏',   short: '屏' },
};
const RES_ORDER = ['A', 'B', 'C', 'D'];

/* 所有动作定义
   kind    : gather / attack / combo
   gain    : 积攒获得的资源
   cost    : 消耗
   dmg     : 伤害值
   cat     : blade(刀系) / qi(气系) / special(只能被防护罩防) / unblockable(无法防)
   defense : 本回合的防御姿态  blade / qi / all
*/
const ACTIONS = {
  /* ---- 积攒 ---- */
  '锻铁': { kind:'gather', gain:'A', desc:'获得 1 钢铁' },
  '攒气': { kind:'gather', gain:'B', desc:'获得 1 聚气' },
  '架盾': { kind:'gather', gain:'C', defense:'blade', desc:'获得 1 盾 · 抵挡刀系' },
  '屏障': { kind:'gather', gain:'D', defense:'qi',    desc:'获得 1 屏 · 抵挡气系' },

  /* ---- 基础攻击 ---- */
  '小刀':   { kind:'attack', cost:{A:1}, dmg:1, cat:'blade', desc:'刀系 · 1 点伤害' },
  '能量波': { kind:'attack', cost:{B:1}, dmg:1, cat:'qi',    desc:'气系 · 1 点伤害' },
  '长刀':   { kind:'attack', cost:{A:2}, dmg:2, cat:'blade', desc:'刀系 · 2 点伤害' },
  '冲击波': { kind:'attack', cost:{B:2}, dmg:2, cat:'qi',    desc:'气系 · 2 点伤害' },

  /* ---- 组合进阶 ---- */
  '防护罩': { kind:'combo', cost:{C:1,D:1}, defense:'all',
              desc:'本回合免疫一切攻击' },
  '气刃斩': { kind:'attack', cost:{A:1,B:1}, dmg:1, cat:'special',
              desc:'1 点伤害 · 仅防护罩可挡' },
  '圣盾阵': { kind:'attack', cost:{A:1,C:1}, dmg:1.5, cat:'blade', defense:'blade',
              desc:'抵挡刀系 · 1.5 刀系伤害' },
  '真空波': { kind:'attack', cost:{B:1,D:1}, dmg:1.5, cat:'qi', defense:'qi',
              desc:'抵挡气系 · 1.5 气系伤害' },
  '降龙十八掌': { kind:'attack', cost:{A:2,B:2,C:2,D:2}, dmg:4, cat:'special',
              desc:'4 点伤害 · 仅防护罩可挡' },
  '毁天灭地':   { kind:'attack', cost:{A:3,B:3,C:3,D:3}, dmg:5, cat:'unblockable',
              desc:'5 点伤害 · 无法防御' },
};

const ACTION_GROUPS = [
  { title: '积 攒', list: ['锻铁','攒气','架盾','屏障'] },
  { title: '攻 击', list: ['小刀','能量波','长刀','冲击波'] },
  { title: '进 阶', list: ['防护罩','气刃斩','圣盾阵','真空波','降龙十八掌','毁天灭地'] },
];

/* 模式配置（Boss 战接口预留） */
const MODES = {
  normal:    { name:'普通模式', timeLimit: 0    },
  realistic: { name:'拟真模式', timeLimit: 3000 },
  boss:      { name:'Boss战',   timeLimit: 0, available:false },
};

/* =========================================================
   工具函数
   ========================================================= */
const $ = id => document.getElementById(id);

function canAfford(res, cost){
  if(!cost) return true;
  for(const k in cost){ if((res[k]||0) < cost[k]) return false; }
  return true;
}

function costText(cost){
  if(!cost) return '';
  return RES_ORDER.filter(k => cost[k]).map(k => RES_META[k].short + '×' + cost[k]).join(' ');
}

function fmt(n){ return Number.isInteger(n) ? String(n) : n.toFixed(1); }
function tidy(n){ return Math.max(0, Math.round(n * 100) / 100); }

/* 防御判定 */
function isBlocked(atkName, def){
  const a = ACTIONS[atkName];
  if(!a || !a.dmg) return false;
  if(a.cat === 'unblockable') return false;   // 毁天灭地无法被防
  if(!def) return false;
  if(def === 'all') return true;              // 防护罩挡一切
  if(a.cat === 'special') return false;       // 气刃斩 / 降龙十八掌 只能被防护罩挡
  if(a.cat === 'blade') return def === 'blade';
  if(a.cat === 'qi')    return def === 'qi';
  return false;
}

/* =========================================================
   游戏状态
   ========================================================= */
let state = null;

function startGame(modeKey){
  if(modeKey === 'boss'){ return; } // Boss 战暂未开放

  const cfg = MODES[modeKey] || MODES.normal;

  state = {
    mode: modeKey,
    timeLimit: cfg.timeLimit,
    round: 1,
    over: false,
    resolving: false,
    timer: null,
    deadline: 0,
    player: { hp:5, res:{A:0,B:0,C:0,D:0}, lastAction:null },
    ai:     { hp:5, res:{A:0,B:0,C:0,D:0}, lastAction:null },
  };

  $('log').innerHTML = '';
  $('overlay').classList.remove('show');
  $('menuScreen').classList.remove('active');
  $('gameScreen').classList.add('active');
  $('timerLabel').textContent = '';
  $('timerLabel').classList.remove('warn');

  addLog(`—— ${cfg.name} 开始 ——`, 'round');
  addLog('提示：双方 HP 上限为 5，同时归零则平局。');

  renderAll();
  beginTurn();
}

function quitToMenu(){
  if(state && state.timer){ clearInterval(state.timer); state.timer = null; }
  state = null;
  $('gameScreen').classList.remove('active');
  $('menuScreen').classList.add('active');
  $('overlay').classList.remove('show');
}

/* =========================================================
   回合流程
   ========================================================= */
function beginTurn(){
  if(!state || state.over) return;
  state.resolving = false;
  state.player.lastAction = null;
  state.ai.lastAction = null;
  renderAll();

  if(state.timeLimit > 0){
    startTimer(state.timeLimit);
  }
}

function startTimer(ms){
  clearTimer();
  state.deadline = Date.now() + ms;
  updateTimerDisplay(ms);
  state.timer = setInterval(() => {
    const left = state.deadline - Date.now();
    if(left <= 0){
      clearTimer();
      updateTimerDisplay(0);
      // 超时 → 自动使用「锻铁」
      if(state && !state.over && !state.resolving){
        addLog('⏱ 超时！自动使用「锻铁」', 'bad');
        submitTurn('锻铁');
      }
    }else{
      updateTimerDisplay(left);
    }
  }, 50);
}

function clearTimer(){
  if(state && state.timer){ clearInterval(state.timer); state.timer = null; }
}

function updateTimerDisplay(ms){
  const el = $('timerLabel');
  if(!el) return;
  if(state && state.timeLimit > 0 && !state.over){
    el.textContent = (ms/1000).toFixed(1) + 's';
    el.classList.toggle('warn', ms <= 1000);
  }else{
    el.textContent = '';
    el.classList.remove('warn');
  }
}

/* 玩家出招 */
function submitTurn(pAction){
  if(!state || state.over || state.resolving) return;
  if(!canAfford(state.player.res, ACTIONS[pAction].cost)) return;

  clearTimer();
  state.resolving = true;

  const aAction = aiDecide();

  state.player.lastAction = pAction;
  state.ai.lastAction = aAction;
  renderAll();

  addLog(`— 第 ${state.round} 回合 —`, 'round');
  addLog(`你使用了「${pAction}」`, 'p');

  // 稍作停顿，展示双方选招
  setTimeout(() => {
    addLog(`对手使用了「${aAction}」`, 'e');
    renderAll();

    setTimeout(() => {
      const logs = resolveTurn(pAction, aAction);
      logs.forEach(l => addLog(l.t, l.c));
      renderAll();

      // 胜负判定
      const pDead = state.player.hp <= 0;
      const aDead = state.ai.hp <= 0;
      if(pDead || aDead){
        finishGame(pDead, aDead);
        return;
      }

      state.round++;
      beginTurn();
    }, 550);
  }, 450);
}

/* 回合结算 */
function resolveTurn(pAction, aAction){
  const pA = ACTIONS[pAction];
  const aA = ACTIONS[aAction];
  const logs = [];

  // 1) 扣除消耗（基于回合开始时的资源）
  const pCost = pA.cost || {};
  const aCost = aA.cost || {};
  for(const k in pCost) state.player.res[k] -= pCost[k];
  for(const k in aCost) state.ai.res[k] -= aCost[k];

  // 2) 获得资源
  if(pA.gain) state.player.res[pA.gain] += 1;
  if(aA.gain) state.ai.res[aA.gain] += 1;

  // 3) 本回合防御姿态
  const pDef = pA.defense || null;
  const aDef = aA.defense || null;

  // 4) 结算对手的攻击 → 玩家
  if(aA.dmg){
    if(isBlocked(aAction, pDef)){
      logs.push({ t:`🛡 你的防御挡下了对手的「${aAction}」！`, c:'good' });
    }else{
      state.player.hp = tidy(state.player.hp - aA.dmg);
      logs.push({ t:`💥 对手的「${aAction}」命中你，造成 <b>${fmt(aA.dmg)}</b> 点伤害。`, c:'bad' });
    }
  }

  // 5) 结算玩家的攻击 → 对手
  if(pA.dmg){
    if(isBlocked(pAction, aDef)){
      logs.push({ t:`对手的防御挡下了你的「${pAction}」！`, c:'bad' });
    }else{
      state.ai.hp = tidy(state.ai.hp - pA.dmg);
      logs.push({ t:`⚔ 你的「${pAction}」命中对手，造成 <b>${fmt(pA.dmg)}</b> 点伤害。`, c:'good' });
    }
  }

  // 6) 无攻无防的提示
  if(!pA.dmg && !aA.dmg && pA.kind === 'gather' && aA.kind === 'gather'){
    logs.push({ t:'双方都在积攒力量……', c:'' });
  }

  return logs;
}

function finishGame(pDead, aDead){
  state.over = true;
  state.resolving = false;
  clearTimer();
  updateTimerDisplay(0);

  let title, sub, cls;
  if(pDead && aDead){
    title = '平 局'; sub = '同归于尽，双双倒下'; cls = 'draw';
    addLog('◆ 双方 HP 同时归零 —— 平局！', 'round');
  }else if(aDead){
    title = '胜 利'; sub = '你击败了对手'; cls = 'win';
    addLog('◆ 你击败了对手！', 'round');
  }else{
    title = '败 北'; sub = '你被对手击败了'; cls = 'lose';
    addLog('◆ 你被对手击败了……', 'round');
  }

  renderAll();

  $('overlayTitle').textContent = title;
  $('overlayTitle').className = cls;
  $('overlaySub').textContent = sub;
  $('overlay').classList.add('show');
}

/* =========================================================
   AI（普通难度）
   —— 预留：后续可替换为「提示词驱动的 AI 行动模式」
   ========================================================= */
function aiDecide(){
  // 预留接口：如果外部注册了自定义 AI，则优先使用
  if(typeof window.__customAIDecide === 'function'){
    try{
      const r = window.__customAIDecide(state);
      if(r && ACTIONS[r]) return r;
    }catch(e){ /* 忽略，回落到内置 AI */ }
  }
  return builtinAIDecide();
}

function builtinAIDecide(){
  const me = state.ai;
  const r  = me.res;
  const foeHp = state.player.hp;
  const myHp  = me.hp;
  const cands = [];

  const add = (name, w) => {
    if(canAfford(r, ACTIONS[name].cost)) cands.push([name, w]);
  };

  /* ---- 1. 斩杀优先 ---- */
  if(foeHp <= 1){
    add('气刃斩', 55);
    add('小刀',   45);
    add('能量波', 45);
    add('降龙十八掌', 60);
  }else if(foeHp <= 2){
    add('长刀',   38);
    add('冲击波', 38);
    add('气刃斩', 22);
    add('降龙十八掌', 30);
  }else if(foeHp <= 4){
    add('降龙十八掌', 12);
  }

  /* ---- 2. 残血自保 ---- */
  if(myHp <= 1){
    add('防护罩', 40);
    add('架盾',   28);
    add('屏障',   28);
  }

  /* ---- 3. 常规进攻 ---- */
  add('气刃斩', 12);
  add('长刀',   10);
  add('冲击波', 10);
  add('小刀',    9);
  add('能量波',  9);
  add('圣盾阵',  7);
  add('真空波',  7);

  /* ---- 4. 防守 ---- */
  add('防护罩',  5);

  /* ---- 5. 无招可用 → 积攒 ---- */
  if(cands.length === 0) return aiGather();

  return weightedPick(cands);
}

function aiGather(){
  const r = state.ai.res;
  const opts = [
    { a:'锻铁', v: (2.5 - r.A) + Math.random() * 1.6 },
    { a:'攒气', v: (2.5 - r.B) + Math.random() * 1.6 },
    { a:'架盾', v: (1.5 - r.C) + Math.random() * 1.6 },
    { a:'屏障', v: (1.5 - r.D) + Math.random() * 1.6 },
  ];
  opts.sort((x, y) => y.v - x.v);
  return opts[0].a;
}

function weightedPick(pairs){
  let total = 0;
  for(const p of pairs) total += p[1];
  let x = Math.random() * total;
  for(const p of pairs){
    x -= p[1];
    if(x <= 0) return p[0];
  }
  return pairs[pairs.length - 1][0];
}

/* =========================================================
   渲染
   ========================================================= */
function renderAll(){
  if(!state) return;
  renderFighter('player', 'p');
  renderFighter('ai', 'e');
  renderActions();
  $('roundLabel').textContent = `回合 ${state.round}`;
}

function renderFighter(side, prefix){
  const f = state[side];

  // HP
  $[prefix + 'HpText'] && 0;
  $(prefix + 'HpText').textContent = `HP ${fmt(f.hp)} / 5`;
  const pct = Math.max(0, Math.min(100, f.hp / 5 * 100));
  const fill = $(prefix + 'HpFill');
  fill.style.width = pct + '%';
  fill.style.background = pct > 60
    ? 'linear-gradient(90deg,#3fb950,#56d364)'
    : pct > 30
      ? 'linear-gradient(90deg,#d29922,#e3b341)'
      : 'linear-gradient(90deg,#f85149,#ff7b72)';

  // 上回合出招
  $(prefix + 'Last').textContent = f.lastAction ? `「${f.lastAction}」` : '';

  // 资源
  const resBox = $(prefix + 'Res');
  resBox.innerHTML = '';
  RES_ORDER.forEach(k => {
    const chip = document.createElement('div');
    chip.className = 'res-chip res-' + k + (f.res[k] > 0 ? '' : ' empty');
    chip.innerHTML = `<i>${RES_META[k].short}</i><span>${f.res[k]}</span>`;
    chip.title = RES_META[k].name;
    resBox.appendChild(chip);
  });
}

function renderActions(){
  const box = $('actions');
  box.innerHTML = '';

  ACTION_GROUPS.forEach(g => {
    const wrap = document.createElement('div');
    wrap.className = 'act-group';

    const title = document.createElement('div');
    title.className = 'act-group-title';
    title.textContent = g.title;
    wrap.appendChild(title);

    const grid = document.createElement('div');
    grid.className = 'act-grid';

    g.list.forEach(name => {
      const a  = ACTIONS[name];
      const ok = canAfford(state.player.res, a.cost) && !state.resolving && !state.over;

      const btn = document.createElement('button');
      btn.className = 'act-btn kind-' + a.kind + (ok ? '' : ' disabled');
      btn.disabled = !ok;

      const costHtml = a.cost
        ? `<em>${costText(a.cost)}</em>`
        : `<em class="gain">+${RES_META[a.gain].short}</em>`;

      btn.innerHTML = `
        <span class="act-top"><b>${name}</b>${costHtml}</span>
        <span class="act-desc">${a.desc}</span>
      `;

      if(ok){
        btn.addEventListener('click', () => submitTurn(name));
      }
      grid.appendChild(btn);
    });

    wrap.appendChild(grid);
    box.appendChild(wrap);
  });
}

function addLog(text, cls){
  const el = $('log');
  const div = document.createElement('div');
  div.className = 'log-line ' + (cls || '');
  div.innerHTML = text;
  el.appendChild(div);
  el.scrollTop = el.scrollHeight;
}

/* =========================================================
   事件绑定
   ========================================================= */
document.querySelectorAll('.mode-btn').forEach(btn => {
  btn.addEventListener('click', () => {
    const mode = btn.dataset.mode;
    if(mode === 'boss'){
      alert('Boss 战模式即将开放，敬请期待！');
      return;
    }
    startGame(mode);
  });
});

$('backBtn').addEventListener('click', quitToMenu);
$('menuBtn').addEventListener('click', quitToMenu);
$('againBtn').addEventListener('click', () => {
  const m = state ? state.mode : 'normal';
  startGame(m);
});

/* =========================================================
   Boss 战 / 自定义 AI 接口（预留）
   ---------------------------------------------------------
   将来接入 Boss 战时，可以这样扩展：

   window.startBossBattle = function(bossConfig){
     // bossConfig: { name, maxHp, phases:[...], decideFn }
     // 复用 startGame 的状态机，把 state.ai 替换为 Boss 实例即可
   };

   自定义 AI 只需注册：
   window.__customAIDecide = function(state){
     // 返回动作名，例如 '长刀'
   };
   ========================================================= */
window.BOBOZAN = {
  ACTIONS,
  MODES,
  startGame,
  getState: () => state,
  setCustomAI: fn => { window.__customAIDecide = fn; },
  clearCustomAI: () => { delete window.__customAIDecide; },
};
</script>
</body>
</html>
