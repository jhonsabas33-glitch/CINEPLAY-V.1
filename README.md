<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="theme-color" content="#090b10">
  <title>MovieBox — Discover Movies</title>
  <style>
    :root{--bg:#090b10;--panel:#121722;--muted:#9ba5b5;--text:#f7f8fb;--accent:#e53945;--line:#252d3a}
    *{box-sizing:border-box} html{scroll-behavior:smooth}
    body{margin:0;background:var(--bg);color:var(--text);font-family:Inter,Arial,sans-serif}
    button,input{font:inherit} button{cursor:pointer}
    .topbar{height:70px;display:flex;align-items:center;justify-content:space-between;padding:0 6%;border-bottom:1px solid var(--line);position:sticky;top:0;background:#090b10ed;backdrop-filter:blur(14px);z-index:5}
    .brand{font-size:24px;font-weight:900;letter-spacing:-1px}.brand span{color:var(--accent)}
    nav{display:flex;gap:24px}nav a{color:#c5ccd7;text-decoration:none;font-size:14px}nav a:hover{color:white}
    .hero{padding:76px 6% 62px;background:radial-gradient(ellipse at 78% 30%,#51202a 0,transparent 42%),linear-gradient(110deg,#151b27,#090b10 65%)}
    .eyebrow{color:#ff7b84;text-transform:uppercase;letter-spacing:3px;font-size:11px;font-weight:800}
    h1{font-size:clamp(38px,6vw,68px);line-height:1.02;max-width:650px;margin:14px 0}
    .hero p{color:#b9c1cd;max-width:520px;line-height:1.7}
    .hero-actions{display:flex;gap:12px;margin-top:26px;flex-wrap:wrap}
    .btn{border:0;border-radius:8px;padding:12px 19px;font-weight:800;background:var(--accent);color:white}
    .btn.secondary{background:#ffffff14;border:1px solid #ffffff30}
    main{padding:30px 6% 70px}.toolbar{display:flex;gap:12px;align-items:center;justify-content:space-between;flex-wrap:wrap;margin-bottom:25px}
    .search{background:var(--panel);border:1px solid var(--line);color:white;padding:13px 15px;border-radius:9px;width:min(100%,330px);outline:none}
    .filters{display:flex;gap:8px;flex-wrap:wrap}.filter{border:1px solid var(--line);background:var(--panel);color:#c5ccd7;border-radius:30px;padding:9px 15px;font-size:13px}
    .filter.active{background:var(--accent);border-color:var(--accent);color:white}
    .section-head{display:flex;align-items:end;justify-content:space-between;margin:30px 0 17px}.section-head h2{margin:0;font-size:23px}.section-head small{color:var(--muted)}
    .grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(155px,1fr));gap:19px}
    .movie{min-width:0;cursor:pointer}.poster{height:235px;border-radius:11px;overflow:hidden;background:linear-gradient(145deg,var(--c1),var(--c2));display:flex;align-items:end;padding:14px;position:relative;box-shadow:0 8px 25px #0005;transition:.2s}
    .movie:hover .poster{transform:translateY(-5px);box-shadow:0 14px 30px #0008}
    .poster:before{content:"";position:absolute;inset:0;background:linear-gradient(0deg,#000c,transparent 70%)}
    .poster .poster-title{position:relative;font-size:19px;font-weight:900;line-height:1.05;text-shadow:0 2px 6px #000}
    .tag{position:absolute;top:10px;left:10px;background:#090b10c9;padding:5px 8px;border-radius:5px;font-size:10px;font-weight:800}
    .movie h3{font-size:14px;margin:10px 0 4px}.meta{font-size:12px;color:var(--muted)}
    .empty{display:none;color:var(--muted);padding:40px;text-align:center;border:1px dashed var(--line);border-radius:12px}
    footer{border-top:1px solid var(--line);padding:25px 6%;color:var(--muted);font-size:12px;line-height:1.7}
    .modal{display:none;position:fixed;inset:0;background:#000c;z-index:10;align-items:center;justify-content:center;padding:20px}
    .modal.open{display:flex}.modal-card{background:#141a25;border:1px solid var(--line);border-radius:15px;max-width:560px;width:100%;padding:25px;position:relative}
    .close{position:absolute;right:17px;top:13px;border:0;background:transparent;color:white;font-size:25px}
    .modal-card h2{margin:0 35px 8px 0}.modal-card p{color:#b7c0ce;line-height:1.6}
    @media(max-width:600px){.topbar{padding:0 5%;height:62px}nav{gap:12px}nav a{font-size:12px}.hero{padding:55px 5%}.grid{grid-template-columns:repeat(2,minmax(0,1fr));gap:13px}.poster{height:220px}main{padding:25px 5% 55px}}
  </style>
</head>
<body>
  <header class="topbar">
    <div class="brand">MOVIE<span>BOX</span></div>
    <nav><a href="#home">Home</a><a href="#movies">Movies</a><a href="#about">About</a></nav>
  </header>

  <section class="hero" id="home">
    <div class="eyebrow">Your movie destination</div>
    <h1>Find your next<br>favorite story.</h1>
    <p>Discover films, explore genres, and keep your watchlist ideas in one place. A clean, free movie-library experience.</p>
    <div class="hero-actions">
      <button class="btn" onclick="document.querySelector('#movies').scrollIntoView({behavior:'smooth'})">Explore Movies</button>
      <button class="btn secondary" onclick="showAbout()">About MovieBox</button>
    </div>
  </section>

  <main id="movies">
    <div class="toolbar">
      <input id="search" class="search" type="search" placeholder="Search movies..." aria-label="Search movies">
      <div class="filters" id="filters">
        <button class="filter active" data-genre="All">All</button>
        <button class="filter" data-genre="Action">Action</button>
        <button class="filter" data-genre="Drama">Drama</button>
        <button class="filter" data-genre="Horror">Horror</button>
        <button class="filter" data-genre="Comedy">Comedy</button>
        <button class="filter" data-genre="Animation">Animation</button>
      </div>
    </div>
    <div class="section-head"><h2>Featured Movies</h2><small id="count"></small></div>
    <div class="grid" id="movieGrid"></div>
    <div class="empty" id="empty">No movies found. Try another title or category.</div>
  </main>

  <footer id="about">
    <strong>MovieBox V1.0</strong><br>
    A free movie discovery website starter project. Only add films you own, public-domain films, or content you have permission to stream. MovieBox does not host or provide unauthorized copies of copyrighted films.
  </footer>

  <div class="modal" id="modal" role="dialog" aria-modal="true" aria-labelledby="modalTitle" onclick="if(event.target===this)closeModal()">
    <div class="modal-card">
      <button class="close" aria-label="Close" onclick="closeModal()">×</button>
      <div class="eyebrow" id="modalGenre">MOVIE DETAILS</div>
      <h2 id="modalTitle">Movie title</h2>
      <div class="meta" id="modalMeta"></div>
      <p id="modalDescription"></p>
      <button class="btn" id="watchButton">Watch movie</button>
      <p class="meta" style="margin-top:13px">The watch link is managed by the site owner. Add an authorized video URL in the movie data.</p>
    </div>
  </div>

  <script>
    // VERSION 1: Replace these sample entries with films you own or are authorized to stream.
    // poster colors are temporary artwork; watchUrl should point to an authorized stream or official viewing page.
    const movies = [
      {id:1,title:"Midnight Run",genre:"Action",year:2025,rating:"Demo",description:"A sample action-movie entry. Replace this description with your own movie information.",c1:"#542b38",c2:"#111827",watchUrl:""},
      {id:2,title:"Silent Promise",genre:"Drama",year:2024,rating:"Demo",description:"A sample drama entry for testing the movie details window.",c1:"#34516a",c2:"#171c2d",watchUrl:""},
      {id:3,title:"The Last Knock",genre:"Horror",year:2025,rating:"Demo",description:"A sample horror entry. Add your authorized movie link when ready.",c1:"#263d35",c2:"#090d12",watchUrl:""},
      {id:4,title:"Laugh Trip",genre:"Comedy",year:2023,rating:"Demo",description:"A sample comedy entry for your first movie catalog.",c1:"#98652d",c2:"#3d2630",watchUrl:""},
      {id:5,title:"Beyond The Sky",genre:"Animation",year:2024,rating:"Demo",description:"A sample animation entry. Replace it with a film you have permission to show.",c1:"#3a6392",c2:"#29234e",watchUrl:""},
      {id:6,title:"Final Mission",genre:"Action",year:2022,rating:"Demo",description:"A second sample action entry to test category filtering.",c1:"#6c3b29",c2:"#171a26",watchUrl:""}
    ];
    let activeGenre = "All";
    const grid = document.getElementById("movieGrid");
    const search = document.getElementById("search");

    function renderMovies(){
      const term = search.value.trim().toLowerCase();
      const shown = movies.filter(m => (activeGenre==="All" || m.genre===activeGenre) && (m.title.toLowerCase().includes(term) || m.genre.toLowerCase().includes(term)));
      grid.innerHTML = shown.map(m => `
        <article class="movie" tabindex="0" role="button" aria-label="View ${m.title}" onclick="openMovie(${m.id})" onkeydown="if(event.key==='Enter'||event.key===' '){event.preventDefault();openMovie(${m.id})}">
          <div class="poster" style="--c1:${m.c1};--c2:${m.c2}">
            <span class="tag">${m.genre}</span><div class="poster-title">${m.title}</div>
          </div>
          <h3>${m.title}</h3><div class="meta">${m.year} · ${m.genre}</div>
        </article>`).join("");
      document.getElementById("count").textContent = `${shown.length} titles`;
      document.getElementById("empty").style.display = shown.length ? "none" : "block";
    }
    document.getElementById("filters").addEventListener("click", e => {
      const btn=e.target.closest("button[data-genre]"); if(!btn)return;
      activeGenre=btn.dataset.genre;
      document.querySelectorAll(".filter").forEach(b=>b.classList.toggle("active",b===btn));
      renderMovies();
    });
    search.addEventListener("input",renderMovies);
    function openMovie(id){
      const m=movies.find(x=>x.id===id); if(!m)return;
      document.getElementById("modalGenre").textContent=m.genre.toUpperCase();
      document.getElementById("modalTitle").textContent=m.title;
      document.getElementById("modalMeta").textContent=`${m.year} · ${m.genre} · ${m.rating}`;
      document.getElementById("modalDescription").textContent=m.description;
      const watch=document.getElementById("watchButton");
      watch.disabled=!m.watchUrl;
      watch.style.opacity=m.watchUrl?1:.55;
      watch.textContent=m.watchUrl?"Watch movie":"Watch link coming soon";
      watch.onclick=()=>{if(m.watchUrl)window.open(m.watchUrl,"_blank","noopener,noreferrer")};
      document.getElementById("modal").classList.add("open");
    }
    function closeModal(){document.getElementById("modal").classList.remove("open")}
    function showAbout(){document.getElementById("about").scrollIntoView({behavior:"smooth"})}
    document.addEventListener("keydown",e=>{if(e.key==="Escape")closeModal()});
    renderMovies();
  </script>
</body>
</html>
