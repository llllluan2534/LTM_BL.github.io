---
title: " "
hidemeta: true
layout: "about"
ShowToc: false
---

<style>
.cert-card{background:var(--entry);border:1px solid var(--border);border-radius:16px;padding:25px;transition:transform 0.3s ease,box-shadow 0.3s ease;display:flex;flex-direction:column;align-items:center;text-align:center;height:100%}
.cert-card:hover{transform:translateY(-5px);box-shadow:0 10px 20px rgba(0,0,0,0.1);border-color:var(--primary)}
.btn-view-cert{margin-top:auto;padding:8px 20px;border:1px solid var(--primary);color:var(--primary);border-radius:50px;text-decoration:none;font-size:0.9rem;font-weight:bold;transition:all 0.2s;cursor:pointer;background:transparent}
.btn-view-cert:hover{background:var(--primary);color:var(--theme)}
#certModal{display:none;position:fixed;z-index:9999;left:0;top:0;width:100%;height:100%;overflow:auto;background-color:rgba(0,0,0,0.9);backdrop-filter:blur(5px);align-items:center;justify-content:center;animation:fadeIn 0.3s}
#modalImg{max-width:90%;max-height:90%;border-radius:8px;box-shadow:0 0 20px rgba(255,255,255,0.2);animation:zoomIn 0.3s}
.close-modal{position:absolute;top:20px;right:30px;color:#f1f1f1;font-size:40px;font-weight:bold;cursor:pointer;transition:0.3s}
.close-modal:hover{color:var(--primary)}
@keyframes fadeIn{from{opacity:0}to{opacity:1}}
@keyframes zoomIn{from{transform:scale(0.8)}to{transform:scale(1)}}
</style>

<div style="max-width:100%">
<div style="display:flex;flex-wrap:wrap;align-items:center;justify-content:space-between;gap:40px;margin-bottom:80px;padding-top:20px">
<div style="flex:1;min-width:300px">
<div style="font-size:1.1rem;color:var(--primary);font-weight:bold;letter-spacing:1px;margin-bottom:10px;text-transform:uppercase">Web Developer</div>
<h1 style="margin:0 0 20px 0;font-size:3.5rem;line-height:1.1">Bich Luan</h1>
<p style="opacity:0.9;line-height:1.6;font-size:1.1rem;margin-bottom:30px">
  Hello! I’m a final-year IT student specializing in <strong>Frontend Development</strong>, 
  with a focus on building modern user interfaces using <strong>React</strong> 
  and optimizing performance and user experience.
</p>

<div style="display:flex;gap:15px">
<a href="mailto:bichluan253@gmail.com" style="text-decoration:none;background:var(--primary);color:var(--theme);padding:12px 35px;border-radius:50px;font-weight:bold;border:2px solid var(--primary);transition:transform 0.2s">Liên hệ tôi</a>
<a href="https://github.com/llllluan2534" style="text-decoration:none;border:2px solid var(--primary);color:var(--primary);padding:12px 35px;border-radius:50px;font-weight:bold;transition:transform 0.2s">GitHub</a>
</div>
</div>
<div style="flex:1;min-width:300px;display:flex;justify-content:center">
<img src="
/assets/avt.jpg" alt="Avatar" style="width:350px;height:350px;border-radius:30% 70% 70% 30% / 30% 30% 70% 70%;object-fit:cover;box-shadow:20px 20px 0px var(--primary);border:2px solid var(--border)">
</div>
</div>

<div style="margin-bottom:80px">
<h2 style="text-align:center;font-size:2rem;margin-bottom:40px;color:var(--primary)">Projects</h2>
<div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(300px,1fr));gap:30px">
<div style="background:var(--entry);border-radius:16px;overflow:hidden;border:1px solid var(--border);transition:transform 0.3s ease">
<div style="height:180px;overflow:hidden">
<img src="/assets/project-cinema.jpg" alt="Giao diện hệ thống bán vé" style="width:100%;height:100%;object-fit:cover;transition:transform 0.5s ease">
</div>
<div style="padding:25px">
<h3 style="margin:0 0 10px 0;font-size:1.3rem">Hệ thống bán vé rạp phim</h3>
<p style="font-size:0.9rem;opacity:0.8;margin-bottom:20px;line-height:1.5">Ứng dụng quản lý lịch chiếu, hỗ trợ khách hàng đặt vé, chọn ghế realtime và thanh toán.</p>
<div style="border-top:1px solid var(--border);padding-top:15px;display:flex;justify-content:space-between">
<span style="font-weight:bold;color:var(--primary)">JavaScripts / SQL</span>
<a href="#" style="font-weight:bold;text-decoration:none">Xem Code ➔</a>
</div>
</div>
</div>
<div style="background:var(--entry);border-radius:16px;overflow:hidden;border:1px solid var(--border);transition:transform 0.3s ease">
<div style="height:180px;overflow:hidden">
<img src="/assets/docmentor.png" alt="Giao diện DocMentor AI" style="width:100%;height:100%;object-fit:cover;transition:transform 0.5s ease">
</div>
<div style="padding:25px">
<h3 style="margin:0 0 10px 0;font-size:1.3rem">DocMentor: AI Agent</h3>
<p style="font-size:0.9rem;opacity:0.8;margin-bottom:20px;line-height:1.5">Trợ lý AI giúp truy xuất, phân tích và tóm tắt kiến thức từ giáo trình, tài liệu học tập.</p>
<div style="border-top:1px solid var(--border);padding-top:15px;display:flex;justify-content:space-between">
<span style="font-weight:bold;color:var(--primary)">AI / RAG / TypeScript</span>
<a href="#" style="font-weight:bold;text-decoration:none">repo ➔</a>
</div>
</div>
</div>
<div style="background:var(--entry);border-radius:16px;overflow:hidden;border:1px solid var(--border);transition:transform 0.3s ease">
<div style="height:180px;overflow:hidden">
<img src="/images/project-blog.jpg" alt="Giao diện Blog cá nhân" style="width:100%;height:100%;object-fit:cover;transition:transform 0.5s ease">
</div>
<div style="padding:25px">
<h3 style="margin:0 0 10px 0;font-size:1.3rem">Blog Lập trình mạng</h3>
<p style="font-size:0.9rem;opacity:0.8;margin-bottom:20px">Website cá nhân Hugo, Markdown, GitHub Pages.</p>
<div style="border-top:1px solid var(--border);padding-top:15px;display:flex;justify-content:space-between">
<span style="font-weight:bold;color:var(--primary)">Hugo / HTML</span>
<a href="#" style="font-weight:bold;text-decoration:none">Xem Demo ➔</a>
</div>
</div>
</div>
</div>
</div>

