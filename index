<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>WoW Campaign Comparator</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.4.1/papaparse.min.js"></script>
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
    :root {
      --t1: #111; --t2: #6b7280; --bg1: #ffffff; --bg2: #f3f4f6;
      --br: rgba(0,0,0,.08); --s: #16a34a; --d: #dc2626;
      --m: ui-monospace,'SF Mono',Menlo,monospace;
      --r-md: 8px; --r-lg: 12px;
    }
    @media (prefers-color-scheme: dark) {
      :root { --t1: #f3f4f6; --t2: #9ca3af; --bg1: #111827; --bg2: #1f2937; --br: rgba(255,255,255,.08); }
    }
    body { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif; background: var(--bg1); color: var(--t1); min-height: 100vh; padding: 2rem 1.5rem; }
    .container { max-width: 900px; margin: 0 auto; }
    h1 { font-size: 18px; font-weight: 500; margin-bottom: 4px; }
    .sub { font-size: 12px; color: var(--t2); margin-bottom: 1.5rem; }
    .g2 { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
    .g3 { display: grid; grid-template-columns: repeat(3, minmax(0,1fr)); gap: 10px; }
    .dz { border: 1.5px dashed #d1d5db; border-radius: var(--r-lg); padding: 2rem 1.5rem; text-align: center; cursor: pointer; background: var(--bg2); user-select: none; transition: background .12s; }
    .dz:hover { background: #eff6ff; border-color: #93c5fd; }
    .dz.ok { border-style: solid; border-color: #86efac; background: #f0fdf4; }
    .slbl { font-size: 10px; font-weight: 500; color: var(--t2); text-transform: uppercase; letter-spacing: .06em; margin-bottom: 8px; }
    .chg-card { background: var(--bg2); border-radius: var(--r-lg); padding: 18px; }
    .chg-lbl { font-size: 10px; font-weight: 500; color: var(--t2); text-transform: uppercase; letter-spacing: .06em; margin-bottom: 14px; }
    .chg-row { display: flex; justify-content: space-between; align-items: center; padding: 9px 0; border-bottom: 0.5px solid var(--br); }
    .chg-row:last-child { border: none; }
    .chg-name { font-size: 12px; color: var(--t1); }
    .chg-right { text-align: right; }
    .big { font-size: 20px; font-weight: 500; font-family: var(--m); }
    .small { font-size: 11px; font-family: var(--m); margin-top: 2px; }
    .pos { color: var(--s); } .neg { color: var(--d); } .neu { color: var(--t2); }
    .metric-card { background: var(--bg2); border-radius: var(--r-lg); padding: 16px; }
    .mc-lbl { font-size: 10px; font-weight: 500; color: var(--t2); text-transform: uppercase; letter-spacing: .06em; margin-bottom: 10px; display: flex; align-items: center; gap: 6px; }
    .mc-val { font-size: 28px; font-weight: 500; font-family: var(--m); color: var(--t1); line-height: 1; }
    .mc-prior { font-size: 12px; font-family: var(--m); margin-top: 6px; }
    .seg-badge { display: inline-block; font-size: 9px; font-weight: 600; padding: 2px 7px; border-radius: 10px; }
    .badge-b { background: #e0e7ff; color: #3730a3; } .badge-nb { background: #fce7f3; color: #9d174d; } .badge-all { background: rgba(0,0,0,.06); color: #6b7280; }
    .tab-row { display: flex; gap: 6px; margin-bottom: 10px; flex-wrap: wrap; }
    .stab { font-size: 11px; padding: 4px 12px; border: 0.5px solid var(--br); border-radius: 20px; background: transparent; cursor: pointer; color: var(--t2); }
    .stab:hover { color: var(--t1); } .stab.on { background: var(--t1); color: var(--bg1); border-color: transparent; }
    .view-toggle { display: flex; gap: 6px; margin-bottom: 16px; }
    .vtab { font-size: 12px; font-weight: 500; padding: 6px 16px; border: 0.5px solid var(--br); border-radius: 20px; background: transparent; cursor: pointer; color: var(--t2); }
    .vtab.on { background: var(--t1); color: var(--bg1); border-color: transparent; }
    .camp-tbl { width: 100%; border-collapse: collapse; font-size: 11px; }
    .camp-tbl th { padding: 7px 10px; border-bottom: 1px solid var(--br); color: var(--t2); font-weight: 500; font-size: 9px; text-transform: uppercase; letter-spacing: .04em; text-align: left; white-space: nowrap; }
    .camp-tbl th.num { text-align: right; }
    .camp-tbl td { padding: 7px 10px; border-bottom: 0.5px solid var(--br); vertical-align: middle; color: var(--t1); }
    .camp-tbl td.num { text-align: right; font-family: var(--m); font-size: 11px; }
    .camp-tbl tr:last-child td { border-bottom: none; }
    .camp-tbl tr:hover td { background: var(--bg2); }
    .cname { max-width: 220px; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; font-size: 11px; }
    .delta-pill { display: inline-block; font-size: 10px; font-family: var(--m); padding: 2px 6px; border-radius: 6px; }
    .pill-pos { background: #f0fdf4; color: var(--s); } .pill-neg { background: #fef2f2; color: var(--d); } .pill-neu { background: var(--bg2); color: var(--t2); }
    .tbl-wrap { overflow-x: auto; border-radius: var(--r-lg); border: 0.5px solid var(--br); }
    .tbl-note { font-size: 10px; color: var(--t2); margin-top: 6px; text-align: right; }
    .seg-toggle { display: inline-flex; align-items: center; gap: 4px; cursor: pointer; user-select: none; padding: 2px 6px; border-radius: 8px; border: 0.5px solid var(--br); font-size: 9px; font-weight: 600; text-transform: uppercase; letter-spacing: .04em; transition: all .1s; white-space: nowrap; }
    .seg-toggle:hover { border-color: #d1d5db; }
    .seg-toggle.st-all { background: transparent; color: var(--t2); }
    .seg-toggle.st-b { background: #e0e7ff; color: #3730a3; border-color: #c7d2fe; }
    .seg-toggle.st-nb { background: #fce7f3; color: #9d174d; border-color: #fbcfe8; }
    .pf-group { margin-bottom: 20px; }
    .pf-group-lbl { font-size: 11px; font-weight: 500; color: var(--t1); margin-bottom: 8px; padding-bottom: 5px; border-bottom: 0.5px solid var(--br); }
    .pf-block { border: 0.5px solid var(--br); border-radius: var(--r-lg); margin-bottom: 8px; overflow: hidden; }
    .pf-block.is-b { border-left: 2px solid #c7d2fe; }
    .pf-header { display: flex; justify-content: space-between; align-items: center; padding: 11px 14px; cursor: pointer; background: var(--bg2); user-select: none; gap: 8px; }
    .pf-header:hover { filter: brightness(.97); }
    .pf-name-row { display: flex; align-items: center; gap: 8px; min-width: 0; }
    .pf-name { font-size: 12px; font-weight: 500; color: var(--t1); white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
    .pf-summary { display: flex; gap: 12px; font-size: 11px; font-family: var(--m); color: var(--t2); align-items: center; flex-wrap: wrap; }
    .pf-chevron { font-size: 10px; color: var(--t2); transition: transform .15s; flex-shrink: 0; }
    .pf-chevron.open { transform: rotate(180deg); }
    .pf-body { display: none; padding: 0 14px 12px; }
    .pf-body.open { display: block; }
    .pf-stats { display: grid; grid-template-columns: repeat(4, minmax(0,1fr)); gap: 8px; margin: 10px 0; }
    .pf-stat { background: var(--bg1); border-radius: var(--r-md); padding: 10px 12px; border: 0.5px solid var(--br); }
    .pf-stat-lbl { font-size: 9px; text-transform: uppercase; letter-spacing: .05em; color: var(--t2); margin-bottom: 4px; }
    .pf-stat-val { font-size: 15px; font-weight: 500; font-family: var(--m); color: var(--t1); }
    .pf-stat-prior { font-size: 10px; font-family: var(--m); margin-top: 3px; }
    .warn { background: #fffbeb; border: 0.5px solid #fcd34d; border-radius: var(--r-md); padding: 10px 14px; font-size: 12px; color: #78350f; margin-top: 10px; }
    .err  { background: #fef2f2; border: 0.5px solid #fca5a5; border-radius: var(--r-md); padding: 10px 14px; font-size: 12px; color: #7f1d1d; margin-top: 10px; }
    .rst-btn { font-size: 11px; padding: 3px 10px; border: 0.5px solid var(--br); border-radius: 20px; background: transparent; cursor: pointer; color: var(--t2); }
    .mb-12 { margin-bottom: 12px; } .mb-16 { margin-bottom: 16px; } .mb-24 { margin-bottom: 24px; } .mt-24 { margin-top: 24px; }
    @media (max-width: 580px) { .g3 { grid-template-columns: repeat(2,1fr); } .pf-stats { grid-template-columns: repeat(2,1fr); } .g2 { grid-template-columns: 1fr; } }
  </style>
</head>
<body>
<div class="container">

  <div id="upload-view">
    <h1>WoW Campaign Comparator</h1>
    <p class="sub">Upload prior week first, then current week. Supports Amazon campaign performance CSV exports.</p>
    <div class="g2">
      <div class="dz" id="dz-prior" onclick="document.getElementById('f-prior').click()" ondragover="event.preventDefault()" ondrop="event.preventDefault();load('prior',event.dataTransfer.files[0])">
        <input type="file" id="f-prior" accept=".csv" style="display:none" onchange="load('prior',this.files[0])">
        <div style="font-size:22px;margin-bottom:8px">📅</div>
        <div style="font-size:13px;font-weight:500;margin-bottom:3px" id="lbl-prior">Prior week</div>
        <div style="font-size:11px;color:var(--t2)">Drop CSV or click to browse</div>
      </div>
      <div class="dz" id="dz-cur" onclick="document.getElementById('f-cur').click()" ondragover="event.preventDefault()" ondrop="event.preventDefault();load('cur',event.dataTransfer.files[0])">
        <input type="file" id="f-cur" accept=".csv" style="display:none" onchange="load('cur',this.files[0])">
        <div style="font-size:22px;margin-bottom:8px">📊</div>
        <div style="font-size:13px;font-weight:500;margin-bottom:3px" id="lbl-cur">Current week</div>
        <div style="font-size:11px;color:var(--t2)">Drop CSV or click to browse</div>
      </div>
    </div>
  </div>

  <div id="results-view" style="display:none;padding-bottom:3rem">
    <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:.5rem">
      <h1>Week-over-week summary</h1>
      <button class="rst-btn" onclick="reset()">↺ Reset</button>
    </div>
    <p class="sub" id="results-sub"></p>

    <div class="slbl mt-24">Revenue & spend</div>
    <div class="chg-card mb-12">
      <div class="chg-lbl">Prior → current</div>
      <div class="chg-row"><span class="chg-name">Ad Revenue</span><div class="chg-right"><div class="big" id="rev-pct">—</div><div class="small" id="rev-abs"></div></div></div>
      <div class="chg-row"><span class="chg-name">Ad Spend</span><div class="chg-right"><div class="big" id="spend-pct">—</div><div class="small" id="spend-abs"></div></div></div>
    </div>

    <div class="slbl">Conversion rate — orders ÷ clicks</div>
    <div class="g3 mb-12">
      <div class="metric-card"><div class="mc-lbl">Overall <span class="seg-badge badge-all">all</span></div><div class="mc-val" id="all-cvr-cur">—</div><div class="mc-prior" id="all-cvr-prior"></div></div>
      <div class="metric-card"><div class="mc-lbl">Branded <span class="seg-badge badge-b">| b |</span></div><div class="mc-val" id="b-cvr-cur">—</div><div class="mc-prior" id="b-cvr-prior"></div></div>
      <div class="metric-card"><div class="mc-lbl">Non-branded <span class="seg-badge badge-nb">| nb |</span></div><div class="mc-val" id="nb-cvr-cur">—</div><div class="mc-prior" id="nb-cvr-prior"></div></div>
    </div>

    <div class="slbl">Volume</div>
    <div class="g3 mb-12">
      <div class="metric-card"><div class="mc-lbl">Impressions</div><div class="mc-val" id="impr-cur">—</div><div class="mc-prior" id="impr-prior"></div></div>
      <div class="metric-card"><div class="mc-lbl">Clicks</div><div class="mc-val" id="clicks-cur">—</div><div class="mc-prior" id="clicks-prior"></div></div>
      <div class="metric-card"><div class="mc-lbl">Orders</div><div class="mc-val" id="orders-cur">—</div><div class="mc-prior" id="orders-prior"></div></div>
    </div>

    <div class="slbl">CPCs</div>
    <div class="g2 mb-24">
      <div class="metric-card"><div class="mc-lbl">Branded CPC <span class="seg-badge badge-b">| b |</span></div><div class="mc-val" id="b-cpc-cur">—</div><div class="mc-prior" id="b-cpc-prior"></div></div>
      <div class="metric-card"><div class="mc-lbl">NB CPC <span class="seg-badge badge-nb">| nb |</span></div><div class="mc-val" id="nb-cpc-cur">—</div><div class="mc-prior" id="nb-cpc-prior"></div></div>
    </div>

    <div class="slbl">Campaigns</div>
    <div class="view-toggle">
      <button class="vtab on" id="view-campaign" onclick="setView('campaign')">By campaign</button>
      <button class="vtab" id="view-portfolio" onclick="setView('portfolio')">By portfolio</button>
    </div>

    <div id="campaign-view">
      <div class="tab-row">
        <button class="stab on" id="tab-spend" onclick="setSort('spend')">By spend</button>
        <button class="stab" id="tab-rev" onclick="setSort('rev')">By revenue</button>
        <button class="stab" id="tab-cvr" onclick="setSort('cvr')">By CVR</button>
      </div>
      <div class="tbl-wrap">
        <table class="camp-tbl">
          <thead><tr>
            <th>Campaign</th>
            <th><span class="seg-toggle st-all" id="seg-toggle" onclick="cycleSeg()" title="Filter by segment"><span id="seg-label">Seg</span> <span style="opacity:.5">⇅</span></span></th>
            <th class="num">Revenue</th><th class="num">Rev Δ</th>
            <th class="num">Spend</th><th class="num">Spend Δ</th>
            <th class="num">CVR</th><th class="num">CVR Δ</th>
          </tr></thead>
          <tbody id="camp-body"></tbody>
        </table>
      </div>
      <div class="tbl-note" id="tbl-note"></div>
    </div>

    <div id="portfolio-view" style="display:none">
      <div class="tab-row">
        <button class="stab on" id="ptab-spend" onclick="setPfSort('spend')">By spend</button>
        <button class="stab" id="ptab-rev" onclick="setPfSort('rev')">By revenue</button>
        <button class="stab" id="ptab-cvr" onclick="setPfSort('cvr')">By CVR</button>
      </div>
      <div class="pf-group">
        <div class="pf-group-lbl">Non-branded <span class="seg-badge badge-nb" style="vertical-align:middle">| nb |</span></div>
        <div id="pf-list-nb"></div>
      </div>
      <div class="pf-group">
        <div class="pf-group-lbl">Branded <span class="seg-badge badge-b" style="vertical-align:middle">| b |</span></div>
        <div id="pf-list-b"></div>
      </div>
      <div class="pf-group" id="pf-group-other" style="display:none">
        <div class="pf-group-lbl">Untagged</div>
        <div id="pf-list-other"></div>
      </div>
    </div>

    <div id="warn-area"></div>
  </div>
</div>

<script>
const parsed={cur:null,prior:null};
let allCampaigns=[],portfoliosNB=[],brandedPortfolio=null,portfoliosOther=[];
let sortKey='spend',segFilter='all',pfSortKey='spend',pfIdx=0;

const SEG_CYCLE=['all','b','nb'],SEG_LABELS={all:'Seg',b:'B',nb:'NB'},SEG_CLS={all:'st-all',b:'st-b',nb:'st-nb'};
function cycleSeg(){
  segFilter=SEG_CYCLE[(SEG_CYCLE.indexOf(segFilter)+1)%SEG_CYCLE.length];
  const t=document.getElementById('seg-toggle');
  t.className='seg-toggle '+SEG_CLS[segFilter];
  document.getElementById('seg-label').textContent=SEG_LABELS[segFilter];
  renderTable();
}

function findCol(h,kws,exc){
  const hl=h.map(x=>(x||'').toLowerCase().trim());
  const i=hl.findIndex(hh=>{if((exc||[]).some(e=>hh.includes(e)))return false;return kws.some(k=>hh.includes(k));});
  return i>=0?h[i]:null;
}
function pn(v){if(!v&&v!==0)return 0;const n=parseFloat(String(v).replace(/[$,%\s]/g,''));return isNaN(n)?0:n;}
const isB=n=>(n||'').toLowerCase().includes('| b |');
const isNB=n=>(n||'').toLowerCase().includes('| nb |');
const segLbl=n=>isB(n)?'b':isNB(n)?'nb':null;
function getPf(name){const p=(name||'').split('|');return p.length>=2?p[1].trim():'Other';}

function extract(res){
  const h=res.meta.fields||[],rows=res.data;
  const nc=findCol(h,['campaign name'],[]);
  const sc=findCol(h,['total cost (converted)','total cost','spend','cost'],['per click','cpc','acos','of sale']);
  const rc=findCol(h,['sales (converted)','7 day total sales','14 day total sales','total sales','attributed sales','revenue','sales'],['(#)','unit','units','new-to-brand','new to brand','percentage','acos']);
  const cc=findCol(h,['clicks'],[]);
  const oc=findCol(h,['purchases','7 day total orders (#)','14 day total orders (#)','7 day total orders','14 day total orders','total orders (#)','total orders','orders (#)','orders','units ordered'],['new-to-brand','new to brand']);
  const ic=findCol(h,['impressions'],[]);
  const seg={all:{cl:0,or:0},b:{cl:0,or:0,sp:0},nb:{cl:0,or:0,sp:0}};
  let tSp=0,tRv=0,tCl=0,tOr=0,tIm=0;
  const by={};
  for(const r of rows){
    const nm=nc?(r[nc]||'').trim():'';if(!nm)continue;
    const cl=pn(r[cc]),or=pn(r[oc]),sp=pn(r[sc]),rv=pn(r[rc]),im=pn(r[ic]);
    tSp+=sp;tRv+=rv;tCl+=cl;tOr+=or;tIm+=im;
    seg.all.cl+=cl;seg.all.or+=or;
    if(isB(nm)){seg.b.cl+=cl;seg.b.or+=or;seg.b.sp+=sp;}
    if(isNB(nm)){seg.nb.cl+=cl;seg.nb.or+=or;seg.nb.sp+=sp;}
    if(!by[nm])by[nm]={name:nm,cl:0,or:0,sp:0,rv:0};
    by[nm].cl+=cl;by[nm].or+=or;by[nm].sp+=sp;by[nm].rv+=rv;
  }
  for(const c of Object.values(by)){c.cvr=c.cl>0?(c.or/c.cl)*100:null;c.cpc=c.cl>0?c.sp/c.cl:null;c.pf=getPf(c.name);}
  const cvr=s=>s.cl>0?(s.or/s.cl)*100:null,cpc=s=>s.cl>0?s.sp/s.cl:null;
  const ns=rows.map(r=>nc?(r[nc]||''):'');
  return{sp:tSp,rv:tRv,cl:tCl,or:tOr,im:tIm,sc,rc,cc,oc,ic,
    all:{...seg.all,cvr:cvr(seg.all)},
    b:{...seg.b,cvr:cvr(seg.b),cpc:cpc(seg.b),cnt:ns.filter(isB).length},
    nb:{...seg.nb,cvr:cvr(seg.nb),cpc:cpc(seg.nb),cnt:ns.filter(isNB).length},by};
}

function joinC(cur,pr){
  const all=new Set([...Object.keys(cur.by),...Object.keys(pr.by)]);
  return Array.from(all).map(n=>({name:n,seg:segLbl(n),pf:getPf(n),cur:cur.by[n]||null,prior:pr.by[n]||null}))
    .filter(c=>c.cur&&(c.cur.sp>0||c.cur.cl>0));
}
function buildNB(cs){
  const m={};
  for(const c of cs){const k=c.pf||'Other';if(!m[k])m[k]={name:k,cur:{cl:0,or:0,sp:0,rv:0},prior:{cl:0,or:0,sp:0,rv:0},campaigns:[]};
    m[k].campaigns.push(c);
    if(c.cur){m[k].cur.cl+=c.cur.cl;m[k].cur.or+=c.cur.or;m[k].cur.sp+=c.cur.sp;m[k].cur.rv+=c.cur.rv;}
    if(c.prior){m[k].prior.cl+=c.prior.cl;m[k].prior.or+=c.prior.or;m[k].prior.sp+=c.prior.sp;m[k].prior.rv+=c.prior.rv;}
  }
  for(const p of Object.values(m)){p.cur.cvr=p.cur.cl>0?(p.cur.or/p.cur.cl)*100:null;p.prior.cvr=p.prior.cl>0?(p.prior.or/p.prior.cl)*100:null;}
  return Object.values(m);
}
function buildB(cs){
  const p={name:'Branded',cur:{cl:0,or:0,sp:0,rv:0},prior:{cl:0,or:0,sp:0,rv:0},campaigns:cs};
  for(const c of cs){if(c.cur){p.cur.cl+=c.cur.cl;p.cur.or+=c.cur.or;p.cur.sp+=c.cur.sp;p.cur.rv+=c.cur.rv;}
    if(c.prior){p.prior.cl+=c.prior.cl;p.prior.or+=c.prior.or;p.prior.sp+=c.prior.sp;p.prior.rv+=c.prior.rv;}}
  p.cur.cvr=p.cur.cl>0?(p.cur.or/p.cur.cl)*100:null;
  p.prior.cvr=p.prior.cl>0?(p.prior.or/p.prior.cl)*100:null;
  return p;
}

const f$=v=>'$'+v.toLocaleString('en-US',{minimumFractionDigits:2,maximumFractionDigits:2});
const fP=v=>v.toFixed(2)+'%',fI=v=>Math.round(v).toLocaleString('en-US');
const fPct=v=>(v>=0?'+':'')+v.toFixed(1)+'%',fPp=v=>(v>=0?'+':'')+v.toFixed(2)+' pp';
function pd(c,p){if(c===null||p===null)return null;if(p===0)return c>0?Infinity:0;return(c-p)/Math.abs(p)*100;}
function cd(c,p){if(!c||!p)return null;return(c.cvr??0)-(p.cvr??0);}
function pill(v,fn,gd){
  if(v===null)return'<span class="delta-pill pill-neu">—</span>';
  const g=gd===1?v>0:gd===-1?v<0:null,cls=g===null?'pill-neu':g?'pill-pos':'pill-neg';
  return`<span class="delta-pill ${cls}">${!isFinite(v)?(v>0?'New':'—'):fn(v)}</span>`;
}

function rChange(pi,ai,cv,pv,fn,gd){
  const pe=document.getElementById(pi),ae=document.getElementById(ai);
  if(cv===null||pv===null){pe.textContent='n/a';pe.className='big neu';ae.textContent='no data';ae.className='small neu';return;}
  const d=cv-pv,p=pv!==0?(d/Math.abs(pv))*100:Infinity;
  pe.textContent=isFinite(p)?fPct(p):(d>0?'New':'—');ae.textContent=`${fn(pv)} → ${fn(cv)}`;
  const g=gd===1?d>0:gd===-1?d<0:null;pe.className='big '+(g===null?'neu':g?'pos':'neg');ae.className='small '+(g===null?'neu':g?'pos':'neg');
}
function rCard(ci,pi,cv,pv,fn,gd){
  const ce=document.getElementById(ci),pe=document.getElementById(pi);
  if(cv===null){ce.textContent='n/a';ce.className='mc-val neu';pe.textContent='';return;}
  ce.textContent=fn(cv);ce.className='mc-val';
  if(pv===null){pe.textContent='no prior data';pe.className='mc-prior neu';return;}
  const d=cv-pv,a=d>0?'↑':d<0?'↓':'→',g=gd===1?d>0:gd===-1?d<0:null;
  pe.textContent=`${a} ${fn(pv)} prior week`;pe.className=`mc-prior ${g===null?'neu':g?'pos':'neg'}`;
}

function renderTable(){
  let cs=[...allCampaigns];
  if(segFilter==='b')cs=cs.filter(c=>c.seg==='b');
  if(segFilter==='nb')cs=cs.filter(c=>c.seg==='nb');
  cs.sort((a,b)=>sortKey==='rev'?(b.cur?.rv||0)-(a.cur?.rv||0):sortKey==='spend'?(b.cur?.sp||0)-(a.cur?.sp||0):(b.cur?.cvr||0)-(a.cur?.cvr||0));
  document.getElementById('camp-body').innerHTML=cs.map(c=>{
    const rv=pd(c.cur?.rv,c.prior?.rv),sp=pd(c.cur?.sp,c.prior?.sp),cv=cd(c.cur,c.prior);
    const sb=c.seg?`<span class="seg-badge badge-${c.seg}">${c.seg==='b'?'B':'NB'}</span>`:'<span style="color:var(--t2);font-size:10px">—</span>';
    return`<tr><td><div class="cname" title="${c.name}">${c.name}</div></td><td>${sb}</td>
      <td class="num">${c.cur?f$(c.cur.rv):'—'}</td><td class="num">${pill(rv,fPct,1)}</td>
      <td class="num">${c.cur?f$(c.cur.sp):'—'}</td><td class="num">${pill(sp,fPct,0)}</td>
      <td class="num">${c.cur?.cvr!=null?fP(c.cur.cvr):'—'}</td>
      <td class="num">${cv!==null?pill(cv,fPp,1):'<span class="delta-pill pill-neu">—</span>'}</td></tr>`;
  }).join('')||'<tr><td colspan="8" style="text-align:center;padding:1.5rem;color:var(--t2)">No campaigns match this filter.</td></tr>';
  const lbl=sortKey==='rev'?'revenue':sortKey==='spend'?'spend':'CVR';
  const sg=segFilter==='b'?'branded only':segFilter==='nb'?'non-branded only':'all segments';
  document.getElementById('tbl-note').textContent=`${cs.length} active campaigns · ${sg} · sorted by ${lbl}`;
}

function pfBlock(pf,isB2){
  const i=pfIdx++,c=pf.cur,p=pf.prior;
  const rd=pd(c.rv,p.rv),sd=pd(c.sp,p.sp),cd2=c.cvr!==null&&p.cvr!==null?c.cvr-p.cvr:null;
  const pl=(cv,pv,fn,d)=>{if(cv===null||pv===null)return'';const diff=cv-pv,a=diff>0?'↑':diff<0?'↓':'→',cl=d===1?(diff>0?'pos':'neg'):(diff<0?'pos':'neg');return`<div class="pf-stat-prior ${cl}">${a} ${fn(pv)} prior</div>`;};
  const cr=pf.campaigns.sort((a,b)=>(b.cur?.sp||0)-(a.cur?.sp||0)).map(c2=>{
    const r=pd(c2.cur?.rv,c2.prior?.rv),s=pd(c2.cur?.sp,c2.prior?.sp),cv=cd(c2.cur,c2.prior);
    const sb=c2.seg?`<span class="seg-badge badge-${c2.seg}">${c2.seg==='b'?'B':'NB'}</span>`:'<span style="color:var(--t2);font-size:10px">—</span>';
    return`<tr><td><div class="cname" title="${c2.name}">${c2.name}</div></td><td>${sb}</td>
      <td class="num">${c2.cur?f$(c2.cur.rv):'—'}</td><td class="num">${pill(r,fPct,1)}</td>
      <td class="num">${c2.cur?f$(c2.cur.sp):'—'}</td><td class="num">${pill(s,fPct,0)}</td>
      <td class="num">${c2.cur?.cvr!=null?fP(c2.cur.cvr):'—'}</td>
      <td class="num">${cv!==null?pill(cv,fPp,1):'<span class="delta-pill pill-neu">—</span>'}</td></tr>`;
  }).join('');
  return`<div class="pf-block${isB2?' is-b':''}">
    <div class="pf-header" onclick="togglePf('pf-${i}')">
      <div class="pf-name-row"><span class="pf-name">${pf.name}</span><span style="font-size:10px;color:var(--t2)">${pf.campaigns.length} campaign${pf.campaigns.length!==1?'s':''}</span></div>
      <div style="display:flex;align-items:center;gap:8px">
        <div class="pf-summary"><span>${f$(c.rv)} rev ${pill(rd,fPct,1)}</span><span>${f$(c.sp)} spend ${pill(sd,fPct,0)}</span><span>${c.cvr!=null?fP(c.cvr):'-'} CVR ${cd2!==null?pill(cd2,fPp,1):'—'}</span></div>
        <span class="pf-chevron" id="chev-${i}">▼</span>
      </div>
    </div>
    <div class="pf-body" id="pf-${i}">
      <div class="pf-stats">
        <div class="pf-stat"><div class="pf-stat-lbl">Revenue</div><div class="pf-stat-val">${f$(c.rv)}</div>${pl(c.rv,p.rv,f$,1)}</div>
        <div class="pf-stat"><div class="pf-stat-lbl">Spend</div><div class="pf-stat-val">${f$(c.sp)}</div>${pl(c.sp,p.sp,f$,-1)}</div>
        <div class="pf-stat"><div class="pf-stat-lbl">CVR</div><div class="pf-stat-val">${c.cvr!=null?fP(c.cvr):'—'}</div>${pl(c.cvr,p.cvr,fP,1)}</div>
        <div class="pf-stat"><div class="pf-stat-lbl">Orders</div><div class="pf-stat-val">${fI(c.or)}</div>${pl(c.or,p.or,fI,1)}</div>
      </div>
      <div class="tbl-wrap" style="margin-top:4px">
        <table class="camp-tbl"><thead><tr><th>Campaign</th><th>Seg</th><th class="num">Revenue</th><th class="num">Rev Δ</th><th class="num">Spend</th><th class="num">Spend Δ</th><th class="num">CVR</th><th class="num">CVR Δ</th></tr></thead><tbody>${cr}</tbody></table>
      </div>
    </div>
  </div>`;
}

function renderPortfolios(){
  pfIdx=0;
  const sf=(a,b)=>pfSortKey==='rev'?(b.cur.rv||0)-(a.cur.rv||0):pfSortKey==='spend'?(b.cur.sp||0)-(a.cur.sp||0):(b.cur.cvr||0)-(a.cur.cvr||0);
  document.getElementById('pf-list-nb').innerHTML=[...portfoliosNB].sort(sf).map(p=>pfBlock(p,false)).join('');
  document.getElementById('pf-list-b').innerHTML=brandedPortfolio&&brandedPortfolio.campaigns.length?pfBlock(brandedPortfolio,true):'<p style="font-size:12px;color:var(--t2);padding:8px 0">No branded campaigns found.</p>';
  const og=document.getElementById('pf-group-other');
  if(portfoliosOther.length){og.style.display='';document.getElementById('pf-list-other').innerHTML=[...portfoliosOther].sort(sf).map(p=>pfBlock(p,false)).join('');}
  else og.style.display='none';
}

function togglePf(id){const b=document.getElementById(id),i=id.split('-')[1],ch=document.getElementById('chev-'+i);ch.classList.toggle('open',b.classList.toggle('open'));}
function setSort(k){sortKey=k;['spend','rev','cvr'].forEach(x=>document.getElementById('tab-'+x).classList.toggle('on',x===k));renderTable();}
function setPfSort(k){pfSortKey=k;['spend','rev','cvr'].forEach(x=>document.getElementById('ptab-'+x).classList.toggle('on',x===k));renderPortfolios();}
function setView(v){
  document.getElementById('view-campaign').classList.toggle('on',v==='campaign');
  document.getElementById('view-portfolio').classList.toggle('on',v==='portfolio');
  document.getElementById('campaign-view').style.display=v==='campaign'?'':'none';
  document.getElementById('portfolio-view').style.display=v==='portfolio'?'':'none';
}

function load(week,file){
  if(!file)return;
  Papa.parse(file,{header:true,skipEmptyLines:true,
    complete(res){
      parsed[week]={...extract(res),filename:file.name};
      document.getElementById('lbl-'+week).textContent=file.name;
      document.getElementById('dz-'+week).classList.add('ok');
      if(parsed.cur&&parsed.prior)renderResults();
    },error(){alert('Could not parse file.');}
  });
}

function renderResults(){
  const{cur,prior}=parsed;
  allCampaigns=joinC(cur,prior);
  portfoliosNB=buildNB(allCampaigns.filter(c=>c.seg==='nb'));
  brandedPortfolio=buildB(allCampaigns.filter(c=>c.seg==='b'));
  portfoliosOther=buildNB(allCampaigns.filter(c=>c.seg===null));
  document.getElementById('results-sub').textContent=`${cur.filename} vs. ${prior.filename} · ${allCampaigns.length} active campaigns`;
  rChange('rev-pct','rev-abs',cur.rv,prior.rv,f$,1);
  rChange('spend-pct','spend-abs',cur.sp,prior.sp,f$,0);
  rCard('all-cvr-cur','all-cvr-prior',cur.all.cvr,prior.all.cvr,fP,1);
  rCard('b-cvr-cur','b-cvr-prior',cur.b.cvr,prior.b.cvr,fP,1);
  rCard('nb-cvr-cur','nb-cvr-prior',cur.nb.cvr,prior.nb.cvr,fP,1);
  const ci=(!cur.ic||cur.im===0)?null:cur.im,pi=(!prior.ic||prior.im===0)?null:prior.im;
  rCard('impr-cur','impr-prior',ci,pi,fI,1);
  rCard('clicks-cur','clicks-prior',cur.cl,prior.cl,fI,1);
  rCard('orders-cur','orders-prior',cur.or,prior.or,fI,1);
  rCard('b-cpc-cur','b-cpc-prior',cur.b.cpc,prior.b.cpc,f$,-1);
  rCard('nb-cpc-cur','nb-cpc-prior',cur.nb.cpc,prior.nb.cpc,f$,-1);
  renderTable();renderPortfolios();
  const w=[];
  if(!cur.rc)w.push({t:'warn',m:`Revenue column not detected in "${cur.filename}".`});
  if(!prior.rc)w.push({t:'warn',m:`Revenue column not detected in "${prior.filename}".`});
  if(!cur.oc)w.push({t:'err',m:`Orders column not detected in "${cur.filename}".`});
  if(cur.or===0&&cur.oc)w.push({t:'err',m:`Orders total to 0 in "${cur.filename}" despite column "${cur.oc}" being detected.`});
  if(cur.b.cnt===0)w.push({t:'warn',m:`No "| b |" campaigns found — branded metrics unavailable.`});
  if(cur.nb.cnt===0)w.push({t:'warn',m:`No "| nb |" campaigns found — non-branded metrics unavailable.`});
  document.getElementById('warn-area').innerHTML=w.map(x=>`<div class="${x.t}">⚠ ${x.m}</div>`).join('');
  document.getElementById('upload-view').style.display='none';
  document.getElementById('results-view').style.display='';
}

function reset(){
  parsed.cur=parsed.prior=null;allCampaigns=[];portfoliosNB=[];brandedPortfolio=null;portfoliosOther=[];segFilter='all';
  const t=document.getElementById('seg-toggle');if(t){t.className='seg-toggle st-all';document.getElementById('seg-label').textContent='Seg';}
  ['cur','prior'].forEach(w=>{document.getElementById('dz-'+w).classList.remove('ok');document.getElementById('lbl-'+w).textContent=w==='cur'?'Current week':'Prior week';document.getElementById('f-'+w).value='';});
  document.getElementById('results-view').style.display='none';document.getElementById('upload-view').style.display='';
}
</script>
</body>
</html>
