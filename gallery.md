---
layout: article
titles: Gallery
# aside:
#   toc: true
---

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

  /* 瀑布流外层容器 */
  .gallery-grid {
    display: flex !important;
    flex-direction: row !important;
    gap: 16px;
    align-items: flex-start;
    width: 100%;
    box-sizing: border-box;
    min-height: 300px; /* 预留高度防止页面抖动 */
  }

  /* 核心修复：JS 运行前，直接存在于外层容器下的原始卡片先隐藏，绝不挤成一排 */
  .gallery-grid > .gallery-card {
    display: none !important;
  }

  /* 当卡片被 JS 顺利分发到列容器 .gallery-col 内部后，立即正常显示 */
  .gallery-col .gallery-card {
    display: block !important;
    width: 100%;
    position: relative;
    border-radius: 10px;
    overflow: hidden;
    background: #000;
    cursor: pointer;
    box-shadow: 0 4px 12px rgba(0,0,0,0.08);
  }
  
  /* 瀑布流列容器：三列等宽 */
  .gallery-col {
    flex: 1 1 0% !important;
    min-width: 0 !important;
    display: flex;
    flex-direction: column;
    gap: 16px;
  }
  
  /* 确保卡片和图片不超过列宽 */
  .gallery-card {
    width: 100%;
    position: relative;
    border-radius: 10px;
    overflow: hidden;
    background: #000;
    cursor: pointer;
    box-shadow: 0 4px 12px rgba(0,0,0,0.08);
  }
  
  .gallery-card img {
    width: 100%;
    height: auto;
    display: block;
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

  /* 加载提示容器 */
  .gallery-loader {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 60px 0;
    color: #64748b;
    font-size: 0.95rem;
    width: 100%;
  }

  /* 旋转 Spinner 圈圈 */
  .spinner {
    width: 32px;
    height: 32px;
    border: 3px solid #e2e8f0;
    border-top: 3px solid #0f172a;
    border-radius: 50%;
    animation: spin 0.8s linear infinite;
    margin-bottom: 12px;
  }

  @keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
  }
</style>

<!-- 分类筛选器 -->
<div class="filter-container">
  <button class="filter-btn active" onclick="filterGallery('all', event)">All</button>
  <button class="filter-btn" onclick="filterGallery('acad', event)">Academic Milestones</button>
  <button class="filter-btn" onclick="filterGallery('soci', event)">Social & Networking</button>
  <button class="filter-btn" onclick="filterGallery('life', event)">Life & Outdoors</button>
</div>

<!-- 加载中提示组件 -->
<div id="gallery-loader" class="gallery-loader">
  <div class="spinner"></div>
  <p>Gathering memories📷✨... Please wait☕</p>
</div>

