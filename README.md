<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0"/>
<title>-CONNECTADON.</title>
<link href="https://fonts.googleapis.com/css2?family=Barlow+Condensed:wght@400;600;700;800;900&family=Barlow:wght@300;400;500;600;700&family=DM+Mono:wght@400;500&display=swap" rel="stylesheet"/>
<script src="https://cdnjs.cloudflare.com/ajax/libs/react/18.2.0/umd/react.production.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/react-dom/18.2.0/umd/react-dom.production.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/babel-standalone/7.23.2/babel.min.js"></script>
<style>
*{box-sizing:border-box;margin:0;padding:0;-webkit-tap-highlight-color:transparent;}
body{background:#fff;font-family:'Barlow',sans-serif;overflow-x:hidden;}
#root{display:flex;justify-content:center;min-height:100vh;background:#f0f0f0;}
.app-shell{width:100%;max-width:430px;background:#fff;min-height:100vh;position:relative;overflow:hidden;box-shadow:0 0 40px rgba(0,0,0,0.1);}
@keyframes fadeUp{from{opacity:0;transform:translateY(14px)}to{opacity:1;transform:translateY(0)}}
@keyframes fadeIn{from{opacity:0}to{opacity:1}}
@keyframes slideIn{from{opacity:0;transform:translateX(22px)}to{opacity:1;transform:translateX(0)}}
@keyframes slideUp{from{opacity:0;transform:translateY(28px)}to{opacity:1;transform:translateY(0)}}
@keyframes spin{to{transform:rotate(360deg)}}
@keyframes pulse{0%,100%{opacity:1}50%{opacity:0.3}}
@keyframes pop{0%{transform:scale(0)}70%{transform:scale(1.2)}100%{transform:scale(1)}}
@keyframes logoIn{0%{opacity:0;transform:scale(0.78)}100%{opacity:1;transform:scale(1)}}
@keyframes dotMove{0%{transform:translateX(0);opacity:0.15}50%{transform:translateX(14px);opacity:1}100%{transform:translateX(28px);opacity:0.15}}
@keyframes lineGrow{from{width:0}to{width:100%}}
@keyframes tagIn{from{opacity:0;letter-spacing:8px}to{opacity:1;letter-spacing:3px}}
@keyframes feedIn{from{opacity:0;transform:translateY(-6px)}to{opacity:1;transform:translateY(0)}}
@keyframes barFill{from{width:0}to{width:var(--w)}}
input::-webkit-inner-spin-button,input::-webkit-outer-spin-button{-webkit-appearance:none;}
input[type=number]{-moz-appearance:textfield;}
::-webkit-scrollbar{width:0;height:0;}
</style>
</head>
<body>
<div id="root"></div>
<script type="text/babel">
const { useState, useEffect, useRef } = React;

// ── TOKENS ──────────────────────────────────────────────────────────────────
const C = {
  black:"#0A0A0A", white:"#FFFFFF", off:"#F8F8F8",
  light:"#F2F2F2", border:"#E4E4E4", muted:"#888",
  gold:"#C9A227", goldD:"#A8841A", goldP:"rgba(201,162,39,0.11)", goldB:"rgba(201,162,39,0.30)",
  green:"#00C853", greenP:"rgba(0,200,83,0.10)", greenB:"rgba(0,200,83,0.24)",
  red:"#E53935", redP:"rgba(229,57,53,0.10)",
};

const fmt = n => `R${Number(n).toLocaleString("en-ZA")}`;

// ── MARGINS ──────────────────────────────────────────────────────────────────
const MARGINS = [
  {cat:"Phone Accessories",emoji:"🔌",min:45,max:60,why:"Buy at R30–R60 wholesale, sell at R120–R250. Mass market, fast turnover.",products:["Chargers","Cables","Cases","Screen Protectors","Power Banks"]},
  {cat:"Fragrances",emoji:"🧴",min:40,max:55,why:"Wholesale fragrance prices are 50–70% below retail. Premium scents.",products:["Dior","Versace","Gucci","Hugo Boss","Calvin Klein"]},
  {cat:"Sunglasses",emoji:"🕶️",min:40,max:55,why:"Fashion frames at R80–R150 wholesale sell at R300–R600 easily.",products:["Ray-Ban","Oakley","Fashion Frames","Sport Shades"]},
  {cat:"Jewellery",emoji:"📿",min:35,max:50,why:"Low wholesale cost, high perceived value. Fast moving items.",products:["Chains","Bracelets","Rings","Earrings"]},
  {cat:"Clothing",emoji:"👕",min:30,max:45,why:"Strong fast fashion margins. Branded items lower.",products:["T-Shirts","Hoodies","Joggers","Dresses","Chinos"]},
  {cat:"Sneakers",emoji:"👟",min:25,max:40,why:"Non-branded and mid-tier styles have strong margins.",products:["Nike","Adidas","Puma","Fashion Sneakers"]},
  {cat:"Headphones",emoji:"🎧",min:25,max:38,why:"Mid-tier audio brands sourced from importers at 40–50% below retail.",products:["Sony","JBL","Samsung","Xiaomi Audio"]},
  {cat:"Smart Watches",emoji:"⌚",min:28,max:40,why:"Growing demand with strong wholesale-to-retail gap.",products:["Samsung Watch","Xiaomi Band","Fitness Trackers"]},
  {cat:"Tablets",emoji:"📟",min:22,max:32,why:"Decent margins on mid-tier Android tablets.",products:["Samsung Tab","Xiaomi Pad","Huawei Tab"]},
  {cat:"Laptops",emoji:"💻",min:20,max:28,why:"Lower markup but higher ticket price makes pools compelling.",products:["Dell","HP","Acer","Lenovo"]},
  {cat:"Phones",emoji:"📱",min:20,max:28,why:"Thinnest margins but highest trust product. Large pool volumes.",products:["iPhone","Samsung","Xiaomi","Huawei"]},
];

// ── POOL DATA ────────────────────────────────────────────────────────────────
const POOLS = [
  {id:1,name:"Mobile Phones",emoji:"📱",cat:"Electronics",min:500,max:5000,funded:33000,target:80000,profit:24,members:142,days:18,stage:2,
   supplier:"Cape Tech Wholesale · Verified · 22 previous orders",
   receipt:{date:"14 Apr 2025",items:"84x Phone cases, 120x Screen protectors, 60x Chargers",cost:"R28 400",supplier:"Cape Tech Wholesale (Pty) Ltd",ref:"CTW-2025-0414"},
   feed:[{time:"2hr ago",event:"12 units sold — Takealot marketplace",icon:"🛒"},{time:"5hr ago",event:"Stock delivery confirmed — 264 units",icon:"📦"},{time:"1d ago",event:"Wholesale purchase completed — R28 400",icon:"✅"},{time:"2d ago",event:"Pool target reached — buying commenced",icon:"🎯"},{time:"3d ago",event:"Tshepo M. contributed R2 000",icon:"💰"}]},
  {id:2,name:"Sneakers & Footwear",emoji:"👟",cat:"Fashion",min:500,max:5000,funded:69000,target:80000,profit:30,members:289,days:6,stage:3,
   supplier:"SA Footwear Imports · Verified · 8 previous orders",
   receipt:{date:"08 Apr 2025",items:"180x Nike AF1, 90x Adidas Superstar, 60x Puma RS-X",cost:"R58 200",supplier:"SA Footwear Imports CC",ref:"SAFI-2025-0408"},
   feed:[{time:"30min ago",event:"47 units sold — Connectadon marketplace",icon:"🛒"},{time:"3hr ago",event:"89 units sold — WhatsApp reseller network",icon:"🛒"},{time:"1d ago",event:"Lerato K. contributed R3 500",icon:"💰"}]},
  {id:3,name:"Fragrances",emoji:"🧴",cat:"Beauty",min:250,max:3000,funded:19000,target:60000,profit:42,members:97,days:22,stage:1,
   supplier:"Luxury Scents SA · Verified · 5 previous orders",receipt:null,
   feed:[{time:"1hr ago",event:"Sipho N. contributed R1 000",icon:"💰"},{time:"4hr ago",event:"Pool opened — contributions live",icon:"🎉"}]},
  {id:4,name:"Headphones & Audio",emoji:"🎧",cat:"Electronics",min:300,max:4000,funded:12000,target:50000,profit:30,members:63,days:30,stage:0,
   supplier:"Tech Sound Distributors · Pending verification",receipt:null,
   feed:[{time:"2hr ago",event:"Dineo M. contributed R2 000",icon:"💰"},{time:"5hr ago",event:"Pool created — open for contributions",icon:"🎉"}]},
  {id:5,name:"Smart Watches",emoji:"⌚",cat:"Electronics",min:800,max:6000,funded:8000,target:70000,profit:34,members:41,days:45,stage:0,
   supplier:"Wearable World SA · Verified · 3 previous orders",receipt:null,
   feed:[{time:"1d ago",event:"Pool opened — contributions live",icon:"🎉"}]},
];

const CYCLE_STAGES = [
  {label:"Pool Open",sub:"Accepting contributions",icon:"🟢"},
  {label:"Target Reached",sub:"Buying pool filled",icon:"🎯"},
  {label:"Stock Purchased",sub:"Products bought",icon:"📦"},
  {label:"Selling",sub:"Products on market",icon:"🛒"},
  {label:"Cycle Closing",sub:"Final sales",icon:"⏰"},
  {label:"Payouts Sent",sub:"Profit distributed",icon:"✅"},
];

const MY_POOLS = [
  {id:1,name:"Mobile Phones",emoji:"📱",contributed:3000,share:720,status:"active",profit:24},
  {id:2,name:"Fragrances",emoji:"🧴",contributed:1000,share:420,status:"paid",profit:42},
];

const HISTORY = [
  {id:1,name:"Mobile Phones",emoji:"📱",date:"12 Apr 2025",contributed:3000,share:720,status:"active",cycleEnd:"30 Apr 2025"},
  {id:2,name:"Fragrances",emoji:"🧴",date:"02 Mar 2025",contributed:1000,share:420,status:"paid",paidOn:"18 Mar 2025",ref:"SPO-2025-0318"},
  {id:3,name:"Sneakers",emoji:"👟",date:"10 Jan 2025",contributed:2500,share:750,status:"paid",paidOn:"28 Jan 2025",ref:"SPO-2025-0128"},
];

const NOTIFS_INIT = [
  {id:1,icon:"💰",title:"Profit Share Paid Out!",body:"Fragrances pool closed. R420 credited. Ref: SPO-2025-0318",time:"2 min ago",read:false},
  {id:2,icon:"⏰",title:"Pool Closing Soon",body:"Sneakers pool closes in 6 days. 86% funded!",time:"1 hr ago",read:false},
  {id:3,icon:"🛒",title:"Products Selling Fast",body:"47 Sneaker units sold today. Your profit share is growing.",time:"3 hrs ago",read:false},
  {id:4,icon:"📦",title:"Stock Purchased!",body:"Mobile Phones: 264 units bought from Cape Tech Wholesale.",time:"1 day ago",read:true},
  {id:5,icon:"🔗",title:"Referral Reward!",body:"Thabo M. contributed. R100 added to your balance.",time:"Yesterday",read:true},
  {id:6,icon:"🆕",title:"New Pool: Smart Watches",body:"Open now. Up to 40% profit share from R800.",time:"3 days ago",read:true},
];

const TECH_PRODUCTS = [
  {id:1,name:"iPhone 14 Pro",brand:"Apple",cat:"Phones",price:8999,was:12000,emoji:"📱",badge:"Best Seller",sizes:[]},
  {id:2,name:"Samsung Galaxy S23",brand:"Samsung",cat:"Phones",price:7999,was:11000,emoji:"📱",badge:"Hot",sizes:[]},
  {id:3,name:"Xiaomi Redmi Note 12",brand:"Xiaomi",cat:"Phones",price:2999,was:4500,emoji:"📱",badge:"Value",sizes:[]},
  {id:4,name:"Dell Latitude i5",brand:"Dell",cat:"Laptops",price:6399,was:9000,emoji:"💻",badge:"",sizes:[]},
  {id:5,name:"MacBook Air M2",brand:"Apple",cat:"Laptops",price:18999,was:24000,emoji:"💻",badge:"Premium",sizes:[]},
  {id:6,name:"Sony WH-1000XM5",brand:"Sony",cat:"Earphones",price:4999,was:7500,emoji:"🎧",badge:"Top Rated",sizes:[]},
  {id:7,name:"AirPods Pro 2",brand:"Apple",cat:"Earphones",price:4499,was:6500,emoji:"🎧",badge:"",sizes:[]},
  {id:8,name:"Samsung Tab S8",brand:"Samsung",cat:"Tablets",price:8999,was:12000,emoji:"📟",badge:"",sizes:[]},
  {id:9,name:"65W GaN Charger",brand:"Xiaomi",cat:"Accessories",price:299,was:500,emoji:"🔌",badge:"",sizes:[]},
  {id:10,name:"USB-C Hub 7-in-1",brand:"Huawei",cat:"Accessories",price:399,was:700,emoji:"🔌",badge:"",sizes:[]},
];

const LIFE_PRODUCTS = [
  {id:101,name:"Nike Air Force 1",brand:"Nike",cat:"Sneakers",price:1299,was:1800,emoji:"👟",badge:"Hot",sizes:["36","37","38","39","40","41","42","43","44","45"]},
  {id:102,name:"Adidas Superstar",brand:"Adidas",cat:"Sneakers",price:1199,was:1700,emoji:"👟",badge:"",sizes:["36","37","38","39","40","41","42","43","44","45"]},
  {id:103,name:"Nike Jordan 1 Retro",brand:"Nike",cat:"Sneakers",price:2499,was:3500,emoji:"👟",badge:"Limited",sizes:["36","37","38","39","40","41","42","43","44","45"]},
  {id:104,name:"Dior Sauvage 100ml",brand:"Dior",cat:"Scents",price:1499,was:2200,emoji:"🧴",badge:"",sizes:[]},
  {id:105,name:"Gucci Flora 50ml",brand:"Gucci",cat:"Scents",price:1799,was:2600,emoji:"🧴",badge:"Premium",sizes:[]},
  {id:106,name:"Ray-Ban Aviator",brand:"Ray-Ban",cat:"Sunglasses",price:1899,was:3200,emoji:"🕶️",badge:"Premium",sizes:[]},
  {id:107,name:"Oakley Holbrook",brand:"Oakley",cat:"Sunglasses",price:1599,was:2400,emoji:"🕶️",badge:"",sizes:[]},
  {id:108,name:"Louis Vuitton Tote",brand:"Louis Vuitton",cat:"Bags",price:8999,was:13000,emoji:"👜",badge:"Luxury",sizes:[]},
  {id:109,name:"Nike Backpack",brand:"Nike",cat:"Bags",price:799,was:1200,emoji:"🎒",badge:"",sizes:[]},
  {id:110,name:"H&M Oversized Tee",brand:"H&M",cat:"Clothing",price:249,was:400,emoji:"👕",badge:"",sizes:["XS","S","M","L","XL","XXL"]},
  {id:111,name:"Zara Floral Dress",brand:"Zara",cat:"Clothing",price:699,was:1100,emoji:"👗",badge:"New",sizes:["XS","S","M","L","XL","XXL"]},
  {id:112,name:"Gold Chain Necklace",brand:"Gucci",cat:"Jewellery",price:1299,was:2000,emoji:"📿",badge:"",sizes:[]},
];

const WEEK_DATA = [1200,1800,2400,2200,3100,8700,6200];
const WDAYS = ["M","T","W","T","F","S","S"];

// ── SHARED ───────────────────────────────────────────────────────────────────
const Bar = ({val,max,color=C.green}) => {
  const pct = Math.min((val/max)*100,100);
  return (
    <div style={{background:C.light,borderRadius:99,height:4,overflow:"hidden"}}>
      <div style={{width:`${pct}%`,background:color,height:"100%",borderRadius:99,transition:"width 0.7s"}}/>
    </div>
  );
};

const Tag = ({children,type="default"}) => {
  const s = {
    default:{bg:C.light,color:C.muted,border:"none"},
    dark:{bg:C.black,color:C.white,border:"none"},
    gold:{bg:C.goldP,color:C.gold,border:`1px solid ${C.goldB}`},
    green:{bg:C.greenP,color:C.green,border:`1px solid ${C.greenB}`},
    red:{bg:C.redP,color:C.red,border:"none"},
  };
  const st = s[type]||s.default;
  return <span style={{background:st.bg,color:st.color,border:st.border,fontSize:9,fontWeight:800,padding:"3px 8px",borderRadius:3,letterSpacing:0.8,textTransform:"uppercase",fontFamily:"'Barlow Condensed',sans-serif",whiteSpace:"nowrap"}}>{children}</span>;
};

const Card = ({children,style={},onClick}) => (
  <div onClick={onClick} style={{background:C.white,border:`1px solid ${C.border}`,borderRadius:14,padding:16,...style,cursor:onClick?"pointer":"default"}}>
    {children}
  </div>
);

const BlackBtn = ({children,onClick,disabled,style={}}) => (
  <button onClick={onClick} disabled={disabled}
    style={{width:"100%",background:disabled?C.light:C.black,color:disabled?C.muted:C.white,border:"none",borderRadius:10,padding:"14px",fontSize:13,fontWeight:800,cursor:disabled?"default":"pointer",fontFamily:"'Barlow Condensed',sans-serif",letterSpacing:1.2,textTransform:"uppercase",display:"flex",alignItems:"center",justifyContent:"center",gap:8,transition:"all 0.2s",...style}}>
    {children}
  </button>
);

const BackBtn = ({onBack,label="Back"}) => (
  <button onClick={onBack} style={{display:"inline-flex",alignItems:"center",gap:6,background:"transparent",border:`1px solid ${C.border}`,borderRadius:8,padding:"7px 13px",color:C.black,fontSize:12,cursor:"pointer",marginBottom:16,fontFamily:"'Barlow',sans-serif",fontWeight:600}}>
    ← {label}
  </button>
);

const SLabel = ({children,style={}}) => (
  <div style={{fontFamily:"'Barlow Condensed',sans-serif",fontWeight:800,fontSize:11,color:C.muted,letterSpacing:2,textTransform:"uppercase",marginBottom:10,...style}}>{children}</div>
);

const Toggle = ({val,onChange}) => (
  <div onClick={()=>onChange(!val)} style={{width:42,height:24,borderRadius:99,background:val?C.black:C.light,position:"relative",cursor:"pointer",transition:"background 0.3s",flexShrink:0}}>
    <div style={{position:"absolute",top:3,left:val?19:3,width:18,height:18,borderRadius:"50%",background:val?C.gold:"#bbb",transition:"left 0.3s"}}/>
  </div>
);

const Spin = () => <div style={{width:15,height:15,border:"2px solid rgba(255,255,255,0.3)",borderTopColor:"#fff",borderRadius:"50%",animation:"spin 0.7s linear infinite"}}/>;

const Logo = ({size=18,light=false}) => (
  <div style={{fontFamily:"'Barlow Condensed',sans-serif",fontWeight:900,fontSize:size,color:light?C.white:C.black,letterSpacing:-0.3,lineHeight:1,userSelect:"none"}}>
    -CONNECTADON.
  </div>
);

const CIcon = ({size=56,light=false}) => (
  <svg width={size} height={size} viewBox="0 0 100 100" fill="none">
    <path d="M62 28C58 24 53 22 47 22C35 22 25 32 25 50C25 68 35 78 47 78C53 78 58 76 62 72" stroke={light?"#fff":C.black} strokeWidth="20" strokeLinecap="round" fill="none"/>
    <rect x="10" y="46" width="16" height="8" rx="4" fill={light?"#fff":C.black}/>
    <circle cx="76" cy="72" r="6" fill={light?"#fff":C.black}/>
  </svg>
);

// ── SPLASH ───────────────────────────────────────────────────────────────────
const Splash = ({onDone}) => {
  const [p,setP] = useState(0);
  useEffect(()=>{
    const t1=setTimeout(()=>setP(1),300);
    const t2=setTimeout(()=>setP(2),1000);
    const t3=setTimeout(()=>setP(3),1800);
    const t4=setTimeout(()=>onDone(),3000);
    return()=>{[t1,t2,t3,t4].forEach(clearTimeout);};
  },[]);
  return (
    <div style={{position:"fixed",inset:0,background:C.black,display:"flex",flexDirection:"column",alignItems:"center",justifyContent:"center",zIndex:1000}}>
      <div style={{position:"absolute",inset:0,backgroundImage:`linear-gradient(rgba(201,162,39,0.04) 1px,transparent 1px),linear-gradient(90deg,rgba(201,162,39,0.04) 1px,transparent 1px)`,backgroundSize:"40px 40px"}}/>
      <div style={{position:"absolute",inset:0,background:"radial-gradient(ellipse at 50% 40%,rgba(201,162,39,0.08) 0%,transparent 60%)"}}/>
      <div style={{position:"relative",zIndex:1,textAlign:"center",display:"flex",flexDirection:"column",alignItems:"center"}}>
        <div style={{animation:p>=1?"logoIn 0.7s ease both":"none",opacity:p>=1?1:0,marginBottom:24}}>
          <CIcon size={72} light={true}/>
        </div>
        {p>=2&&(
          <div style={{display:"flex",alignItems:"center",justifyContent:"center",gap:2,marginBottom:20,animation:"fadeIn 0.4s ease both"}}>
            {[0,1,2,3,4].map(i=>(
              <div key={i} style={{display:"flex",alignItems:"center"}}>
                <div style={{width:i===2?9:6,height:i===2?9:6,borderRadius:"50%",background:i===2?C.gold:"rgba(255,255,255,0.2)",animation:`dotMove 1.3s ${i*0.14}s ease-in-out infinite`}}/>
                {i<4&&<div style={{width:12,height:1,background:"rgba(255,255,255,0.08)",margin:"0 2px"}}/>}
              </div>
            ))}
          </div>
        )}
        {p>=2&&(
          <div style={{animation:"fadeUp 0.5s 0.1s ease both"}}>
            <Logo size={22} light={true}/>
            <div style={{fontSize:9,color:"rgba(255,255,255,0.3)",letterSpacing:3,textTransform:"uppercase",marginTop:8,fontFamily:"'Barlow',sans-serif",animation:p>=3?"tagIn 0.6s ease both":"none"}}>
              Tech & Lifestyle, Elevated.
            </div>
          </div>
        )}
      </div>
      {p>=3&&<div style={{position:"absolute",bottom:0,left:0,right:0,height:3,background:C.gold,animation:"lineGrow 0.5s ease both"}}/>}
    </div>
  );
};

// ── ONBOARDING ───────────────────────────────────────────────────────────────
const SLIDES = [
  {icon:"🤝",title:"Welcome to\n-CONNECTADON.",sub:"South Africa's community buying platform. Pool together, buy smarter, share the profit.",gold:false},
  {icon:"🛍️",title:"Shop Tech &\nLifestyle.",sub:"Browse phones, laptops, sneakers, fragrances and more — sourced directly at great prices.",gold:false},
  {icon:"💰",title:"Earn 20%–60%\nProfit Share.",sub:"Contribute to a buying pool. We source and sell. You earn your fair share — transparently.",gold:true},
  {icon:"🔍",title:"Radical\nTransparency.",sub:"See every purchase receipt, live pool activity, cycle progress and supplier details. Every cent accounted for.",gold:false},
  {icon:"🛡️",title:"90-Day\nGuarantee.",sub:"If products don't sell within 90 days, your full contribution is returned. No questions asked.",gold:false},
];

const Onboard = ({onDone}) => {
  const [i,setI] = useState(0);
  const s = SLIDES[i], last = i===SLIDES.length-1;
  return (
    <div style={{position:"fixed",inset:0,background:C.white,display:"flex",flexDirection:"column",zIndex:999,fontFamily:"'Barlow',sans-serif"}}>
      <div style={{padding:"18px 20px 0",display:"flex",justifyContent:"space-between",alignItems:"center"}}>
        <Logo size={15}/>
        {!last&&<button onClick={onDone} style={{background:"transparent",border:`1px solid ${C.border}`,borderRadius:99,padding:"5px 13px",color:C.muted,fontSize:11,cursor:"pointer"}}>Skip</button>}
      </div>
      <div style={{flex:1,display:"flex",flexDirection:"column",alignItems:"center",justifyContent:"center",padding:"0 32px"}}>
        <div key={i} style={{textAlign:"center",animation:"fadeUp 0.4s ease both"}}>
          <div style={{width:100,height:100,borderRadius:24,background:s.gold?C.goldP:C.light,border:s.gold?`2px solid ${C.goldB}`:`2px solid ${C.border}`,margin:"0 auto 28px",display:"flex",alignItems:"center",justifyContent:"center",fontSize:52}}>
            {s.icon}
          </div>
          <h2 style={{fontSize:26,fontWeight:900,color:C.black,lineHeight:1.1,marginBottom:14,fontFamily:"'Barlow Condensed',sans-serif",letterSpacing:-0.5,textTransform:"uppercase",whiteSpace:"pre-line"}}>{s.title}</h2>
          <p style={{fontSize:14,color:C.muted,lineHeight:1.75,maxWidth:280,margin:"0 auto"}}>{s.sub}</p>
        </div>
      </div>
      <div style={{padding:"22px 24px 44px"}}>
        <div style={{display:"flex",justifyContent:"center",gap:8,marginBottom:22}}>
          {SLIDES.map((_,j)=><div key={j} onClick={()=>setI(j)} style={{height:3,borderRadius:99,background:j===i?C.black:C.light,width:j===i?28:7,transition:"all 0.3s",cursor:"pointer"}}/>)}
        </div>
        <BlackBtn onClick={()=>last?onDone():setI(i+1)}>{last?"Get Started →":"Next"}</BlackBtn>
      </div>
    </div>
  );
};

// ── MODE SELECTOR ─────────────────────────────────────────────────────────────
const ModeSelect = ({onPick}) => (
  <div style={{position:"fixed",inset:0,background:C.white,display:"flex",flexDirection:"column",zIndex:998,fontFamily:"'Barlow',sans-serif"}}>
    <div style={{position:"absolute",bottom:0,left:0,right:0,height:"35%",background:"linear-gradient(to top,rgba(201,162,39,0.04),transparent)"}}/>
    <div style={{position:"relative",zIndex:1,flex:1,display:"flex",flexDirection:"column",padding:"40px 22px 44px"}}>
      <div style={{textAlign:"center",marginBottom:44,animation:"fadeUp 0.5s ease both"}}>
        <CIcon size={44}/>
        <div style={{marginTop:12}}><Logo size={20}/></div>
        <div style={{width:32,height:2,background:C.black,margin:"14px auto 12px"}}/>
        <div style={{fontSize:13,color:C.muted}}>What would you like to do?</div>
      </div>
      <div style={{flex:1,display:"flex",flexDirection:"column",gap:14,justifyContent:"center"}}>
        <div onClick={()=>onPick("shop")} style={{background:C.black,borderRadius:20,padding:"26px 22px",cursor:"pointer",position:"relative",overflow:"hidden",animation:"slideUp 0.4s 0.05s ease both"}}>
          <div style={{position:"absolute",bottom:0,left:0,right:0,height:3,background:C.gold}}/>
          <div style={{fontSize:36,marginBottom:10}}>🛍️</div>
          <div style={{fontFamily:"'Barlow Condensed',sans-serif",fontWeight:900,fontSize:24,color:C.white,textTransform:"uppercase",letterSpacing:-0.3,marginBottom:5}}>Shop</div>
          <div style={{fontSize:12,color:"rgba(255,255,255,0.4)",lineHeight:1.6,marginBottom:13}}>Browse and buy Tech & Lifestyle products at great prices.</div>
          <div style={{display:"flex",gap:7}}>
            <Tag type="dark">Tech</Tag>
            <Tag type="dark">Lifestyle</Tag>
          </div>
        </div>
        <div onClick={()=>onPick("pool")} style={{background:C.white,border:`2px solid ${C.black}`,borderRadius:20,padding:"26px 22px",cursor:"pointer",position:"relative",overflow:"hidden",animation:"slideUp 0.4s 0.12s ease both"}}>
          <div style={{position:"absolute",bottom:0,left:0,right:0,height:3,background:C.gold}}/>
          <div style={{fontSize:36,marginBottom:10}}>💰</div>
          <div style={{fontFamily:"'Barlow Condensed',sans-serif",fontWeight:900,fontSize:24,color:C.black,textTransform:"uppercase",letterSpacing:-0.3,marginBottom:5}}>Pool & Earn</div>
          <div style={{fontSize:12,color:C.muted,lineHeight:1.6,marginBottom:13}}>Contribute to a buying pool. Earn 20%–60% profit share when products sell.</div>
          <div style={{display:"flex",gap:7}}>
            <Tag type="gold">20–60% Profit</Tag>
            <Tag type="gold">5 Active Pools</Tag>
          </div>
        </div>
      </div>
      <div style={{textAlign:"center",marginTop:22,fontSize:11,color:C.muted}}>Switch between Shop and Pool anytime inside the app.</div>
    </div>
  </div>
);

// ── NOTIFICATIONS ─────────────────────────────────────────────────────────────
const NotifScreen = ({onBack,notifs,setNotifs}) => {
  const unread = notifs.filter(n=>!n.read).length;
  return (
    <div style={{minHeight:"100vh",background:C.white,padding:"20px 16px",fontFamily:"'Barlow',sans-serif"}}>
      <div style={{display:"flex",justifyContent:"space-between",alignItems:"center",marginBottom:22}}>
        <div style={{display:"flex",alignItems:"center",gap:10}}>
          <button onClick={onBack} style={{width:34,height:34,borderRadius:9,background:C.light,border:"none",cursor:"pointer",fontSize:14,display:"flex",alignItems:"center",justifyContent:"center"}}>←</button>
          <span style={{fontFamily:"'Barlow Condensed',sans-serif",fontWeight:900,fontSize:20,color:C.black,textTransform:"uppercase"}}>Notifications</span>
          {unread>0&&<div style={{background:C.red,color:"#fff",borderRadius:99,padding:"2px 7px",fontSize:10,fontWeight:800,animation:"pop 0.4s ease both"}}>{unread}</div>}
        </div>
        {unread>0&&<button onClick={()=>setNotifs(notifs.map(n=>({...n,read:true})))} style={{background:"none",border:"none",color:C.black,fontSize:11,fontWeight:700,cursor:"pointer",textDecoration:"underline"}}>Mark all read</button>}
      </div>
      {notifs.map((n,i)=>(
        <div key={n.id} onClick={()=>setNotifs(notifs.map(x=>x.id===n.id?{...x,read:true}:x))}
          style={{display:"flex",gap:12,padding:"13px 14px",marginBottom:8,background:n.read?C.white:C.off,border:`1px solid ${n.read?C.border:C.black}`,borderRadius:13,cursor:"pointer",animation:`fadeUp 0.4s ${i*0.05}s ease both`,position:"relative",overflow:"hidden"}}>
          {!n.read&&<div style={{position:"absolute",left:0,top:0,bottom:0,width:3,background:C.black}}/>}
          <div style={{width:40,height:40,borderRadius:10,background:C.light,display:"flex",alignItems:"center",justifyContent:"center",fontSize:18,flexShrink:0}}>{n.icon}</div>
          <div style={{flex:1}}>
            <div style={{fontWeight:n.read?500:700,fontSize:13,color:C.black,marginBottom:3}}>{n.title}</div>
            <div style={{fontSize:11,color:C.muted,lineHeight:1.5}}>{n.body}</div>
            <div style={{fontSize:10,color:C.muted,marginTop:4,fontWeight:600}}>{n.time}</div>
          </div>
          {!n.read&&<div style={{width:7,height:7,borderRadius:"50%",background:C.black,flexShrink:0,marginTop:4}}/>}
        </div>
      ))}
    </div>
  );
};

// ── PROFIT BREAKDOWN ──────────────────────────────────────────────────────────
const ProfitBreakdown = ({onBack}) => {
  const [activeCat,setActiveCat] = useState(0);
  const [example,setExample] = useState(1000);
  const m = MARGINS[activeCat];
  const gross = Math.round(example*(m.min/100));
  const fee = Math.round(gross*0.08);
  const net = gross-fee;

  return (
    <div style={{minHeight:"100vh",background:C.white,fontFamily:"'Barlow',sans-serif",paddingBottom:40}}>
      <div style={{padding:"14px 16px",borderBottom:`1px solid ${C.border}`,display:"flex",alignItems:"center",gap:10,position:"sticky",top:0,background:C.white,zIndex:10}}>
        <button onClick={onBack} style={{width:32,height:32,borderRadius:8,background:C.light,border:"none",cursor:"pointer",fontSize:13,display:"flex",alignItems:"center",justifyContent:"center"}}>←</button>
        <div><Logo size={14}/><div style={{fontSize:8,color:C.muted,letterSpacing:1.5,textTransform:"uppercase",fontFamily:"'Barlow Condensed',sans-serif"}}>Profit Breakdown</div></div>
      </div>
      <div style={{padding:"16px"}}>
        <div style={{background:C.black,borderRadius:16,padding:20,marginBottom:16,position:"relative",overflow:"hidden"}}>
          <div style={{position:"absolute",bottom:0,left:0,right:0,height:3,background:C.gold}}/>
          <div style={{fontSize:10,color:C.gold,fontWeight:700,letterSpacing:2,marginBottom:6,fontFamily:"'Barlow Condensed',sans-serif",textTransform:"uppercase"}}>Full Transparency</div>
          <div style={{fontSize:20,fontWeight:900,color:C.white,fontFamily:"'Barlow Condensed',sans-serif",textTransform:"uppercase",marginBottom:8,letterSpacing:-0.3}}>How Your Profit Share<br/>is Calculated.</div>
          <div style={{fontSize:11,color:"rgba(255,255,255,0.5)",lineHeight:1.6}}>Platform fee is 8% of gross profit. You receive the rest proportionally. No hidden charges. Ever.</div>
        </div>

        <SLabel>The Formula</SLabel>
        <Card style={{marginBottom:16}}>
          {[
            {n:"01",t:"Pool Reaches Target",d:"Members contribute until buying target is met. No buying before pool is full.",c:C.black},
            {n:"02",t:"We Buy in Bulk",d:"Connectadon purchases products at wholesale — 30–70% below retail.",c:C.gold},
            {n:"03",t:"Products Are Sold",d:"Sold through Connectadon marketplace, WhatsApp network & other channels.",c:C.green},
            {n:"04",t:"Gross Profit Calculated",d:"Gross profit = Total sales revenue minus wholesale cost paid.",c:C.green},
            {n:"05",t:"8% Platform Fee Deducted",d:"Covers operations, sourcing, logistics, banking fees & reserve fund.",c:C.red},
            {n:"06",t:"Your Net Share is Paid",d:"(Your contribution ÷ Total pool) × 92% of net profit = Your share.",c:C.green},
          ].map((s,i)=>(
            <div key={i} style={{display:"flex",gap:12,marginBottom:i<5?14:0,alignItems:"flex-start"}}>
              <div style={{fontFamily:"'Barlow Condensed',sans-serif",fontWeight:900,fontSize:18,color:s.c,flexShrink:0,width:24,lineHeight:1}}>{s.n}</div>
              <div><div style={{fontWeight:700,fontSize:13,color:C.black,marginBottom:2}}>{s.t}</div><div style={{fontSize:11,color:C.muted,lineHeight:1.5}}>{s.d}</div></div>
            </div>
          ))}
        </Card>

        <SLabel>Select a Category</SLabel>
        <div style={{display:"flex",gap:7,flexWrap:"wrap",marginBottom:12}}>
          {MARGINS.map((m,i)=>(
            <button key={i} onClick={()=>setActiveCat(i)}
              style={{background:activeCat===i?C.black:C.white,border:`1.5px solid ${activeCat===i?C.black:C.border}`,borderRadius:8,padding:"6px 11px",color:activeCat===i?C.white:C.muted,fontSize:11,fontWeight:700,cursor:"pointer",fontFamily:"'Barlow Condensed',sans-serif",textTransform:"uppercase",transition:"all 0.2s",display:"flex",alignItems:"center",gap:4}}>
              <span>{m.emoji}</span>{m.cat.split(" ")[0]}
            </button>
          ))}
        </div>

        <Card style={{marginBottom:16}}>
          <div style={{display:"flex",justifyContent:"space-between",alignItems:"flex-start",marginBottom:10}}>
            <div style={{fontWeight:800,fontSize:15,color:C.black}}>{m.emoji} {m.cat}</div>
            <div style={{background:C.goldP,border:`1px solid ${C.goldB}`,borderRadius:6,padding:"4px 12px"}}>
              <span style={{fontSize:13,fontWeight:900,color:C.gold,fontFamily:"'Barlow Condensed',sans-serif"}}>{m.min}–{m.max}%</span>
            </div>
          </div>
          <div style={{fontSize:12,color:C.muted,lineHeight:1.6,marginBottom:12}}>{m.why}</div>
          <div style={{display:"flex",gap:5,flexWrap:"wrap"}}>
            {m.products.map(p=><Tag key={p}>{p}</Tag>)}
          </div>
        </Card>

        <SLabel>Calculator</SLabel>
        <Card style={{marginBottom:16}}>
          <div style={{fontSize:12,color:C.muted,marginBottom:10}}>If I contribute to <strong style={{color:C.black}}>{m.emoji} {m.cat}</strong>:</div>
          <div style={{display:"flex",gap:7,marginBottom:14,flexWrap:"wrap"}}>
            {[250,500,1000,2500,5000].map(v=>(
              <button key={v} onClick={()=>setExample(v)}
                style={{flex:1,minWidth:46,background:example===v?C.black:C.white,border:`1.5px solid ${example===v?C.black:C.border}`,borderRadius:8,padding:"9px 4px",fontSize:10,fontWeight:700,color:example===v?C.white:C.muted,cursor:"pointer",fontFamily:"'DM Mono',monospace",transition:"all 0.2s"}}>
                {fmt(v)}
              </button>
            ))}
          </div>
          <div style={{background:C.light,borderRadius:10,padding:12,marginBottom:10}}>
            {[
              {l:"Your contribution",v:fmt(example),c:C.black},
              {l:`Gross share (${m.min}% min)`,v:`+${fmt(gross)}`,c:C.black},
              {l:"Platform fee (8%)",v:`-${fmt(fee)}`,c:C.red},
            ].map((r,i)=>(
              <div key={i} style={{display:"flex",justifyContent:"space-between",padding:"5px 0",borderBottom:i<2?`1px solid ${C.border}`:"none"}}>
                <span style={{fontSize:11,color:C.muted}}>{r.l}</span>
                <span style={{fontSize:11,fontWeight:700,color:r.c,fontFamily:"'DM Mono',monospace"}}>{r.v}</span>
              </div>
            ))}
            <div style={{display:"flex",justifyContent:"space-between",marginTop:8}}>
              <span style={{fontSize:12,fontWeight:700,color:C.green}}>Your net profit share</span>
              <span style={{fontSize:15,fontWeight:900,color:C.green,fontFamily:"'DM Mono',monospace"}}>+{fmt(net)}</span>
            </div>
          </div>
          <div style={{fontSize:10,color:C.muted,textAlign:"center",lineHeight:1.6}}>Actual profit depends on final wholesale cost and market conditions. Platform fee of 8% deducted from gross profit.</div>
        </Card>

        <SLabel>All Category Rates</SLabel>
        <Card style={{padding:"4px 0"}}>
          {MARGINS.map((m,i)=>(
            <div key={i} style={{display:"flex",alignItems:"center",gap:12,padding:"11px 16px",borderBottom:i<MARGINS.length-1?`1px solid ${C.border}`:"none"}}>
              <span style={{fontSize:20,flexShrink:0}}>{m.emoji}</span>
              <div style={{flex:1}}>
                <div style={{fontSize:12,fontWeight:600,color:C.black}}>{m.cat}</div>
                <div style={{fontSize:10,color:C.muted,marginTop:1}}>{m.products.slice(0,3).join(", ")}</div>
              </div>
              <div style={{textAlign:"right"}}>
                <div style={{fontSize:13,fontWeight:900,color:C.gold,fontFamily:"'DM Mono',monospace"}}>{m.min}–{m.max}%</div>
                <div style={{fontSize:9,color:C.muted}}>profit share</div>
              </div>
            </div>
          ))}
        </Card>
      </div>
    </div>
  );
};

// ── POOL DETAIL ───────────────────────────────────────────────────────────────
const PoolDetail = ({pool,onBack,onContrib}) => {
  const [tab,setTab] = useState("overview");
  const pct = Math.round((pool.funded/pool.target)*100);
  const tabs = ["overview","activity","receipt","info"];

  return (
    <div style={{minHeight:"100vh",background:C.white,fontFamily:"'Barlow',sans-serif",paddingBottom:80}}>
      <div style={{background:C.black,position:"relative",overflow:"hidden"}}>
        <div style={{position:"absolute",bottom:0,left:0,right:0,height:3,background:C.gold}}/>
        <div style={{padding:"14px 16px",display:"flex",alignItems:"center",gap:10}}>
          <button onClick={onBack} style={{width:32,height:32,borderRadius:8,background:"rgba(255,255,255,0.1)",border:"none",cursor:"pointer",fontSize:13,display:"flex",alignItems:"center",justifyContent:"center",color:C.white}}>←</button>
          <Logo size={13} light={true}/>
        </div>
        <div style={{padding:"0 16px 20px",textAlign:"center"}}>
          <div style={{fontSize:48,marginBottom:8}}>{pool.emoji}</div>
          <h2 style={{fontSize:22,fontWeight:900,color:C.white,fontFamily:"'Barlow Condensed',sans-serif",textTransform:"uppercase",marginBottom:10}}>{pool.name}</h2>
          <div style={{display:"flex",justifyContent:"center",gap:7,marginBottom:14,flexWrap:"wrap"}}>
            <Tag type="dark">{pool.cat}</Tag>
            <Tag type="gold">+{pool.profit}% profit share</Tag>
            <Tag type="dark">{pool.members} contributors</Tag>
            <Tag type="dark">{pool.days}d left</Tag>
          </div>
          <div style={{display:"flex",justifyContent:"space-between",marginBottom:7}}>
            <span style={{fontSize:11,color:"rgba(255,255,255,0.35)"}}>Progress</span>
            <span style={{fontSize:11,color:C.white,fontFamily:"'DM Mono',monospace"}}>{fmt(pool.funded)} / {fmt(pool.target)}</span>
          </div>
          <Bar val={pool.funded} max={pool.target} color={C.green}/>
          <div style={{fontSize:10,color:"rgba(255,255,255,0.3)",marginTop:5,textAlign:"right"}}>{pct}% funded</div>
        </div>
      </div>

      <div style={{display:"flex",borderBottom:`1px solid ${C.border}`,background:C.white,position:"sticky",top:0,zIndex:10}}>
        {tabs.map(t=>(
          <button key={t} onClick={()=>setTab(t)}
            style={{flex:1,padding:"12px 4px",border:"none",background:"none",cursor:"pointer",fontSize:11,fontWeight:tab===t?800:500,color:tab===t?C.black:C.muted,fontFamily:"'Barlow Condensed',sans-serif",letterSpacing:0.5,textTransform:"uppercase",borderBottom:tab===t?`2px solid ${C.gold}`:"2px solid transparent",transition:"all 0.2s"}}>
            {t.charAt(0).toUpperCase()+t.slice(1)}
          </button>
        ))}
      </div>

      <div style={{padding:"16px"}}>
        {tab==="overview"&&(
          <div style={{animation:"fadeUp 0.3s ease both"}}>
            <div style={{display:"flex",gap:1,background:C.border,borderRadius:10,overflow:"hidden",marginBottom:16}}>
              {[{l:"Min",v:fmt(pool.min)},{l:"Max",v:fmt(pool.max)},{l:"Profit Share",v:`+${pool.profit}%`,g:true}].map((s,i)=>(
                <div key={i} style={{flex:1,background:s.g?C.goldP:C.white,padding:"12px 8px",textAlign:"center"}}>
                  <div style={{fontSize:9,color:C.muted,fontWeight:700,letterSpacing:0.5,textTransform:"uppercase",marginBottom:4,fontFamily:"'Barlow Condensed',sans-serif"}}>{s.l}</div>
                  <div style={{fontSize:14,fontWeight:900,color:s.g?C.gold:C.black,fontFamily:"'DM Mono',monospace"}}>{s.v}</div>
                </div>
              ))}
            </div>

            <SLabel>Cycle Stage</SLabel>
            <Card style={{marginBottom:16}}>
              {CYCLE_STAGES.map((stage,i)=>{
                const done=i<pool.stage, active=i===pool.stage;
                return (
                  <div key={i} style={{display:"flex",gap:12,alignItems:"flex-start",marginBottom:i<CYCLE_STAGES.length-1?14:0}}>
                    <div style={{display:"flex",flexDirection:"column",alignItems:"center"}}>
                      <div style={{width:28,height:28,borderRadius:"50%",background:done?C.green:active?C.black:C.light,border:`2px solid ${done?C.green:active?C.black:C.border}`,display:"flex",alignItems:"center",justifyContent:"center",fontSize:13,flexShrink:0}}>
                        {done?<span style={{color:"#fff",fontSize:11}}>✓</span>:active?<span>{stage.icon}</span>:<span style={{color:C.muted,fontSize:10}}>○</span>}
                      </div>
                      {i<CYCLE_STAGES.length-1&&<div style={{width:2,height:12,background:done?C.green:C.light,marginTop:2}}/>}
                    </div>
                    <div style={{paddingTop:4,flex:1}}>
                      <div style={{fontSize:12,fontWeight:done?600:active?800:400,color:done?C.green:active?C.black:C.muted}}>{stage.label}</div>
                      <div style={{fontSize:10,color:C.muted,marginTop:1}}>{stage.sub}</div>
                    </div>
                    {active&&<Tag type="gold">Current</Tag>}
                  </div>
                );
              })}
            </Card>

            <div style={{background:C.black,borderRadius:14,padding:16,marginBottom:16,display:"flex",gap:12,alignItems:"center"}}>
              <span style={{fontSize:28}}>🛡️</span>
              <div>
                <div style={{fontWeight:800,fontSize:13,color:C.gold,fontFamily:"'Barlow Condensed',sans-serif",textTransform:"uppercase",marginBottom:3}}>90-Day Guarantee</div>
                <div style={{fontSize:11,color:"rgba(255,255,255,0.5)",lineHeight:1.5}}>If products don't sell within 90 days, your full contribution is returned. No conditions.</div>
              </div>
            </div>

            <Card style={{background:C.off}}>
              <SLabel>Fee Structure</SLabel>
              {[
                {l:"Gross profit share",v:`${pool.profit}% of your proportional share`},
                {l:"Platform fee",v:"8% deducted from gross profit"},
                {l:"Net profit to you",v:`~${pool.profit-Math.round(pool.profit*0.08)}% net after fee`,green:true},
                {l:"Payout timeline",v:"Within 48 hours of cycle close"},
              ].map((r,i)=>(
                <div key={i} style={{display:"flex",justifyContent:"space-between",padding:"7px 0",borderBottom:i<3?`1px solid ${C.border}`:"none",flexWrap:"wrap",gap:4}}>
                  <span style={{fontSize:11,color:C.muted}}>{r.l}</span>
                  <span style={{fontSize:11,fontWeight:r.green?800:600,color:r.green?C.green:C.black}}>{r.v}</span>
                </div>
              ))}
            </Card>
          </div>
        )}

        {tab==="activity"&&(
          <div style={{animation:"fadeUp 0.3s ease both"}}>
            <div style={{display:"inline-flex",alignItems:"center",gap:6,background:C.greenP,border:`1px solid ${C.greenB}`,borderRadius:6,padding:"4px 10px",marginBottom:14}}>
              <div style={{width:5,height:5,borderRadius:"50%",background:C.green,animation:"pulse 2s infinite"}}/>
              <span style={{fontSize:9,color:C.green,fontWeight:700,letterSpacing:1,fontFamily:"'Barlow Condensed',sans-serif",textTransform:"uppercase"}}>Live Updates</span>
            </div>
            {pool.feed.map((f,i)=>(
              <div key={i} style={{display:"flex",gap:12,marginBottom:12,animation:`feedIn 0.4s ${i*0.07}s ease both`}}>
                <div style={{width:32,height:32,borderRadius:99,background:C.light,display:"flex",alignItems:"center",justifyContent:"center",fontSize:16,flexShrink:0}}>{f.icon}</div>
                <div style={{flex:1,borderBottom:`1px solid ${C.border}`,paddingBottom:12}}>
                  <div style={{fontSize:13,color:C.black,fontWeight:500,lineHeight:1.4}}>{f.event}</div>
                  <div style={{fontSize:10,color:C.muted,marginTop:4}}>{f.time}</div>
                </div>
              </div>
            ))}
          </div>
        )}

        {tab==="receipt"&&(
          <div style={{animation:"fadeUp 0.3s ease both"}}>
            {pool.receipt?(
              <>
                <div style={{background:C.greenP,border:`1px solid ${C.greenB}`,borderRadius:10,padding:12,marginBottom:14,display:"flex",gap:8,alignItems:"center"}}>
                  <span>✅</span>
                  <div style={{fontSize:12,color:C.green,fontWeight:600}}>Purchase verified and completed</div>
                </div>
                <Card>
                  <div style={{fontFamily:"'Barlow Condensed',sans-serif",fontWeight:900,fontSize:14,color:C.black,textTransform:"uppercase",marginBottom:12,borderBottom:`1px solid ${C.border}`,paddingBottom:10}}>Purchase Receipt</div>
                  {[{l:"Date",v:pool.receipt.date},{l:"Supplier",v:pool.receipt.supplier},{l:"Reference",v:pool.receipt.ref},{l:"Items Purchased",v:pool.receipt.items},{l:"Total Wholesale Cost",v:pool.receipt.cost,bold:true}].map((r,i)=>(
                    <div key={i} style={{padding:"8px 0",borderBottom:i<4?`1px solid ${C.border}`:"none"}}>
                      <div style={{fontSize:9,color:C.muted,fontWeight:700,letterSpacing:0.5,textTransform:"uppercase",marginBottom:3}}>{r.l}</div>
                      <div style={{fontSize:12,fontWeight:r.bold?900:500,color:C.black,fontFamily:r.bold?"'DM Mono',monospace":"inherit"}}>{r.v}</div>
                    </div>
                  ))}
                </Card>
              </>
            ):(
              <div style={{textAlign:"center",padding:"40px 0"}}>
                <div style={{fontSize:36,marginBottom:10}}>⏳</div>
                <div style={{fontWeight:700,fontSize:14,color:C.black,marginBottom:6}}>Receipt not yet available</div>
                <div style={{fontSize:12,color:C.muted,lineHeight:1.6}}>Will appear once pool reaches target and purchase is completed.</div>
              </div>
            )}
          </div>
        )}

        {tab==="info"&&(
          <div style={{animation:"fadeUp 0.3s ease both"}}>
            <SLabel>Supplier</SLabel>
            <Card style={{marginBottom:14}}>
              <div style={{display:"flex",gap:10,alignItems:"flex-start",marginBottom:8}}>
                <span style={{fontSize:22}}>🏭</span>
                <div>
                  <div style={{fontWeight:700,fontSize:13,color:C.black,marginBottom:4}}>{pool.supplier}</div>
                  <Tag type="green">Verified</Tag>
                </div>
              </div>
              <div style={{fontSize:11,color:C.muted,lineHeight:1.6}}>All suppliers vetted through business registration, sample orders and delivery history checks.</div>
            </Card>
            <SLabel>Connectadon Business Details</SLabel>
            <Card style={{background:C.off}}>
              {[{l:"Registered Name",v:"Connectadon (Pty) Ltd"},{l:"CIPC Registration",v:"2024/XXXXXX/07"},{l:"Business Bank",v:"FNB Gold Business Account"},{l:"Pool Funds",v:"Ring-fenced separate account"},{l:"Reserve Fund",v:"Active — contributor protection"}].map((r,i)=>(
                <div key={i} style={{display:"flex",justifyContent:"space-between",padding:"7px 0",borderBottom:i<4?`1px solid ${C.border}`:"none",flexWrap:"wrap",gap:4}}>
                  <span style={{fontSize:11,color:C.muted}}>{r.l}</span>
                  <span style={{fontSize:11,fontWeight:600,color:C.black,textAlign:"right"}}>{r.v}</span>
                </div>
              ))}
            </Card>
          </div>
        )}
      </div>

      <div style={{position:"fixed",bottom:0,left:"50%",transform:"translateX(-50%)",width:"100%",maxWidth:430,padding:"12px 16px",background:"rgba(255,255,255,0.97)",borderTop:`1px solid ${C.border}`,backdropFilter:"blur(20px)"}}>
        <BlackBtn onClick={onContrib}>Contribute to This Pool →</BlackBtn>
      </div>
    </div>
  );
};

// ── CONTRIBUTION FLOW ─────────────────────────────────────────────────────────
const ContribFlow = ({pool,onBack,onDone}) => {
  const [amount,setAmount] = useState("");
  const [joined,setJoined] = useState(false);
  const [loading,setLoading] = useState(false);
  const gross = amount&&Number(amount)>=pool.min?Math.round(Number(amount)*(pool.profit/100)):null;
  const fee = gross?Math.round(gross*0.08):null;
  const net = gross?gross-fee:null;

  if(joined) return (
    <div style={{minHeight:"100vh",background:C.white,fontFamily:"'Barlow',sans-serif",display:"flex",flexDirection:"column",alignItems:"center",justifyContent:"center",padding:"40px 24px",textAlign:"center"}}>
      <div style={{width:72,height:72,borderRadius:20,background:C.greenP,border:`2px solid ${C.greenB}`,margin:"0 auto 20px",display:"flex",alignItems:"center",justifyContent:"center",fontSize:30,animation:"pop 0.5s ease both"}}>🎉</div>
      <div style={{fontWeight:900,fontSize:22,color:C.green,fontFamily:"'Barlow Condensed',sans-serif",textTransform:"uppercase",marginBottom:8}}>You're In the Pool!</div>
      <div style={{fontSize:26,fontWeight:900,color:C.black,fontFamily:"'DM Mono',monospace",marginBottom:6}}>{fmt(amount)}</div>
      <div style={{fontSize:12,color:C.muted,marginBottom:20,lineHeight:1.7}}>
        Pool: <strong style={{color:C.black}}>{pool.name}</strong><br/>
        Gross profit share: <strong style={{color:C.gold}}>+{gross?fmt(gross):"—"}</strong><br/>
        Platform fee (8%): <strong style={{color:C.red}}>-{fee?fmt(fee):"—"}</strong><br/>
        Net profit share: <strong style={{color:C.green}}>+{net?fmt(net):"—"}</strong>
      </div>
      <div style={{background:C.off,borderRadius:12,padding:14,marginBottom:20,width:"100%",textAlign:"left"}}>
        <div style={{fontSize:11,fontWeight:700,color:C.black,marginBottom:4}}>🛡️ Your 90-Day Guarantee is Active</div>
        <div style={{fontSize:11,color:C.muted,lineHeight:1.6}}>If products don't sell within 90 days, your {fmt(amount)} contribution is fully returned.</div>
      </div>
      <BlackBtn onClick={onDone} style={{maxWidth:200}}>Done</BlackBtn>
    </div>
  );

  return (
    <div style={{minHeight:"100vh",background:C.white,fontFamily:"'Barlow',sans-serif",padding:"20px 16px 40px"}}>
      <BackBtn onBack={onBack}/>
      <div style={{background:C.black,borderRadius:16,padding:20,textAlign:"center",marginBottom:16,position:"relative",overflow:"hidden"}}>
        <div style={{position:"absolute",bottom:0,left:0,right:0,height:3,background:C.gold}}/>
        <div style={{fontSize:40,marginBottom:8}}>{pool.emoji}</div>
        <div style={{fontWeight:900,fontSize:18,color:C.white,fontFamily:"'Barlow Condensed',sans-serif",textTransform:"uppercase",marginBottom:6}}>{pool.name}</div>
        <div style={{display:"flex",justifyContent:"center",gap:6}}>
          <Tag type="gold">+{pool.profit}% profit share</Tag>
          <Tag type="dark">{pool.days}d left</Tag>
        </div>
      </div>
      <Card style={{marginBottom:14}}>
        <div style={{fontWeight:700,fontSize:13,color:C.black,marginBottom:3}}>Your Contribution</div>
        <div style={{fontSize:11,color:C.muted,marginBottom:12}}>Min {fmt(pool.min)} · Max {fmt(pool.max)}</div>
        <div style={{position:"relative",marginBottom:10}}>
          <span style={{position:"absolute",left:13,top:"50%",transform:"translateY(-50%)",color:C.muted,fontSize:14,fontWeight:700}}>R</span>
          <input type="number" value={amount} onChange={e=>setAmount(e.target.value)} placeholder="0.00"
            style={{width:"100%",background:C.light,border:`1.5px solid ${amount&&Number(amount)>=pool.min?C.green:C.border}`,borderRadius:10,padding:"13px 13px 13px 28px",fontSize:22,color:C.black,fontFamily:"'DM Mono',monospace",outline:"none",transition:"border-color 0.2s"}}/>
        </div>
        <div style={{display:"flex",gap:7,marginBottom:12}}>
          {[pool.min,1000,2500,pool.max].filter((v,i,a)=>a.indexOf(v)===i).slice(0,4).map(v=>(
            <button key={v} onClick={()=>setAmount(String(v))} style={{flex:1,background:Number(amount)===v?C.black:C.white,border:`1px solid ${Number(amount)===v?C.black:C.border}`,borderRadius:7,padding:"7px 3px",fontSize:9,color:Number(amount)===v?C.white:C.muted,cursor:"pointer",fontWeight:700,fontFamily:"'DM Mono',monospace"}}>{fmt(v)}</button>
          ))}
        </div>
        {gross&&(
          <div style={{background:C.light,borderRadius:10,padding:12,marginBottom:12,animation:"fadeUp 0.2s ease both"}}>
            {[{l:`Gross share (${pool.profit}%)`,v:`+${fmt(gross)}`,c:C.black},{l:"Platform fee (8%)",v:`-${fmt(fee)}`,c:C.red}].map((r,i)=>(
              <div key={i} style={{display:"flex",justifyContent:"space-between",padding:"5px 0",borderBottom:i<1?`1px solid ${C.border}`:"none"}}>
                <span style={{fontSize:11,color:C.muted}}>{r.l}</span>
                <span style={{fontSize:11,fontWeight:700,color:r.c,fontFamily:"'DM Mono',monospace"}}>{r.v}</span>
              </div>
            ))}
            <div style={{display:"flex",justifyContent:"space-between",marginTop:8}}>
              <span style={{fontSize:12,fontWeight:700,color:C.green}}>Net profit share</span>
              <span style={{fontSize:16,fontWeight:900,color:C.green,fontFamily:"'DM Mono',monospace"}}>+{fmt(net)}</span>
            </div>
          </div>
        )}
        <BlackBtn disabled={!amount||Number(amount)<pool.min||loading}
          onClick={()=>{if(!amount||Number(amount)<pool.min||loading)return;setLoading(true);setTimeout(()=>{setLoading(false);setJoined(true);},1800);}}>
          {loading&&<Spin/>}{loading?"Processing…":"Confirm Contribution"}
        </BlackBtn>
      </Card>
      <div style={{background:C.goldP,border:`1px solid ${C.goldB}`,borderRadius:10,padding:12,display:"flex",gap:8,alignItems:"flex-start"}}>
        <span style={{fontSize:16}}>🛡️</span>
        <div style={{fontSize:11,color:C.muted,lineHeight:1.6}}>Your contribution is backed by the <strong style={{color:C.black}}>90-day guarantee</strong>. Full amount returned if products don't sell.</div>
      </div>
    </div>
  );
};

// ── POOLS LIST ────────────────────────────────────────────────────────────────
const PoolsList = () => {
  const [search,setSearch] = useState("");
  const [sel,setSel] = useState(null);
  const [showContrib,setShowContrib] = useState(false);
  const filtered = POOLS.filter(p=>p.name.toLowerCase().includes(search.toLowerCase()));

  if(showContrib&&sel) return <ContribFlow pool={sel} onBack={()=>setShowContrib(false)} onDone={()=>{setShowContrib(false);setSel(null);}}/>;
  if(sel) return <PoolDetail pool={sel} onBack={()=>setSel(null)} onContrib={()=>setShowContrib(true)}/>;

  return (
    <div style={{padding:"20px 16px 0",fontFamily:"'Barlow',sans-serif"}}>
      <div style={{position:"relative",marginBottom:14}}>
        <span style={{position:"absolute",left:12,top:"50%",transform:"translateY(-50%)",color:C.muted}}>⌕</span>
        <input value={search} onChange={e=>setSearch(e.target.value)} placeholder="Search buying pools…"
          style={{width:"100%",background:C.light,border:`1px solid ${C.border}`,borderRadius:10,padding:"11px 11px 11px 34px",fontSize:13,color:C.black,outline:"none",fontFamily:"'Barlow',sans-serif"}}/>
      </div>
      <div style={{display:"flex",alignItems:"center",gap:7,marginBottom:14}}>
        <div style={{width:22,height:22,borderRadius:6,background:C.black,color:C.gold,fontSize:11,fontWeight:900,display:"flex",alignItems:"center",justifyContent:"center",fontFamily:"'Barlow Condensed',sans-serif"}}>{filtered.length}</div>
        <span style={{fontSize:10,color:C.muted,fontWeight:700,letterSpacing:1,textTransform:"uppercase",fontFamily:"'Barlow Condensed',sans-serif"}}>Active Buying Pools</span>
      </div>
      {filtered.map((pool,i)=>{
        const pct=Math.round((pool.funded/pool.target)*100);
        const stage=CYCLE_STAGES[pool.stage];
        return (
          <div key={pool.id} onClick={()=>setSel(pool)}
            style={{background:C.white,border:`1px solid ${C.border}`,borderRadius:14,padding:16,marginBottom:11,cursor:"pointer",animation:`fadeUp 0.4s ${i*0.07}s ease both`,position:"relative",overflow:"hidden"}}>
            <div style={{position:"absolute",top:0,left:0,bottom:0,width:3,background:pct>80?C.gold:C.border}}/>
            <div style={{display:"flex",gap:12,alignItems:"flex-start",marginBottom:12,paddingLeft:8}}>
              <div style={{width:48,height:48,borderRadius:12,background:C.light,display:"flex",alignItems:"center",justifyContent:"center",fontSize:22,flexShrink:0}}>{pool.emoji}</div>
              <div style={{flex:1}}>
                <div style={{fontWeight:800,fontSize:15,color:C.black,marginBottom:5,fontFamily:"'Barlow Condensed',sans-serif"}}>{pool.name}</div>
                <div style={{display:"flex",gap:6,flexWrap:"wrap"}}>
                  <Tag>{pool.cat}</Tag>
                  <Tag type="gold">+{pool.profit}% profit share</Tag>
                  <Tag type="green">{stage.icon} {stage.label}</Tag>
                </div>
              </div>
            </div>
            <div style={{paddingLeft:8}}>
              <div style={{display:"flex",justifyContent:"space-between",marginBottom:5}}>
                <span style={{fontSize:11,color:C.muted}}>{fmt(pool.funded)} contributed</span>
                <span style={{fontSize:11,color:C.black,fontFamily:"'DM Mono',monospace",fontWeight:700}}>{pct}%</span>
              </div>
              <Bar val={pool.funded} max={pool.target} color={C.green}/>
              <div style={{display:"flex",justifyContent:"space-between",marginTop:12,alignItems:"center"}}>
                <div>
                  <div style={{fontSize:11,color:C.muted}}>{pool.members} contributors · {pool.days}d left</div>
                  <div style={{fontSize:9,color:C.muted,marginTop:2}}>🛡️ 90-day guarantee · 8% platform fee</div>
                </div>
                <button style={{background:C.black,color:C.gold,border:"none",borderRadius:7,padding:"7px 14px",fontSize:10,fontWeight:800,cursor:"pointer",fontFamily:"'Barlow Condensed',sans-serif",letterSpacing:0.5,textTransform:"uppercase"}}>Details →</button>
              </div>
            </div>
          </div>
        );
      })}
    </div>
  );
};

// ── POOL HOME ─────────────────────────────────────────────────────────────────
const PoolHome = ({setPoolTab,setSubScreen}) => (
  <div style={{padding:"0 0 24px",animation:"fadeUp 0.5s ease both",fontFamily:"'Barlow',sans-serif"}}>
    <div style={{background:C.black,padding:"32px 22px 38px",position:"relative",overflow:"hidden"}}>
      <div style={{position:"absolute",bottom:0,left:0,right:0,height:2,background:C.gold}}/>
      <div style={{display:"inline-flex",alignItems:"center",gap:6,background:"rgba(0,200,83,0.12)",border:"1px solid rgba(0,200,83,0.25)",borderRadius:6,padding:"4px 11px",marginBottom:18}}>
        <div style={{width:5,height:5,borderRadius:"50%",background:C.green,animation:"pulse 2s infinite"}}/>
        <span style={{fontSize:9,color:C.green,fontWeight:700,letterSpacing:1.5,fontFamily:"'Barlow Condensed',sans-serif",textTransform:"uppercase"}}>5 Active Buying Pools</span>
      </div>
      <h1 style={{fontSize:30,fontWeight:900,color:C.white,lineHeight:1.1,marginBottom:12,fontFamily:"'Barlow Condensed',sans-serif",textTransform:"uppercase",letterSpacing:-0.5}}>
        Contribute Together.<br/><span style={{color:C.gold}}>Share the Profit.</span>
      </h1>
      <p style={{fontSize:12,color:"rgba(255,255,255,0.45)",lineHeight:1.7,marginBottom:20}}>Pool your buying power. Connectadon sources and sells products. Everyone gets their fair share — transparently.</p>
      <div style={{display:"flex",gap:10,flexWrap:"wrap"}}>
        <button onClick={()=>setPoolTab("pools")} style={{background:C.gold,color:C.black,border:"none",borderRadius:9,padding:"12px 20px",fontSize:12,fontWeight:800,cursor:"pointer",fontFamily:"'Barlow Condensed',sans-serif",letterSpacing:1,textTransform:"uppercase"}}>Browse Pools →</button>
        <button onClick={()=>setSubScreen("breakdown")} style={{background:"transparent",border:"1px solid rgba(255,255,255,0.2)",borderRadius:9,padding:"12px 14px",fontSize:11,fontWeight:700,cursor:"pointer",color:"rgba(255,255,255,0.6)",fontFamily:"'Barlow Condensed',sans-serif",textTransform:"uppercase",letterSpacing:0.5}}>📊 Profit Rates</button>
      </div>
    </div>

    <div style={{display:"flex",gap:1,background:C.border,margin:"0 0 22px"}}>
      {[{l:"Members",v:"1,240+"},{l:"Profit Shared",v:"R890K+"},{l:"Completed",v:"34",g:true}].map((s,i)=>(
        <div key={i} style={{flex:1,background:s.g?C.greenP:C.white,padding:"15px 10px",textAlign:"center"}}>
          <div style={{fontSize:9,color:C.muted,fontWeight:700,letterSpacing:1,textTransform:"uppercase",marginBottom:5,fontFamily:"'Barlow Condensed',sans-serif"}}>{s.l}</div>
          <div style={{fontSize:16,fontWeight:900,color:s.g?C.green:C.black,fontFamily:"'DM Mono',monospace"}}>{s.v}</div>
        </div>
      ))}
    </div>

    <div style={{padding:"0 16px",marginBottom:20}}>
      <SLabel>Earnings This Week</SLabel>
      <Card style={{padding:"12px 10px"}}>
        <div style={{display:"flex",alignItems:"flex-end",gap:6,height:80,paddingBottom:4}}>
          {WEEK_DATA.map((v,i)=>{
            const h=Math.round((v/Math.max(...WEEK_DATA))*100);
            const top=i===5;
            return (
              <div key={i} style={{flex:1,display:"flex",flexDirection:"column",alignItems:"center",gap:4}}>
                <div style={{width:"100%",height:`${h}%`,borderRadius:"5px 5px 0 0",background:top?C.gold:C.light,position:"relative",transition:"height 0.6s"}}>
                  {top&&<div style={{position:"absolute",top:-20,left:"50%",transform:"translateX(-50%)",background:C.black,color:C.gold,fontSize:8,fontWeight:800,padding:"2px 5px",borderRadius:4,whiteSpace:"nowrap",fontFamily:"'DM Mono',monospace"}}>+R8,700</div>}
                </div>
                <span style={{fontSize:9,color:top?C.gold:C.muted,fontFamily:"'DM Mono',monospace",fontWeight:top?700:400}}>{WDAYS[i]}</span>
              </div>
            );
          })}
        </div>
        <div style={{display:"flex",justifyContent:"space-between",padding:"8px 4px 0",borderTop:`1px solid ${C.border}`,marginTop:8}}>
          {[{l:"Pool Sales",v:"R117K"},{l:"Profit Shared",v:"R32K",g:true},{l:"New Members",v:"+47"}].map((s,i)=>(
            <div key={i} style={{textAlign:"center"}}>
              <div style={{fontSize:9,color:C.muted,marginBottom:2}}>{s.l}</div>
              <div style={{fontSize:13,fontWeight:900,color:s.g?C.gold:C.black,fontFamily:"'DM Mono',monospace"}}>{s.v}</div>
            </div>
          ))}
        </div>
      </Card>
    </div>

    <div style={{padding:"0 16px",marginBottom:20}}>
      <div style={{background:C.off,border:`1px solid ${C.border}`,borderRadius:14,padding:16}}>
        <SLabel>Our Transparency Promise</SLabel>
        {[
          {icon:"📄",t:"Purchase receipts visible",s:"Every wholesale purchase documented and viewable"},
          {icon:"📊",t:"Live pool activity feed",s:"Real-time contributions, purchases and sales"},
          {icon:"💸",t:"8% platform fee — published openly",s:"No hidden charges, ever"},
          {icon:"🏦",t:"Ring-fenced pool funds",s:"Separate FNB Gold business account"},
          {icon:"🛡️",t:"90-day money-back guarantee",s:"Full return if products don't sell in 90 days"},
        ].map((t,i)=>(
          <div key={i} style={{display:"flex",gap:10,alignItems:"flex-start",marginBottom:i<4?10:0}}>
            <span style={{fontSize:16,flexShrink:0}}>{t.icon}</span>
            <div>
              <div style={{fontSize:12,fontWeight:700,color:C.black}}>{t.t}</div>
              <div style={{fontSize:10,color:C.muted,lineHeight:1.4,marginTop:1}}>{t.s}</div>
            </div>
          </div>
        ))}
      </div>
    </div>

    <div style={{padding:"0 16px"}}>
      <SLabel>How It Works</SLabel>
      {[{n:"01",i:"🤝",t:"Contribute",d:"Join from as little as R250. No experience needed."},{n:"02",i:"🛒",t:"We Source & Sell",d:"Connectadon buys in bulk and sells through active markets."},{n:"03",i:"💰",t:"Receive Your Share",d:"Profit shared within 48hrs of cycle close, after our 8% fee."}].map((s,i)=>(
        <div key={i} style={{display:"flex",gap:12,marginBottom:14,alignItems:"flex-start"}}>
          <div style={{fontFamily:"'Barlow Condensed',sans-serif",fontWeight:900,fontSize:20,color:C.gold,flexShrink:0,width:28,lineHeight:1.1}}>{s.n}</div>
          <div style={{width:38,height:38,borderRadius:11,background:C.light,border:`1px solid ${C.border}`,display:"flex",alignItems:"center",justifyContent:"center",fontSize:17,flexShrink:0}}>{s.i}</div>
          <div><div style={{fontWeight:700,fontSize:13,color:C.black,marginBottom:2}}>{s.t}</div><div style={{fontSize:12,color:C.muted,lineHeight:1.5}}>{s.d}</div></div>
        </div>
      ))}
    </div>
  </div>
);

// ── EARNINGS ──────────────────────────────────────────────────────────────────
const EarningsScreen = ({setSubScreen}) => {
  const maxV = Math.max(...WEEK_DATA);
  return (
    <div style={{padding:"20px 16px",fontFamily:"'Barlow',sans-serif"}}>
      <div style={{display:"flex",gap:1,background:C.border,borderRadius:11,overflow:"hidden",marginBottom:18}}>
        {[{l:"Total Earned",v:"R5 050"},{l:"Pool Sales",v:"R67 859"},{l:"This Week",v:"+R13 200",g:true}].map((s,i)=>(
          <div key={i} style={{flex:1,background:s.g?C.goldP:C.white,padding:"13px 6px",textAlign:"center"}}>
            <div style={{fontSize:8,color:C.muted,fontWeight:700,letterSpacing:0.5,textTransform:"uppercase",marginBottom:4,fontFamily:"'Barlow Condensed',sans-serif"}}>{s.l}</div>
            <div style={{fontSize:13,fontWeight:900,color:s.g?C.gold:C.black,fontFamily:"'DM Mono',monospace"}}>{s.v}</div>
          </div>
        ))}
      </div>

      <Card style={{marginBottom:16,padding:"14px 10px"}}>
        <div style={{display:"flex",justifyContent:"space-between",alignItems:"center",marginBottom:14,padding:"0 4px"}}>
          <div>
            <SLabel style={{marginBottom:2}}>Profit Share History</SLabel>
            <div style={{fontSize:18,fontWeight:900,color:C.gold,fontFamily:"'DM Mono',monospace"}}>R32 000 <span style={{fontSize:12,color:C.green,fontWeight:700}}>↑ 24%</span></div>
          </div>
          <button onClick={()=>setSubScreen&&setSubScreen("breakdown")} style={{background:C.light,border:"none",borderRadius:6,padding:"6px 10px",fontSize:10,fontWeight:700,color:C.black,cursor:"pointer",fontFamily:"'Barlow Condensed',sans-serif"}}>Rates →</button>
        </div>
        <div style={{display:"flex",alignItems:"flex-end",gap:7,height:100,paddingBottom:4}}>
          {WEEK_DATA.map((v,i)=>{
            const h=Math.round((v/maxV)*100),top=i===5;
            return (
              <div key={i} style={{flex:1,display:"flex",flexDirection:"column",alignItems:"center",gap:5}}>
                <div style={{width:"100%",height:`${h}%`,borderRadius:"5px 5px 0 0",background:top?C.gold:C.light,position:"relative",transition:"all 0.6s"}}>
                  {top&&<div style={{position:"absolute",top:-20,left:"50%",transform:"translateX(-50%)",background:C.black,color:C.gold,fontSize:8,fontWeight:800,padding:"2px 5px",borderRadius:4,whiteSpace:"nowrap",fontFamily:"'DM Mono',monospace"}}>+R8,700</div>}
                </div>
                <span style={{fontSize:9,color:top?C.gold:C.muted,fontWeight:top?700:400,fontFamily:"'DM Mono',monospace"}}>{WDAYS[i]}</span>
              </div>
            );
          })}
        </div>
      </Card>

      <SLabel>My Contributions</SLabel>
      {MY_POOLS.map(p=>(
        <Card key={p.id} style={{marginBottom:11}}>
          <div style={{display:"flex",justifyContent:"space-between",alignItems:"center",marginBottom:11}}>
            <div style={{display:"flex",gap:11,alignItems:"center"}}>
              <div style={{width:38,height:38,borderRadius:11,background:C.light,display:"flex",alignItems:"center",justifyContent:"center",fontSize:18}}>{p.emoji}</div>
              <div>
                <div style={{fontWeight:700,fontSize:13,color:C.black}}>{p.name}</div>
                <div style={{fontSize:11,color:C.muted,marginTop:1}}>Contributed: <span style={{fontFamily:"'DM Mono',monospace",fontWeight:600,color:C.black}}>{fmt(p.contributed)}</span></div>
              </div>
            </div>
            <Tag type={p.status==="paid"?"green":"dark"}>{p.status==="paid"?"Paid Out":"Active"}</Tag>
          </div>
          <div style={{background:C.light,borderRadius:9,padding:"10px 12px",marginBottom:8}}>
            <div style={{display:"flex",justifyContent:"space-between",marginBottom:4}}>
              <span style={{fontSize:10,color:C.muted}}>Gross profit share ({p.profit}%)</span>
              <span style={{fontSize:10,fontWeight:700,color:C.black,fontFamily:"'DM Mono',monospace"}}>+{fmt(p.share)}</span>
            </div>
            <div style={{display:"flex",justifyContent:"space-between",marginBottom:4}}>
              <span style={{fontSize:10,color:C.red}}>Platform fee (8%)</span>
              <span style={{fontSize:10,fontWeight:700,color:C.red,fontFamily:"'DM Mono',monospace"}}>-{fmt(Math.round(p.share*0.08))}</span>
            </div>
            <div style={{height:1,background:C.border,marginBottom:4}}/>
            <div style={{display:"flex",justifyContent:"space-between"}}>
              <span style={{fontSize:11,fontWeight:700,color:C.green}}>Net profit share</span>
              <span style={{fontSize:13,fontWeight:900,color:C.green,fontFamily:"'DM Mono',monospace"}}>+{fmt(p.share-Math.round(p.share*0.08))}</span>
            </div>
          </div>
        </Card>
      ))}

      <SLabel>History</SLabel>
      {HISTORY.map((h,i)=>(
        <Card key={h.id} style={{marginBottom:10}}>
          <div style={{display:"flex",gap:11,alignItems:"center",marginBottom:10}}>
            <div style={{width:38,height:38,borderRadius:11,background:C.light,display:"flex",alignItems:"center",justifyContent:"center",fontSize:18,flexShrink:0}}>{h.emoji}</div>
            <div style={{flex:1}}><div style={{fontWeight:700,fontSize:13,color:C.black}}>{h.name}</div><div style={{fontSize:10,color:C.muted,marginTop:1}}>Joined {h.date}</div></div>
            <Tag type={h.status==="paid"?"green":"dark"}>{h.status==="paid"?"Paid Out":"Active"}</Tag>
          </div>
          <div style={{display:"flex",gap:8}}>
            <div style={{flex:1,background:C.light,borderRadius:8,padding:"8px 10px"}}><div style={{fontSize:9,color:C.muted,marginBottom:2}}>Contributed</div><div style={{fontSize:12,fontWeight:800,color:C.black,fontFamily:"'DM Mono',monospace"}}>{fmt(h.contributed)}</div></div>
            <div style={{flex:1,background:h.status==="paid"?C.greenP:C.goldP,border:`1px solid ${h.status==="paid"?C.greenB:C.goldB}`,borderRadius:8,padding:"8px 10px"}}><div style={{fontSize:9,color:C.muted,marginBottom:2}}>Net Share</div><div style={{fontSize:12,fontWeight:800,color:h.status==="paid"?C.green:C.gold,fontFamily:"'DM Mono',monospace"}}>+{fmt(h.share)}</div></div>
          </div>
          {h.ref&&<div style={{marginTop:7,fontSize:9,color:C.muted,textAlign:"right"}}>Ref: {h.ref} · {h.paidOn}</div>}
          {h.cycleEnd&&<div style={{marginTop:7,fontSize:9,color:C.black,fontWeight:600,textAlign:"right"}}>Closes {h.cycleEnd}</div>}
        </Card>
      ))}
    </div>
  );
};

// ── PROFILE ───────────────────────────────────────────────────────────────────
const ProfileScreen = ({setSubScreen}) => {
  const [twoFA,setTwoFA]=useState(true);
  const [bio,setBio]=useState(false);
  const [notifToggle,setNotifToggle]=useState(true);
  const [copied,setCopied]=useState(false);
  const [withdraw,setWithdraw]=useState(false);
  const [wAmt,setWAmt]=useState("");
  const [wDone,setWDone]=useState(false);
  const [wLoading,setWLoading]=useState(false);
  const [helpOpen,setHelpOpen]=useState(false);
  const [helpMsg,setHelpMsg]=useState("");
  const [helpSent,setHelpSent]=useState(false);
  const [faqOpen,setFaqOpen]=useState(null);

  const faqs=[
    {q:"How is my profit share calculated?",a:"Your share = (Your contribution ÷ Total pool) × Net profit. Net profit = Gross profit minus 8% platform fee."},
    {q:"When do I receive my profit share?",a:"Within 48 hours of a buying cycle closing. Balance updates automatically with a notification."},
    {q:"What is the 90-day guarantee?",a:"If products haven't sold within 90 days, Connectadon returns your full contribution. No conditions."},
    {q:"Is my money safe?",a:"Pool funds held in a dedicated Connectadon FNB Gold business account, separate from operational funds."},
    {q:"Can I withdraw mid-cycle?",a:"Contributions are locked for the cycle duration. Full share available once cycle closes."},
  ];

  if(withdraw) return (
    <div style={{padding:"20px 16px",fontFamily:"'Barlow',sans-serif",minHeight:"100vh",background:C.white}}>
      <BackBtn onBack={()=>{setWithdraw(false);setWDone(false);setWAmt("");}}/>
      <div style={{fontWeight:900,fontSize:22,color:C.black,marginBottom:16,fontFamily:"'Barlow Condensed',sans-serif",textTransform:"uppercase"}}>Withdraw Earnings</div>
      {wDone?(
        <div style={{textAlign:"center",padding:"40px 0",animation:"fadeUp 0.4s ease both"}}>
          <div style={{width:68,height:68,borderRadius:18,background:C.greenP,border:`2px solid ${C.greenB}`,margin:"0 auto 18px",display:"flex",alignItems:"center",justifyContent:"center",fontSize:28}}>✓</div>
          <div style={{fontWeight:900,fontSize:20,color:C.green,fontFamily:"'Barlow Condensed',sans-serif",textTransform:"uppercase",marginBottom:8}}>Submitted!</div>
          <div style={{fontSize:28,fontWeight:900,color:C.black,fontFamily:"'DM Mono',monospace",marginBottom:12}}>{fmt(wAmt)}</div>
          <div style={{fontSize:13,color:C.muted,lineHeight:1.7,marginBottom:24}}>Arrives in your FNB account within <strong style={{color:C.black}}>1–2 business days.</strong></div>
          <BlackBtn onClick={()=>{setWDone(false);setWAmt("");setWithdraw(false);}} style={{maxWidth:160,margin:"0 auto"}}>Done</BlackBtn>
        </div>
      ):(
        <>
          <div style={{background:C.black,borderRadius:14,padding:18,textAlign:"center",marginBottom:18}}>
            <div style={{fontSize:9,color:"rgba(255,255,255,0.35)",fontWeight:700,letterSpacing:1,textTransform:"uppercase",marginBottom:7,fontFamily:"'Barlow Condensed',sans-serif"}}>Available Balance</div>
            <div style={{fontSize:30,fontWeight:900,color:C.white,fontFamily:"'DM Mono',monospace"}}>R5 500<span style={{fontSize:14,color:"rgba(255,255,255,0.3)"}}>.00</span></div>
            <div style={{fontSize:10,color:C.green,marginTop:4}}>Minimum: R50</div>
          </div>
          <div style={{position:"relative",marginBottom:9}}>
            <span style={{position:"absolute",left:13,top:"50%",transform:"translateY(-50%)",color:C.muted,fontSize:15,fontWeight:700}}>R</span>
            <input type="number" value={wAmt} onChange={e=>setWAmt(e.target.value)} placeholder="0.00"
              style={{width:"100%",background:C.light,border:`1.5px solid ${wAmt&&Number(wAmt)>=50?C.green:C.border}`,borderRadius:10,padding:"14px 14px 14px 30px",fontSize:20,color:C.black,fontFamily:"'DM Mono',monospace",outline:"none"}}/>
          </div>
          <div style={{display:"flex",gap:7,marginBottom:18}}>
            {[500,1000,2500,5500].map(q=><button key={q} onClick={()=>setWAmt(String(q))} style={{flex:1,background:Number(wAmt)===q?C.black:C.white,border:`1px solid ${Number(wAmt)===q?C.black:C.border}`,borderRadius:7,padding:"7px 3px",fontSize:10,color:Number(wAmt)===q?C.gold:C.muted,cursor:"pointer",fontWeight:700,fontFamily:"'DM Mono',monospace"}}>{q===5500?"All":fmt(q)}</button>)}
          </div>
          <BlackBtn disabled={!wAmt||Number(wAmt)<50||Number(wAmt)>5500||wLoading}
            onClick={()=>{if(!wAmt||Number(wAmt)<50)return;setWLoading(true);setTimeout(()=>{setWLoading(false);setWDone(true);},2000);}}>
            {wLoading&&<Spin/>}{wLoading?"Processing…":`Withdraw ${wAmt?fmt(wAmt):""}`}
          </BlackBtn>
        </>
      )}
    </div>
  );

  if(helpOpen) return (
    <div style={{padding:"20px 16px",fontFamily:"'Barlow',sans-serif",minHeight:"100vh",background:C.white}}>
      <BackBtn onBack={()=>setHelpOpen(false)}/>
      <div style={{fontWeight:900,fontSize:22,color:C.black,marginBottom:18,fontFamily:"'Barlow Condensed',sans-serif",textTransform:"uppercase"}}>Help & Support</div>
      <div style={{display:"flex",gap:9,marginBottom:20}}>
        {[{icon:"💬",l:"Live Chat",s:"Online now",c:C.green},{icon:"📧",l:"Email Us",s:"support@connectadon.com",c:C.black}].map(c=>(
          <div key={c.l} style={{flex:1,background:C.white,border:`1px solid ${C.border}`,borderRadius:14,padding:14,textAlign:"center",cursor:"pointer"}}>
            <div style={{fontSize:24,marginBottom:7}}>{c.icon}</div>
            <div style={{fontWeight:700,fontSize:12,color:C.black,marginBottom:2}}>{c.l}</div>
            <div style={{fontSize:10,color:c.c,fontWeight:700}}>{c.s}</div>
          </div>
        ))}
      </div>
      <SLabel>FAQs</SLabel>
      {faqs.map((faq,i)=>(
        <div key={i} onClick={()=>setFaqOpen(faqOpen===i?null:i)} style={{background:C.white,border:`1px solid ${faqOpen===i?C.black:C.border}`,borderRadius:11,marginBottom:7,overflow:"hidden",cursor:"pointer"}}>
          <div style={{display:"flex",gap:11,alignItems:"center",padding:"13px 14px"}}>
            <span style={{fontSize:12,fontWeight:600,color:C.black,flex:1,lineHeight:1.4}}>{faq.q}</span>
            <span style={{color:C.muted,fontSize:15,transform:faqOpen===i?"rotate(90deg)":"none",transition:"transform 0.2s",flexShrink:0}}>›</span>
          </div>
          {faqOpen===i&&<div style={{padding:"0 14px 14px",borderTop:`1px solid ${C.border}`,paddingTop:11,fontSize:12,color:C.muted,lineHeight:1.7}}>{faq.a}</div>}
        </div>
      ))}
      <SLabel style={{marginTop:16}}>Send a Message</SLabel>
      {helpSent?(
        <div style={{background:C.greenP,border:`1px solid ${C.greenB}`,borderRadius:11,padding:18,textAlign:"center"}}>
          <div style={{fontSize:26,marginBottom:7}}>📨</div>
          <div style={{fontWeight:700,color:C.green}}>Message Sent!</div>
          <div style={{fontSize:11,color:C.muted,marginTop:3}}>We'll reply within 24 hours.</div>
        </div>
      ):(
        <Card>
          <textarea value={helpMsg} onChange={e=>setHelpMsg(e.target.value)} placeholder="Describe your issue…"
            style={{width:"100%",background:C.light,border:`1px solid ${C.border}`,borderRadius:9,padding:12,fontSize:12,color:C.black,minHeight:80,resize:"none",outline:"none",fontFamily:"'Barlow',sans-serif",marginBottom:12}}/>
          <BlackBtn disabled={!helpMsg.trim()} onClick={()=>{if(helpMsg.trim())setHelpSent(true);}}>Send Message</BlackBtn>
        </Card>
      )}
    </div>
  );

  const menu=[
    {icon:"📄",label:"Contribution History",sub:"5 pools · R13 500 earned"},
    {icon:"💧",label:"My Active Pools",sub:"3 pools currently active"},
    {icon:"💸",label:"Withdraw Earnings",sub:"R5 500.00 available",action:()=>setWithdraw(true)},
    {icon:"📊",label:"Profit Breakdown",sub:"Rates, formula & calculator",action:()=>setSubScreen&&setSubScreen("breakdown")},
    {icon:"🛡️",label:"Security Settings",sub:"2FA enabled"},
    {icon:"❓",label:"Help & Support",sub:"FAQs, live chat & contact",action:()=>setHelpOpen(true)},
    {icon:"🔗",label:"Refer a Member",sub:"Earn R100 per referral"},
  ];

  return (
    <div style={{padding:"20px 16px",fontFamily:"'Barlow',sans-serif"}}>
      <div style={{background:C.black,borderRadius:18,padding:22,textAlign:"center",marginBottom:14,position:"relative",overflow:"hidden"}}>
        <div style={{position:"absolute",bottom:0,left:0,right:0,height:2,background:C.gold}}/>
        <div style={{width:64,height:64,borderRadius:"50%",background:C.gold,margin:"0 auto 11px",display:"flex",alignItems:"center",justifyContent:"center",fontSize:24}}>👤</div>
        <div style={{fontWeight:900,fontSize:18,color:C.white,fontFamily:"'Barlow Condensed',sans-serif",textTransform:"uppercase"}}>Bokamoso Moshokwa</div>
        <div style={{fontSize:11,color:"rgba(255,255,255,0.35)",marginTop:3,marginBottom:13}}>Bokamoso@connectadon.com</div>
        <button style={{background:"transparent",border:"1px solid rgba(255,255,255,0.14)",borderRadius:7,padding:"6px 20px",fontSize:11,color:"rgba(255,255,255,0.55)",cursor:"pointer",fontFamily:"'Barlow Condensed',sans-serif",fontWeight:700,letterSpacing:0.5,textTransform:"uppercase"}}>Edit Profile</button>
      </div>

      <div style={{background:C.white,border:`1.5px solid ${C.black}`,borderRadius:14,padding:16,marginBottom:11,display:"flex",justifyContent:"space-between",alignItems:"center"}}>
        <div>
          <div style={{fontSize:9,color:C.muted,marginBottom:3,fontWeight:700,letterSpacing:1,textTransform:"uppercase",fontFamily:"'Barlow Condensed',sans-serif"}}>Available Balance</div>
          <div style={{fontSize:24,fontWeight:900,color:C.black,fontFamily:"'DM Mono',monospace"}}>R5 500.00</div>
        </div>
        <button onClick={()=>setWithdraw(true)} style={{background:C.black,color:C.gold,border:"none",borderRadius:9,padding:"10px 17px",fontSize:11,fontWeight:800,cursor:"pointer",fontFamily:"'Barlow Condensed',sans-serif",letterSpacing:1,textTransform:"uppercase"}}>Withdraw</button>
      </div>

      <div style={{display:"flex",gap:1,background:C.border,borderRadius:11,overflow:"hidden",marginBottom:18}}>
        <div style={{flex:1,background:C.greenP,padding:13,textAlign:"center"}}><div style={{fontSize:9,color:C.green,fontWeight:700,letterSpacing:0.5,textTransform:"uppercase",marginBottom:4,fontFamily:"'Barlow Condensed',sans-serif"}}>This Month</div><div style={{fontSize:17,fontWeight:900,color:C.green,fontFamily:"'DM Mono',monospace"}}>+R2 800</div></div>
        <div style={{flex:1,background:C.goldP,padding:13,textAlign:"center"}}><div style={{fontSize:9,color:C.gold,fontWeight:700,letterSpacing:0.5,textTransform:"uppercase",marginBottom:4,fontFamily:"'Barlow Condensed',sans-serif"}}>All Time</div><div style={{fontSize:17,fontWeight:900,color:C.goldD,fontFamily:"'DM Mono',monospace"}}>R13 500</div></div>
      </div>

      <Card style={{padding:"4px 0",marginBottom:14}}>
        {menu.map((item,i)=>(
          <div key={i} onClick={item.action||undefined} style={{display:"flex",alignItems:"center",gap:12,padding:"13px 16px",borderBottom:i<menu.length-1?`1px solid ${C.border}`:"none",cursor:item.action?"pointer":"default"}}>
            <div style={{width:36,height:36,borderRadius:9,background:C.light,display:"flex",alignItems:"center",justifyContent:"center",fontSize:16,flexShrink:0}}>{item.icon}</div>
            <div style={{flex:1}}><div style={{fontSize:13,fontWeight:600,color:C.black}}>{item.label}</div><div style={{fontSize:11,color:C.muted,marginTop:1}}>{item.sub}</div></div>
            <span style={{color:C.muted,fontSize:17}}>›</span>
          </div>
        ))}
      </Card>

      <Card style={{padding:"14px 16px",marginBottom:14}}>
        <div style={{fontSize:11,fontWeight:800,color:C.black,fontFamily:"'Barlow Condensed',sans-serif",letterSpacing:1,textTransform:"uppercase",marginBottom:12}}>Security</div>
        {[{l:"Two-Factor Auth",s:"Extra verification on login",v:twoFA,fn:setTwoFA},{l:"Biometric Login",s:"Fingerprint or Face ID",v:bio,fn:setBio},{l:"Security Alerts",s:"Alerts on suspicious activity",v:notifToggle,fn:setNotifToggle}].map((item,i)=>(
          <div key={i} style={{display:"flex",alignItems:"center",gap:12,marginBottom:i<2?12:0}}>
            <div style={{flex:1}}><div style={{fontSize:12,fontWeight:600,color:C.black}}>{item.l}</div><div style={{fontSize:10,color:C.muted}}>{item.s}</div></div>
            <Toggle val={item.v} onChange={item.fn}/>
          </div>
        ))}
      </Card>

      <div style={{background:C.black,borderRadius:16,padding:18,marginBottom:14,position:"relative",overflow:"hidden"}}>
        <div style={{position:"absolute",bottom:0,left:0,right:0,height:2,background:C.gold}}/>
        <div style={{fontSize:28,marginBottom:8}}>🎁</div>
        <div style={{fontWeight:900,fontSize:16,color:C.white,fontFamily:"'Barlow Condensed',sans-serif",textTransform:"uppercase",marginBottom:5}}>Earn R100 Per Referral</div>
        <div style={{fontSize:11,color:"rgba(255,255,255,0.4)",marginBottom:14}}>Share your code. They contribute. You earn instantly.</div>
        <div style={{background:"rgba(255,255,255,0.06)",borderRadius:10,padding:12,display:"flex",justifyContent:"space-between",alignItems:"center"}}>
          <div style={{fontSize:18,fontWeight:900,color:C.gold,fontFamily:"'DM Mono',monospace",letterSpacing:3}}>BOKA2025</div>
          <button onClick={()=>{setCopied(true);setTimeout(()=>setCopied(false),2000);}} style={{background:copied?C.greenP:C.gold,color:copied?C.green:C.black,border:copied?`1px solid ${C.green}`:"none",borderRadius:7,padding:"8px 14px",fontSize:11,fontWeight:800,cursor:"pointer",fontFamily:"'Barlow Condensed',sans-serif",letterSpacing:0.5,textTransform:"uppercase",transition:"all 0.3s"}}>{copied?"Copied ✓":"Copy"}</button>
        </div>
      </div>
      <button style={{width:"100%",background:"transparent",border:`1px solid ${C.red}44`,borderRadius:9,padding:13,fontSize:12,fontWeight:700,color:C.red,cursor:"pointer",fontFamily:"'Barlow Condensed',sans-serif",letterSpacing:1,textTransform:"uppercase"}}>Log Out</button>
    </div>
  );
};

// ── POOL APP ──────────────────────────────────────────────────────────────────
const PoolApp = ({onSwitchToShop,notifs,setNotifs}) => {
  const [tab,setTab] = useState("home");
  const [subScreen,setSubScreen] = useState(null);
  const [showNotif,setShowNotif] = useState(false);
  const unread = notifs.filter(n=>!n.read).length;
  const tabs = [{id:"home",icon:"⌂",label:"Home"},{id:"pools",icon:"◈",label:"Pools"},{id:"wallet",icon:"◎",label:"Earnings"},{id:"profile",icon:"◉",label:"Profile"}];

  if(showNotif) return <NotifScreen onBack={()=>setShowNotif(false)} notifs={notifs} setNotifs={setNotifs}/>;
  if(subScreen==="breakdown") return <ProfitBreakdown onBack={()=>setSubScreen(null)}/>;

  return (
    <div style={{minHeight:"100vh",background:C.white,display:"flex",flexDirection:"column"}}>
      <div style={{padding:"14px 18px",borderBottom:`1px solid ${C.border}`,background:C.white,position:"sticky",top:0,zIndex:20}}>
        <div style={{display:"flex",alignItems:"center",justifyContent:"space-between"}}>
          <Logo size={17}/>
          <div style={{display:"flex",gap:8,alignItems:"center"}}>
            <button onClick={()=>setShowNotif(true)} style={{position:"relative",width:34,height:34,borderRadius:9,background:C.light,border:"none",fontSize:14,cursor:"pointer",display:"flex",alignItems:"center",justifyContent:"center"}}>
              🔔{unread>0&&<div style={{position:"absolute",top:-2,right:-2,width:14,height:14,borderRadius:"50%",background:C.red,color:"#fff",fontSize:8,fontWeight:800,display:"flex",alignItems:"center",justifyContent:"center",animation:"pop 0.4s ease both"}}>{unread}</div>}
            </button>
            <button onClick={()=>setSubScreen("breakdown")} style={{width:34,height:34,borderRadius:9,background:C.light,border:"none",fontSize:14,cursor:"pointer",display:"flex",alignItems:"center",justifyContent:"center",title:"Profit Rates"}}>📊</button>
            <button onClick={onSwitchToShop} style={{background:C.black,border:"none",borderRadius:8,padding:"7px 12px",color:C.white,fontSize:10,fontWeight:800,cursor:"pointer",fontFamily:"'Barlow Condensed',sans-serif",letterSpacing:0.5,textTransform:"uppercase",display:"flex",alignItems:"center",gap:5}}>🛍️ Shop</button>
          </div>
        </div>
      </div>
      <div style={{flex:1,overflowY:"auto",paddingBottom:88}}>
        {tab==="home"&&<PoolHome setPoolTab={setTab} setSubScreen={setSubScreen}/>}
        {tab==="pools"&&<PoolsList/>}
        {tab==="wallet"&&<EarningsScreen setSubScreen={setSubScreen}/>}
        {tab==="profile"&&<ProfileScreen setSubScreen={setSubScreen}/>}
      </div>
      <div style={{position:"fixed",bottom:0,left:"50%",transform:"translateX(-50%)",width:"100%",maxWidth:430,background:"rgba(255,255,255,0.97)",borderTop:`1px solid ${C.border}`,display:"flex",zIndex:30}}>
        {tabs.map(t=>(
          <button key={t.id} onClick={()=>setTab(t.id)} style={{flex:1,padding:"11px 0 9px",background:"none",border:"none",cursor:"pointer",display:"flex",flexDirection:"column",alignItems:"center",gap:3,position:"relative"}}>
            {tab===t.id&&<div style={{position:"absolute",top:0,left:"50%",transform:"translateX(-50%)",width:26,height:2,borderRadius:99,background:C.gold}}/>}
            <span style={{fontSize:17,opacity:tab===t.id?1:0.3}}>{t.icon}</span>
            <span style={{fontSize:8,fontWeight:tab===t.id?800:400,color:tab===t.id?C.black:C.muted,letterSpacing:1,textTransform:"uppercase",fontFamily:"'Barlow Condensed',sans-serif"}}>{t.label}</span>
          </button>
        ))}
      </div>
    </div>
  );
};

// ── SHOP ──────────────────────────────────────────────────────────────────────
const ShopApp = ({onSwitchToPool,notifs,setNotifs}) => {
  const [shopSide,setShopSide] = useState(null);
  const [subView,setSubView] = useState(null);
  const [cart,setCart] = useState([]);
  const [showNotif,setShowNotif] = useState(false);
  const [catFilter,setCatFilter] = useState("All");
  const [brandFilter,setBrandFilter] = useState("All");
  const [sortBy,setSortBy] = useState("default");
  const [search,setSearch] = useState("");
  const [selSize,setSelSize] = useState(null);
  const [orderDone,setOrderDone] = useState(false);
  const cartCount = cart.reduce((a,i)=>a+i.qty,0);
  const unread = notifs.filter(n=>!n.read).length;
  const add = p=>setCart(prev=>{const e=prev.find(i=>i.id===p.id);return e?prev.map(i=>i.id===p.id?{...i,qty:i.qty+1}:i):[...prev,{...p,qty:1}];});

  if(showNotif) return <NotifScreen onBack={()=>setShowNotif(false)} notifs={notifs} setNotifs={setNotifs}/>;

  if(subView==="cart") return (
    <div style={{minHeight:"100vh",background:C.white,fontFamily:"'Barlow',sans-serif",padding:"20px 16px"}}>
      <BackBtn onBack={()=>setSubView(null)}/>
      <div style={{fontWeight:900,fontSize:22,color:C.black,marginBottom:18,fontFamily:"'Barlow Condensed',sans-serif",textTransform:"uppercase"}}>Your Cart ({cart.length})</div>
      {cart.length===0?<div style={{textAlign:"center",padding:"60px 0",color:C.muted}}><div style={{fontSize:44,marginBottom:10}}>🛒</div>Your cart is empty</div>:(
        <>
          {cart.map((item,i)=>(
            <Card key={item.id} style={{marginBottom:10}}>
              <div style={{display:"flex",gap:12,alignItems:"center"}}>
                <div style={{width:48,height:48,background:C.light,borderRadius:10,display:"flex",alignItems:"center",justifyContent:"center",fontSize:22,flexShrink:0}}>{item.emoji}</div>
                <div style={{flex:1}}>
                  <div style={{fontWeight:600,fontSize:13,color:C.black,marginBottom:1}}>{item.name}</div>
                  {item.size&&<div style={{fontSize:10,color:C.muted}}>Size: {item.size}</div>}
                  <div style={{fontSize:14,fontWeight:900,color:C.black,fontFamily:"'DM Mono',monospace"}}>{fmt(item.price)}</div>
                </div>
                <div style={{display:"flex",alignItems:"center",gap:7}}>
                  <button onClick={()=>setCart(prev=>prev.map(i=>i.id===item.id?{...i,qty:i.qty-1}:i).filter(i=>i.qty>0))} style={{width:28,height:28,borderRadius:6,background:C.light,border:"none",color:C.black,fontSize:16,cursor:"pointer",display:"flex",alignItems:"center",justifyContent:"center"}}>−</button>
                  <span style={{fontWeight:800,fontFamily:"'DM Mono',monospace",minWidth:16,textAlign:"center"}}>{item.qty}</span>
                  <button onClick={()=>setCart(prev=>prev.map(i=>i.id===item.id?{...i,qty:i.qty+1}:i))} style={{width:28,height:28,borderRadius:6,background:C.light,border:"none",color:C.black,fontSize:16,cursor:"pointer",display:"flex",alignItems:"center",justifyContent:"center"}}>+</button>
                </div>
              </div>
            </Card>
          ))}
          {!orderDone&&(
            <Card style={{marginTop:8,marginBottom:18}}>
              {[{l:"Subtotal",v:fmt(cart.reduce((a,i)=>a+i.price*i.qty,0))},{l:"Delivery",v:"R99.00"},{l:"Total",v:fmt(cart.reduce((a,i)=>a+i.price*i.qty,0)+99),bold:true}].map((r,i)=>(
                <div key={i} style={{display:"flex",justifyContent:"space-between",padding:"8px 0",borderBottom:i<2?`1px solid ${C.border}`:"none"}}>
                  <span style={{fontSize:13,color:r.bold?C.black:C.muted,fontWeight:r.bold?700:400}}>{r.l}</span>
                  <span style={{fontSize:13,fontWeight:r.bold?900:600,color:C.black,fontFamily:"'DM Mono',monospace"}}>{r.v}</span>
                </div>
              ))}
            </Card>
          )}
          {orderDone?(
            <div style={{textAlign:"center",padding:"20px 0",animation:"fadeUp 0.4s ease both"}}>
              <div style={{fontSize:40,marginBottom:10}}>✓</div>
              <div style={{fontWeight:900,fontSize:18,color:C.green,fontFamily:"'Barlow Condensed',sans-serif",textTransform:"uppercase",marginBottom:8}}>Order Placed!</div>
              <BlackBtn onClick={()=>{setOrderDone(false);setCart([]);setSubView(null);}} style={{maxWidth:180,margin:"0 auto"}}>Continue</BlackBtn>
            </div>
          ):(
            <BlackBtn onClick={()=>{setTimeout(()=>setOrderDone(true),1500);}}>Place Order</BlackBtn>
          )}
        </>
      )}
    </div>
  );

  if(subView&&typeof subView==="object") return (
    <div style={{minHeight:"100vh",background:C.white,fontFamily:"'Barlow',sans-serif",paddingBottom:40}}>
      <div style={{padding:"14px 16px",borderBottom:`1px solid ${C.border}`,display:"flex",alignItems:"center",justifyContent:"space-between",position:"sticky",top:0,background:C.white,zIndex:10}}>
        <BackBtn onBack={()=>{setSubView(null);setSelSize(null);}} label="Back"/>
      </div>
      <div style={{width:"100%",aspectRatio:"1.2",background:C.light,display:"flex",alignItems:"center",justifyContent:"center",fontSize:80,position:"relative"}}>
        {subView.emoji}
        {subView.badge&&<div style={{position:"absolute",top:16,left:16,background:C.black,color:C.white,borderRadius:4,padding:"3px 10px",fontSize:10,fontWeight:800,fontFamily:"'Barlow Condensed',sans-serif"}}>{subView.badge}</div>}
        <div style={{position:"absolute",top:16,right:16,background:"#FEE2E2",color:C.red,borderRadius:4,padding:"3px 10px",fontSize:10,fontWeight:800}}>-{Math.round(((subView.was-subView.price)/subView.was)*100)}%</div>
      </div>
      <div style={{padding:"20px 16px"}}>
        <div style={{display:"flex",gap:6,marginBottom:10}}><Tag>{subView.brand}</Tag><Tag>{subView.cat}</Tag></div>
        <h2 style={{fontSize:22,fontWeight:900,color:C.black,fontFamily:"'Barlow Condensed',sans-serif",textTransform:"uppercase",marginBottom:12,lineHeight:1.1}}>{subView.name}</h2>
        <div style={{display:"flex",alignItems:"center",gap:12,marginBottom:20}}>
          <div style={{fontSize:28,fontWeight:900,color:C.black,fontFamily:"'DM Mono',monospace"}}>{fmt(subView.price)}</div>
          <div style={{fontSize:16,color:C.muted,textDecoration:"line-through",fontFamily:"'DM Mono',monospace"}}>{fmt(subView.was)}</div>
        </div>
        {subView.sizes.length>0&&(
          <div style={{marginBottom:20}}>
            <SLabel>Select Size</SLabel>
            <div style={{display:"flex",gap:8,flexWrap:"wrap"}}>
              {subView.sizes.map(sz=><button key={sz} onClick={()=>setSelSize(sz)} style={{width:44,height:44,borderRadius:8,background:selSize===sz?C.black:C.white,border:`1.5px solid ${selSize===sz?C.black:C.border}`,color:selSize===sz?C.white:C.black,fontSize:12,fontWeight:700,cursor:"pointer",fontFamily:"'DM Mono',monospace",transition:"all 0.2s"}}>{sz}</button>)}
            </div>
            {!selSize&&<div style={{fontSize:11,color:C.muted,marginTop:8}}>Please select a size</div>}
          </div>
        )}
        <BlackBtn disabled={subView.sizes.length>0&&!selSize} style={{marginBottom:12}}
          onClick={()=>{if(subView.sizes.length>0&&!selSize)return;add({...subView,size:selSize});setSubView(null);setSelSize(null);}}>
          Add to Cart
        </BlackBtn>
        <div onClick={onSwitchToPool} style={{background:C.goldP,border:`1px solid ${C.goldB}`,borderRadius:10,padding:"12px 16px",display:"flex",alignItems:"center",justifyContent:"space-between",cursor:"pointer"}}>
          <div>
            <div style={{fontSize:11,fontWeight:700,color:C.black,marginBottom:2}}>Earn from this product instead?</div>
            <div style={{fontSize:10,color:C.muted}}>Join the pool and earn up to 60% profit share</div>
          </div>
          <span style={{color:C.gold,fontSize:16,fontWeight:900}}>→</span>
        </div>
      </div>
    </div>
  );

  if(!shopSide) return (
    <div style={{minHeight:"100vh",background:C.white,display:"flex",flexDirection:"column",fontFamily:"'Barlow',sans-serif"}}>
      <div style={{padding:"14px 18px",borderBottom:`1px solid ${C.border}`,display:"flex",alignItems:"center",justifyContent:"space-between",background:C.white}}>
        <Logo size={17}/>
        <div style={{display:"flex",gap:8}}>
          <button onClick={()=>setShowNotif(true)} style={{position:"relative",width:34,height:34,borderRadius:9,background:C.light,border:"none",fontSize:14,cursor:"pointer",display:"flex",alignItems:"center",justifyContent:"center"}}>
            🔔{unread>0&&<div style={{position:"absolute",top:-2,right:-2,width:14,height:14,borderRadius:"50%",background:C.red,color:"#fff",fontSize:8,fontWeight:800,display:"flex",alignItems:"center",justifyContent:"center",animation:"pop 0.4s ease both"}}>{unread}</div>}
          </button>
          <button onClick={()=>setSubView("cart")} style={{position:"relative",width:34,height:34,borderRadius:9,background:C.light,border:"none",fontSize:14,cursor:"pointer",display:"flex",alignItems:"center",justifyContent:"center"}}>
            🛒{cartCount>0&&<div style={{position:"absolute",top:-2,right:-2,width:14,height:14,borderRadius:"50%",background:C.black,color:"#fff",fontSize:8,fontWeight:800,display:"flex",alignItems:"center",justifyContent:"center"}}>{cartCount}</div>}
          </button>
        </div>
      </div>
      <div style={{flex:1,padding:"28px 20px",display:"flex",flexDirection:"column",gap:14,justifyContent:"center"}}>
        <div style={{textAlign:"center",marginBottom:16}}>
          <div style={{fontFamily:"'Barlow Condensed',sans-serif",fontWeight:900,fontSize:22,color:C.black,textTransform:"uppercase",letterSpacing:-0.4}}>What are you shopping for?</div>
        </div>
        <div onClick={()=>{setShopSide("tech");setCatFilter("All");setBrandFilter("All");setSearch("");}}
          style={{background:C.black,borderRadius:20,padding:"28px 22px",cursor:"pointer",position:"relative",overflow:"hidden"}}>
          <div style={{position:"absolute",bottom:0,left:0,right:0,height:3,background:C.gold}}/>
          <div style={{fontSize:40,marginBottom:10}}>🔌</div>
          <div style={{fontFamily:"'Barlow Condensed',sans-serif",fontWeight:900,fontSize:26,color:C.white,textTransform:"uppercase",letterSpacing:-0.5,marginBottom:6}}>Tech</div>
          <div style={{fontSize:12,color:"rgba(255,255,255,0.4)",lineHeight:1.6,marginBottom:14}}>Phones, laptops, tablets, earphones, chargers & accessories</div>
          <div style={{display:"flex",gap:7,flexWrap:"wrap"}}>
            {["Phones","Laptops","Tablets","Earphones","Accessories"].map(t=><Tag key={t} type="dark">{t}</Tag>)}
          </div>
        </div>
        <div onClick={()=>{setShopSide("lifestyle");setCatFilter("All");setBrandFilter("All");setSearch("");}}
          style={{background:C.white,border:`2px solid ${C.black}`,borderRadius:20,padding:"28px 22px",cursor:"pointer",position:"relative",overflow:"hidden"}}>
          <div style={{position:"absolute",bottom:0,left:0,right:0,height:3,background:C.gold}}/>
          <div style={{fontSize:40,marginBottom:10}}>✨</div>
          <div style={{fontFamily:"'Barlow Condensed',sans-serif",fontWeight:900,fontSize:26,color:C.black,textTransform:"uppercase",letterSpacing:-0.5,marginBottom:6}}>Lifestyle</div>
          <div style={{fontSize:12,color:C.muted,lineHeight:1.6,marginBottom:14}}>Clothing, sneakers, bags, jewellery, scents & sunglasses</div>
          <div style={{display:"flex",gap:7,flexWrap:"wrap"}}>
            {["Clothing","Sneakers","Bags","Jewellery","Scents"].map(t=><Tag key={t}>{t}</Tag>)}
          </div>
        </div>
      </div>
      <div style={{position:"fixed",bottom:20,right:16,zIndex:50}}>
        <button onClick={onSwitchToPool} style={{background:C.black,border:`2px solid ${C.gold}`,borderRadius:99,padding:"11px 18px",color:C.gold,fontSize:11,fontWeight:800,cursor:"pointer",fontFamily:"'Barlow Condensed',sans-serif",letterSpacing:0.5,textTransform:"uppercase",display:"flex",alignItems:"center",gap:7,boxShadow:"0 4px 20px rgba(0,0,0,0.3)"}}>
          💰 Pool & Earn
        </button>
      </div>
    </div>
  );

  const isTech = shopSide==="tech";
  const allProducts = isTech?TECH_PRODUCTS:LIFE_PRODUCTS;
  const cats = isTech?["All","Phones","Laptops","Tablets","Earphones","Accessories"]:["All","Sneakers","Scents","Sunglasses
