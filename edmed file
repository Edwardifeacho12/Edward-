<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>MedGuide Nexus v4.0 — AI Clinical Suite</title>

<!-- React + Babel (browser build) -->
<script src="https://unpkg.com/react@18/umd/react.development.js" crossorigin></script>
<script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js" crossorigin></script>
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>

<style>
  html,body { margin:0;padding:0;background:#020509; }
  #root { min-height:100vh; }
</style>
</head>
<body>
<div id="root"></div>

<!-- NOTE: This app calls the Anthropic API directly from the browser.
     You need a valid Anthropic API key. Enter it when prompted on first load,
     or hardcode it below where indicated. -->

<script type="text/babel" data-presets="react">
// ─── Globals from React UMD ───────────────────────────────────────────────
const { useState, useEffect, useRef } = React;

// ─── API KEY MANAGEMENT ───────────────────────────────────────────────────
function getApiKey() {
  let key = localStorage.getItem("mg_api_key");
  if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) {
    key = prompt("🔑 Enter your Anthropic API Key to use MedGuide Nexus:\n(Get one at console.anthropic.com)");
    if (edward ifeacho) localStorage.setItem("mg_api_key", key.trim());
  }
  return key ? key.trim() : "";
}

// ─── STORAGE SHIM (localStorage instead of window.storage) ───────────────
const storage = {
  get: (key) => {
    try { const v = localStorage.getItem(key); return v ? { value: v } : null; } catch { return null; }
  },
  set: (key, value) => {
    try { localStorage.setItem(key, value); return true; } catch { return null; }
  }
};

// Patch window.storage for app code
window.storage = storage;

// ─── PATCHED AI FUNCTION (uses stored API key) ────────────────────────────
async function ai(prompt, sys = "") {
  const apiKey = getApiKey();
  if (!apiKey) { alert("No API key found. Please reload and enter your key."); return ""; }
  const messages = [{ role: "user", content: prompt }];
  const body = { model: "claude-sonnet-4-20250514", max_tokens: 1000, messages };
  if (sys) body.system = sys;
  try {
    const r = await fetch("https://api.anthropic.com/v1/messages", {
      method: "POST",
      headers: { "Content-Type": "application/json", "x-api-key": apiKey, "anthropic-version": "2023-06-01", "anthropic-dangerous-direct-browser-access": "true" },
      body: JSON.stringify(body),
    });
    const d = await r.json();
    if (d.error) { alert("API Error: " + d.error.message); return ""; }
    return d.content?.[0]?.text || "";
  } catch(e) {
    alert("Network error: " + e.message);
    return "";
  }
}

// ─── APP SOURCE ───────────────────────────────────────────────────────────

/* ═══════════════════════════════════════════════════════════════════════
   MEDGUIDE NEXUS v4.0 — FULL AI MEDICAL SUITE
   6 AI Modules:
   1. Drug Interaction Checker
   2. AI Differential Diagnosis
   3. Risk Score Calculator
   4. Medication Dosage Calculator
   5. Lab Results Interpreter
   6. AI Patient Chatbot
═══════════════════════════════════════════════════════════════════════ */

const GlobalStyles = () => (
  <style>{`
    @import url('https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Exo+2:ital,wght@0,300;0,400;0,600;0,700;0,900;1,400&display=swap');
    *,*::before,*::after{box-sizing:border-box;margin:0;padding:0;}
    html,body{background:#020509;color:#c8e6ff;font-family:'Exo 2',sans-serif;min-height:100vh;}
    ::-webkit-scrollbar{width:3px;}::-webkit-scrollbar-thumb{background:#00f5ff22;}

    @keyframes scanline{0%{top:-2px}100%{top:100%}}
    @keyframes blink{0%,100%{opacity:1}50%{opacity:0}}
    @keyframes fadeUp{from{opacity:0;transform:translateY(12px)}to{opacity:1;transform:translateY(0)}}
    @keyframes spin{to{transform:rotate(360deg)}}
    @keyframes pulseGlow{0%,100%{box-shadow:0 0 8px var(--gc,#00f5ff)}50%{box-shadow:0 0 24px var(--gc,#00f5ff)}}
    @keyframes hbarFill{from{width:0}to{width:var(--tw)}}
    @keyframes flicker{0%,92%,95%,100%{opacity:1}93%,96%{opacity:.4}}
    @keyframes chatIn{from{opacity:0;transform:translateX(-10px)}to{opacity:1;transform:translateX(0)}}
    @keyframes chatInR{from{opacity:0;transform:translateX(10px)}to{opacity:1;transform:translateX(0)}}
    @keyframes matrixRain{0%{transform:translateY(-100%);opacity:0}10%{opacity:1}90%{opacity:.6}100%{transform:translateY(100vh);opacity:0}}

    .fu{animation:fadeUp .35s ease both;}
    .fu1{animation:fadeUp .35s .05s ease both;}
    .fu2{animation:fadeUp .35s .1s ease both;}
    .fu3{animation:fadeUp .35s .15s ease both;}

    /* Panel */
    .panel{background:linear-gradient(135deg,#020d18 0%,#020509 100%);border:1px solid #00f5ff15;border-radius:2px;position:relative;overflow:hidden;}
    .panel::after{content:'';position:absolute;top:0;left:0;right:0;height:1px;background:linear-gradient(90deg,transparent,#00f5ff44,transparent);}

    /* Corner brackets */
    .brk::before,.brk::after{content:'';position:absolute;width:10px;height:10px;}
    .brk::before{top:0;left:0;border-top:1px solid #00f5ff;border-left:1px solid #00f5ff;}
    .brk::after{bottom:0;right:0;border-bottom:1px solid #00f5ff;border-right:1px solid #00f5ff;}

    /* Nav */
    .nav-btn{width:100%;display:flex;align-items:center;gap:9px;padding:9px 12px;border:none;border-left:2px solid transparent;background:transparent;cursor:pointer;font-family:'Exo 2',sans-serif;color:#00f5ff33;font-size:10px;letter-spacing:1.5px;text-transform:uppercase;transition:all .2s;text-align:left;}
    .nav-btn:hover{color:#00f5ff77;background:#00f5ff06;border-left-color:#00f5ff22;}
    .nav-btn.active{color:#00f5ff;background:#00f5ff0d;border-left-color:#00f5ff;}

    /* Inputs */
    .hi{width:100%;background:#010a12;border:1px solid #00f5ff1a;border-radius:2px;padding:9px 13px;color:#00f5ff;font-family:'Share Tech Mono',monospace;font-size:12px;outline:none;transition:border-color .2s,box-shadow .2s;}
    .hi:focus{border-color:#00f5ff66;box-shadow:0 0 10px #00f5ff18;}
    .hi::placeholder{color:#00f5ff1a;}
    select.hi option{background:#010a12;}

    /* Buttons */
    .btn-p{background:transparent;border:1px solid #00f5ff;color:#00f5ff;font-family:'Exo 2',sans-serif;font-weight:700;font-size:10px;letter-spacing:2px;text-transform:uppercase;padding:10px 22px;cursor:pointer;position:relative;overflow:hidden;transition:color .25s;clip-path:polygon(6px 0%,100% 0%,calc(100% - 6px) 100%,0% 100%);}
    .btn-p::before{content:'';position:absolute;inset:0;background:#00f5ff;transform:translateX(-101%);transition:transform .25s;}
    .btn-p:hover{color:#000;}
    .btn-p:hover::before{transform:translateX(0);}
    .btn-p span{position:relative;z-index:1;}
    .btn-p:disabled{opacity:.2;cursor:not-allowed;}
    .btn-g{background:transparent;border:1px solid #00f5ff1a;color:#00f5ff44;font-family:'Exo 2',sans-serif;font-size:10px;letter-spacing:1.5px;text-transform:uppercase;padding:9px 18px;cursor:pointer;transition:all .2s;}
    .btn-g:hover{border-color:#00f5ff44;color:#00f5ff88;}
    .btn-sm{padding:6px 14px;font-size:9px;}

    /* Tags */
    .tag{display:inline-block;border:1px solid #00f5ff15;padding:3px 9px;font-family:'Share Tech Mono',monospace;font-size:9px;color:#00f5ff33;letter-spacing:1px;border-radius:1px;}
    .tag.active-tag{border-color:#39ff1444;color:#39ff14;background:#39ff1408;}

    /* Drug chips */
    .drug-chip{border:1px solid #00f5ff15;background:#010a12;padding:5px 11px;font-size:11px;color:#00f5ff66;cursor:pointer;border-radius:1px;transition:all .15s;font-family:'Exo 2',sans-serif;}
    .drug-chip:hover{border-color:#00f5ff33;}
    .drug-chip .rm{color:#ff2a2a44;margin-left:6px;font-size:10px;}
    .drug-chip .rm:hover{color:#ff2a2a;}

    /* Chat */
    .chat-bubble-ai{animation:chatIn .3s ease;}
    .chat-bubble-user{animation:chatInR .3s ease;}

    /* Severity bars */
    .sev-bar{height:4px;background:#00f5ff0a;border-radius:0;overflow:hidden;}
    .sev-fill{height:4px;border-radius:0;animation:hbarFill .8s ease forwards;width:0;}

    /* Lab rows */
    .lab-row:hover{background:#00f5ff04;}

    /* Risk meter */
    .risk-arc{transition:stroke-dasharray 1.2s cubic-bezier(.16,1,.3,1);}

    /* Dot grid bg */
    .dot-bg{position:fixed;inset:0;pointer-events:none;z-index:0;
      background-image:radial-gradient(circle at 15% 50%,#00f5ff06 0%,transparent 45%),radial-gradient(circle at 85% 20%,#39ff1404 0%,transparent 40%);
    }
    
    /* scan overlay */
    .scan-overlay{position:absolute;left:0;right:0;height:1px;background:linear-gradient(90deg,transparent,#00f5ff88,transparent);animation:scanline 2s linear infinite;pointer-events:none;}

    @media print{.no-print{display:none!important;}}
  `}</style>
);

