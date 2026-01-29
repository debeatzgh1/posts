<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Debeatzgh AI Blog Hub</title>

<style>
:root{
  --bg:#0b1220;
  --card:#111827;
  --muted:#9ca3af;
  --accent:#38bdf8;
  --radius:16px;
}
*{box-sizing:border-box}
body{
  margin:0;
  font-family:system-ui,-apple-system,Segoe UI,Roboto;
  background:var(--bg);
  color:#fff;
}

/* Header */
.header{
  position:sticky;top:0;z-index:1000;
  background:#020617;
  padding:12px;
}
.search{
  width:100%;
  padding:10px 14px;
  border-radius:999px;
  border:none;
  background:var(--card);
  color:#fff;
  font-size:14px;
}

/* Tabs */
.tabs{
  display:flex;
  gap:8px;
  margin-top:10px;
  overflow-x:auto;
}
.tabs button{
  background:var(--card);
  color:#fff;
  border:0;
  padding:8px 14px;
  border-radius:999px;
  cursor:pointer;
}
.tabs button.active{
  background:var(--accent);
  color:#000;
}

/* Sections */
.section{padding:14px}
.section h2{
  font-size:16px;
  display:flex;
  gap:8px;
  align-items:center;
}
.badge{
  font-size:11px;
  background:var(--accent);
  color:#000;
  padding:3px 8px;
  border-radius:999px;
}

/* Cards */
.carousel{
  display:flex;
  gap:12px;
  overflow-x:auto;
}
.card{
  min-width:260px;
  background:var(--card);
  border-radius:var(--radius);
  padding:12px;
  cursor:pointer;
}
.card img{
  width:100%;
  height:140px;
  object-fit:cover;
  border-radius:12px;
}
.card h3{font-size:14px}
.meta{font-size:11px;color:var(--muted)}
.tags{display:flex;gap:6px;flex-wrap:wrap}
.tag{
  font-size:10px;
  background:#020617;
  padding:3px 7px;
  border-radius:999px;
  color:var(--muted);
}

/* Lists */
.list .item{
  background:var(--card);
  padding:12px;
  border-radius:12px;
  margin-bottom:10px;
  cursor:pointer;
}

/* Viewer */
#viewer{
  position:fixed;inset:0;
  background:#000;
  display:none;
  z-index:99999;
}
.viewer-bar{
  position:absolute;
  top:8px;left:8px;right:8px;
  display:flex;justify-content:space-between;
}
.viewer-bar button{
  background:#020617;
  color:#fff;
  border:0;
  padding:8px 12px;
  border-radius:10px;
}
#viewer iframe{width:100%;height:100%;border:0}

/* AI Summary */
#summary{
  position:absolute;
  bottom:0;
  left:0;right:0;
  background:rgba(2,6,23,.95);
  padding:14px;
  font-size:13px;
  display:none;
}
#summary strong{color:var(--accent)}
</style>
</head>

<body>

<div class="header">
  <input id="search" class="search" placeholder="🔍 Search across all Debeatzgh blogs…">
  <div class="tabs">
    <button class="active" onclick="showTab('all')">All Blogs</button>
    <button onclick="showTab('trending')">🔥 Trending</button>
    <button onclick="showTab('recent')">🆕 Most Recent</button>
  </div>
</div>

<div id="all"></div>
<div id="trending" style="display:none" class="section list"></div>
<div id="recent" style="display:none" class="section list"></div>

<!-- Viewer -->
<div id="viewer">
  <div class="viewer-bar">
    <button onclick="toggleFull()">⛶ Full</button>
    <button onclick="toggleSummary()">🧠 AI Summary</button>
    <button onclick="closeViewer()">✖ Close</button>
  </div>
  <iframe id="frame"></iframe>
  <div id="summary"></div>
</div>