<div style="margin-bottom:80px">
<h2 style="text-align:center;font-size:2rem;margin-bottom:40px;color:var(--primary)">Kỹ Năng & Công Nghệ</h2>
<div style="display:flex;flex-wrap:wrap;justify-content:center;gap:20px;max-width:1000px;margin:0 auto">
<div style="background:var(--entry);border:1px solid var(--border);padding:15px;border-radius:12px;width:110px;height:110px;display:flex;align-items:center;justify-content:center;transition:transform 0.2s">
<img src="/assets/Java-Logo.png" alt="Java" style="width:50px;height:50px;object-fit:contain">
</div>
<div style="background:var(--entry);border:1px solid var(--border);padding:15px;border-radius:12px;width:110px;height:110px;display:flex;align-items:center;justify-content:center;transition:transform 0.2s">
<img src="/assets/spring-boot.png" alt="Spring" style="width:50px;height:50px;object-fit:contain">
</div>
<div style="background:var(--entry);border:1px solid var(--border);padding:15px;border-radius:12px;width:110px;height:110px;display:flex;align-items:center;justify-content:center;transition:transform 0.2s">
<img src="/assets/mysql-logo.png" alt="SQL" style="width:50px;height:50px;object-fit:contain">
</div>
<div style="background:var(--entry);border:1px solid var(--border);padding:15px;border-radius:12px;width:110px;height:110px;display:flex;align-items:center;justify-content:center;transition:transform 0.2s">
<img src="/assets/java.png" alt="JS" style="width:50px;height:50px;object-fit:contain">
</div>
<div style="background:var(--entry);border:1px solid var(--border);padding:15px;border-radius:12px;width:110px;height:110px;display:flex;align-items:center;justify-content:center;transition:transform 0.2s">
<img src="/assets/Github-Logo.png" alt="GitHub" style="width:50px;height:50px;object-fit:contain">
</div>
<div style="background:var(--entry);border:1px solid var(--border);padding:15px;border-radius:12px;width:110px;height:110px;display:flex;align-items:center;justify-content:center;transition:transform 0.2s">
<img src="/assets/git-logo.png" alt="Git" style="width:50px;height:50px;object-fit:contain">
</div>
</div>
</div>