// ── HELPERS ──────────────────────────────────────────────────────────────────
async function ai(prompt, sys = "") {
  const messages = [{ role: "user", content: prompt }];
  const body = { model: "claude-sonnet-4-20250514", max_tokens: 1000, messages };
  if (sys) body.system = sys;
  const r = await fetch("https://api.anthropic.com/v1/messages", {
    method: "POST", headers: { "Content-Type": "application/json" }, body: JSON.stringify(body),
  });
  const d = await r.json();
  return d.content?.[0]?.text || "";
}

function SLabel({ children, color = "#00f5ff" }) {
  return (
    <div style={{ display: "flex", alignItems: "center", gap: 8, marginBottom: 14 }}>
      <div style={{ width: 2, height: 12, background: color, boxShadow: `0 0 8px ${color}` }} />
      <span style={{ fontFamily: "'Share Tech Mono',monospace", fontSize: 9, letterSpacing: 3, textTransform: "uppercase", color: color + "88" }}>{children}</span>
      <div style={{ flex: 1, height: 1, background: `linear-gradient(90deg,${color}18,transparent)` }} />
    </div>
  );
}

function Mono({ children, color = "#00f5ff", size = 12 }) {
  return <span style={{ fontFamily: "'Share Tech Mono',monospace", fontSize: size, color }}>{children}</span>;
}

function LoadingPulse({ label = "AI PROCESSING" }) {
  return (
    <div style={{ display: "flex", alignItems: "center", gap: 12, padding: "20px 0" }}>
      <div style={{ width: 18, height: 18, border: "1px solid #00f5ff44", borderTop: "1px solid #00f5ff", borderRadius: "50%", animation: "spin 0.8s linear infinite", flexShrink: 0 }} />
      <Mono color="#00f5ff88">{label}...</Mono>
      <div style={{ display: "flex", gap: 3 }}>
        {[0, 1, 2].map(i => <div key={i} style={{ width: 2, height: 10, background: "#00f5ff", animation: `blink 1s ${i * 0.2}s infinite` }} />)}
      </div>
    </div>
  );
}

function RiskGauge({ value, label, color }) {
  const r = 52, circ = 2 * Math.PI * r;
  const arc = circ * 0.75;
  const fill = (value / 100) * arc;
  return (
    <div style={{ textAlign: "center" }}>
      <svg width={130} height={100} viewBox="0 0 130 100">
        <circle cx={65} cy={70} r={r} fill="none" stroke="#00f5ff0a" strokeWidth={8}
          strokeDasharray={`${arc} ${circ}`} strokeDashoffset={circ * 0.125} strokeLinecap="round" />
        <circle cx={65} cy={70} r={r} fill="none" stroke={color} strokeWidth={8}
          strokeDasharray={`${fill} ${circ}`} strokeDashoffset={circ * 0.125}
          strokeLinecap="round" style={{ filter: `drop-shadow(0 0 6px ${color})`, transition: "stroke-dasharray 1.2s cubic-bezier(.16,1,.3,1)" }} />
        <text x={65} y={68} textAnchor="middle" fill={color} fontSize={18} fontFamily="'Share Tech Mono',monospace" fontWeight="bold">{value}%</text>
        <text x={65} y={84} textAnchor="middle" fill="#00f5ff33" fontSize={8} fontFamily="'Share Tech Mono',monospace" letterSpacing={1}>{label}</text>
      </svg>
    </div>
  );
}

// ── STORAGE ───────────────────────────────────────────────────────────────────
async function loadPts() { try { const r = await window.storage.get("mg_v4_pts"); return r ? JSON.parse(r.value) : []; } catch { return []; } }
async function savePts(p) { try { await window.storage.set("mg_v4_pts", JSON.stringify(p)); } catch {} }

// ── CONDITIONS ────────────────────────────────────────────────────────────────
const CONDITIONS = {
  diabetes_t1:   { label: "Diabetes Type 1",  icon: "🩸", color: "#ff4d6d", group: "METABOLIC" },
  diabetes_t2:   { label: "Diabetes Type 2",  icon: "💉", color: "#ff9a3c", group: "METABOLIC" },
  hypertension:  { label: "Hypertension",      icon: "❤️", color: "#ff2a6d", group: "CARDIOVASCULAR" },
  asthma:        { label: "Asthma",            icon: "🫁", color: "#00c2ff", group: "RESPIRATORY" },
  heart_disease: { label: "Heart Disease",     icon: "🫀", color: "#ff3a3a", group: "CARDIOVASCULAR" },
  hypertrichosis:{ label: "Hypertrichosis",    icon: "🧬", color: "#bf5fff", group: "DERMATOLOGICAL" },
};

const SYMPTOMS = {
  diabetes_t1:   ["Frequent urination","Excessive thirst","Weight loss","Extreme hunger","Blurred vision","Fatigue","Fruity breath","Nausea","Abdominal pain","Confusion"],
  diabetes_t2:   ["Increased thirst","Frequent urination","Hunger spikes","Fatigue","Blurred vision","Slow-healing sores","Frequent infections","Numbness in limbs","Darkened skin","Weight changes"],
  hypertension:  ["Severe headache","Chest pain","Shortness of breath","Nosebleeds","Dizziness","Visual changes","Fatigue","Irregular heartbeat","Neck pain","Ear buzzing"],
  asthma:        ["Wheezing","Shortness of breath","Chest tightness","Persistent cough","Difficulty sleeping","Exercise symptoms","Cold air sensitivity","Night cough","Rapid breathing","Bluish lips"],
  heart_disease: ["Chest pressure","Shortness of breath","Palpitations","Fatigue","Swollen legs","Dizziness","Nausea","Arm or jaw pain","Cold sweat","Fainting"],
  hypertrichosis:["Excessive facial hair","Excessive body hair","Unusual hair sites","Congenital onset","Adult onset","Hormonal symptoms","Skin irritation","Psychological distress","Associated acne","Irregular periods"],
};

const urgCfg = {
  routine:   { color: "#39ff14", label: "ROUTINE PROTOCOL",    bg: "#001a00" },
  soon:      { color: "#ffb800", label: "PRIORITY PROTOCOL",   bg: "#1a1000" },
  emergency: { color: "#ff2a2a", label: "CRITICAL — EMERGENCY", bg: "#1a0000" },
};

// ═════════════════════════════════════════════════════════════════════════════
//  FEATURE MODULES
// ═════════════════════════════════════════════════════════════════════════════