<script>
const blogs=[
 {name:"Debeatzgh WP",url:"https://debeatzgh.wordpress.com/feed/",badge:"WordPress"},
 {name:"Debeatzgh Blogspot",url:"https://debeatzgh1.blogspot.com/feeds/posts/default?alt=rss",badge:"Blogspot"},
 {name:"Beatzde4",url:"https://beatzde4.blogspot.com/feeds/posts/default?alt=rss",badge:"AI Hustle"},
 {name:"DigiMart GH",url:"https://digimartgh.blogspot.com/feeds/posts/default?alt=rss",badge:"E-Commerce"},
 {name:"Debeatzgh 2",url:"https://debeatzgh2.blogspot.com/feeds/posts/default?alt=rss",badge:"Updates"},
 {name:"My Brands Online",url:"https://mybrandsonline.blogspot.com/feeds/posts/default?alt=rss",badge:"Branding"}
];

const allPosts=[];
const allContainer=document.getElementById("all");
const trending=document.getElementById("trending");
const recent=document.getElementById("recent");

/* Load blogs */
blogs.forEach(async(b)=>{
  const sec=document.createElement("div");
  sec.className="section";
  sec.innerHTML=`<h2>${b.name}<span class="badge">${b.badge}</span></h2><div class="carousel"></div>`;
  allContainer.appendChild(sec);
  const car=sec.querySelector(".carousel");

  const res=await fetch(`https://api.rss2json.com/v1/api.json?rss_url=${encodeURIComponent(b.url)}`);
  const data=await res.json();

  data.items.slice(0,10).forEach(p=>{
    const post={...p,source:b.name};
    allPosts.push(post);

    const c=document.createElement("div");
    c.className="card";
    c.innerHTML=`
      <img src="${p.thumbnail||'https://via.placeholder.com/400x200'}">
      <h3>${p.title}</h3>
      <div class="meta">${b.name} · ${p.pubDate.split(" ")[0]}</div>
      <div class="tags">${(p.categories||[]).slice(0,4).map(t=>`<span class="tag">${t}</span>`).join("")}</div>
    `;
    c.onclick=()=>openViewer(p.link,p.description);
    car.appendChild(c);
  });

  setInterval(()=>car.scrollBy({left:280,behavior:"smooth"}),5000);
});

/* Trending & Recent */
setTimeout(()=>{
  const sorted=[...allPosts].sort((a,b)=>new Date(b.pubDate)-new Date(a.pubDate));
  sorted.slice(0,15).forEach(p=>{
    const el=document.createElement("div");
    el.className="item";
    el.innerHTML=`<strong>${p.title}</strong><br><small>${p.source}</small>`;
    el.onclick=()=>openViewer(p.link,p.description);
    recent.appendChild(el);
  });
  sorted.slice(0,15).sort(()=>Math.random()-0.5).forEach(p=>{
    const el=document.createElement("div");
    el.className="item";
    el.innerHTML=`🔥 <strong>${p.title}</strong><br><small>${p.source}</small>`;
    el.onclick=()=>openViewer(p.link,p.description);
    trending.appendChild(el);
  });
},3000);

/* Tabs */
function showTab(id){
  ["all","trending","recent"].forEach(t=>{
    document.getElementById(t).style.display=t===id?"block":"none";
  });
  document.querySelectorAll(".tabs button").forEach(b=>b.classList.remove("active"));
  event.target.classList.add("active");
}

/* Viewer + AI Summary */
const viewer=document.getElementById("viewer");
const frame=document.getElementById("frame");
const summaryBox=document.getElementById("summary");
let currentDesc="";

function openViewer(url,desc){
  frame.src=url;
  currentDesc=desc||"";
  summaryBox.style.display="none";
  viewer.style.display="block";
}
function closeViewer(){
  frame.src="";
  viewer.style.display="none";
}
function toggleFull(){
  !document.fullscreenElement?viewer.requestFullscreen():document.exitFullscreen();
}

/* Simple AI-style summarizer */
function summarize(text){
  const clean=text.replace(/<[^>]+>/g,"");
  const sentences=clean.split(".").filter(s=>s.length>40);
  return sentences.slice(0,3).join(". ")+"…";
}
function toggleSummary(){
  if(!summaryBox.style.display || summaryBox.style.display==="none"){
    summaryBox.innerHTML="<strong>AI Summary:</strong><br>"+summarize(currentDesc);
    summaryBox.style.display="block";
  }else summaryBox.style.display="none";
}