<!-- 相册瀑布流展示区 -->
<div class="gallery-grid">

  <div class="gallery-card" data-category="acad" onclick="openLightbox(this)">
    <img src="/assets/gallery/acad/14.jpg" alt="At My PhD Defense" loading="lazy" decoding="async">
    <div class="gallery-overlay">
      <h4>At My PhD Defense</h4>
      <p>@Hong Kong, China</p>
    </div>
  </div>

  <div class="gallery-card" data-category="acad" onclick="openLightbox(this)">
    <img src="/assets/gallery/acad/13.jpg" alt="PhD Lab Mates" loading="lazy" decoding="async">
    <div class="gallery-overlay">
      <h4>PhD Lab Mates</h4>
      <p>@Hong Kong, China</p>
    </div>
  </div>

  <div class="gallery-card" data-category="acad" data-priority="1" onclick="openLightbox(this)">
    <img src="/assets/gallery/acad/12.JPG" alt="Master's graduation" loading="lazy" decoding="async">
    <div class="gallery-overlay">
      <h4>Master's Graduation</h4>
      <p>@Nanjing, China</p>
    </div>
  </div>

  <!-- 修正了 data-priority="2" 的位置 -->
  <div class="gallery-card" data-category="acad" data-priority="2" onclick="openLightbox(this)">
    <img src="/assets/gallery/acad/11.JPG" alt="Master's graduation" loading="lazy" decoding="async">
    <div class="gallery-overlay">
      <h4>Master's Graduation</h4>
      <p>@Nanjing, China</p>
    </div>
  </div>

  <div class="gallery-card" data-category="acad" data-priority="3" onclick="openLightbox(this)">
    <img src="/assets/gallery/acad/10.JPG" alt="Master's graduation" loading="lazy" decoding="async">
    <div class="gallery-overlay">
      <h4>Master's Graduation</h4>
      <p>@Nanjing, China</p>
    </div>
  </div>

  <div class="gallery-card" data-category="acad" onclick="openLightbox(this)">
    <img src="/assets/gallery/acad/9.JPG" alt="With Prof. Chen Bai" loading="lazy" decoding="async">
    <div class="gallery-overlay">
      <h4>With Prof. Chen Bai</h4>
      <p>@Nanjing, China</p>
    </div>
  </div>

  <div class="gallery-card" data-category="acad" onclick="openLightbox(this)">
    <img src="/assets/gallery/acad/8.JPG" alt="With Prof. Ju Feng" loading="lazy" decoding="async">
    <div class="gallery-overlay">
      <h4>With Prof. Ju Feng</h4>
      <p>@Nanjing, China</p>
    </div>
  </div>

  <div class="gallery-card" data-category="acad" onclick="openLightbox(this)">
    <img src="/assets/gallery/acad/7_2.JPG" alt="At My Master's Defense" loading="lazy" decoding="async">
    <div class="gallery-overlay">
      <h4>At My Master's Defense</h4>
      <p>@Nanjing, China</p>
    </div>
  </div>

  <div class="gallery-card" data-category="acad" onclick="openLightbox(this)">
    <img src="/assets/gallery/acad/7.JPG" alt="Master's Lab Mates" loading="lazy" decoding="async">
    <div class="gallery-overlay">
      <h4>Master's Lab Mates</h4>
      <p>@Nanjing, China</p>
    </div>
  </div>

  <div class="gallery-card" data-category="acad" onclick="openLightbox(this)">
    <img src="/assets/gallery/acad/6.JPG" alt="Master's Lab Mates" loading="lazy" decoding="async">
    <div class="gallery-overlay">
      <h4>Master's Lab Mates</h4>
      <p>@Nanjing, China</p>
    </div>
  </div>

  <div class="gallery-card" data-category="acad" data-priority="4" onclick="openLightbox(this)">
    <img src="/assets/gallery/acad/5.JPG" alt="Master's Lab Mates" loading="lazy" decoding="async">
    <div class="gallery-overlay">
      <h4>Master's Lab Mates</h4>
      <p>@Nanjing, China</p>
    </div>
  </div>

  <div class="gallery-card" data-category="acad" onclick="openLightbox(this)">
    <img src="/assets/gallery/acad/4.JPG" alt="Master's Lab Mates" loading="lazy" decoding="async">
    <div class="gallery-overlay">
      <h4>Master's Lab Mates</h4>
      <p>@Nanjing, China</p>
    </div>
  </div>

  <div class="gallery-card" data-category="acad" onclick="openLightbox(this)">
    <img src="/assets/gallery/acad/3.JPG" alt="Master's Lab Mates" loading="lazy" decoding="async">
    <div class="gallery-overlay">
      <h4>Master's Lab Mates</h4>
      <p>@Nanjing, China</p>
    </div>
  </div>

  <div class="gallery-card" data-category="acad" onclick="openLightbox(this)">
    <img src="/assets/gallery/acad/2.JPG" alt="Master's Lab Mates" loading="lazy" decoding="async">
    <div class="gallery-overlay">
      <h4>Master's Lab Mates</h4>
      <p>@Nanjing, China</p>
    </div>
  </div>

  <div class="gallery-card" data-category="acad" data-priority="5" onclick="openLightbox(this)">
    <img src="/assets/gallery/acad/1.JPG" alt="Master's Lab Mates" loading="lazy" decoding="async">
    <div class="gallery-overlay">
      <h4>Master's Lab Mates</h4>
      <p>@Nanjing, China</p>
    </div>
  </div>

  <div class="gallery-card" data-category="acad" onclick="openLightbox(this)">
    <img src="/assets/gallery/acad/0_3.jpg" alt="Undergrad Graduation" loading="lazy" decoding="async">
    <div class="gallery-overlay">
      <h4>Undergrad Graduation</h4>
      <p>@Nanjing, China</p>
    </div>
  </div>

  <div class="gallery-card" data-category="acad" onclick="openLightbox(this)">
    <img src="/assets/gallery/acad/0_2.jpg" alt="Undergrad Class" loading="lazy" decoding="async">
    <div class="gallery-overlay">
      <h4>Undergrad Class</h4>
      <p>@Nanjing, China</p>
    </div>
  </div>

  <div class="gallery-card" data-category="acad" onclick="openLightbox(this)">
    <img src="/assets/gallery/acad/0_1.JPG" alt="Undergrad National Scholarship Trophy" loading="lazy" decoding="async">
    <div class="gallery-overlay">
      <h4>Undergrad National Scholarship Trophy🏆</h4>
      <p>@Nanjing, China</p>
    </div>
  </div>

  <div class="gallery-card" data-category="soci" data-priority="3.1" onclick="openLightbox(this)">
    <img src="/assets/gallery/soci/2.jpg" alt="Showing Amos, Editor-in-Chief of Science Robotics, around DJI" loading="lazy" decoding="async">
    <div class="gallery-overlay">
      <h4>Showing Amos, Editor-in-Chief of Science Robotics, around DJI</h4>
      <p>@Shenzhen, China</p>
    </div>
  </div>

  <div class="gallery-card" data-category="soci" onclick="openLightbox(this)">
    <img src="/assets/gallery/soci/1.JPG" alt="Dinner with Prof. Hongsoo Choi's Group at DGIST" loading="lazy" decoding="async">
    <div class="gallery-overlay">
      <h4>Dinner with Prof. Hongsoo Choi's Group at DGIST</h4>
      <p>@Daegu, South Korea</p>
    </div>
  </div>

  <div class="gallery-card" data-category="life" data-priority="7" onclick="openLightbox(this)">
    <img src="/assets/gallery/life/10.jpg" alt="Green Egg Island Day Trip" loading="lazy" decoding="async">
    <div class="gallery-overlay">
      <h4>Green Egg Island Day Trip</h4>
      <p>@Hong Kong, China</p>
    </div>
  </div>

  <div class="gallery-card" data-category="life" onclick="openLightbox(this)">
    <img src="/assets/gallery/life/9.jpg" alt="Celebrating New Year's Eve with CUHK Colleagues" loading="lazy" decoding="async">
    <div class="gallery-overlay">
      <h4>Celebrating New Year's Eve with CUHK Colleagues</h4>
      <p>@Shenzhen, China</p>
    </div>
  </div>

  <div class="gallery-card" data-category="life" data-priority="6" onclick="openLightbox(this)">
    <img src="/assets/gallery/life/8.jpg" alt="Daya Bay Tour" loading="lazy" decoding="async">
    <div class="gallery-overlay">
      <h4>Daya Bay Tour</h4>
      <p>@Hong Kong, China</p>
    </div>
  </div>

  <div class="gallery-card" data-category="life" onclick="openLightbox(this)">
    <img src="/assets/gallery/life/7.JPG" alt="Dinner with CUHK Colleagues" loading="lazy" decoding="async">
    <div class="gallery-overlay">
      <h4>Dinner with CUHK Colleagues</h4>
      <p>@Hong Kong, China</p>
    </div>
  </div>

  <div class="gallery-card" data-category="life" data-priority="10" onclick="openLightbox(this)">
    <img src="/assets/gallery/life/6.JPG" alt="Just Dance 2019 Live Event" loading="lazy" decoding="async">
    <div class="gallery-overlay">
      <h4>Just Dance 2019 Live Event</h4>
      <p>@Shanghai, China</p>
    </div>
  </div>

  <div class="gallery-card" data-category="life" onclick="openLightbox(this)">
    <img src="/assets/gallery/life/5.JPG" alt="Master's Lab Dinner" loading="lazy" decoding="async">
    <div class="gallery-overlay">
      <h4>Master's Lab Dinner</h4>
      <p>@Nanjing, China</p>
    </div>
  </div>

  <div class="gallery-card" data-category="life" onclick="openLightbox(this)">
    <img src="/assets/gallery/life/4.JPG" alt="Encountering a Giant Totoro" loading="lazy" decoding="async">
    <div class="gallery-overlay">
      <h4>Encountering a Giant Totoro</h4>
      <p>@Seoul, South Korea</p>
    </div>
  </div>

  <div class="gallery-card" data-category="life" onclick="openLightbox(this)">
    <img src="/assets/gallery/life/3.JPG" alt="Waving Hi with Haechi" loading="lazy" decoding="async">
    <div class="gallery-overlay">
      <h4>Waving Hi with Haechi</h4>
      <p>@Seoul, South Korea</p>
    </div>
  </div>

  <div class="gallery-card" data-category="life" data-priority="8" onclick="openLightbox(this)">
    <img src="/assets/gallery/life/2.jpg" alt="Undergrad Graduation Farewell Dinner & House Party" loading="lazy" decoding="async">
    <div class="gallery-overlay">
      <h4>Undergrad Graduation Farewell Dinner & House Party</h4>
      <p>@Nanjing, China</p>
    </div>
  </div>

  <div class="gallery-card" data-category="life" data-priority="9" onclick="openLightbox(this)">
    <img src="/assets/gallery/life/1.JPG" alt="Undergrad Roommate Get-Together" loading="lazy" decoding="async">
    <div class="gallery-overlay">
      <h4>Undergrad Roommate Get-Together</h4>
      <p>@Nanjing, China</p>
    </div>
  </div>