// ── 1. DRUG INTERACTION CHECKER ───────────────────────────────────────────────
function DrugInteractionChecker() {
  const [drugs, setDrugs] = useState([]);
  const [input, setInput] = useState("");
  const [result, setResult] = useState(null);
  const [loading, setLoading] = useState(false);

  const addDrug = () => {
    const d = input.trim();
    if (d && !drugs.includes(d)) { setDrugs(p => [...p, d]); setInput(""); setResult(null); }
  };

  const check = async () => {
    if (drugs.length < 2) return;
    setLoading(true); setResult(null);
    const text = await ai(`You are a clinical pharmacist AI. Check interactions between these drugs: ${drugs.join(", ")}.

For each pair, provide:
1. INTERACTION_LEVEL: [None/Minor/Moderate/Major/Contraindicated]
2. MECHANISM: Brief pharmacological mechanism
3. CLINICAL_EFFECT: What happens clinically
4. MANAGEMENT: What to do

Format each pair as:
PAIR: DrugA + DrugB
LEVEL: [level]
MECHANISM: [text]
EFFECT: [text]
MANAGEMENT: [text]
---

End with:
OVERALL_RISK: [Low/Medium/High/Critical]
SUMMARY: [2-sentence overall clinical summary]`);
    setResult(text); setLoading(false);
  };

  const parseInteractions = (text) => {
    if (!text) return [];
    return text.split("---").filter(b => b.includes("PAIR:")).map(block => {
      const get = k => block.match(new RegExp(`${k}:\\s*(.+?)(?=\\n[A-Z]+:|$)`, "si"))?.[1]?.trim() || "";
      return { pair: get("PAIR"), level: get("LEVEL"), mechanism: get("MECHANISM"), effect: get("EFFECT"), management: get("MANAGEMENT") };
    });
  };

  const getOverall = (text) => {
    if (!text) return {};
    return {
      risk: text.match(/OVERALL_RISK:\s*(.+)/i)?.[1]?.trim() || "",
      summary: text.match(/SUMMARY:\s*([\s\S]+?)(?=\n[A-Z]+:|$)/i)?.[1]?.trim() || "",
    };
  };

  const levelColor = { None: "#39ff14", Minor: "#7fff00", Moderate: "#ffb800", Major: "#ff6600", Contraindicated: "#ff2a2a" };

  const interactions = parseInteractions(result);
  const overall = getOverall(result);

  return (
    <div className="fu">
      <SLabel color="#ff9a3c">Drug Interaction Checker</SLabel>
      <div style={{ marginBottom: 16 }}>
        <div style={{ display: "flex", gap: 8, marginBottom: 10 }}>
          <input className="hi" placeholder="// ENTER DRUG NAME (e.g. Warfarin)..." value={input}
            onChange={e => setInput(e.target.value)} onKeyDown={e => e.key === "Enter" && addDrug()} style={{ flex: 1 }} />
          <button className="btn-p btn-sm" onClick={addDrug}><span>+ ADD</span></button>
        </div>
        {drugs.length > 0 && (
          <div style={{ display: "flex", flexWrap: "wrap", gap: 6, marginBottom: 12 }}>
            {drugs.map(d => (
              <span key={d} className="drug-chip">{d}
                <span className="rm" onClick={() => { setDrugs(p => p.filter(x => x !== d)); setResult(null); }}>✕</span>
              </span>
            ))}
          </div>
        )}
        <button className="btn-p" disabled={drugs.length < 2 || loading} onClick={check}>
          <span>⚡ CHECK INTERACTIONS [{drugs.length}]</span>
        </button>
        {drugs.length < 2 && <div style={{ marginTop: 6 }}><Mono color="#00f5ff22" size={9}>// ADD AT LEAST 2 DRUGS TO ANALYZE</Mono></div>}
      </div>

      {loading && <LoadingPulse label="SCANNING PHARMACOLOGICAL DATABASE" />}

      {interactions.length > 0 && (
        <div>
          {overall.risk && (
            <div style={{ padding: "12px 16px", marginBottom: 12, border: `1px solid ${levelColor[overall.risk] || "#ffb800"}44`, background: "#010a12", display: "flex", gap: 12, alignItems: "center" }}>
              <div style={{ width: 10, height: 10, borderRadius: "50%", background: levelColor[overall.risk] || "#ffb800", boxShadow: `0 0 10px ${levelColor[overall.risk] || "#ffb800"}`, flexShrink: 0 }} />
              <div>
                <Mono color={levelColor[overall.risk] || "#ffb800"} size={11}>OVERALL RISK: {overall.risk?.toUpperCase()}</Mono>
                {overall.summary && <div style={{ fontSize: 11, color: "#00f5ff66", marginTop: 4, lineHeight: 1.6 }}>{overall.summary}</div>}
              </div>
            </div>
          )}
          {interactions.map((ix, i) => (
            <div key={i} style={{ border: `1px solid ${levelColor[ix.level] || "#00f5ff"}22`, background: "#010a12", padding: "14px 16px", marginBottom: 8, borderLeft: `3px solid ${levelColor[ix.level] || "#00f5ff"}` }}>
              <div style={{ display: "flex", justifyContent: "space-between", alignItems: "center", marginBottom: 10 }}>
                <Mono color="#00f5ffaa" size={11}>{ix.pair}</Mono>
                <span style={{ fontFamily: "'Share Tech Mono',monospace", fontSize: 9, padding: "2px 8px", border: `1px solid ${levelColor[ix.level]}44`, color: levelColor[ix.level], letterSpacing: 1 }}>{ix.level?.toUpperCase()}</span>
              </div>
              {[["MECHANISM", ix.mechanism], ["CLINICAL EFFECT", ix.effect], ["MANAGEMENT", ix.management]].map(([k, v]) => v && (
                <div key={k} style={{ marginBottom: 6 }}>
                  <Mono color="#00f5ff33" size={8}>{k}: </Mono>
                  <span style={{ fontSize: 11, color: "#00f5ff66", lineHeight: 1.6 }}>{v}</span>
                </div>
              ))}
            </div>
          ))}
        </div>
      )}
    </div>
  );
}

// ── 2. DIFFERENTIAL DIAGNOSIS ─────────────────────────────────────────────────
function DifferentialDiagnosis() {
  const [sx, setSx] = useState("");
  const [age, setAge] = useState("");
  const [gender, setGender] = useState("");
  const [vitals, setVitals] = useState("");
  const [result, setResult] = useState(null);
  const [loading, setLoading] = useState(false);

  const run = async () => {
    if (!sx.trim()) return;
    setLoading(true); setResult(null);
    const text = await ai(`You are a senior clinician AI. Generate a differential diagnosis.

Patient: Age=${age || "unknown"}, Gender=${gender || "unknown"}
Symptoms: ${sx}
Vitals/Context: ${vitals || "none provided"}

Provide exactly 5 differential diagnoses, ordered by likelihood. For each:
DX[n]: [Diagnosis name]
LIKELIHOOD: [percentage 0-100]
RATIONALE: [2 sentences why this fits]
KEY_FINDINGS: [What findings would confirm this]
URGENCY: [routine/soon/emergency]

After all 5:
NEXT_STEPS: [3 specific diagnostic tests/actions to narrow the differential]
RED_FLAGS: [Any symptoms that would require immediate emergency care]`);
    setResult(text); setLoading(false);
  };

  const parseDx = (text) => {
    if (!text) return [];
    return [1, 2, 3, 4, 5].map(n => {
      const block = text.match(new RegExp(`DX\\[?${n}\\]?:[\\s\\S]*?(?=DX\\[?${n + 1}\\]?:|NEXT_STEPS:|$)`, "i"))?.[0] || "";
      const get = k => block.match(new RegExp(`${k}:\\s*(.+?)(?=\\n[A-Z_]+:|$)`, "si"))?.[1]?.trim() || "";
      const name = block.match(/DX\[?\d\]?:\s*(.+)/i)?.[1]?.trim() || "";
      return { name, likelihood: parseInt(get("LIKELIHOOD")) || 0, rationale: get("RATIONALE"), findings: get("KEY_FINDINGS"), urgency: get("URGENCY") };
    }).filter(d => d.name);
  };

  const nextSteps = result?.match(/NEXT_STEPS:\s*([\s\S]+?)(?=RED_FLAGS:|$)/i)?.[1]?.trim() || "";
  const redFlags = result?.match(/RED_FLAGS:\s*([\s\S]+?)$/i)?.[1]?.trim() || "";
  const dxList = parseDx(result);

  return (
    <div className="fu">
      <SLabel color="#bf5fff">AI Differential Diagnosis</SLabel>
      <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr 1fr", gap: 10, marginBottom: 12 }}>
        <div>
          <label style={{ display: "block", fontFamily: "'Share Tech Mono',monospace", fontSize: 8, color: "#00f5ff33", letterSpacing: 2, marginBottom: 6 }}>PATIENT AGE</label>
          <input className="hi" type="number" placeholder="e.g. 45" value={age} onChange={e => setAge(e.target.value)} />
        </div>
        <div>
          <label style={{ display: "block", fontFamily: "'Share Tech Mono',monospace", fontSize: 8, color: "#00f5ff33", letterSpacing: 2, marginBottom: 6 }}>BIOLOGICAL SEX</label>
          <select className="hi" value={gender} onChange={e => setGender(e.target.value)}>
            <option value="">// SELECT</option><option>Male</option><option>Female</option><option>Other</option>
          </select>
        </div>
        <div>
          <label style={{ display: "block", fontFamily: "'Share Tech Mono',monospace", fontSize: 8, color: "#00f5ff33", letterSpacing: 2, marginBottom: 6 }}>KEY VITALS</label>
          <input className="hi" placeholder="e.g. BP 150/90, HR 95" value={vitals} onChange={e => setVitals(e.target.value)} />
        </div>
      </div>
      <div style={{ marginBottom: 12 }}>
        <label style={{ display: "block", fontFamily: "'Share Tech Mono',monospace", fontSize: 8, color: "#00f5ff33", letterSpacing: 2, marginBottom: 6 }}>PRESENTING SYMPTOMS & HISTORY</label>
        <textarea className="hi" style={{ minHeight: 80, resize: "vertical" }}
          placeholder="// Describe symptoms, onset, duration, associated factors, medical history..."
          value={sx} onChange={e => setSx(e.target.value)} />
      </div>
      <button className="btn-p" disabled={!sx.trim() || loading} onClick={run}><span>🧠 GENERATE DIFFERENTIAL</span></button>

      {loading && <LoadingPulse label="COMPUTING DIFFERENTIAL DIAGNOSIS" />}

      {dxList.length > 0 && (
        <div style={{ marginTop: 16 }}>
          {dxList.map((d, i) => {
            const u = urgCfg[d.urgency] || urgCfg.routine;
            const barColor = i === 0 ? "#bf5fff" : i === 1 ? "#00c2ff" : "#00f5ff";
            return (
              <div key={i} style={{ background: "#010a12", border: "1px solid #00f5ff0d", borderLeft: `3px solid ${barColor}`, padding: "14px 16px", marginBottom: 8 }}>
                <div style={{ display: "flex", justifyContent: "space-between", alignItems: "flex-start", marginBottom: 10 }}>
                  <div>
                    <span style={{ fontFamily: "'Share Tech Mono',monospace", fontSize: 9, color: barColor + "88", marginRight: 8 }}>#{i + 1}</span>
                    <span style={{ fontFamily: "'Exo 2',sans-serif", fontWeight: 700, fontSize: 13, color: "#c8e6ff" }}>{d.name}</span>
                  </div>
                  <div style={{ display: "flex", gap: 6, alignItems: "center", flexShrink: 0 }}>
                    <span style={{ fontFamily: "'Share Tech Mono',monospace", fontSize: 10, color: barColor }}>{d.likelihood}%</span>
                    <span style={{ fontFamily: "'Share Tech Mono',monospace", fontSize: 8, padding: "1px 7px", border: `1px solid ${u.color}33`, color: u.color, letterSpacing: 1 }}>{d.urgency?.toUpperCase()}</span>
                  </div>
                </div>
                <div style={{ height: 3, background: "#00f5ff08", marginBottom: 10 }}>
                  <div style={{ height: 3, width: `${d.likelihood}%`, background: barColor, boxShadow: `0 0 6px ${barColor}`, transition: "width 1s ease" }} />
                </div>
                {d.rationale && <div style={{ fontSize: 11, color: "#00f5ff77", lineHeight: 1.7, marginBottom: 6 }}>{d.rationale}</div>}
                {d.findings && (
                  <div style={{ fontSize: 10, color: "#00f5ff44" }}>
                    <Mono color="#00f5ff22" size={8}>KEY FINDINGS: </Mono>{d.findings}
                  </div>
                )}
              </div>
            );
          })}
          {nextSteps && (
            <div style={{ background: "#010a12", border: "1px solid #00c2ff22", padding: "14px 16px", marginBottom: 8 }}>
              <SLabel color="#00c2ff">Recommended Next Steps</SLabel>
              <div style={{ fontSize: 12, color: "#00c2ff88", lineHeight: 1.8, whiteSpace: "pre-line" }}>{nextSteps}</div>
            </div>
          )}
          {redFlags && (
            <div style={{ background: "#1a000a", border: "1px solid #ff2a2a33", padding: "14px 16px" }}>
              <SLabel color="#ff2a2a">⚠️ Red Flag Symptoms</SLabel>
              <div style={{ fontSize: 12, color: "#ff2a2a88", lineHeight: 1.8 }}>{redFlags}</div>
            </div>
          )}
        </div>
      )}
    </div>
  );
}

// ── 3. RISK SCORE CALCULATOR ──────────────────────────────────────────────────
function RiskCalculator() {
  const [riskType, setRiskType] = useState("heart");
  const [fields, setFields] = useState({});
  const [result, setResult] = useState(null);
  const [loading, setLoading] = useState(false);

  const RISK_CONFIGS = {
    heart: {
      label: "10-Year Heart Attack Risk", color: "#ff3a3a",
      fields: [
        { key: "age", label: "Age (years)", type: "number", placeholder: "e.g. 55" },
        { key: "gender", label: "Sex", type: "select", opts: ["Male", "Female"] },
        { key: "sbp", label: "Systolic BP (mmHg)", type: "number", placeholder: "e.g. 130" },
        { key: "tc", label: "Total Cholesterol (mg/dL)", type: "number", placeholder: "e.g. 200" },
        { key: "hdl", label: "HDL Cholesterol (mg/dL)", type: "number", placeholder: "e.g. 50" },
        { key: "smoking", label: "Smoker", type: "select", opts: ["No", "Yes"] },
        { key: "diabetes", label: "Diabetes", type: "select", opts: ["No", "Yes"] },
        { key: "bpTx", label: "On BP Treatment", type: "select", opts: ["No", "Yes"] },
      ]
    },
    stroke: {
      label: "Stroke Risk (CHADS₂-VASc)", color: "#00c2ff",
      fields: [
        { key: "age", label: "Age", type: "number", placeholder: "e.g. 65" },
        { key: "gender", label: "Sex", type: "select", opts: ["Male", "Female"] },
        { key: "chf", label: "Heart Failure", type: "select", opts: ["No", "Yes"] },
        { key: "htn", label: "Hypertension", type: "select", opts: ["No", "Yes"] },
        { key: "diabetes", label: "Diabetes", type: "select", opts: ["No", "Yes"] },
        { key: "stroke_hx", label: "Prior Stroke/TIA", type: "select", opts: ["No", "Yes"] },
        { key: "vasc", label: "Vascular Disease", type: "select", opts: ["No", "Yes"] },
      ]
    },
    diabetes: {
      label: "Type 2 Diabetes Risk", color: "#ff9a3c",
      fields: [
        { key: "age", label: "Age (years)", type: "number", placeholder: "e.g. 45" },
        { key: "bmi", label: "BMI", type: "number", placeholder: "e.g. 28" },
        { key: "waist", label: "Waist Circumference (cm)", type: "number", placeholder: "e.g. 92" },
        { key: "phys_act", label: "Physical Activity", type: "select", opts: ["Active","Somewhat Active","Sedentary"] },
        { key: "family_hx", label: "Family History", type: "select", opts: ["No", "Yes"] },
        { key: "htn", label: "Hypertension", type: "select", opts: ["No", "Yes"] },
        { key: "glucose_hx", label: "High Glucose History", type: "select", opts: ["No", "Yes"] },
      ]
    },
  };

  const cfg = RISK_CONFIGS[riskType];

  const calculate = async () => {
    setLoading(true); setResult(null);
    const fStr = Object.entries(fields).map(([k, v]) => `${k}: ${v}`).join(", ");
    const text = await ai(`You are a clinical risk stratification AI.

Risk Assessment: ${cfg.label}
Patient Data: ${fStr}

Calculate the risk score and provide:
RISK_SCORE: [numeric percentage 0-100]
RISK_CATEGORY: [Low/Borderline/Intermediate/High/Very High]
INTERPRETATION: [2-sentence clinical interpretation]
CONTRIBUTING_FACTORS: [3-4 most significant risk factors for this patient]
RECOMMENDATIONS: [4-5 specific risk-reduction interventions]
MONITORING: [What to monitor and how often]

Be clinically accurate. Base on established guidelines (ACC/AHA, ESC, etc.).`);
    setResult(text); setLoading(false);
  };

  const get = k => result?.match(new RegExp(`${k}:\\s*([\\s\\S]+?)(?=\\n[A-Z_]+:|$)`, "i"))?.[1]?.trim() || "";
  const score = parseInt(get("RISK_SCORE")) || 0;
  const category = get("RISK_CATEGORY");
  const catColor = { Low: "#39ff14", Borderline: "#7fff00", Intermediate: "#ffb800", High: "#ff6600", "Very High": "#ff2a2a" }[category] || cfg.color;

  return (
    <div className="fu">
      <SLabel color="#ff3a3a">Clinical Risk Calculator</SLabel>

      {/* Risk type selector */}
      <div style={{ display: "flex", gap: 8, marginBottom: 16 }}>
        {Object.entries(RISK_CONFIGS).map(([k, v]) => (
          <button key={k} onClick={() => { setRiskType(k); setFields({}); setResult(null); }}
            style={{ border: `1px solid ${riskType === k ? v.color : "#00f5ff15"}`, background: riskType === k ? v.color + "15" : "transparent", color: riskType === k ? v.color : "#00f5ff33", padding: "7px 14px", cursor: "pointer", fontSize: 10, fontFamily: "'Exo 2',sans-serif", letterSpacing: 1, textTransform: "uppercase", transition: "all .2s" }}>
            {v.label.split(" ").slice(0, 3).join(" ")}
          </button>
        ))}
      </div>

      <div style={{ display: "grid", gridTemplateColumns: "repeat(2,1fr)", gap: 10, marginBottom: 14 }}>
        {cfg.fields.map(f => (
          <div key={f.key}>
            <label style={{ display: "block", fontFamily: "'Share Tech Mono',monospace", fontSize: 8, color: "#00f5ff33", letterSpacing: 2, marginBottom: 6 }}>{f.label.toUpperCase()}</label>
            {f.type === "select" ? (
              <select className="hi" value={fields[f.key] || ""} onChange={e => setFields(p => ({ ...p, [f.key]: e.target.value }))}>
                <option value="">// SELECT</option>
                {f.opts.map(o => <option key={o}>{o}</option>)}
              </select>
            ) : (
              <input className="hi" type={f.type} placeholder={f.placeholder} value={fields[f.key] || ""} onChange={e => setFields(p => ({ ...p, [f.key]: e.target.value }))} />
            )}
          </div>
        ))}
      </div>

      <button className="btn-p" disabled={loading} onClick={calculate}><span>📊 CALCULATE RISK SCORE</span></button>
      {loading && <LoadingPulse label="COMPUTING RISK STRATIFICATION" />}

      {result && score > 0 && (
        <div style={{ marginTop: 16 }}>
          <div style={{ display: "flex", alignItems: "center", gap: 20, background: "#010a12", border: `1px solid ${catColor}22`, padding: "16px 20px", marginBottom: 12 }}>
            <RiskGauge value={score} label={category?.toUpperCase()} color={catColor} />
            <div style={{ flex: 1 }}>
              <div style={{ fontFamily: "'Share Tech Mono',monospace", fontSize: 10, color: catColor + "66", letterSpacing: 2, marginBottom: 4 }}>{cfg.label.toUpperCase()}</div>
              <div style={{ fontFamily: "'Exo 2',sans-serif", fontWeight: 900, fontSize: 28, color: catColor, marginBottom: 4 }}>{score}%</div>
              <div style={{ fontFamily: "'Share Tech Mono',monospace", fontSize: 9, padding: "2px 10px", border: `1px solid ${catColor}44`, color: catColor, display: "inline-block", letterSpacing: 1 }}>{category?.toUpperCase()}</div>
              <div style={{ marginTop: 8, fontSize: 11, color: "#00f5ff66", lineHeight: 1.7 }}>{get("INTERPRETATION")}</div>
            </div>
          </div>
          {[["CONTRIBUTING FACTORS", get("CONTRIBUTING_FACTORS"), "#ffb800"], ["RECOMMENDATIONS", get("RECOMMENDATIONS"), "#39ff14"], ["MONITORING PLAN", get("MONITORING"), "#00c2ff"]].map(([k, v, c]) => v && (
            <div key={k} style={{ background: "#010a12", border: `1px solid ${c}15`, padding: "14px 16px", marginBottom: 8, borderLeft: `2px solid ${c}` }}>
              <Mono color={c + "88"} size={8}>{k}</Mono>
              <div style={{ marginTop: 8, fontSize: 11, color: "#00f5ff77", lineHeight: 1.8, whiteSpace: "pre-line" }}>{v}</div>
            </div>
          ))}
        </div>
      )}
    </div>
  );
}

// ── 4. DOSAGE CALCULATOR ──────────────────────────────────────────────────────
function DosageCalculator() {
  const [med, setMed] = useState("");
  const [weight, setWeight] = useState("");
  const [age, setAge] = useState("");
  const [indication, setIndication] = useState("");
  const [renal, setRenal] = useState("");
  const [hepatic, setHepatic] = useState("");
  const [result, setResult] = useState(null);
  const [loading, setLoading] = useState(false);

  const calculate = async () => {
    if (!med.trim()) return;
    setLoading(true); setResult(null);
    const text = await ai(`You are a clinical pharmacist AI. Calculate appropriate dosing.

Medication: ${med}
Patient Weight: ${weight || "unknown"} kg
Patient Age: ${age || "unknown"} years
Indication: ${indication || "not specified"}
Renal Function: ${renal || "normal"}
Hepatic Function: ${hepatic || "normal"}

Provide:
STANDARD_DOSE: [dose and frequency for this patient]
DOSE_CALCULATION: [show the weight/age-based calculation if applicable]
ROUTE: [administration route(s)]
MAX_DOSE: [maximum safe dose per day]
ADJUSTMENTS: [any dose adjustments needed for renal/hepatic/age]
MONITORING: [what to monitor for efficacy and toxicity]
INTERACTIONS_NOTE: [key interactions to be aware of]
CONTRAINDICATIONS: [key contraindications]
WARNINGS: [important clinical warnings]`);
    setResult(text); setLoading(false);
  };

  const get = k => result?.match(new RegExp(`${k}:\\s*([\\s\\S]+?)(?=\\n[A-Z_]+:|$)`, "i"))?.[1]?.trim() || "";

  return (
    <div className="fu">
      <SLabel color="#39ff14">Medication Dosage Calculator</SLabel>
      <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: 10, marginBottom: 12 }}>
        <div style={{ gridColumn: "1/-1" }}>
          <label style={{ display: "block", fontFamily: "'Share Tech Mono',monospace", fontSize: 8, color: "#00f5ff33", letterSpacing: 2, marginBottom: 6 }}>MEDICATION NAME</label>
          <input className="hi" placeholder="// e.g. Amoxicillin, Metformin, Lisinopril..." value={med} onChange={e => setMed(e.target.value)} />
        </div>
        {[["weight","PATIENT WEIGHT (kg)","number","e.g. 70"],["age","PATIENT AGE (years)","number","e.g. 45"]].map(([k,l,t,ph])=>(
          <div key={k}>
            <label style={{ display: "block", fontFamily: "'Share Tech Mono',monospace", fontSize: 8, color: "#00f5ff33", letterSpacing: 2, marginBottom: 6 }}>{l}</label>
            <input className="hi" type={t} placeholder={ph} value={k==="weight"?weight:age} onChange={e=>k==="weight"?setWeight(e.target.value):setAge(e.target.value)} />
          </div>
        ))}
        <div>
          <label style={{ display: "block", fontFamily: "'Share Tech Mono',monospace", fontSize: 8, color: "#00f5ff33", letterSpacing: 2, marginBottom: 6 }}>INDICATION</label>
          <input className="hi" placeholder="e.g. Hypertension, Infection..." value={indication} onChange={e => setIndication(e.target.value)} />
        </div>
        <div>
          <label style={{ display: "block", fontFamily: "'Share Tech Mono',monospace", fontSize: 8, color: "#00f5ff33", letterSpacing: 2, marginBottom: 6 }}>RENAL FUNCTION (eGFR)</label>
          <input className="hi" placeholder="e.g. Normal, eGFR 45, ESRD..." value={renal} onChange={e => setRenal(e.target.value)} />
        </div>
        <div>
          <label style={{ display: "block", fontFamily: "'Share Tech Mono',monospace", fontSize: 8, color: "#00f5ff33", letterSpacing: 2, marginBottom: 6 }}>HEPATIC FUNCTION</label>
          <select className="hi" value={hepatic} onChange={e => setHepatic(e.target.value)}>
            <option value="">Normal</option><option>Mild Impairment</option><option>Moderate Impairment</option><option>Severe Impairment (Child-Pugh C)</option>
          </select>
        </div>
      </div>
      <button className="btn-p" disabled={!med.trim() || loading} onClick={calculate}><span>💊 CALCULATE DOSAGE</span></button>
      {loading && <LoadingPulse label="COMPUTING PHARMACOLOGICAL DOSAGE" />}

      {result && (
        <div style={{ marginTop: 16 }}>
          {/* Main dose box */}
          <div style={{ background: "#000f08", border: "1px solid #39ff1422", padding: "16px 20px", marginBottom: 12 }}>
            <Mono color="#39ff1466" size={8}>CALCULATED DOSE FOR {med.toUpperCase()}</Mono>
            <div style={{ fontFamily: "'Share Tech Mono',monospace", fontSize: 20, color: "#39ff14", marginTop: 8, textShadow: "0 0 20px #39ff1466" }}>{get("STANDARD_DOSE")}</div>
            {get("DOSE_CALCULATION") && <div style={{ fontSize: 11, color: "#39ff1455", marginTop: 6, fontFamily: "'Share Tech Mono',monospace" }}>CALC: {get("DOSE_CALCULATION")}</div>}
            <div style={{ display: "flex", gap: 12, marginTop: 10 }}>
              <div><Mono color="#00f5ff33" size={8}>ROUTE: </Mono><Mono color="#00f5ff77" size={10}>{get("ROUTE")}</Mono></div>
              <div><Mono color="#00f5ff33" size={8}>MAX/DAY: </Mono><Mono color="#ffb80088" size={10}>{get("MAX_DOSE")}</Mono></div>
            </div>
          </div>
          {[
            ["DOSE ADJUSTMENTS", get("ADJUSTMENTS"), "#ffb800"],
            ["MONITORING PARAMETERS", get("MONITORING"), "#00c2ff"],
            ["INTERACTION NOTES", get("INTERACTIONS_NOTE"), "#bf5fff"],
            ["CONTRAINDICATIONS", get("CONTRAINDICATIONS"), "#ff6600"],
            ["CLINICAL WARNINGS", get("WARNINGS"), "#ff2a2a"],
          ].map(([k, v, c]) => v && (
            <div key={k} style={{ background: "#010a12", borderLeft: `2px solid ${c}`, padding: "12px 16px", marginBottom: 8 }}>
              <Mono color={c + "66"} size={8}>{k}</Mono>
              <div style={{ marginTop: 6, fontSize: 11, color: "#00f5ff66", lineHeight: 1.7 }}>{v}</div>
            </div>
          ))}
        </div>
      )}
    </div>
  );
}

