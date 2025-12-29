---
title: "Blog Lập Trình"
description: "Chào mừng đến với Blog cá nhân của mình. Nơi chia sẻ kiến thức về Lập trình mạng, Java và JavaScript."
hidemeta: true
---

<div style="margin-bottom:40px">
<div style="display:flex;align-items:center;justify-content:space-between;gap:30px;flex-wrap:wrap">
<div style="flex:1;min-width:300px">
<p style="margin-bottom:15px;font-size:1.1rem"></p>
<div style="margin-top:20px;padding:20px;background:#f7f7f7;border-radius:12px;border:1px solid #eee;position:relative">
<div style="display:flex;gap:10px">
<input type="text" id="live-search-input" placeholder="Gõ từ khóa để tìm nhanh..." onkeyup="liveSearch()" style="flex:1;padding:12px;border-radius:6px;border:1px solid #ccc;outline:none">
</div>
<ul id="live-search-results" style="list-style:none;padding:0;margin:10px 0 0 0;display:none;background:white;border:1px solid #eee;border-radius:6px;max-height:200px;overflow-y:auto;box-shadow:0 4px 6px rgba(0,0,0,0.05)"></ul>
</div>
<div style="margin-top:15px;display:flex;gap:10px;flex-wrap:wrap">
<span style="font-weight:bold;font-size:0.9rem;align-self:center">Chủ đề:</span>
<a href="/categories/java/" style="background:#eee;padding:5px 12px;border-radius:15px;font-size:0.85rem;text-decoration:none;color:#333;font-weight:500;transition:0.2s">Java</a>
<a href="/categories/network/" style="background:#eee;padding:5px 12px;border-radius:15px;font-size:0.85rem;text-decoration:none;color:#333;font-weight:500;transition:0.2s">Network</a>
<a href="/categories/javascript/" style="background:#eee;padding:5px 12px;border-radius:15px;font-size:0.85rem;text-decoration:none;color:#333;font-weight:500;transition:0.2s">JS</a>
</div>
</div>
<img src="/assets/meo.png" alt="Logo Blog" style="width:100px;height:200px;object-fit:cover;border-radius:0%;box-shadow:0 0px 0px rgba(0,0,0,0.1);border:0px solid white">
</div>
</div>

<script>
let searchData=null;async function liveSearch(){const query=document.getElementById('live-search-input').value.toLowerCase();const resultsList=document.getElementById('live-search-results');if(query.length===0){resultsList.style.display='none';return;}if(!searchData){try{const response=await fetch('/index.json');searchData=await response.json();}catch(error){console.error("Lỗi tải dữ liệu tìm kiếm:",error);return;}}const results=searchData.filter(item=>{const title=item.title?item.title.toLowerCase():"";const content=item.content?item.content.toLowerCase():"";return title.includes(query)||content.includes(query);});resultsList.innerHTML='';resultsList.style.display='block';if(results.length===0){resultsList.innerHTML='<li style="padding:10px;color:#666;">Không tìm thấy kết quả nào.</li>';}else{results.slice(0,5).forEach(item=>{const li=document.createElement('li');li.innerHTML=`<a href="${item.permalink}" style="display:block;padding:10px;text-decoration:none;border-bottom:1px solid #f0f0f0;color:#333;font-weight:500;">${item.title}</a>`;li.querySelector('a').onmouseover=function(){this.style.backgroundColor='#f9f9f9';};li.querySelector('a').onmouseout=function(){this.style.backgroundColor='transparent';};resultsList.appendChild(li);});}}
</script>