/* Search */
document.getElementById("search").addEventListener("input",e=>{
  const q=e.target.value.toLowerCase();
  recent.innerHTML="";
  if(!q){return;}
  showTab("recent");
  allPosts.filter(p=>p.title.toLowerCase().includes(q))
    .slice(0,20)
    .forEach(p=>{
      const el=document.createElement("div");
      el.className="item";
      el.innerHTML=`<strong>${p.title}</strong><br><small>${p.source}</small>`;
      el.onclick=()=>openViewer(p.link,p.description);
      recent.appendChild(el);
    });
});
</script>

</body>
</html>





# 📚 **AI DIGITAL LIBRARY — 20 Premium Blog Posts + Tools**

Welcome to your **AI-powered Digital Library**, a clean and interactive GitHub-style UI featuring 20 curated guides, toolkits, and AI content resources.

Use the buttons to navigate each post instantly.

---

# 🎨 **📌 Global Styles (GitHub-Safe)**

> These inline CSS styles are fully allowed inside README files.

```html
<style>
.library-card {
  border: 2px solid #e5e7eb;
  border-radius: 15px;
  padding: 20px;
  margin-bottom: 30px;
  background: #fafafa;
}

.card-title {
  font-size: 22px;
  font-weight: bold;
  color: #111827;
  margin-bottom: 10px;
}

.card-desc {
  font-size: 16px;
  color: #374151;
  margin-bottom: 15px;
}

.btn {
  display: inline-block;
  padding: 12px 18px;
  border-radius: 10px;
  margin-right: 10px;
  text-decoration: none;
  font-weight: 600;
}

.btn-view {
  background: #2563eb;
  color: white;
}

.btn-alt {
  background: #10b981;
  color: white;
}
</style>
```

---

# 📘 **20-Post Digital Library (With Thumbnails + Buttons)**

Below are the full 20 posts displayed using GitHub-friendly HTML.

---

## **1. AI Prompts for Startups**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=AI+Prompts+for+Startups" width="100%">
<p class="card-desc">A complete beginner-friendly guide helping startup founders use AI to automate tasks, build workflows, and scale faster.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/">Read Guide</a>
</div>

---

## **2. AI Prompts for Bloggers**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=AI+Prompts+for+Bloggers" width="100%">
<p class="card-desc">Master AI content generation — write posts, headlines, SEO content, and niche ideas effortlessly.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/">Open Article</a>
</div>

---

## **3. Side Hustle Starter Kit**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=Side+Hustle+Starter+Kit" width="100%">
<p class="card-desc">A complete starter kit for beginners who want to launch profitable online side hustles.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/Side-hustle-starter-kit-/">Open Kit</a>
</div>

---

## **4. Online Business Tools & Ideas**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=Online+Business+Tools" width="100%">
<p class="card-desc">A full-stack guide with automation tools, AI workflow builders, and business scaling strategies.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/Curated-Guides-for-Online-Business-AI-Product-Creation/">Read Guide</a>
</div>

---

## **5. Tech Business Tools & Ideas**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=Tech+Business+Tools" width="100%">
<p class="card-desc">Top digital tools & strategies curated for creators, devs, and tech entrepreneurs.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/Home-/">Explore</a>
</div>

---

## **6. AI Affiliate Marketing Guide**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=AI+Affiliate+Marketing" width="100%">
<p class="card-desc">A full roadmap on building AI-powered systems to automate affiliate marketing.</p>
<a class="btn btn-view" href="https://beatzde4.blogspot.com/2025/08/ai-affiliate-marketing.html">Open Article</a>
</div>

---

## **7. AI Photography Prompts**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=AI+Photography+Prompts" width="100%">
<p class="card-desc">Perfect prompts for AI photography, concept art, and creative projects.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/">Read Guide</a>
</div>

---

## **8. Blogging: From Passion to Profits**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=Blogging+Profit+Guide" width="100%">
<p class="card-desc">Your ultimate guide to turning blogging passion into long-term income streams.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/">Open Resource</a>
</div>

---

## **9. Free AI Web App Promo Post**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=Free+AI+Web+App" width="100%">
<p class="card-desc">A full social campaign template: Followers comment “Free App” to claim a custom AI tool.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/">Details</a>
</div>