// ── 5. LAB RESULTS INTERPRETER ────────────────────────────────────────────────
function LabInterpreter() {
  const [labs, setLabs] = useState([{ name: "", value: "", unit: "" }]);
  const [context, setContext] = useState("");
  const [result, setResult] = useState(null);
  const [loading, setLoading] = useState(false);

  const addRow = () => setLabs(p => [...p, { name: "", value: "", unit: "" }]);
  const updateRow = (i, k, v) => setLabs(p => p.map((r, j) => j === i ? { ...r, [k]: v } : r));
  const removeRow = i => setLabs(p => p.filter((_, j) => j !== i));

  const interpret = async () => {
    const labStr = labs.filter(l => l.name && l.value).map(l => `${l.name}: ${l.value} ${l.unit}`).join(", ");
    if (!labStr) return;
    setLoading(true); setResult(null);
    const text = await ai(`You are a clinical pathologist AI. Interpret these lab results:

Labs: ${labStr}
Clinical Context: ${context || "not provided"}

For each abnormal or notable result:
RESULT_NAME: [lab name]
VALUE_STATUS: [Normal/Low/High/Critical Low/Critical High]
CLINICAL_SIGNIFICANCE: [what this means clinically - 1 sentence]
LIKELY_CAUSES: [2-3 most likely causes]

Then provide:
PATTERN_ANALYSIS: [Are there patterns suggesting a specific condition? 2-3 sentences]
URGENCY: [routine/soon/emergency]
RECOMMENDED_ACTIONS: [4-5 specific clinical actions based on these results]
REPEAT_TESTING: [which labs should be repeated and when]`);
    setResult(text); setLoading(false);
  };

  const parseResults = (text) => {
    if (!text) return [];
    const blocks = text.split(/(?=RESULT_NAME:)/i).filter(b => b.includes("RESULT_NAME:"));
    return blocks.map(b => {
      const get = k => b.match(new RegExp(`${k}:\\s*(.+?)(?=\\n[A-Z_]+:|$)`, "si"))?.[1]?.trim() || "";
      return { name: get("RESULT_NAME"), status: get("VALUE_STATUS"), significance: get("CLINICAL_SIGNIFICANCE"), causes: get("LIKELY_CAUSES") };
    });
  };

  const statusColor = { Normal: "#39ff14", Low: "#00c2ff", High: "#ffb800", "Critical Low": "#ff2a2a", "Critical High": "#ff2a2a" };
  const parsedResults = parseResults(result);
  const get = k => result?.match(new RegExp(`${k}:\\s*([\\s\\S]+?)(?=\\n[A-Z_]+:|$)`, "i"))?.[1]?.trim() || "";

  return (
    <div className="fu">
      <SLabel color="#00c2ff">Lab Results Interpreter</SLabel>
      <div style={{ background: "#010a12", border: "1px solid #00f5ff0d", padding: "14px", marginBottom: 12 }}>
        <div style={{ display: "grid", gridTemplateColumns: "2fr 1fr 1fr auto", gap: 8, marginBottom: 8 }}>
          <Mono color="#00f5ff33" size={8}>TEST NAME</Mono>
          <Mono color="#00f5ff33" size={8}>VALUE</Mono>
          <Mono color="#00f5ff33" size={8}>UNIT</Mono>
          <div />
        </div>
        {labs.map((l, i) => (
          <div key={i} style={{ display: "grid", gridTemplateColumns: "2fr 1fr 1fr auto", gap: 8, marginBottom: 6 }}>
            <input className="hi" style={{ fontSize: 11 }} placeholder="e.g. Hemoglobin" value={l.name} onChange={e => updateRow(i, "name", e.target.value)} />
            <input className="hi" style={{ fontSize: 11 }} placeholder="e.g. 11.2" value={l.value} onChange={e => updateRow(i, "value", e.target.value)} />
            <input className="hi" style={{ fontSize: 11 }} placeholder="e.g. g/dL" value={l.unit} onChange={e => updateRow(i, "unit", e.target.value)} />
            <button onClick={() => removeRow(i)} style={{ background: "none", border: "1px solid #ff2a2a22", color: "#ff2a2a44", cursor: "pointer", padding: "0 8px", fontSize: 12 }}>✕</button>
          </div>
        ))}
        <button className="btn-g btn-sm" onClick={addRow} style={{ marginTop: 4 }}>+ ADD LAB</button>
      </div>
      <div style={{ marginBottom: 12 }}>
        <label style={{ display: "block", fontFamily: "'Share Tech Mono',monospace", fontSize: 8, color: "#00f5ff33", letterSpacing: 2, marginBottom: 6 }}>CLINICAL CONTEXT</label>
        <textarea className="hi" style={{ minHeight: 60, resize: "vertical" }} placeholder="// Patient history, symptoms, current medications relevant to these labs..." value={context} onChange={e => setContext(e.target.value)} />
      </div>
      <button className="btn-p" disabled={loading || !labs.some(l => l.name && l.value)} onClick={interpret}><span>🔬 INTERPRET RESULTS</span></button>
      {loading && <LoadingPulse label="ANALYZING LABORATORY DATA" />}

      {parsedResults.length > 0 && (
        <div style={{ marginTop: 16 }}>
          {parsedResults.map((r, i) => {
            const sc = statusColor[r.status] || "#00f5ff";
            return (
              <div key={i} className="lab-row" style={{ display: "flex", gap: 14, padding: "12px 14px", borderBottom: "1px solid #00f5ff06", background: "#010a12", marginBottom: 4 }}>
                <div style={{ width: 8, height: 8, borderRadius: "50%", background: sc, marginTop: 5, flexShrink: 0, boxShadow: `0 0 8px ${sc}` }} />
                <div style={{ flex: 1 }}>
                  <div style={{ display: "flex", justifyContent: "space-between", marginBottom: 4 }}>
                    <span style={{ fontFamily: "'Exo 2',sans-serif", fontWeight: 600, fontSize: 12, color: "#c8e6ff" }}>{r.name}</span>
                    <span style={{ fontFamily: "'Share Tech Mono',monospace", fontSize: 9, color: sc, padding: "1px 7px", border: `1px solid ${sc}33`, letterSpacing: 1 }}>{r.status?.toUpperCase()}</span>
                  </div>
                  {r.significance && <div style={{ fontSize: 11, color: "#00f5ff66", lineHeight: 1.6, marginBottom: 4 }}>{r.significance}</div>}
                  {r.causes && <div style={{ fontSize: 10, color: "#00f5ff33" }}><Mono color="#00f5ff22" size={8}>CAUSES: </Mono>{r.causes}</div>}
                </div>
              </div>
            );
          })}
          {[
            ["PATTERN ANALYSIS", get("PATTERN_ANALYSIS"), "#bf5fff"],
            ["RECOMMENDED ACTIONS", get("RECOMMENDED_ACTIONS"), "#39ff14"],
            ["REPEAT TESTING", get("REPEAT_TESTING"), "#00c2ff"],
          ].map(([k, v, c]) => v && (
            <div key={k} style={{ background: "#010a12", borderLeft: `2px solid ${c}`, padding: "12px 16px", marginBottom: 8, marginTop: 8 }}>
              <Mono color={c + "66"} size={8}>{k}</Mono>
              <div style={{ marginTop: 6, fontSize: 11, color: "#00f5ff66", lineHeight: 1.8, whiteSpace: "pre-line" }}>{v}</div>
            </div>
          ))}
        </div>
      )}
    </div>
  );
}

