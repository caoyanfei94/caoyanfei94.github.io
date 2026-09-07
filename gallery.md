---
layout: article
titles: Gallery
# aside:
#   toc: true
---

<!-- <br> -->

## Gallery

<style>
  /* 筛选按钮组 */
  .filter-container {
    display: flex;
    justify-content: center;
    gap: 10px;
    margin: 20px 0 30px 0;
    flex-wrap: wrap;
  }
  .filter-btn {
    background: #f1f5f9;
    border: 1px solid #cbd5e1;
    padding: 6px 16px;
    border-radius: 20px;
    cursor: pointer;
    font-size: 0.9rem;
    color: #475569;
    transition: all 0.3s ease;
  }
  .filter-btn:hover, .filter-btn.active {
    background: #0f172a;
    color: #ffffff;
    border-color: #0f172a;
  }

  /* 酷炫瀑布流网格 */
  .gallery-grid {
    column-count: 3;
    column-gap: 16px;
  }
  @media (max-width: 800px) { .gallery-grid { column-count: 2; } }
  @media (max-width: 500px) { .gallery-grid { column-count: 1; } }

  .gallery-card {
    break-inside: avoid;
    margin-bottom: 16px;
    position: relative;
    border-radius: 10px;
    overflow: hidden;
    background: #000;
    cursor: pointer;
    box-shadow: 0 4px 12px rgba(0,0,0,0.08);
  }
  .gallery-card img {
    width: 100%;
    display: block;
    border-radius: 10px;
    transition: transform 0.5s ease, opacity 0.5s ease;
  }
  .gallery-card:hover img {
    transform: scale(1.06);
    opacity: 0.85;
  }

  /* 悬停文字浮层 */
  .gallery-overlay {
    position: absolute;
    inset: 0;
    background: linear-gradient(to top, rgba(0,0,0,0.85) 0%, rgba(0,0,0,0) 60%);
    opacity: 0;
    transition: opacity 0.3s ease;
    display: flex;
    flex-direction: column;
    justify-content: flex-end;
    padding: 15px;
    color: #fff;
  }
  .gallery-card:hover .gallery-overlay { opacity: 1; }
  .gallery-overlay h4 { margin: 0 0 4px 0; font-size: 1rem; color: #fff; font-weight: 600; }
  .gallery-overlay p { margin: 0; font-size: 0.8rem; color: #cbd5e1; }

  /* Lightbox 弹窗大图预览 */
  .lightbox-modal {
    display: none;
    position: fixed;
    inset: 0;
    background: rgba(0, 0, 0, 0.9);
    z-index: 9999;
    justify-content: center;
    align-items: center;
    backdrop-filter: blur(5px);
  }
  .lightbox-modal.active { display: flex; }
  .lightbox-modal img { max-width: 90vw; max-height: 85vh; border-radius: 8px; }
  .lightbox-close {
    position: absolute;
    top: 20px;
    right: 30px;
    color: #fff;
    font-size: 30px;
    cursor: pointer;
  }
</style>

<!-- 分类筛选器 -->
<div class="filter-container">
  <button class="filter-btn active" onclick="filterGallery('all')">All</button>
  <button class="filter-btn" onclick="filterGallery('acad')">Academic Milestones</button>
  <button class="filter-btn" onclick="filterGallery('soci')">Social & Networking</button>
  <button class="filter-btn" onclick="filterGallery('life')">Life & Outdoors</button>
</div>

<!-- 相册瀑布流展示区 -->
<div class="gallery-grid">

  <div class="gallery-card" data-category="acad" onclick="openLightbox(this)">
    <img src="/assets/gallery/acad/12.JPG" alt="Master's graduation">
    <div class="gallery-overlay">
      <h4>Master's graduation</h4>
      <p>Nanjing</p>
    </div>
  </div>

  <div class="gallery-card" data-category="soci" onclick="openLightbox(this)">
    <img src="/assets/gallery/soci/2.jpg" alt="Amos">
    <div class="gallery-overlay">
      <h4>Showing Amos, Editor-in-Chief of Science Robotics, around DJI</h4>
      <p>Shenzhen</p>
    </div>
  </div>

  <div class="gallery-card" data-category="life" onclick="openLightbox(this)">
    <img src="/assets/gallery/life/8.jpg" alt="Lab Setup">
    <div class="gallery-overlay">
      <h4>Daya Bay Tour</h4>
      <p>Hong Kong</p>
    </div>
  </div>

</div>

<!-- 大图弹出框 -->
<div class="lightbox-modal" id="lightbox" onclick="closeLightbox(event)">
  <span class="lightbox-close">&times;</span>
  <img id="lightbox-img" src="" alt="">
</div>

<script>
  function filterGallery(category) {
    document.querySelectorAll('.filter-btn').forEach(btn => btn.classList.remove('active'));
    event.target.classList.add('active');

    document.querySelectorAll('.gallery-card').forEach(item => {
      if (category === 'all' || item.dataset.category === category) {
        item.style.display = 'block';
      } else {
        item.style.display = 'none';
      }
    });
  }

  function openLightbox(element) {
    const img = element.querySelector('img');
    document.getElementById('lightbox-img').src = img.src;
    document.getElementById('lightbox').classList.add('active');
  }

  function closeLightbox(event) {
    document.getElementById('lightbox').classList.remove('active');
  }
</script>

<hr class="hr-edge-weak">