</div>

<!-- 大图弹出框 -->
<div class="lightbox-modal" id="lightbox" onclick="closeLightbox(event)">
  <span class="lightbox-close">&times;</span>
  <img id="lightbox-img" src="" alt="">
</div>

<script>
  let currentCategory = 'all';
  let rawCards = [];
  
  // 立即尝试获取 DOM 并渲染，不必等 DOMContentLoaded
  function initGallery() {
    const container = document.querySelector('.gallery-grid');
    if (!container) return;
  
    // 1. 抓取所有卡片节点
    rawCards = Array.from(container.querySelectorAll('.gallery-card'));
    
    if (rawCards.length > 0) {
      // 2. 执行排版计算
      renderGallery();
      // 3. 标记渲染完成，渐变显示瀑布流
      container.classList.add('rendered');
    }
  }
  
  function renderGallery() {
    const container = document.querySelector('.gallery-grid');
    if (!container || rawCards.length === 0) return;
  
    let visibleCards = rawCards.filter(card => {
      return currentCategory === 'all' || card.dataset.category === currentCategory;
    });
  
    visibleCards.sort((a, b) => {
      const priorityA = parseFloat(a.dataset.priority) || 999;
      const priorityB = parseFloat(b.dataset.priority) || 999;
      return priorityA - priorityB;
    });
  
    const width = window.innerWidth;
    let colsCount = 3;
    if (width <= 500) colsCount = 1;
    else if (width <= 800) colsCount = 2;
  
    container.innerHTML = '';
    const cols = [];
    for (let i = 0; i < colsCount; i++) {
      const col = document.createElement('div');
      col.className = 'gallery-col';
      container.appendChild(col);
      cols.push(col);
    }
  
    visibleCards.forEach((card, index) => {
      cols[index % colsCount].appendChild(card);
    });
  }
  
  // 快速触发初始化
  if (document.readyState === 'loading') {
    document.addEventListener('DOMContentLoaded', initGallery);
  } else {
    initGallery();
  }
  
  window.addEventListener('resize', renderGallery);

  function filterGallery(category, e) {
    currentCategory = category;
    document.querySelectorAll('.filter-btn').forEach(btn => btn.classList.remove('active'));
    if (e && e.target) {
      e.target.classList.add('active');
    }
    renderGallery();
  }
  
  function renderGallery() {
    // 瀑布流开始渲染时，立即隐藏加载提示
    const loader = document.getElementById('gallery-loader');
    if (loader) {
      loader.style.display = 'none';
    }

    const container = document.querySelector('.gallery-grid');
    if (!container || rawCards.length === 0) return;
  
    // 1. 根据分类筛选
    let visibleCards = rawCards.filter(card => {
      return currentCategory === 'all' || card.dataset.category === currentCategory;
    });
  
    // 2. 按 priority 权重/小数进行排序 (支持 3.1 等小数)
    visibleCards.sort((a, b) => {
      const priorityA = parseFloat(a.dataset.priority) || 999;
      const priorityB = parseFloat(b.dataset.priority) || 999;
      return priorityA - priorityB;
    });
  
    // 3. 计算响应式列数
    const width = window.innerWidth;
    let colsCount = 3;
    if (width <= 500) colsCount = 1;
    else if (width <= 800) colsCount = 2;
  
    // 4. 清空外层容器并创建新列容器
    container.innerHTML = '';
    const cols = [];
    for (let i = 0; i < colsCount; i++) {
      const col = document.createElement('div');
      col.className = 'gallery-col';
      container.appendChild(col);
      cols.push(col);
    }
  
    // 5. 横向轮流把卡片分配到各列中 (1->左, 2->中, 3->右)
    visibleCards.forEach((card, index) => {
      cols[index % colsCount].appendChild(card);
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