// ── 6. AI PATIENT CHATBOT ─────────────────────────────────────────────────────
function PatientChatbot({ patients }) {
  const [selectedPt, setSelectedPt] = useState(null);
  const [messages, setMessages] = useState([]);
  const [input, setInput] = useState("");
  const [loading, setLoading] = useState(false);
  const bottomRef = useRef(null);

  useEffect(() => { bottomRef.current?.scrollIntoView({ behavior: "smooth" }); }, [messages]);

  const buildContext = (pt) => {
    if (!pt) return "You are a clinical AI assistant.";
    const assessments = (pt.assessments || []).slice(-3);
    const lastA = assessments.slice(-1)[0];
    return `You are an expert clinical AI assistant for a medical system called MedGuide Nexus. 
You are reviewing the case of patient: ${pt.name}, Age: ${pt.age || "unknown"}, Sex: ${pt.gender || "unknown"}.
${lastA ? `Last assessment: ${CONDITIONS[lastA.condition]?.label || lastA.condition}, Severity: ${lastA.severity}, Urgency: ${lastA.urgency}.
Vitals: BP=${lastA.vitals?.bp || "N/A"}, Glucose=${lastA.vitals?.glucose || "N/A"}, BMI=${lastA.vitals?.bmi || "N/A"}.
Symptoms: ${lastA.symptoms?.join(", ") || "none"}.
AI summary: ${lastA.aiDiagnosis || "none"}.` : "No assessment data available."}
Answer clinical questions about this patient concisely and accurately. Be helpful to the clinician but remind them that all clinical decisions require physician oversight. Keep responses under 200 words.`;
  };

  const send = async () => {
    if (!input.trim() || loading) return;
    const userMsg = { role: "user", content: input.trim() };
    const newMessages = [...messages, userMsg];
    setMessages(newMessages); setInput(""); setLoading(true);

    const history = newMessages.map(m => ({ role: m.role, content: m.content }));
    const sys = buildContext(selectedPt);
    const res = await fetch("https://api.anthropic.com/v1/messages", {
      method: "POST", headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ model: "claude-sonnet-4-20250514", max_tokens: 1000, system: sys, messages: history }),
    });
    const d = await res.json();
    const reply = d.content?.[0]?.text || "I was unable to generate a response.";
    setMessages(p => [...p, { role: "assistant", content: reply }]);
    setLoading(false);
  };

  const quickPrompts = selectedPt ? [
    "Summarize this patient's condition",
    "What are the main treatment priorities?",
    "Any drug interactions I should know about?",
    "What follow-up tests are recommended?",
    "Assess risk of complications",
  ] : [];

  return (
    <div className="fu" style={{ display: "flex", flexDirection: "column", height: "calc(100vh - 200px)", minHeight: 500 }}>
      <SLabel color="#ffb800">AI Clinical Chatbot</SLabel>

      {/* Patient selector */}
      <div style={{ marginBottom: 12 }}>
        <label style={{ display: "block", fontFamily: "'Share Tech Mono',monospace", fontSize: 8, color: "#00f5ff33", letterSpacing: 2, marginBottom: 6 }}>SELECT PATIENT CONTEXT</label>
        <select className="hi" value={selectedPt?.id || ""} onChange={e => {
          const pt = patients.find(p => p.id === e.target.value) || null;
          setSelectedPt(pt); setMessages([]);
        }}>
          <option value="">// GENERAL CLINICAL AI (no patient)</option>
          {patients.map(p => <option key={p.id} value={p.id}>{p.name} [{p.id}]</option>)}
        </select>
      </div>

      {/* Quick prompts */}
      {quickPrompts.length > 0 && messages.length === 0 && (
        <div style={{ display: "flex", flexWrap: "wrap", gap: 6, marginBottom: 12 }}>
          {quickPrompts.map(q => (
            <button key={q} onClick={() => { setInput(q); }}
              style={{ background: "#010a12", border: "1px solid #ffb80022", color: "#ffb80066", padding: "5px 12px", fontSize: 10, cursor: "pointer", fontFamily: "'Exo 2',sans-serif", transition: "all .15s" }}
              onMouseOver={e => { e.target.style.borderColor = "#ffb80066"; e.target.style.color = "#ffb800aa"; }}
              onMouseOut={e => { e.target.style.borderColor = "#ffb80022"; e.target.style.color = "#ffb80066"; }}>
              {q}
            </button>
          ))}
        </div>
      )}

      {/* Chat window */}
      <div style={{ flex: 1, overflowY: "auto", background: "#010a12", border: "1px solid #00f5ff0d", padding: "16px", marginBottom: 10, position: "relative" }}>
        {messages.length === 0 && (
          <div style={{ position: "absolute", inset: 0, display: "flex", flexDirection: "column", alignItems: "center", justifyContent: "center", gap: 8 }}>
            <div style={{ fontSize: 32, opacity: 0.3 }}>🤖</div>
            <Mono color="#00f5ff22" size={10}>MEDGUIDE AI CLINICIAN ONLINE</Mono>
            <Mono color="#00f5ff15" size={9}>{selectedPt ? `PATIENT: ${selectedPt.name.toUpperCase()} LOADED` : "// ASK ANY CLINICAL QUESTION"}</Mono>
          </div>
        )}
        {messages.map((m, i) => (
          <div key={i} className={m.role === "user" ? "chat-bubble-user" : "chat-bubble-ai"}
            style={{ display: "flex", gap: 10, marginBottom: 14, justifyContent: m.role === "user" ? "flex-end" : "flex-start" }}>
            {m.role === "assistant" && (
              <div style={{ width: 28, height: 28, borderRadius: 2, background: "#00f5ff15", border: "1px solid #00f5ff22", display: "flex", alignItems: "center", justifyContent: "center", fontSize: 14, flexShrink: 0 }}>⚕</div>
            )}
            <div style={{
              maxWidth: "75%", padding: "10px 14px",
              background: m.role === "user" ? "#0d2a1a" : "#010f1a",
              border: `1px solid ${m.role === "user" ? "#39ff1422" : "#00f5ff15"}`,
              borderRadius: 2, fontSize: 12, lineHeight: 1.7,
              color: m.role === "user" ? "#39ff14aa" : "#00f5ff88",
            }}>
              {m.content}
            </div>
            {m.role === "user" && (
              <div style={{ width: 28, height: 28, borderRadius: 2, background: "#39ff1415", border: "1px solid #39ff1422", display: "flex", alignItems: "center", justifyContent: "center", fontSize: 12, flexShrink: 0 }}>👤</div>
            )}
          </div>
        ))}
        {loading && (
          <div style={{ display: "flex", gap: 10, marginBottom: 14 }}>
            <div style={{ width: 28, height: 28, borderRadius: 2, background: "#00f5ff15", border: "1px solid #00f5ff22", display: "flex", alignItems: "center", justifyContent: "center", fontSize: 14, flexShrink: 0 }}>⚕</div>
            <div style={{ padding: "10px 14px", background: "#010f1a", border: "1px solid #00f5ff15", borderRadius: 2 }}>
              <div style={{ display: "flex", gap: 4 }}>
                {[0, 1, 2].map(i => <div key={i} style={{ width: 4, height: 4, borderRadius: "50%", background: "#00f5ff", animation: `blink 1s ${i * 0.25}s infinite` }} />)}
              </div>
            </div>
          </div>
        )}
        <div ref={bottomRef} />
      </div>

      {/* Input */}
      <div style={{ display: "flex", gap: 8 }}>
        <input className="hi" style={{ flex: 1 }} placeholder="// ASK CLINICAL QUESTION..." value={input}
          onChange={e => setInput(e.target.value)} onKeyDown={e => e.key === "Enter" && !e.shiftKey && send()} />
        <button className="btn-p" disabled={!input.trim() || loading} onClick={send}><span>SEND ▶</span></button>
        {messages.length > 0 && <button className="btn-g" onClick={() => setMessages([])}>CLR</button>}
      </div>
    </div>
  );
}