<div style="margin-bottom:80px">
<h2 style="text-align:center;font-size:2rem;margin-bottom:40px;color:var(--primary)">Học Vấn</h2>
<div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(300px,1fr));gap:30px">
<div style="background:var(--entry);border-radius:12px;border:1px solid var(--border);padding:20px;display:flex;align-items:center;gap:20px">
<img src="/assets/logo.png" style="width:60px;height:60px;object-fit:contain;background:#fff;border-radius:8px;padding:5px">
<div>
<h3 style="margin:0 0 5px 0;font-size:1.1rem">Trường Công Nghệ Thành Phố Hồ Chí Minh</h3>
<p style="margin:0;font-size:0.9rem;opacity:0.8">Chuyên ngành: Công Nghệ Phần Mềm</p>
</div>
</div>
</div>
</div>

<div style="margin-bottom:80px">
<h2 style="text-align:center;font-size:2rem;margin-bottom:40px;color:var(--primary)">Cuộc Thi & Giải Thưởng</h2>
<div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(300px,1fr));gap:30px">
<div style="background:var(--entry);border-radius:16px;overflow:hidden;border:1px solid var(--border);transition:transform 0.3s ease">
<div style="height:250px;overflow:hidden">
<img src="/assets/pione.jpg" alt="Pione Dream" style="width:100%;height:100%;object-fit:cover;transition:transform 0.5s ease">
</div>
<div style="padding:25px">
<h3 style="margin:0 0 10px 0;font-size:1.3rem">Giải Ba - Pione Dream Hackathon</h3>
<p style="font-size:0.9rem;opacity:0.8;margin-bottom:20px;line-height:1.5">Cuộc thi AI & Blockchain 2025. Dự án: Pione Game Trace.</p>
<div style="border-top:1px solid var(--border);padding-top:15px;display:flex;justify-content:space-between">
<span style="font-weight:bold;color:var(--primary)">Cấp Trường</span>
</div>
</div>
</div>
<div style="background:var(--entry);border-radius:16px;overflow:hidden;border:1px solid var(--border);transition:transform 0.3s ease">
<div style="height:250px;overflow:hidden">
<img src="/assets/genz.jpg" alt="GenZ Thinking" style="width:100%;height:100%;object-fit:cover;transition:transform 0.5s ease">
</div>
<div style="padding:25px">
<h3 style="margin:0 0 10px 0;font-size:1.3rem">Giải Ba - GenZ Thinking</h3>
<p style="font-size:0.9rem;opacity:0.8;margin-bottom:20px;line-height:1.5">Cuộc thi ý tưởng khởi nghiệp và tư duy sáng tạo 2023.</p>
<div style="border-top:1px solid var(--border);padding-top:15px;display:flex;justify-content:space-between">
<span style="font-weight:bold;color:var(--primary)">Học Thuật</span>
</div>
</div>
</div>
<div style="background:var(--entry);border-radius:16px;overflow:hidden;border:1px solid var(--border);transition:transform 0.3s ease">
<div style="height:250px;overflow:hidden;display:flex;align-items:center;justify-content:center;background:#fff">
<img src="/assets/svtb.png" alt="Học bổng" style="width:100%;height:100%;object-fit:contain">
</div>
<div style="padding:25px">
<h3 style="margin:0 0 10px 0;font-size:1.3rem">Sinh viên Tiêu biểu & 5 Tốt</h3>
<p style="font-size:0.9rem;opacity:0.8;margin-bottom:10px;line-height:1.5"><strong>Sinh viên 5 tốt:</strong> Cấp khoa - 2024</p>
<p style="font-size:0.9rem;opacity:0.8;margin-bottom:20px;line-height:1.5"><strong>Sinh viên tiêu biểu:</strong> Cấp khoa - 2025</p>
<div style="border-top:1px solid var(--border);padding-top:15px;display:flex;justify-content:space-between">
<span style="font-weight:bold;color:var(--primary)">Thành Tích</span>
</div>
</div>
</div>
</div>
</div>

