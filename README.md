<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>英语户外研学营会</title>
    <script src="https://res.wx.qq.com/open/js/jweixin-1.6.0.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: "PingFang SC", "Helvetica Neue", Arial, sans-serif;
        }
        
        body {
            overflow-x: hidden;
            color: #333;
        }
        
        /* 加载动画 */
        .loading {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: white;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 9999;
        }
        
        .loader {
            width: 50px;
            height: 50px;
            border: 5px solid #f3f3f3;
            border-top: 5px solid #4caf50;
            border-radius: 50%;
            animation: spin 1s linear infinite;
            margin-bottom: 20px;
        }
        
        .loading-text {
            font-size: 1.2rem;
            color: #4caf50;
        }
        
        .container {
            width: 100vw;
            height: 100vh;
            overflow: hidden;
            position: relative;
            display: none;
        }
        
        .page {
            width: 100%;
            height: 100%;
            position: absolute;
            transition: transform 0.8s ease;
            padding: 20px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            text-align: center;
        }
        
        /* 第1屏样式 */
        #page1 {
            background: linear-gradient(rgba(0,0,0,0.3), rgba(0,0,0,0.3)), 
                        url('https://images.unsplash.com/photo-1523050854058-8df90110c9f1?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80') no-repeat center/cover;
            color: white;
        }
        
        #page1 h1 {
            font-size: 2.2rem;
            margin-bottom: 1rem;
            opacity: 0;
            transform: translateY(30px);
            animation: fadeInUp 1s forwards 0.5s;
        }
        
        #page1 h2 {
            font-size: 1.5rem;
            margin-bottom: 2rem;
            opacity: 0;
            transform: translateY(30px);
            animation: fadeInUp 1s forwards 0.8s;
        }
        
        #page1 p {
            opacity: 0;
            transform: translateY(30px);
            animation: fadeInUp 1s forwards 1.1s;
        }
        
        .arrow-down {
            position: absolute;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            font-size: 2rem;
            animation: bounce 2s infinite;
            color: white;
        }
        
        /* 第2屏样式 */
        #page2 {
            background-color: #f9f9f9;
        }
        
        .split-container {
            display: flex;
            width: 100%;
            height: 60%;
            margin-top: 20px;
        }
        
        .split-half {
            flex: 1;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 10px;
            position: relative;
            overflow: hidden;
        }
        
        .split-half img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            border-radius: 10px;
        }
        
        .split-half.left {
            margin-right: 5px;
        }
        
        .split-half.right {
            margin-left: 5px;
        }
        
        .bw-filter {
            filter: grayscale(100%);
        }
        
        /* 第3屏样式 */
        #page3 {
            background-color: #e8f5e9;
        }
        
        .features {
            display: flex;
            justify-content: space-around;
            width: 100%;
            margin-top: 30px;
            flex-wrap: wrap;
        }
        
        .feature {
            flex: 1;
            margin: 0 10px 20px;
            min-width: 120px;
            opacity: 0;
            transform: scale(0.8);
        }
        
        .feature-icon {
            width: 80px;
            height: 80px;
            background-color: #4caf50;
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            margin: 0 auto 15px;
            font-size: 2rem;
            color: white;
        }
        
        /* 第4屏样式 */
        #page4 {
            background-color: #fff;
        }
        
        .gallery {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            margin-top: 20px;
        }
        
        .gallery-item {
            width: 45%;
            margin: 5px;
            border-radius: 8px;
            overflow: hidden;
            box-shadow: 0 3px 6px rgba(0,0,0,0.1);
            position: relative;
        }
        
        .gallery-item img {
            width: 100%;
            height: 120px;
            object-fit: cover;
        }
        
        .testimonial {
            margin-top: 20px;
            padding: 15px;
            background-color: #f5f5f5;
            border-radius: 10px;
            font-style: italic;
            opacity: 0;
        }
        
        /* 第5屏样式 */
        #page5 {
            background: linear-gradient(rgba(0,0,0,0.3), rgba(0,0,0,0.3)), 
                        url('https://images.unsplash.com/photo-1588072432836-e10032774350?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80') no-repeat center/cover;
            color: white;
        }
        
        .form-container {
            background-color: rgba(255,255,255,0.9);
            padding: 20px;
            border-radius: 10px;
            width: 90%;
            max-width: 400px;
            color: #333;
        }
        
        .form-group {
            margin-bottom: 15px;
            text-align: left;
        }
        
        .form-group label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }
        
        .form-group input, .form-group select {
            width: 100%;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 5px;
        }
        
        .submit-btn {
            background-color: #4caf50;
            color: white;
            border: none;
            padding: 12px 20px;
            border-radius: 5px;
            font-size: 1rem;
            cursor: pointer;
            width: 100%;
            margin-top: 10px;
            transition: background-color 0.3s;
        }
        
        .submit-btn:hover {
            background-color: #3e8e41;
        }
        
        .payment-options {
            margin-top: 15px;
            text-align: center;
        }
        
        .payment-btn {
            display: inline-block;
            margin: 5px;
            padding: 8px 15px;
            background-color: #2196F3;
            color: white;
            border-radius: 5px;
            text-decoration: none;
            font-size: 0.9rem;
        }
        
        .share-container {
            position: fixed;
            top: 10px;
            right: 10px;
            z-index: 100;
        }
        
        .share-btn {
            width: 40px;
            height: 40px;
            background-color: rgba(0,0,0,0.5);
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            color: white;
            font-size: 1.2rem;
        }
        
        /* 动画效果 */
        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translateY(30px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
        
        @keyframes bounce {
            0%, 20%, 50%, 80%, 100% {
                transform: translateY(0) translateX(-50%);
            }
            40% {
                transform: translateY(-20px) translateX(-50%);
            }
            60% {
                transform: translateY(-10px) translateX(-50%);
            }
        }
        
        @keyframes scaleIn {
            from {
                transform: scale(0.8);
                opacity: 0;
            }
            to {
                transform: scale(1);
                opacity: 1;
            }
        }
        
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        
        /* 响应式调整 */
        @media (max-width: 480px) {
            #page1 h1 {
                font-size: 1.8rem;
            }
            
            #page1 h2 {
                font-size: 1.2rem;
            }
            
            .feature {
                min-width: 100%;
                margin-bottom: 15px;
            }
        }
    </style>