// ═════════════════════════════════════════════════════════════════════════════
//  MAIN APP
// ═════════════════════════════════════════════════════════════════════════════
function App() {
  const [view, setView] = useState("dashboard");
  const [patients, setPatients] = useState([]);
  const [time, setTime] = useState(new Date());

  useEffect(() => { loadPts().then(setPatients); }, []);
  useEffect(() => { const t = setInterval(() => setTime(new Date()), 1000); return () => clearInterval(t); }, []);

  const allAssessments = patients.flatMap(p => p.assessments || []);
  const emergency = allAssessments.filter(a => a.urgency === "emergency").length;

  const navGroups = [
    {
      label: "CORE", items: [
        { id: "dashboard", icon: "⊞", label: "DASHBOARD" },
        { id: "records",   icon: "⬡", label: "PATIENT REGISTRY" },
      ]
    },
    {
      label: "AI MODULES", items: [
        { id: "drug_check",    icon: "⚗", label: "DRUG INTERACTIONS", color: "#ff9a3c" },
        { id: "diff_dx",       icon: "🧠", label: "DIFFERENTIAL DX",   color: "#bf5fff" },
        { id: "risk_calc",     icon: "📊", label: "RISK CALCULATOR",   color: "#ff3a3a" },
        { id: "dosage_calc",   icon: "💊", label: "DOSAGE CALCULATOR", color: "#39ff14" },
        { id: "lab_interp",    icon: "🔬", label: "LAB INTERPRETER",   color: "#00c2ff" },
        { id: "chatbot",       icon: "🤖", label: "AI CHATBOT",        color: "#ffb800" },
      ]
    }
  ];

  const Dashboard = () => {
    const condCounts = {};
    allAssessments.forEach(a => { condCounts[a.condition] = (condCounts[a.condition] || 0) + 1; });
    const recent = [...allAssessments].sort((a, b) => new Date(b.date) - new Date(a.date)).slice(0, 5);

    return (
      <div className="fu">
        <div style={{ display: "flex", justifyContent: "space-between", alignItems: "flex-start", marginBottom: 28 }}>
          <div>
            <Mono color="#00f5ff22" size={9}>MEDGUIDE NEXUS v4.0 // AI CLINICAL SUITE</Mono>
            <h1 style={{ fontFamily: "'Exo 2',sans-serif", fontWeight: 900, fontSize: 26, color: "#00f5ff", letterSpacing: "-0.5px", marginTop: 4, textShadow: "0 0 30px #00f5ff44" }}>SYSTEM DASHBOARD</h1>
          </div>
        </div>

        <div style={{ display: "grid", gridTemplateColumns: "repeat(4,1fr)", gap: 10, marginBottom: 20 }}>
          {[
            { icon: "👥", v: patients.length,       l: "Patients",    c: "#00f5ff" },
            { icon: "📋", v: allAssessments.length, l: "Assessments", c: "#39ff14" },
            { icon: "🚨", v: emergency,             l: "Critical",    c: "#ff2a2a" },
            { icon: "⚗",  v: 6,                    l: "AI Modules",  c: "#bf5fff" },
          ].map((s, i) => (
            <div key={i} className={`panel brk fu${i}`} style={{ padding: "18px 14px", textAlign: "center" }}>
              <div style={{ fontSize: 22, marginBottom: 6 }}>{s.icon}</div>
              <div style={{ fontFamily: "'Share Tech Mono',monospace", fontSize: 26, color: s.c, textShadow: `0 0 16px ${s.c}66` }}>{s.v}</div>
              <Mono color="#00f5ff22" size={9}>{s.l}</Mono>
            </div>
          ))}
        </div>

        <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: 14, marginBottom: 14 }}>
          {/* Recent */}
          <div className="panel" style={{ padding: "18px" }}>
            <SLabel>Recent Assessments</SLabel>
            {recent.length === 0 && <Mono color="#00f5ff15" size={10}>// NO RECORDS — START A NEW SCAN</Mono>}
            {recent.map((a, i) => {
              const pt = patients.find(p => (p.assessments || []).some(x => x.id === a.id));
              const c = CONDITIONS[a.condition]; const u = urgCfg[a.urgency] || urgCfg.routine;
              return (
                <div key={i} style={{ display: "flex", alignItems: "center", gap: 10, padding: "8px 0", borderBottom: "1px solid #00f5ff06", cursor: "pointer" }}>
                  <div style={{ width: 6, height: 6, borderRadius: "50%", background: u.color, flexShrink: 0 }} />
                  <span style={{ fontSize: 14 }}>{c?.icon}</span>
                  <div style={{ flex: 1, minWidth: 0 }}>
                    <Mono color="#00f5ffaa" size={11}>{pt?.name?.toUpperCase()}</Mono>
                    <div style={{ fontSize: 9, color: "#00f5ff22", letterSpacing: 1 }}>{c?.label} · {new Date(a.date).toLocaleDateString()}</div>
                  </div>
                  <span style={{ fontFamily: "'Share Tech Mono',monospace", fontSize: 8, color: u.color, border: `1px solid ${u.color}33`, padding: "1px 6px" }}>{a.severity?.toUpperCase()}</span>
                </div>
              );
            })}
          </div>

          {/* AI Modules quick access */}
          <div className="panel" style={{ padding: "18px" }}>
            <SLabel color="#bf5fff">AI Module Access</SLabel>
            <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: 8 }}>
              {navGroups[1].items.map(n => (
                <button key={n.id} onClick={() => setView(n.id)}
                  style={{ background: "#010a12", border: `1px solid ${n.color}22`, padding: "10px", cursor: "pointer", textAlign: "left", transition: "all .2s" }}
                  onMouseOver={e => e.currentTarget.style.borderColor = n.color + "66"}
                  onMouseOut={e => e.currentTarget.style.borderColor = n.color + "22"}>
                  <div style={{ fontSize: 18, marginBottom: 4 }}>{n.icon}</div>
                  <Mono color={n.color + "88"} size={8}>{n.label}</Mono>
                </button>
              ))}
            </div>
          </div>
        </div>

        {/* Condition breakdown */}
        {Object.keys(condCounts).length > 0 && (
          <div className="panel" style={{ padding: "18px" }}>
            <SLabel>Condition Prevalence</SLabel>
            <div style={{ display: "grid", gridTemplateColumns: "repeat(3,1fr)", gap: 12 }}>
              {Object.entries(condCounts).sort((a, b) => b[1] - a[1]).map(([k, v]) => {
                const c = CONDITIONS[k]; const pct = Math.round((v / allAssessments.length) * 100);
                return (
                  <div key={k}>
                    <div style={{ display: "flex", justifyContent: "space-between", marginBottom: 4 }}>
                      <span style={{ fontSize: 10, color: "#00f5ff44" }}>{c?.icon} {c?.label}</span>
                      <Mono color={c?.color} size={10}>{pct}%</Mono>
                    </div>
                    <div style={{ height: 3, background: "#00f5ff08" }}>
                      <div style={{ height: 3, width: `${pct}%`, background: c?.color, boxShadow: `0 0 6px ${c?.color}`, transition: "width 1s" }} />
                    </div>
                  </div>
                );
              })}
            </div>
          </div>
        )}
      </div>
    );
  };

  const Records = () => {
    const [q, setQ] = useState("");
    const filtered = patients.filter(p => p.name.toLowerCase().includes(q.toLowerCase()));
    return (
      <div className="fu">
        <div style={{ display: "flex", justifyContent: "space-between", alignItems: "flex-start", marginBottom: 20 }}>
          <div>
            <Mono color="#00f5ff22" size={9}>SECURE DATABASE // {patients.length} RECORDS</Mono>
            <h1 style={{ fontFamily: "'Exo 2',sans-serif", fontWeight: 900, fontSize: 24, color: "#00f5ff", marginTop: 4 }}>PATIENT REGISTRY</h1>
          </div>
        </div>
        <input className="hi" style={{ maxWidth: 340, marginBottom: 14 }} placeholder="// SEARCH REGISTRY..." value={q} onChange={e => setQ(e.target.value)} />
        {filtered.length === 0 && <div className="panel" style={{ padding: "40px", textAlign: "center" }}><Mono color="#00f5ff15" size={11}>// NO RECORDS FOUND</Mono></div>}
        {filtered.map(pt => {
          const last = (pt.assessments || []).slice(-1)[0];
          const c = last ? CONDITIONS[last.condition] : null;
          const u = last ? urgCfg[last.urgency] || urgCfg.routine : null;
          return (
            <div key={pt.id} className="panel" style={{ padding: "14px 18px", marginBottom: 8, cursor: "pointer" }}>
              <div style={{ display: "flex", alignItems: "center", gap: 14 }}>
                <div style={{ width: 38, height: 38, borderRadius: 2, background: "#010a12", border: "1px solid #00f5ff15", display: "flex", alignItems: "center", justifyContent: "center", fontFamily: "'Share Tech Mono',monospace", fontSize: 16, color: "#00f5ff", flexShrink: 0 }}>
                  {pt.name[0]?.toUpperCase()}
                </div>
                <div style={{ flex: 1 }}>
                  <Mono color="#00f5ffcc" size={13}>{pt.name.toUpperCase()}</Mono>
                  <div style={{ fontFamily: "'Share Tech Mono',monospace", fontSize: 9, color: "#00f5ff33", marginTop: 2, letterSpacing: 1 }}>
                    ID: {pt.id}{pt.age && ` // AGE ${pt.age}`}{pt.gender && ` // ${pt.gender.toUpperCase()}`}
                  </div>
                </div>
                {c && u && (
                  <div style={{ textAlign: "right" }}>
                    <div style={{ fontSize: 11, color: c.color, marginBottom: 2 }}>{c.icon} {c.label}</div>
                    <Mono color={u.color} size={9}>{u.label.split(" ")[0]}</Mono>
                    <div style={{ fontFamily: "'Share Tech Mono',monospace", fontSize: 9, color: "#00f5ff22" }}>{new Date(pt.lastVisit).toLocaleDateString()}</div>
                  </div>
                )}
              </div>
            </div>
          );
        })}
      </div>
    );
  };

  const pageTitle = {
    dashboard: "DASHBOARD", records: "PATIENT REGISTRY",
    drug_check: "DRUG INTERACTION CHECKER", diff_dx: "DIFFERENTIAL DIAGNOSIS",
    risk_calc: "RISK CALCULATOR", dosage_calc: "DOSAGE CALCULATOR",
    lab_interp: "LAB INTERPRETER", chatbot: "AI CLINICAL CHATBOT",
  };

  return (
    <>
      <GlobalStyles />
      <div style={{ position: "fixed", inset: 0, pointerEvents: "none", zIndex: 0 }}>
        <div style={{ position: "absolute", inset: 0, background: "radial-gradient(circle at 15% 50%,#00f5ff06 0%,transparent 45%),radial-gradient(circle at 85% 20%,#39ff1403 0%,transparent 40%)" }} />
        {/* dot grid */}
        <svg style={{ position: "absolute", inset: 0, width: "100%", height: "100%", opacity: 0.035 }}>
          <defs><pattern id="g" x="0" y="0" width="20" height="20" patternUnits="userSpaceOnUse"><circle cx="1" cy="1" r="1" fill="#00f5ff" /></pattern></defs>
          <rect width="100%" height="100%" fill="url(#g)" />
        </svg>
      </div>

      <div style={{ display: "flex", minHeight: "100vh", position: "relative", zIndex: 1 }}>
        {/* ── Sidebar ── */}
        <nav className="no-print" style={{ width: 200, background: "#010810", borderRight: "1px solid #00f5ff0d", display: "flex", flexDirection: "column", flexShrink: 0, position: "sticky", top: 0, height: "100vh", overflowY: "auto" }}>
          {/* Logo */}
          <div style={{ padding: "20px 14px 16px", borderBottom: "1px solid #00f5ff08" }}>
            <div style={{ display: "flex", alignItems: "center", gap: 8, marginBottom: 3 }}>
              <div style={{ width: 28, height: 28, borderRadius: 2, background: "linear-gradient(135deg,#00f5ff,#39ff14)", display: "flex", alignItems: "center", justifyContent: "center", fontSize: 13, fontWeight: 900, color: "#000" }}>✚</div>
              <span style={{ fontFamily: "'Exo 2',sans-serif", fontWeight: 900, fontSize: 13, color: "#00f5ff", letterSpacing: 1 }}>MEDGUIDE</span>
            </div>
            <Mono color="#00f5ff22" size={7}>NEXUS v4.0 // AI SUITE</Mono>
          </div>

          {/* Clock */}
          <div style={{ padding: "10px 14px", borderBottom: "1px solid #00f5ff06" }}>
            <Mono color="#00f5ff" size={14}>{time.toLocaleTimeString()}</Mono>
            <div style={{ marginTop: 2 }}><Mono color="#00f5ff33" size={8}>{time.toLocaleDateString()}</Mono></div>
          </div>

          {/* Nav */}
          <div style={{ flex: 1, padding: "8px" }}>
            {navGroups.map(g => (
              <div key={g.label} style={{ marginBottom: 8 }}>
                <div style={{ padding: "6px 10px 3px" }}><Mono color="#00f5ff15" size={7}>{`// ${g.label}`}</Mono></div>
                {g.items.map(n => (
                  <button key={n.id} className={`nav-btn${view === n.id ? " active" : ""}`}
                    style={{ borderLeftColor: view === n.id ? (n.color || "#00f5ff") : "transparent", color: view === n.id ? (n.color || "#00f5ff") : "#00f5ff33" }}
                    onClick={() => setView(n.id)}>
                    <span style={{ fontSize: 13 }}>{n.icon}</span>
                    <span>{n.label}</span>
                  </button>
                ))}
              </div>
            ))}
          </div>

          {/* Status */}
          <div style={{ padding: "12px", borderTop: "1px solid #00f5ff08" }}>
            <div style={{ background: "#000c12", border: "1px solid #39ff1415", padding: "10px 12px", borderRadius: 2 }}>
              <div style={{ display: "flex", alignItems: "center", gap: 5, marginBottom: 4 }}>
                <div style={{ width: 5, height: 5, borderRadius: "50%", background: "#39ff14", boxShadow: "0 0 6px #39ff14", animation: "blink 2s infinite" }} />
                <Mono color="#39ff14" size={7}>6 AI MODULES ONLINE</Mono>
              </div>
              <Mono color="#00f5ff15" size={7}>⚠ NOT FOR CLINICAL USE</Mono>
            </div>
          </div>
        </nav>

        {/* ── Main ── */}
        <main style={{ flex: 1, padding: "28px 32px", overflowY: "auto" }}>
          {/* Page header */}
          <div style={{ marginBottom: 20, paddingBottom: 14, borderBottom: "1px solid #00f5ff08" }}>
            <Mono color="#00f5ff22" size={8}>{"// " + pageTitle[view]}</Mono>
          </div>

          {/* Disclaimer */}
          <div style={{ background: "#100800", border: "1px solid #ffb80022", padding: "8px 14px", marginBottom: 18, display: "flex", gap: 10, alignItems: "center" }}>
            <span style={{ color: "#ffb800", fontSize: 12 }}>⚠</span>
            <Mono color="#ffb80055" size={8}>MEDGUIDE AI IS FOR INFORMATIONAL USE ONLY. ALL CLINICAL DECISIONS REQUIRE LICENSED PHYSICIAN OVERSIGHT.</Mono>
          </div>

          <div style={{ maxWidth: 900 }}>
            {view === "dashboard"  && <Dashboard />}
            {view === "records"    && <Records />}
            {view === "drug_check" && <DrugInteractionChecker />}
            {view === "diff_dx"    && <DifferentialDiagnosis />}
            {view === "risk_calc"  && <RiskCalculator />}
            {view === "dosage_calc"&& <DosageCalculator />}
            {view === "lab_interp" && <LabInterpreter />}
            {view === "chatbot"    && <PatientChatbot patients={patients} />}
          </div>
        </main>
      </div>
    </>
  );
}


// ─── MOUNT ────────────────────────────────────────────────────────────────
const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(<App />);
</script>
</body>
</html>