<div style="margin-bottom:80px">
<h2 style="text-align:center;font-size:2rem;margin-bottom:40px;color:var(--primary)">Chứng Chỉ Chuyên Môn</h2>
<div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(300px,1fr));gap:30px">
<div class="cert-card">
<img src="/assets/networking.png" style="width:80px;height:80px;object-fit:contain;margin-bottom:15px">
<h3 style="margin:0 0 10px 0;font-size:1.2rem">Networking Basics</h3>
<p style="font-size:0.9rem;opacity:0.8;margin-bottom:20px;line-height:1.5">Kiến thức nền tảng về mạng máy tính, giao thức và cách vận hành hệ thống mạng.</p>
<p style="font-size:0.85rem;color:var(--primary);margin-bottom:15px;font-weight:bold">Cisco | 2025</p>
<button class="btn-view-cert" onclick="showModal('//LTM_BL.github.io/assets/networking1.png')">Xem Chứng Chỉ</button>
</div>
<div class="cert-card">
<img src="/assets/jse1.png" style="width:80px;height:80px;object-fit:contain;margin-bottom:15px">
<h3 style="margin:0 0 10px 0;font-size:1.2rem">JavaScript Essentials 1</h3>
<p style="font-size:0.9rem;opacity:0.8;margin-bottom:20px;line-height:1.5">Thiết kế, phát triển và tối ưu hóa chương trình JavaScript căn bản.</p>
<p style="font-size:0.85rem;color:var(--primary);margin-bottom:15px;font-weight:bold">Cisco / JS Institute | 2025</p>
<button class="btn-view-cert" onclick="showModal('//LTM_BL.github.io/assets/jse1.jpg')">Xem Chứng Chỉ</button>
</div>
<div class="cert-card">
<img src="/assets/jse2.png" style="width:80px;height:80px;object-fit:contain;margin-bottom:15px">
<h3 style="margin:0 0 10px 0;font-size:1.2rem">JavaScript Essentials 2</h3>
<p style="font-size:0.9rem;opacity:0.8;margin-bottom:20px;line-height:1.5">Nắm vững JS nâng cao, chuẩn bị cho kỳ thi chứng chỉ JSA Certified Associate.</p>
<p style="font-size:0.85rem;color:var(--primary);margin-bottom:15px;font-weight:bold">Cisco / JS Institute | 2025</p>
<button class="btn-view-cert" onclick="showModal('//LTM_BL.github.io/assets/jse2.jpg')">Xem Chứng Chỉ</button>
</div>
</div>
</div>

<div style="background:var(--entry);padding:40px;text-align:center;border-top:4px solid var(--primary)">
<h2 style="margin:0 0 20px 0">Liên hệ</h2>
<div style="display:flex;justify-content:center;gap:30px;flex-wrap:wrap">
<a href="mailto:email@example.com" style="text-decoration:none;font-weight:bold;display:flex;align-items:center;gap:10px">📧 bichluan253@example.com</a>
<a href="https://github.com/llllluan2534" style="text-decoration:none;font-weight:bold;display:flex;align-items:center;gap:10px">🐈 GitHub</a>
</div>
</div>
</div>

<div id="certModal">
<span class="close-modal" onclick="closeModal()">&times;</span>
<img id="modalImg" src="">
</div>
<script>
var modal = document.getElementById("certModal");
var modalImg = document.getElementById("modalImg");
function showModal(src){modal.style.display="flex";modalImg.src=src;}
function closeModal(){modal.style.display="none";}
window.onclick=function(event){if(event.target==modal){modal.style.display="none";}}
</script>