</head>
<body>
    <!-- 加载动画 -->
    <div class="loading" id="loading">
        <div class="loader"></div>
        <div class="loading-text">加载中...</div>
    </div>
    
    <!-- 分享按钮 -->
    <div class="share-container">
        <div class="share-btn" id="shareBtn">↗</div>
    </div>
    
    <!-- 主容器 -->
    <div class="container" id="container">
        <!-- 第1屏 -->
        <div class="page" id="page1">
            <h1>让英语跳出课本，走进自然！</h1>
            <h2>英语户外研学营会 · 春季招募中</h2>
            <p>每周末一场 · 限15组家庭</p>
            <div class="arrow-down">↓</div>
        </div>
        
        <!-- 第2屏 -->
        <div class="page" id="page2">
            <h2>还在为孩子的英语学习发愁吗？</h2>
            <p>死记硬背 × 不敢开口 × 缺乏兴趣</p>
            <p>我们需要全新的学习方式！</p>
            
            <div class="split-container">
                <div class="split-half left">
                    <img src="https://images.unsplash.com/photo-1503676260728-1c00da094a0b?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80" class="bw-filter" alt="传统课堂">
                </div>
                <div class="split-half right">
                    <img src="https://images.unsplash.com/photo-1523050854058-8df90110c9f1?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80" alt="户外学习">
                </div>
            </div>
        </div>
        
        <!-- 第3屏 -->
        <div class="page" id="page3">
            <h2>我们的特色学习方法</h2>
            
            <div class="features">
                <div class="feature" id="feature1">
                    <div class="feature-icon">🗣️</div>
                    <h3>沉浸式英语环境</h3>
                    <p>全英文任务挑战，自然开口</p>
                </div>
                
                <div class="feature" id="feature2">
                    <div class="feature-icon">🌳</div>
                    <h3>户外场景教学</h3>
                    <p>公园、博物馆、市集...真实场景应用</p>
                </div>
                
                <div class="feature" id="feature3">
                    <div class="feature-icon">🎯</div>
                    <h3>主题式学习</h3>
                    <p>每周不同主题，保持新鲜感</p>
                </div>
                
                <div class="feature" id="feature4">
                    <div class="feature-icon">👨‍🏫</div>
                    <h3>专业外教团队</h3>
                    <p>持证外教+中教辅助，确保理解</p>
                </div>
                
                <div class="feature" id="feature5">
                    <div class="feature-icon">📷</div>
                    <h3>活动全程跟拍</h3>
                    <p>记录孩子成长瞬间</p>
                </div>
            </div>
        </div>
        
        <!-- 第4屏 -->
        <div class="page" id="page4">
            <h2>往期活动精彩瞬间</h2>
            
            <div class="gallery">
                <div class="gallery-item">
                    <img src="https://images.unsplash.com/photo-1549056572-75914d5d5fd4?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80" alt="活动照片1">
                </div>
                <div class="gallery-item">
                    <img src="https://images.unsplash.com/photo-1541178735493-479c1a27ed24?ixlib=rb-1.2.1&auto=format&fit=crop&w=1351&q=80" alt="活动照片2">
                </div>
                <div class="gallery-item">
                    <img src="https://images.unsplash.com/photo-1542623027-a0f677c5e37f?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80" alt="活动照片3">
                </div>
                <div class="gallery-item">
                    <img src="https://images.unsplash.com/photo-1541692641319-981cc79ee10a?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80" alt="活动照片4">
                </div>
            </div>
            
            <div class="testimonial" id="testimonial1">
                "孩子回家主动说英语了！ - 李妈妈"
            </div>
            <div class="testimonial" id="testimonial2">
                "第一次看到孩子这么自信和外教交流 - 张爸爸"
            </div>
        </div>
        
        <!-- 第5屏 -->
        <div class="page" id="page5">
            <div class="form-container">
                <h2 style="color: #4caf50;">立即报名春季研学营</h2>
                <p style="margin-bottom: 20px; color: #666;">前10名报名赠户外英语手册！</p>
                
                <div class="form-group">
                    <label for="name">孩子姓名</label>
                    <input type="text" id="name" placeholder="请输入孩子姓名">
                </div>
                
                <div class="form-group">
                    <label for="age">孩子年龄</label>
                    <input type="text" id="age" placeholder="请输入孩子年龄">
                </div>
                
                <div class="form-group">
                    <label for="phone">家长电话</label>
                    <input type="tel" id="phone" placeholder="请输入联系电话">
                </div>
                
                <div class="form-group">
                    <label for="session">选择场次</label>
                    <select id="session">
                        <option value="">请选择场次</option>
                        <option value="weekend1">4月15日 森林公园</option>
                        <option value="weekend2">4月22日 自然博物馆</option>
                        <option value="weekend3">4月29日 农贸市场</option>
                    </select>
                </div>
                
                <button class="submit-btn" id="submitBtn">立即报名</button>
                
                <div class="payment-options">
                    <p>或直接支付预定名额</p>
                    <a href="#" class="payment-btn" id="wechatPay">微信支付</a>
                    <a href="#" class="payment-btn" id="aliPay">支付宝</a>
                </div>
            </div>
        </div>
    </div>

    <script>
        // 显示主内容，隐藏加载动画
        window.addEventListener('load', function() {
            setTimeout(function() {
                document.getElementById('loading').style.display = 'none';
                document.getElementById('container').style.display = 'block';
                
                // 初始化微信分享
                initWechatShare();
            }, 1500);
        });
        
        // 当前页索引
        let currentPage = 0;
        const pages = document.querySelectorAll('.page');
        const totalPages = pages.length;
        
        // 初始化页面位置
        function initPages() {
            pages.forEach((page, index) => {
                page.style.transform = `translateY(${index * 100}vh)`;
            });
        }
        
        // 滚动到指定页
        function scrollToPage(index) {
            if (index < 0 || index >= totalPages) return;
            
            currentPage = index;
            pages.forEach((page, i) => {
                page.style.transform = `translateY(${(i - currentPage) * 100}vh)`;
            });
            
            // 触发各页动画
            triggerAnimations();
        }
        
        // 触发各页动画
        function triggerAnimations() {
            // 第3屏特性动画
            if (currentPage === 2) {
                setTimeout(() => {
                    document.getElementById('feature1').style.animation = 'scaleIn 0.6s forwards';
                }, 200);
                
                setTimeout(() => {
                    document.getElementById('feature2').style.animation = 'scaleIn 0.6s forwards';
                }, 400);
                
                setTimeout(() => {
                    document.getElementById('feature3').style.animation = 'scaleIn 0.6s forwards';
                }, 600);
                
                setTimeout(() => {
                    document.getElementById('feature4').style.animation = 'scaleIn 0.6s forwards';
                }, 800);
                
                setTimeout(() => {
                    document.getElementById('feature5').style.animation = 'scaleIn 0.6s forwards';
                }, 1000);
            }
            
            // 第4屏评价动画
            if (currentPage === 3) {
                setTimeout(() => {
                    document.getElementById('testimonial1').style.animation = 'fadeInUp 1s forwards';
                }, 500);
                
                setTimeout(() => {
                    document.getElementById('testimonial2').style.animation = 'fadeInUp 1s forwards';
                }, 1000);
            }
        }
        
        // 触摸事件处理
        let startY = 0;
        let isScrolling = false;
        
        document.addEventListener('touchstart', (e) => {
            startY = e.touches[0].clientY;
            isScrolling = false;
        }, { passive: true });
        
        document.addEventListener('touchmove', (e) => {
            if (isScrolling) return;
            
            const y = e.touches[0].clientY;
            const dy = y - startY;
            
            // 垂直滑动超过30px才认为是滚动
            if (Math.abs(dy) > 30) {
                isScrolling = true;
                
                if (dy > 0 && currentPage > 0) {
                    // 向下滑动，上一页
                    scrollToPage(currentPage - 1);
                } else if (dy < 0 && currentPage < totalPages - 1) {
                    // 向上滑动，下一页
                    scrollToPage(currentPage + 1);
                }
            }
        }, { passive: true });
        
        // 鼠标滚轮事件
        document.addEventListener('wheel', (e) => {
            if (e.deltaY > 0 && currentPage < totalPages - 1) {
                // 向下滚动，下一页
                scrollToPage(currentPage + 1);
            } else if (e.deltaY < 0 && currentPage > 0) {
                // 向上滚动，上一页
                scrollToPage(currentPage - 1);
            }
        }, { passive: true });
        
        // 初始化
        initPages();
        triggerAnimations();
        
        // 表单提交
        document.getElementById('submitBtn').addEventListener('click', function() {
            const name = document.getElementById('name').value;
            const age = document.getElementById('age').value;
            const phone = document.getElementById('phone').value;
            const session = document.getElementById('session').value;
            
            if (!name || !age || !phone || !session) {
                alert('请填写完整信息！');
                return;
            }
            
            // 显示加载状态
            this.textContent = '提交中...';
            this.disabled = true;
            
            // 模拟AJAX提交
            setTimeout(() => {
                // 这里应该是实际的AJAX请求
                // fetch('your-backend-url', {
                //     method: 'POST',
                //     body: JSON.stringify({name, age, phone, session}),
                //     headers: {
                //         'Content-Type': 'application/json'
                //     }
                // })
                // .then(response => response.json())
                // .then(data => {
                //     alert('报名成功！');
                // })
                // .catch(error => {
                //     alert('提交失败，请重试');
                // });
                
                // 模拟成功响应
                alert('报名信息已提交！我们会尽快与您联系确认详情。');
                this.textContent = '立即报名';
                this.disabled = false;
            }, 1000);
        });
        
        // 微信支付按钮
        document.getElementById('wechatPay').addEventListener('click', function(e) {
            e.preventDefault();
            alert('即将跳转微信支付...');
            // 实际应用中这里应该调用微信JS-SDK的支付接口
            // WeixinJSBridge.invoke('getBrandWCPayRequest', {
            //     // 支付参数
            // }, function(res) {
            //     if(res.err_msg == "get_brand_wcpay_request:ok" ) {
            //         // 支付成功
            //     }
            // });
        });
        
        // 支付宝支付按钮
        document.getElementById('aliPay').addEventListener('click', function(e) {
            e.preventDefault();
            alert('即将跳转支付宝支付...');
            // 实际应用中这里应该调用支付宝的支付接口
        });
        
        // 分享按钮
        document.getElementById('shareBtn').addEventListener('click', function() {
            // 在微信环境中会触发JS-SDK的分享接口
            // 在其他环境中显示提示
            if (typeof WeixinJSBridge === 'undefined') {
                alert('请点击浏览器菜单选择分享');
            }
        });
        
        // 初始化微信分享
        function initWechatShare() {
            // 这里需要你的后端提供微信JS-SDK的配置
            // 实际应用中应该通过AJAX获取签名等参数
            /*
            fetch('your-wechat-config-api')
                .then(response => response.json())
                .then(config => {
                    wx.config({
                        debug: false,
                        appId: config.appId,
                        timestamp: config.timestamp,
                        nonceStr: config.nonceStr,
                        signature: config.signature,
                        jsApiList: [
                            'onMenuShareTimeline',
                            'onMenuShareAppMessage',
                            'onMenuShareQQ',
                            'onMenuShareWeibo',
                            'onMenuShareQZone'
                        ]
                    });
                    
                    wx.ready(function() {
                        // 分享到朋友圈
                        wx.onMenuShareTimeline({
                            title: '英语户外研学营会 - 让孩子在自然中学英语',
                            link: window.location.href,
                            imgUrl: 'https://your-domain.com/share-image.jpg'
                        });
                        
                        // 分享给朋友
                        wx.onMenuShareAppMessage({
                            title: '发现一个超棒的英语学习活动',
                            desc: '英语户外研学营会，让孩子在真实场景中运用英语',
                            link: window.location.href,
                            imgUrl: 'https://your-domain.com/share-image.jpg',
                            type: 'link'
                        });
                    });
                });
            */
            
            // 模拟配置
            console.log('微信分享功能已初始化');
        }
    </script>
</body>
</html>
