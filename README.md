<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
    <title>AI演讲助手 - 智能时间管理</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        :root {
            --primary: #3B82F6;
            --primary-dark: #2563EB;
            --success: #10B981;
            --warning: #F59E0B;
            --danger: #EF4444;
            --text-primary: #1F2937;
            --text-secondary: #6B7280;
            --bg-primary: #FFFFFF;
            --bg-secondary: #F9FAFB;
            --border: #E5E7EB;
            --shadow: 0 1px 3px rgba(0,0,0,0.1);
            --shadow-lg: 0 10px 15px -3px rgba(0,0,0,0.1);
        }

        @media (prefers-color-scheme: dark) {
            :root {
                --text-primary: #F9FAFB;
                --text-secondary: #9CA3AF;
                --bg-primary: #111827;
                --bg-secondary: #1F2937;
                --border: #374151;
            }
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'PingFang SC', 'Inter', sans-serif;
            background: var(--bg-secondary);
            color: var(--text-primary);
            line-height: 1.6;
        }

        /* 主容器 */
        .container {
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            padding-bottom: 100px;
        }

        /* 头部 */
        .header {
            background: var(--bg-primary);
            border-radius: 20px;
            padding: 20px;
            margin-bottom: 20px;
            box-shadow: var(--shadow);
        }

        .date {
            font-size: 14px;
            color: var(--text-secondary);
            margin-bottom: 10px;
        }

        .title {
            font-size: 28px;
            font-weight: 700;
            margin-bottom: 15px;
        }

        /* 快速输入 */
        .quick-input {
            display: flex;
            gap: 10px;
            margin-top: 15px;
        }

        .quick-input input {
            flex: 1;
            padding: 12px;
            border: 1px solid var(--border);
            border-radius: 12px;
            background: var(--bg-secondary);
            color: var(--text-primary);
            font-size: 16px;
        }

        .quick-input button {
            padding: 12px 20px;
            background: var(--primary);
            color: white;
            border: none;
            border-radius: 12px;
            cursor: pointer;
            font-weight: 600;
        }

        /* 任务卡片 */
        .task-list {
            display: flex;
            flex-direction: column;
            gap: 12px;
        }

        .task-card {
            background: var(--bg-primary);
            border-radius: 16px;
            padding: 16px;
            box-shadow: var(--shadow);
            transition: all 0.3s;
            animation: slideIn 0.3s ease;
        }

        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .task-card.completed {
            opacity: 0.6;
        }

        .task-card.completed .task-title {
            text-decoration: line-through;
        }

        .task-header {
            display: flex;
            justify-content: space-between;
            align-items: start;
            margin-bottom: 12px;
        }

        .task-title {
            font-size: 18px;
            font-weight: 600;
            flex: 1;
        }

        .task-badge {
            padding: 4px 8px;
            border-radius: 8px;
            font-size: 12px;
            font-weight: 500;
        }

        .badge-speech {
            background: var(--primary);
            color: white;
        }

        .badge-normal {
            background: var(--bg-secondary);
            color: var(--text-secondary);
        }

        .task-time {
            font-size: 14px;
            color: var(--text-secondary);
            margin-bottom: 12px;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .task-actions {
            display: flex;
            gap: 10px;
            margin-top: 12px;
        }

        .btn {
            padding: 8px 12px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 14px;
            font-weight: 500;
            transition: all 0.2s;
        }

        .btn-primary {
            background: var(--primary);
            color: white;
        }

        .btn-success {
            background: var(--success);
            color: white;
        }

        .btn-danger {
            background: var(--danger);
            color: white;
        }

        .btn-secondary {
            background: var(--bg-secondary);
            color: var(--text-primary);
        }

        /* 浮动语音按钮 */
        .voice-fab {
            position: fixed;
            bottom: 30px;
            right: 30px;
            width: 70px;
            height: 70px;
            border-radius: 50%;
            background: var(--primary);
            color: white;
            border: none;
            cursor: pointer;
            box-shadow: var(--shadow-lg);
            font-size: 30px;
            transition: all 0.3s;
            z-index: 1000;
        }

        .voice-fab:hover {
            transform: scale(1.1);
        }

        .voice-fab.recording {
            background: var(--danger);
            animation: pulse 1.5s infinite;
        }

        @keyframes pulse {
            0%, 100% {
                transform: scale(1);
            }
            50% {
                transform: scale(1.1);
            }
        }

        /* 演讲模式 */
        .speech-mode {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: #000;
            color: #fff;
            z-index: 2000;
            overflow-y: auto;
            padding: 40px;
            display: none;
        }

        .speech-mode.active {
            display: block;
        }

        .speech-content {
            max-width: 800px;
            margin: 0 auto;
            font-size: 28px;
            line-height: 1.8;
            font-family: 'Georgia', 'Times New Roman', serif;
        }

        .speech-controls {
            position: fixed;
            bottom: 20px;
            right: 20px;
            display: flex;
            gap: 10px;
        }

        .speech-controls button {
            padding: 12px 20px;
            background: rgba(255,255,255,0.2);
            border: none;
            color: white;
            border-radius: 8px;
            cursor: pointer;
        }

        /* 模态框 */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            z-index: 1500;
            align-items: center;
            justify-content: center;
        }

        .modal.active {
            display: flex;
        }

        .modal-content {
            background: var(--bg-primary);
            border-radius: 20px;
            padding: 24px;
            max-width: 500px;
            width: 90%;
        }

        .modal-content input, .modal-content textarea {
            width: 100%;
            padding: 12px;
            margin: 10px 0;
            border: 1px solid var(--border);
            border-radius: 8px;
            background: var(--bg-secondary);
            color: var(--text-primary);
        }

        .toast {
            position: fixed;
            bottom: 100px;
            left: 50%;
            transform: translateX(-50%);
            background: var(--text-primary);
            color: var(--bg-primary);
            padding: 12px 24px;
            border-radius: 40px;
            z-index: 1100;
            animation: fadeInUp 0.3s;
        }

        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translateX(-50%) translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateX(-50%) translateY(0);
            }
        }

        @media (max-width: 640px) {
            .container {
                padding: 15px;
            }
            
            .speech-content {
                font-size: 20px;
                padding: 20px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <div class="date" id="currentDate"></div>
            <div class="title">🎯 我的任务</div>
            <div class="quick-input">
                <input type="text" id="quickTaskInput" placeholder="快速添加任务...">
                <button id="quickAddBtn">添加</button>
            </div>
        </div>

        <div class="task-list" id="taskList"></div>
    </div>

    <button class="voice-fab" id="voiceBtn">🎤</button>

    <!-- 演讲模式 -->
    <div class="speech-mode" id="speechMode">
        <div class="speech-content" id="speechContent"></div>
        <div class="speech-controls">
            <button id="closeSpeechBtn">✕ 退出</button>
        </div>
    </div>

    <!-- 讲稿生成模态框 -->
    <div class="modal" id="speechModal">
        <div class="modal-content">
            <h3>📝 生成演讲提纲</h3>
            <input type="text" id="speechTopic" placeholder="演讲主题" value="">
            <input type="text" id="speechAudience" placeholder="受众群体" value="内部同事">
            <input type="text" id="speechDuration" placeholder="时长（分钟）" value="10">
            <button id="generateSpeechBtn" class="btn btn-primary">生成提纲</button>
            <button id="closeModalBtn" class="btn btn-secondary" style="margin-top: 10px;">取消</button>
        </div>
    </div>

    <script>
        // ==================== 数据模型 ====================
        let tasks = [];
        let currentSpeechTaskId = null;

        // 加载本地数据
        function loadTasks() {
            const saved = localStorage.getItem('speech_tasks');
            if (saved) {
                tasks = JSON.parse(saved);
                renderTasks();
            }
            checkReminders();
        }

        // 保存数据
        function saveTasks() {
            localStorage.setItem('speech_tasks', JSON.stringify(tasks));
        }

        // 渲染任务列表
        function renderTasks() {
            const taskList = document.getElementById('taskList');
            const sortedTasks = [...tasks].sort((a, b) => {
                if (a.time && b.time) return new Date(a.time) - new Date(b.time);
                return 0;
            });

            if (sortedTasks.length === 0) {
                taskList.innerHTML = '<div style="text-align: center; padding: 40px; color: var(--text-secondary);">✨ 暂无任务，点击右下角麦克风创建</div>';
                return;
            }

            taskList.innerHTML = sortedTasks.map(task => `
                <div class="task-card ${task.status === 'completed' ? 'completed' : ''}" data-id="${task.id}">
                    <div class="task-header">
                        <div class="task-title">${escapeHtml(task.title)}</div>
                        <span class="task-badge ${task.type === 'speech' ? 'badge-speech' : 'badge-normal'}">
                            ${task.type === 'speech' ? '🎤 演讲' : '📋 任务'}
                        </span>
                    </div>
                    <div class="task-time">
                        <span>🕐 ${task.time ? formatTime(task.time) : '未设定时间'}</span>
                        ${task.speechContent ? '<span>📄 提纲已生成</span>' : ''}
                    </div>
                    <div class="task-actions">
                        ${task.status !== 'completed' ? `<button class="btn btn-success" onclick="completeTask('${task.id}')">✓ 完成</button>` : ''}
                        ${task.type === 'speech' && !task.speechContent ? `<button class="btn btn-primary" onclick="generateSpeechForTask('${task.id}')">📝 生成提纲</button>` : ''}
                        ${task.speechContent ? `<button class="btn btn-primary" onclick="startSpeech('${task.id}')">🎤 开始演讲</button>` : ''}
                        <button class="btn btn-danger" onclick="deleteTask('${task.id}')">🗑 删除</button>
                    </div>
                </div>
            `).join('');
        }

        // 创建新任务
        function createTask(title, time = null, type = 'normal') {
            const task = {
                id: Date.now().toString(),
                title: title,
                time: time,
                type: type,
                status: 'pending',
                speechContent: null,
                createdAt: new Date().toISOString()
            };
            
            tasks.push(task);
            saveTasks();
            renderTasks();
            showToast(`✅ 已创建任务：${title}`);
            
            // 如果是演讲任务，询问是否生成提纲
            if (type === 'speech') {
                setTimeout(() => {
                    if (confirm('🎤 检测到这是演讲任务，是否立即生成演讲提纲？')) {
                        generateSpeechForTask(task.id);
                    }
                }, 500);
            }
            
            return task;
        }

        // 完成任务
        function completeTask(id) {
            const task = tasks.find(t => t.id === id);
            if (task) {
                task.status = 'completed';
                saveTasks();
                renderTasks();
                showToast(`🎉 恭喜完成：${task.title}`);
            }
        }

        // 删除任务
        function deleteTask(id) {
            if (confirm('确定要删除这个任务吗？')) {
                tasks = tasks.filter(t => t.id !== id);
                saveTasks();
                renderTasks();
                showToast('🗑 任务已删除');
            }
        }

        // 生成演讲提纲（使用本地模板）
        function generateSpeechForTask(taskId) {
            const task = tasks.find(t => t.id === taskId);
            if (!task) return;
            
            currentSpeechTaskId = taskId;
            document.getElementById('speechTopic').value = task.title;
            document.getElementById('speechModal').classList.add('active');
        }

        // 实际生成提纲内容
        function generateSpeechContent(topic, audience, duration) {
            // 本地模板库
            const templates = {
                '季度': generateQuarterlyReport,
                '汇报': generateReport,
                '周会': generateWeeklyReport,
                '产品': generateProductSpeech,
                '项目': generateProjectSpeech
            };
            
            // 根据关键词选择模板
            let generator = templates['季度'];
            for (let key in templates) {
                if (topic.includes(key)) {
                    generator = templates[key];
                    break;
                }
            }
            
            return generator ? generator(topic, audience, duration) : generateDefaultSpeech(topic, audience, duration);
        }
        
        // 季度汇报模板
        function generateQuarterlyReport(topic, audience, duration) {
            return `# ${topic}\n\n## 一、开场（${Math.floor(duration * 0.1)}分钟）\n大家好，今天我将为大家汇报本季度的主要工作成果。\n\n## 二、核心业绩（${Math.floor(duration * 0.4)}分钟）\n### 1. 关键指标完成情况\n- 营收增长：XX%\n- 用户增长：XX%\n- 项目完成率：XX%\n\n### 2. 重点项目进展\n- 项目A：已完成XX%\n- 项目B：预计下月上线\n\n## 三、问题与挑战（${Math.floor(duration * 0.2)}分钟）\n1. 遇到的困难\n2. 解决方案\n3. 需要的支持\n\n## 四、下季度计划（${Math.floor(duration * 0.2)}分钟）\n1. 重点工作\n2. 目标设定\n3. 资源需求\n\n## 五、结尾（${Math.floor(duration * 0.1)}分钟）\n感谢大家的聆听，欢迎提问。`;
        }
        
        // 周会模板
        function generateWeeklyReport(topic, audience, duration) {
            return `# ${topic}\n\n## 本周完成\n1. 主要工作1\n2. 主要工作2\n3. 主要工作3\n\n## 遇到的问题\n- 问题1及解决方案\n- 问题2及解决方案\n\n## 下周计划\n- 计划1\n- 计划2\n\n## 需要支持\n请相关同事协助...`;
        }
        
        // 产品发布模板
        function generateProductSpeech(topic, audience, duration) {
            return `# ${topic}\n\n## 产品背景\n我们为什么要做这个产品？\n\n## 核心功能\n1. 功能A：解决XX问题\n2. 功能B：提升XX效率\n3. 功能C：优化XX体验\n\n## 使用演示\n现场演示核心流程\n\n## 后续规划\n下一步迭代方向\n\n## Q&A`;
        }
        
        // 项目总结模板
        function generateProjectSpeech(topic, audience, duration) {
            return `# ${topic}\n\n## 项目目标回顾\n最初设定的目标是什么？\n\n## 实际成果\n- 交付内容\n- 时间把控\n- 质量评估\n\n## 经验总结\n1. 做得好的地方\n2. 需要改进的地方\n\n## 未来建议\n对后续项目的建议`;
        }
        
        // 默认模板
        function generateDefaultSpeech(topic, audience, duration) {
            return `# ${topic}\n\n## 开场（1-2分钟）\n大家好，今天我要分享的主题是《${topic}》。\n\n## 主要内容（${Math.floor(duration * 0.7)}分钟）\n### 1. 背景介绍\n为什么这个话题重要？\n\n### 2. 核心观点\n- 观点一\n- 观点二  \n- 观点三\n\n### 3. 案例分享\n具体案例说明\n\n## 总结（1-2分钟）\n回顾要点，呼吁行动\n\n## Q&A\n欢迎提问交流`;
        }

        // 保存生成的提纲
        function saveSpeechToTask(taskId, content) {
            const task = tasks.find(t => t.id === taskId);
            if (task) {
                task.speechContent = content;
                saveTasks();
                renderTasks();
                showToast('✅ 演讲提纲已生成！');
            }
        }

        // 开始演讲模式
        function startSpeech(taskId) {
            const task = tasks.find(t => t.id === taskId);
            if (!task || !task.speechContent) return;
            
            const speechMode = document.getElementById('speechMode');
            const speechContent = document.getElementById('speechContent');
            
            // 将Markdown转换为HTML
            let html = task.speechContent
                .replace(/^# (.*$)/gm, '<h1>$1</h1>')
                .replace(/^## (.*$)/gm, '<h2>$1</h2>')
                .replace(/^### (.*$)/gm, '<h3>$1</h3>')
                .replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>')
                .replace(/\n- (.*$)/gm, '<li>$1</li>')
                .replace(/\n\d\. (.*$)/gm, '<li>$1</li>')
                .replace(/<li>(.*?)<\/li>/g, '<ul><li>$1</li></ul>')
                .replace(/<\/ul><ul>/g, '')
                .replace(/\n\n/g, '</p><p>')
                .replace(/\n/g, '<br>');
            
            speechContent.innerHTML = `<p>${html}</p>`;
            speechMode.classList.add('active');
            
            // 点击内容翻页（简单滚动）
            speechContent.onclick = () => {
                window.scrollBy(0, window.innerHeight * 0.8);
            };
        }

        // ==================== 语音识别 ====================
        let recognition = null;
        let isRecording = false;

        function initSpeechRecognition() {
            if (!('webkitSpeechRecognition' in window) && !('SpeechRecognition' in window)) {
                showToast('❌ 您的浏览器不支持语音识别');
                return false;
            }
            
            const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
            recognition = new SpeechRecognition();
            recognition.lang = 'zh-CN';
            recognition.interimResults = false;
            recognition.maxAlternatives = 1;
            
            recognition.onresult = (event) => {
                const text = event.results[0][0].transcript;
                showToast(`🎤 识别结果：${text}`);
                processVoiceInput(text);
                stopRecording();
            };
            
            recognition.onerror = (event) => {
                console.error('语音识别错误', event.error);
                showToast('❌ 识别失败，请重试');
                stopRecording();
            };
            
            return true;
        }

        function startRecording() {
            if (!recognition && !initSpeechRecognition()) return;
            
            try {
                recognition.start();
                isRecording = true;
                const voiceBtn = document.getElementById('voiceBtn');
                voiceBtn.classList.add('recording');
                showToast('🎤 正在录音，请说话...');
            } catch (e) {
                console.error('启动失败', e);
                showToast('❌ 请点击允许麦克风权限');
            }
        }

        function stopRecording() {
            if (recognition && isRecording) {
                try {
                    recognition.stop();
                } catch(e) {}
                isRecording = false;
                const voiceBtn = document.getElementById('voiceBtn');
                voiceBtn.classList.remove('recording');
            }
        }

        function processVoiceInput(text) {
            // 解析时间和任务类型
            let time = null;
            let taskType = 'normal';
            let taskTitle = text;
            
            // 简单时间解析
            if (text.includes('明天')) {
                const tomorrow = new Date();
                tomorrow.setDate(tomorrow.getDate() + 1);
                time = tomorrow.toISOString();
            } else if (text.includes('今天')) {
                time = new Date().toISOString();
            }
            
            // 检查是否为演讲任务
            if (text.includes('汇报') || text.includes('演讲') || text.includes('会议') || text.includes('分享')) {
                taskType = 'speech';
            }
            
            // 提取标题（去除时间词）
            taskTitle = text.replace(/明天|今天|下午|上午|点|分/g, '').trim();
            
            createTask(taskTitle, time, taskType);
        }

        // ==================== 提醒功能 ====================
        function checkReminders() {
            const now = new Date();
            tasks.forEach(task => {
                if (task.time && task.status !== 'completed') {
                    const taskTime = new Date(task.time);
                    const diff = (taskTime - now) / 1000 / 60; // 分钟差
                    
                    if (diff <= 15 && diff > 0 && !task.reminded) {
                        task.reminded = true;
                        saveTasks();
                        
                        if (Notification.permission === 'granted') {
                            new Notification('⏰ 任务提醒', {
                                body: `任务"${task.title}"将在15分钟后开始`,
                                icon: 'https://example.com/icon.png'
                            });
                        }
                        showToast(`⏰ 提醒：${task.title} 将在15分钟后开始`);
                    }
                }
            });
            
            setTimeout(checkReminders, 60000); // 每分钟检查一次
        }

        // 请求通知权限
        function requestNotificationPermission() {
            if ('Notification' in window) {
                Notification.requestPermission();
            }
        }

        // ==================== 辅助函数 ====================
        function formatTime(isoString) {
            if (!isoString) return '未设定';
            const date = new Date(isoString);
            return `${date.getMonth()+1}月${date.getDate()}日 ${date.getHours()}:${String(date.getMinutes()).padStart(2,'0')}`;
        }

        function escapeHtml(text) {
            const div = document.createElement('div');
            div.textContent = text;
            return div.innerHTML;
        }

        function showToast(message) {
            const toast = document.createElement('div');
            toast.className = 'toast';
            toast.textContent = message;
            document.body.appendChild(toast);
            setTimeout(() => toast.remove(), 3000);
        }

        function updateDate() {
            const now = new Date();
            const options = { year: 'numeric', month: 'long', day: 'numeric', weekday: 'long' };
            document.getElementById('currentDate').textContent = now.toLocaleDateString('zh-CN', options);
        }

        // ==================== 事件绑定 ====================
        document.addEventListener('DOMContentLoaded', () => {
            updateDate();
            loadTasks();
            requestNotificationPermission();
            initSpeechRecognition();
            
            // 语音按钮
            document.getElementById('voiceBtn').onclick = startRecording;
            
            // 快速添加
            document.getElementById('quickAddBtn').onclick = () => {
                const input = document.getElementById('quickTaskInput');
                if (input.value.trim()) {
                    createTask(input.value.trim());
                    input.value = '';
                }
            };
            
            document.getElementById('quickTaskInput').onkeypress = (e) => {
                if (e.key === 'Enter') {
                    document.getElementById('quickAddBtn').click();
                }
            };
            
            // 演讲模态框
            document.getElementById('generateSpeechBtn').onclick = () => {
                const topic = document.getElementById('speechTopic').value;
                const audience = document.getElementById('speechAudience').value;
                const duration = parseInt(document.getElementById('speechDuration').value) || 10;
                
                const content = generateSpeechContent(topic, audience, duration);
                if (currentSpeechTaskId) {
                    saveSpeechToTask(currentSpeechTaskId, content);
                }
                document.getElementById('speechModal').classList.remove('active');
            };
            
            document.getElementById('closeModalBtn').onclick = () => {
                document.getElementById('speechModal').classList.remove('active');
            };
            
            // 关闭演讲模式
            document.getElementById('closeSpeechBtn').onclick = () => {
                document.getElementById('speechMode').classList.remove('active');
            };
        });
    </script>
</body>
</html>