---

## **10. Digital Products Affiliate Shop**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=Digital+Products+Affiliate+Shop" width="100%">
<p class="card-desc">Your curated store of digital tools, templates, and online business assets.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/-My-Brand-Online-Digital-Products-Affiliate-Shop/">Visit Shop</a>
</div>

---

# 🔟 **Additional 10 Posts (11–20)**

*(All with thumbnails + buttons)*

---

## **11. AI Tools for Content Creators**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=AI+Content+Creator+Tools" width="100%">
<p class="card-desc">A curated list of creator-friendly AI tools for video, blogs, graphics, and automation.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/">Read More</a>
</div>

---

## **12. Ultimate AI Automation Tools List**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=Automation+Tools" width="100%">
<p class="card-desc">A powerful collection of automation apps for workflows, tasks, and productivity.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/">Open List</a>
</div>

---

## **13. Beginner’s Guide to AI Productivity Hacks**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=AI+Productivity+Hacks" width="100%">
<p class="card-desc">Use AI to save time, simplify workflows, and improve daily operations.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/">Read Article</a>
</div>

---

## **14. Top AI Tools for Students**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=AI+for+Students" width="100%">
<p class="card-desc">From study tools to research engines—AI that boosts academic performance.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/">Access Guide</a>
</div>

---

## **15. Zero-Budget Business Tools**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=Zero+Budget+Tools" width="100%">
<p class="card-desc">Free digital tools to build, launch, and run an online business without spending.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/">Open Guide</a>
</div>

---

## **16. AI Branding & Design Toolkit**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=Branding+Toolkit" width="100%">
<p class="card-desc">Logos, banner prompts, color palettes, brand kits—all powered by AI.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/">Explore Toolkit</a>
</div>

---

## **17. AI Video Creation Tools**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=AI+Video+Tools" width="100%">
<p class="card-desc">Tools for auto-generating videos, animations, and social media visuals.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/">Open Article</a>
</div>

---

## **18. Passive Income with AI**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=Passive+Income+with+AI" width="100%">
<p class="card-desc">Learn how to create digital assets that generate money on autopilot.</p>
<a class="btn btn-view" href="https://beatzde4.blogspot.com/">Read Article</a>
</div>

---

## **19. AI Tools for Social Media Marketing**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=AI+Social+Media+Marketing" width="100%">
<p class="card-desc">Automate posts, create viral content, and analyze engagement using AI.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/">Open Guide</a>
</div>

---

## **20. How to Build an AI Portfolio**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=AI+Portfolio" width="100%">
<p class="card-desc">A step-by-step guide for creating a professional AI portfolio on GitHub.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/">Start Now</a>
</div>

---
<!-- Start of OpenWidget (www.openwidget.com) code -->
<script>
  window.__ow = window.__ow || {};
  window.__ow.organizationId = "8dbd48aa-17f7-49aa-b323-23eb6ad358fa";
  window.__ow.integration_name = "manual_settings";
  window.__ow.product_name = "openwidget";   
  ;(function(n,t,c){function i(n){return e._h?e._h.apply(null,n):e._q.push(n)}var e={_q:[],_h:null,_v:"2.0",on:function(){i(["on",c.call(arguments)])},once:function(){i(["once",c.call(arguments)])},off:function(){i(["off",c.call(arguments)])},get:function(){if(!e._h)throw new Error("[OpenWidget] You can't use getters before load.");return i(["get",c.call(arguments)])},call:function(){i(["call",c.call(arguments)])},init:function(){var n=t.createElement("script");n.async=!0,n.type="text/javascript",n.src="https://cdn.openwidget.com/openwidget.js",t.head.appendChild(n)}};!n.__ow.asyncInit&&e.init(),n.OpenWidget=n.OpenWidget||e}(window,document,[].slice))
</script>
<noscript>You need to <a href="https://www.openwidget.com/enable-javascript" rel="noopener nofollow">enable JavaScript</a> to use the communication tool powered by <a href="https://www.openwidget.com/" rel="noopener nofollow" target="_blank">OpenWidget</a></noscript>
<!-- End of OpenWidget code -->
